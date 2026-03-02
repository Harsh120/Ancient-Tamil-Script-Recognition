# Ancient-Tamil-Script-Recognition
The Ancient Tamil Script is one of the earliest known writing systems evidenced in many parts of India by Epigraphic records found on Rock edicts and Hero stones. Although these inscriptions are a rich source of history, very few applications have been developed to recognize and translate ancient Tamil Script characters. This is mainly due to the lack of proper datasets and very few experts on these long-lost ancient scripts. Furthermore, most stone inscriptions are in conditions that require Image enhancement and Noise removal after the capture of the image. The objective of this project is to develop an architecture to deal with unknown scripts in a systematic way and to curate a dataset necessary for future work. In addition, the aim of this project is to come up with an approach that overcomes the challenges of dealing with stone inscriptions to make it easier to translate them, using Convolutional Neural Networks and other Deep Learning techniques.

---

## Complete Implementation Workflow

The project is notebook-based. Files must be executed in the correct order.

---

## End-to-End Pipeline Overview

**Input:** Raw inscription image  
**Output:** Recognized character labels  

### Workflow

1. Image preprocessing (cleaning + deskew)  
2. Character segmentation (extract individual characters)  
3. (Optional) Multipart character handling  
4. Dataset preparation  
5. CNN model training  
6. Character prediction  

---

# File Execution Order & Explanation

---

## 1 `1 Image_Preprocessing.ipynb`

### When to Run
Run this first when you have a raw inscription image.

### Input Required
```
Original.jpg
```

(Place in the same directory)

### What It Does
- Deskews image  
- Applies thresholding  
- Cleans noise  

### Output Generated
```
ImagePreProcessingFinal.jpg
```

### Pipeline So Far
```
Original.jpg → ImagePreProcessingFinal.jpg
```

---

## 2 `2 character_segmentation.ipynb`

### When to Run
After preprocessing.

### Input Required
```
ImagePreProcessingFinal.jpg
```

### What It Does
- Detects contours  
- Draws bounding boxes  
- Extracts character regions  

### Output Generated
```
box.jpg
Images/roi0.png
Images/roi1.png
...
```

### Pipeline Now
```
ImagePreProcessingFinal.jpg → Images/roi*.png
```

Ensure the folder `Images/` exists before running.

---

## 3 `Image_Clustering.ipynb` (Optional)

### When to Run
Use only if you want to cluster unlabeled character images.

### Required Setup
Modify paths inside notebook:

```python
imdir = r'path_to/Images'
targetdir = r'path_to/output'
```

### What It Does
- Uses VGG16 feature extraction  
- Applies KMeans clustering  
- Groups similar characters  

### Output
Creates cluster folders inside `output/`

---

## 4 `Multi-Part/Multipart_Concatenation.ipynb` (Optional)

### When to Run
If characters are split into multiple pieces and need merging.

### Required Setup

```python
imdir = "path_to/input"
outdir = "path_to/output"
```

### Important
This notebook loads:

```
CNN.model
```

Ensure the model exists before running.

---

# Model Training Workflow

These notebooks are inside `Model-Creation/`

---

## 5 `Recognition_1.ipynb`

### When to Run
When you have a labeled dataset.

### Dataset Format Required

```
Labelled Dataset/
  class1/
    img1.JPG
  class2/
    img2.JPG
  ...
```

### Modify Path

```python
DATADIR = r'path_to/Labelled Dataset'
```

### Output Generated
- `X.pickle`
- `y.pickle`

---

## 6 `Recognition_2.ipynb`

### When to Run
After `Recognition_1.ipynb`

### What It Does
- Builds CNN model  
- Trains for 40 epochs  
- Saves trained model  

### Output Generated
- `CNN.model`
- `model.h5`

---

## 7 `Recognition_3.ipynb`

### When to Run
For prediction on new images.

### Requirements
- `CNN.model`
- Update test image path inside notebook

### What It Does
- Loads trained model  
- Predicts character class  
- Prints predicted label  

---

# Practical Run Order

## If Training Model from Scratch

1. `Recognition_1.ipynb`  
2. `Recognition_2.ipynb`  
3. `Recognition_3.ipynb`  

---

## If Recognizing from Raw Inscription Image

1. `1 Image_Preprocessing.ipynb`  
2. `2 character_segmentation.ipynb`  
3. (Optional) `Multipart_Concatenation.ipynb`  
4. Use trained model to predict each `roi*.png`  

---

# ⚙ Important Notes

- Update all placeholder paths like:
  ```
  path_to\...
  pathto\...
  ```

- Maintain consistent file names:
  ```
  Original.jpg
  ImagePreProcessingFinal.jpg
  ```

- Ensure required folders exist before writing images.

---

# Requirements

- Python 3.x  
- TensorFlow / Keras  
- OpenCV  
- NumPy  
- scikit-learn  
- glob  
- matplotlib  

### Install Dependencies

```bash
pip install tensorflow opencv-python numpy scikit-learn matplotlib
```

---

# Summary

```
Raw Image
   ↓
Preprocessing
   ↓
Segmentation
   ↓
(Optional Cleanup)
   ↓
Dataset Creation
   ↓
CNN Training
   ↓
Prediction
```

Each notebook must be executed in the correct order for successful implementation.
