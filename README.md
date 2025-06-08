<h1 align="left">Hey </h1>

###

<p align="left">My name is Srinidhi and I'm a Full Stack Developer</p>

###

<h2 align="left">About My Project</h2>
 Universal Object Analyzer

## Overview

In many real-world scenarios, identifying objects with **accurate semantic detail**—such as **color, shape, category, and weight**—remains a challenge for AI systems.  
Existing object detection solutions typically focus on object localization and classification but lack **multi-dimensional descriptive capabilities**. This limits their usability in domains like **assistive technology** for visually impaired users, **robotics**, and **context-aware AI applications**.

The **Universal Object Analyzer** addresses this gap by integrating multiple AI components into a single, real-time, multimodal object analysis pipeline.

---

## Solution Pipeline

- 🟢 **YOLOv8**  
  Detects and localizes objects in real time using bounding boxes.

- 🟣 **CLIP (Contrastive Language-Image Pretraining)**  
  Performs zero-shot classification to identify objects from a set of candidate labels.

- 🟡 **KMeans Clustering**  
  Detects the object’s **dominant color**.

- 🟠 **OpenCV**  
  Analyzes the object’s **geometric shape** through contour detection.

- 🔵 **Estimated Weight Lookup**  
  Retrieves an estimated weight from a predefined lookup table based on the identified object.

- 🔊 **gTTS (Google Text-to-Speech)**  
  Converts the object description into **audio output**.

- 🌐 **Gradio Web Interface**  
  Provides an intuitive web-based interface allowing users to upload or capture images and view both **text and audio descriptions** of detected objects.

---

## Features

✅ Real-time object detection and localization  
✅ Zero-shot object classification  
✅ Dominant color extraction  
✅ Geometric shape analysis  
✅ Estimated object weight  
✅ Audio output for accessibility  
✅ User-friendly web interface (local or cloud deployment)  

---

## Applications

- **Assistive Technology** for visually impaired users  
- **Robotics** and human-robot interaction  
- **Smart devices** and home automation  
- **Interactive AI agents**  
- **Educational tools**  

---

## Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/universal-object-analyzer.git
cd universal-object-analyzer

# Create a virtual environment (optional but recommended)
python -m venv venv
source venv/bin/activate  # Linux/Mac
venv\Scripts\activate     # Windows

# Install dependencies
pip install -r requirements.txt


###

<h2 align="left">I code with</h2>

###

<div align="left">
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/javascript/javascript-original.svg" height="40" alt="javascript logo"  />
  <img width="12" />
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/typescript/typescript-original.svg" height="40" alt="typescript logo"  />
  <img width="12" />
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/react/react-original.svg" height="40" alt="react logo"  />
  <img width="12" />
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/nextjs/nextjs-original.svg" height="40" alt="nextjs logo"  />
  <img width="12" />
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/storybook/storybook-original.svg" height="40" alt="storybook logo"  />
  <img width="12" />
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/nodejs/nodejs-original.svg" height="40" alt="nodejs logo"  />
  <img width="12" />
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/nestjs/nestjs-original.svg" height="40" alt="nestjs logo"  />
  <img width="12" />
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/jest/jest-plain.svg" height="40" alt="jest logo"  />
</div>

###
