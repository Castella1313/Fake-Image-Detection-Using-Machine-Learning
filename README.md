# 🕵️ Fake Image Detection Using Machine Learning

A deep learning-based image forgery detection system that uses **Error Level Analysis (ELA)** and a **Convolutional Neural Network (CNN)** to classify images as **Real** or **Fake/Manipulated**.

The project uses the **CASIA 2.0 Image Tampering Dataset** and processes images using ELA before feeding them into a CNN classifier.

---

## 📌 Project Overview

With the increasing availability of image editing tools, detecting whether an image has been digitally manipulated has become an important problem in digital forensics.

This project aims to identify manipulated images by analyzing compression-level inconsistencies using **Error Level Analysis (ELA)**.

The processed ELA images are resized to **128 × 128 pixels** and classified using a CNN into two categories:

- 🟢 **Real** — Original/authentic image
- 🔴 **Fake** — Tampered/manipulated image

---

## 🔍 How It Works

The project follows the following pipeline:

```text
Input Image
     ↓
Error Level Analysis (ELA)
     ↓
Resize to 128 × 128
     ↓
Normalize Pixel Values
     ↓
CNN Model
     ↓
Classification
     ↓
Real / Fake + Confidence
```

### 1. Error Level Analysis

ELA works by resaving an image at a specific JPEG quality and comparing the original image with the recompressed version.

The difference between the two images is amplified to highlight areas that may have been manipulated.

The project uses a JPEG quality of **90** for ELA processing.

### 2. Image Preprocessing

ELA images are:

- Resized to **128 × 128**
- Converted into NumPy arrays
- Normalized by dividing pixel values by 255
- Reshaped into CNN-compatible input dimensions



### 3. Dataset

The project uses the **CASIA2** dataset.

The dataset is divided into:

| Folder | Description | Label |
|---|---|---:|
| `Au` | Authentic images | `1` |
| `Tp` | Tampered/manipulated images | `0` |

The implementation samples **2,100 authentic images** and uses the tampered images available in the `Tp` directory.

---

## 🧠 CNN Architecture

The classification model is built using TensorFlow/Keras.

The architecture consists of:

```text
Input: 128 × 128 × 3
        ↓
Conv2D – 32 filters, 5 × 5
        ↓
Conv2D – 32 filters, 5 × 5
        ↓
MaxPooling – 2 × 2
        ↓
Dropout – 25%
        ↓
Flatten
        ↓
Dense – 256 neurons
        ↓
Dropout – 50%
        ↓
Dense – 2 neurons
        ↓
Softmax
```

The model architecture is implemented using Keras `Sequential`, `Conv2D`, `MaxPool2D`, `Dropout`, `Flatten`, and `Dense` layers.

---

## ⚙️ Training Configuration

The model is trained using the following configuration:

| Parameter | Value |
|---|---:|
| Image Size | 128 × 128 |
| Batch Size | 32 |
| Epochs | 5 |
| Initial Learning Rate | 0.0001 |
| Optimizer | Adam |
| Learning Rate Schedule | Exponential Decay |
| Validation Split | 20% |
| Random State | 5 |
| Loss Function | Binary Cross-Entropy |

The learning rate uses an exponential decay schedule with a decay rate of `0.9`.

---

## 📊 Model Evaluation

The project evaluates the trained model using:

### Accuracy

The model calculates prediction accuracy for both:

- Fake/tampered images
- Real/authentic images
- Overall dataset

### Confusion Matrix

A confusion matrix is generated to visualize:

- Correctly classified real images
- Incorrectly classified real images
- Correctly classified fake images
- Incorrectly classified fake images



### Training Curves

Training and validation:

- Loss
- Accuracy

are plotted to visualize model performance during training.

---

## 🔮 Image Prediction

After training, the model can classify an individual image.

The prediction returns:

```text
Class: real
Confidence: XX.XX%
```

or

```text
Class: fake
Confidence: XX.XX%
```

The project defines the classes as:

```python
class_names = ['fake', 'real']
```

and uses the softmax output to determine the predicted class and confidence score.

---

## 🛠️ Technologies Used

- **Python**
- **TensorFlow**
- **Keras**
- **NumPy**
- **Matplotlib**
- **Scikit-learn**
- **Pillow (PIL)**
- **Google Colab**
- **CASIA2 Dataset**

---

## 📦 Installation

Clone the repository:

```bash
git clone https://github.com/<your-username>/<your-repository>.git
cd <your-repository>
```

Install the required Python packages:

```bash
pip install numpy matplotlib scikit-learn tensorflow pillow
```

---

## 📁 Dataset Setup

This project was developed using the **CASIA2 dataset**.

The code expects the dataset to contain the following directories:

```text
CASIA2/
├── Au/
│   ├── authentic_image_1.jpg
│   ├── authentic_image_2.jpg
│   └── ...
│
└── Tp/
    ├── tampered_image_1.jpg
    ├── tampered_image_2.jpg
    └── ...
```

The original implementation accesses the dataset through Google Drive in Google Colab.

You will need to update the dataset paths in the Python file according to your local environment.

---

## ▶️ Running the Project

### Using Google Colab

1. Upload/open the Python code in Google Colab.
2. Upload the CASIA2 dataset to Google Drive.
3. Mount Google Drive.
4. Update the dataset paths if required.
5. Run the notebook/script.
6. The model will preprocess the images using ELA.
7. The CNN will train on the processed dataset.
8. Training metrics and the confusion matrix will be generated.
9. The trained model can then be used for image prediction.

---

## 💾 Model Output

The trained model is saved as:

```text
model_casia_run1.h5
```

The implementation saves the trained Keras model after training.

---

## 📈 Evaluation Results

The project evaluates the model on both fake and real image sets and reports:

```text
Total
Correct
Accuracy
```

for each category, followed by an overall accuracy calculation.

> **Note:** The repository should only list a specific accuracy percentage if it has been obtained from an actual execution of the model. The uploaded source code itself does not contain a fixed final accuracy result.

---

## 🎯 Applications

This type of image forgery detection system can be useful for:

- 📰 Digital media verification
- 🔎 Digital forensics
- 📱 Social media content verification
- 🛡️ Fake image detection
- 🖼️ Image authenticity analysis
- 🔐 Cybersecurity and misinformation analysis

---

## 🚀 Future Improvements

Possible improvements to the project include:

- Use a larger and more balanced training dataset
- Add data augmentation
- Experiment with deeper CNN architectures
- Compare CNN performance with transfer-learning models such as ResNet or EfficientNet
- Add precision, recall, and F1-score
- Add ROC-AUC evaluation
- Build a web interface for uploading images
- Provide visual ELA heatmaps alongside predictions
- Improve inference speed
- Test the model on datasets other than CASIA2
- Add automated batch prediction
- Deploy the trained model as an API

---

## ⚠️ Limitations

The model's performance depends heavily on the dataset and preprocessing technique used.

ELA-based detection may not reliably identify every type of image manipulation, particularly when images have undergone additional compression, resizing, screenshots, or other transformations.

Therefore, a prediction should be treated as an **indication of possible manipulation rather than definitive proof of image authenticity**.

---

## 📚 Dataset

This project uses the **CASIA Image Tampering Dataset (CASIA 2.0)** for training and evaluation.

The implementation distinguishes authentic images from tampered images using the dataset's `Au` and `Tp` directories.

---

## 👨‍💻 Project Structure

A suggested repository structure is:

```text
Fake-Image-Detection/
│
├── fake_image_detection_using_ML.py
├── README.md
├── requirements.txt
├── model/
│   └── model_casia_run1.h5
│
├── results/
│   ├── confusion_matrix.png
│   └── training_curves.png
│
└── dataset/
    └── CASIA2/
        ├── Au/
        └── Tp/
```

> The dataset and trained model can be excluded from GitHub if they are too large or subject to redistribution restrictions.

---

## ⭐ Key Takeaway

This project demonstrates how **Error Level Analysis combined with Convolutional Neural Networks** can be used as an approach for detecting manipulated images.

The system converts images into ELA representations, extracts visual patterns using a CNN, and predicts whether an image is **fake or real**, along with a confidence score.

---

## 📄 License

Add an appropriate license to this repository depending on how you intend to distribute and use the project.

---

## 🙌 Acknowledgements

- **CASIA Image Tampering Dataset**
- **TensorFlow / Keras**
- **Scikit-learn**
- **Pillow**
- **Google Colab**
