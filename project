import cv2
import numpy as np
from PIL import Image
from gtts import gTTS
import tempfile
import os
from sklearn.cluster import KMeans
import webcolors
from ultralytics import YOLO
import gradio as gr
import torch
from transformers import CLIPProcessor, CLIPModel

# Step 3: Load models
yolo_model = YOLO("yolov8n.pt")
clip_model = CLIPModel.from_pretrained("openai/clip-vit-base-patch32")
clip_processor = CLIPProcessor.from_pretrained("openai/clip-vit-base-patch32")

# Step 4: Utility functions for color and shape detection
def closest_color(requested_color):
    min_colors = {}
    for key, name in webcolors.CSS3_HEX_TO_NAMES.items():
        r_c, g_c, b_c = webcolors.hex_to_rgb(key)
        rd = (r_c - requested_color[0]) ** 2
        gd = (g_c - requested_color[1]) ** 2
        bd = (b_c - requested_color[2]) ** 2
        min_colors[(rd + gd + bd)] = name
    return min_colors[min(min_colors.keys())]

def get_dominant_color(image):
    img = cv2.resize(image, (50, 50))
    img = img.reshape((img.shape[0] * img.shape[1], 3))
    kmeans = KMeans(n_clusters=1, random_state=0)
    kmeans.fit(img)
    dominant_rgb = kmeans.cluster_centers_[0].astype(int)
    try:
        color_name = closest_color(tuple(dominant_rgb))
    except:
        color_name = f"RGB({dominant_rgb[0]}, {dominant_rgb[1]}, {dominant_rgb[2]})"
    return color_name

def detect_shape(image):
    gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
    blur = cv2.GaussianBlur(gray, (5, 5), 0)
    _, thresh = cv2.threshold(blur, 60, 255, cv2.THRESH_BINARY + cv2.THRESH_OTSU)
    contours, _ = cv2.findContours(thresh, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
    if not contours:
        return "unknown shape"
    c = max(contours, key=cv2.contourArea)
    approx = cv2.approxPolyDP(c, 0.04 * cv2.arcLength(c, True), True)
    sides = len(approx)
    if sides == 3:
        return "triangle"
    elif sides == 4:
        return "square or rectangle"
    elif sides > 6:
        return "circle or oval"
    else:
        return "irregular shape"

# Step 5: Zero-shot classification with CLIP
def clip_zero_shot_classify(image_crop, candidate_labels):
    image = Image.fromarray(cv2.cvtColor(image_crop, cv2.COLOR_BGR2RGB))
    inputs = clip_processor(text=candidate_labels, images=image, return_tensors="pt", padding=True)
    outputs = clip_model(**inputs)
    logits_per_image = outputs.logits_per_image
    probs = logits_per_image.softmax(dim=1)
    best_idx = probs.argmax().item()
    return candidate_labels[best_idx]

# Estimated weights (in grams) for sample objects
estimated_weights_lookup = {
    "apple": "150g",
    "banana": "120g",
    "book": "400g",
    "chair": "3000g",
    "tv": "5000g",
    "pen": "10g",
    "bench": "8000g",
    "bottle": "500g",
    "laptop": "2000g",
    "remote": "150g",
    "cell phone": "180g",
    "cup": "250g",
    "keyboard": "600g",
    "backpack": "800g",
    "suitcase": "3000g",
    "chocolate": "100g",
    "table": "7000g",
    "dog": "10000g",
    "cat": "4000g",
    "phone": "180g",
    "screen": "4500g",
    "wall": "N/A",
    "car": "1500000g",
    "fruit": "200g",
    "vegetable": "150g",
    "mouse": "100g",
    "shoe": "700g",
    "hat": "200g",
    "bicycle": "12000g",
    "clock": "800g",
    "glasses": "100g",
    "flower": "50g"
}

def get_estimated_weight(label):
    return estimated_weights_lookup.get(label.lower(), "unknown")

# Step 6: Full analysis function
def analyze(image_pil):
    img_cv = cv2.cvtColor(np.array(image_pil), cv2.COLOR_RGB2BGR)
    results = yolo_model(img_cv)[0]

    if len(results.boxes) == 0:
        return "No object detected.", None

    candidate_labels = [
        "apple", "banana", "book", "chair", "tv", "pen", "bench", "bottle", "laptop",
        "remote", "cell phone", "cup", "keyboard", "backpack", "suitcase", "chocolate",
        "bottle", "table", "dog", "cat", "phone", "screen", "wall", "car", "fruit",
        "vegetable", "mouse", "shoe", "hat", "bicycle", "clock", "glasses", "flower"
    ]

    descriptions = []
    audio_files = []

    for box in results.boxes:
        x1, y1, x2, y2 = map(int, box.xyxy[0])
        obj_crop = img_cv[y1:y2, x1:x2]
        if obj_crop.size == 0:
            continue

        label = clip_zero_shot_classify(obj_crop, candidate_labels)
        color = get_dominant_color(obj_crop)
        shape = detect_shape(obj_crop)
        weight = get_estimated_weight(label)

        description = f"object: {label}\ncolor: {color}\nshape: {shape}\nestimated weight: {weight}"
        descriptions.append(description)

        tts = gTTS(description)
        tmp_audio = tempfile.NamedTemporaryFile(delete=False, suffix=".mp3")
        tts.save(tmp_audio.name)
        audio_files.append(tmp_audio.name)

    combined_desc = "\n\n".join(descriptions)
    return combined_desc, audio_files[0]

# Step 7: Gradio app interface
app = gr.Interface(
    fn=analyze,
    inputs=gr.Image(type="pil", label="Upload or capture an object image"),
    outputs=[
        gr.Textbox(label="Object Description"),
        gr.Audio(label="Audio Description")
    ],
    title="Universal Object Analyzer",
    description="Upload any object image and get its name, color, shape, estimated weight, and hear the description."
)

app.launch()
