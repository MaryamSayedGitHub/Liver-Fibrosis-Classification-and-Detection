# 🩺 Liver Fibrosis Detection & Classification System

A deep learning-based system that automatically **classifies liver fibrosis stages** from medical imaging data (Ultrasound & CT scans), built as a Streamlit web application.

---

## 👥 Team Members

| Name |
|------|
| Fatma El-Zahraa Mahmoud Hamed |
| Gehad El-Sayed Mahmoud |
| Farah Hussein Mohammed |
| Nada Azab Mohamed |
| Mariem Sayed Ahmed |
| Fatma El-Zahraa Gamal Hussein |

---

## 📌 Project Overview

Liver fibrosis is the buildup of scar tissue in the liver caused by repeated damage from:
- Viral hepatitis (Hepatitis B, C)
- Alcohol abuse
- Non-alcoholic fatty liver disease (NAFLD)
- Autoimmune liver diseases

If left undetected, fibrosis can progress to **cirrhosis**, **liver failure**, or **liver cancer**.

Traditional diagnosis relies on invasive **liver biopsies** — painful, costly, and prone to sampling errors. This system provides a **non-invasive, automated alternative** using deep learning on ultrasound and CT images.

---

## 🎯 Objective

Build a system to help liver patients by **detecting and classifying liver cirrhosis** using deep learning models on Ultrasound and CT images.

---

## 🏗️ System Architecture

```
Input Image → Preprocessing → Deep Learning Model → Classification / Segmentation Output
     ↑                                                          ↓
  Dataset                                              Streamlit UI / FastAPI
```

**Layers:**
- **User Interface** — Streamlit web app / Flutter mobile app
- **Application Layer** — FastAPI REST API deployed on Hugging Face
- **Data Access Layer** — Trained model (.h5) + preprocessing pipeline

---

## 🧠 Models

### Classification Models (Ultrasound Images)

| Model | Best Val Accuracy | Best Parameters |
|-------|:-----------------:|-----------------|
| **DenseNet-121** ⭐ | **98.50%** | batch=8, epochs=30, optimizer=adam, lr=0.00005 |
| VGG-19 | 98.11% | batch=16, epochs=30, optimizer=adam, lr=0.0001 |
| InceptionResNetV2 | 97.95% | batch=8, epochs=10, optimizer=adam, lr=0.0001 |
| ResNet-50 | 97.63% | batch=16, epochs=10, optimizer=adam |
| NasNetMobile | 95.74% | batch=16, epochs=30, optimizer=adam |

### Segmentation Model (CT Images)

| Model | Val Accuracy | Parameters |
|-------|:-----------:|------------|
| UNet++ (EfficientNet-B5) | 97.90% | batch=32, epochs=10, optimizer=RMSprop |
| UNet++ (ResNeXt50) | 97.60% | batch=32, epochs=10, optimizer=RMSprop |

---

## 📂 Datasets

### Ultrasound Dataset

| Stage | Description | Initial Samples | Train | Validation |
|-------|-------------|:--------------:|:-----:|:----------:|
| F0 | No fibrosis | 2114 | 3382 | 423 |
| F1 | Mild fibrosis | 861 | 1376 | 173 |
| F2 | Moderate fibrosis | 793 | 1268 | 159 |
| F3 | Severe fibrosis | 857 | 1370 | 172 |
| F4 | Cirrhosis | 1698 | 2716 | 340 |

### CT Dataset

| Split | Samples |
|-------|:-------:|
| Total | 58,638 |
| Training (80%) | 46,910 |
| Validation (10%) | 5,863 |
| Testing (10%) | 5,865 |

Data type: 2D slices extracted from 3D CT scans, includes masks for liver, tumor, bone, arteries, and kidneys.

---

## ⚙️ Preprocessing

### Ultrasound Images
1. **RGB Conversion** — ensure consistent 3-channel format
2. **Resizing** — 224×224 pixels
3. **Normalization** — pixel values scaled from [0, 255] → [0, 1]
4. **Data Augmentation** — horizontal flipping to increase diversity and prevent overfitting
5. **Class Balancing** — underrepresented classes augmented more
6. **Train/Validation Split** — 80% / 20% (validation not augmented)

### CT Images
1. **Resizing** — 224×224 pixels
2. **Normalization** — pixel values scaled to [0, 1]
3. **Mask Conversion** — binary format
4. **Train/Val/Test Split** — 80% / 10% / 10%

---

## 🚀 Getting Started

### Prerequisites

```bash
pip install streamlit tensorflow pillow numpy
```

### Project Structure

```
liver-fibrosis-classifier/
├── liver_fibrosis_app.py       # Main Streamlit application
├── README.md
└── models/
    ├── DenseNet121_model.h5
    ├── VGG19_model.h5
    ├── InceptionResNetV2_model.h5
    ├── ResNet50_model.h5
    ├── NasNetMobile_model.h5
    └── UNetPP_EfficientB5_model.h5
```

### Run the App

```bash
streamlit run liver_fibrosis_app.py
```

---

## 🖥️ How It Works

1. Select a model from the sidebar
2. Upload a liver **ultrasound image** (JPG, JPEG, PNG) — or a CT slice for UNet++
3. The model preprocesses and analyzes the image
4. Results displayed:
   - **Classification** → predicted fibrosis stage (F0–F4) + confidence + per-class probabilities
   - **Segmentation** → predicted liver mask overlay

---

## 🔬 Fibrosis Stages

| Stage | Label | Description |
|-------|-------|-------------|
| F0 | 🟢 No Fibrosis | No scarring. Liver appears healthy. |
| F1 | 🟡 Mild Fibrosis | Scarring confined to portal areas. |
| F2 | 🟠 Moderate Fibrosis | Scarring spread around portal areas. |
| F3 | 🔴 Severe Fibrosis | Bridging fibrosis across liver sections. |
| F4 | 🔴 Cirrhosis | Advanced scarring. Liver function impaired. |

---

## 🛠️ Technologies Used

| Category | Tools |
|----------|-------|
| Deep Learning | TensorFlow / Keras, PyTorch |
| Image Processing | OpenCV, PIL, torchvision |
| Segmentation | segmentation_models_pytorch |
| Web App | Streamlit |
| Mobile App | Flutter |
| Backend API | FastAPI |
| Deployment | Hugging Face, Oracle APEX |
| Evaluation | scikit-learn |

---

## 📈 Performance Criteria

- **Accuracy** — ratio of correctly predicted instances to total predictions (classification)
- **Dice Similarity Coefficient (Dice Score)** — measures overlap between predicted and actual liver region (segmentation)

---

## 🔭 What's Next

- Collect real-world data from **Egyptian hospitals** to train models on local datasets reflecting patient diversity and imaging conditions
- Develop a **fully featured mobile application** (Flutter) with all web platform functionalities for clinicians and patients

---

## 📚 References

1. H.-C. Park et al., "Automated classification of liver fibrosis stages using ultrasound imaging," *BMC Medical Imaging*, vol. 24, no. 36, 2024.
2. Y. Joo et al., "Classification of Liver Fibrosis from Heterogeneous Ultrasound Image," *IEEE Access*, vol. 11, pp. 9920–9930, Jan. 2023.
3. M. Al-Hasani et al., "Ultrasound Radiomics for the Detection of Early-Stage Liver Fibrosis," *Diagnostics*, vol. 12, no. 11, Nov. 2022.
4. J.H. Lee et al., "Deep learning with ultrasonography: automated classification of liver fibrosis using a deep convolutional neural network," *Eur. Radiol.*, vol. 30, pp. 1264–1273, 2020.
5. D. Meng et al., "Liver Fibrosis Classification Based on Transfer Learning and FCNet for Ultrasound Images," *IEEE Access*, Mar. 2017.
6. G. N. Yashaswini et al., "Deep learning technique for automatic liver and liver tumor segmentation in CT images," *Journal of Liver Transplantation*, vol. 17, Feb. 2025.
