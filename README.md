# Tumor-Detection-Model
# Brain Tumor Detection

A Flask web app that classifies brain MRI scans into one of four categories — **pituitary**, **glioma**, **meningioma**, or **no tumor** — using a CNN trained in Keras/TensorFlow.

## Demo
<img width="1920" height="1080" alt="Screenshot (199)" src="https://github.com/user-attachments/assets/20c83c26-fef6-4923-a8a2-1ac5f074d721" />
<img width="1920" height="1080" alt="Screenshot (200)" src="https://github.com/user-attachments/assets/9b7bb51c-0873-4e53-b2b8-78c40eae9c0c" />


## Project Structure

```
TumorDetection/
├── main.py                  # Flask app — loads the model and serves predictions
├── requirements.txt         # Python dependencies
├── model_training.ipynb     # Jupyter notebook used to train the model
├── templates/
│   └── index.html           # Web page (must be in this exact folder for Flask to find it)
├── models/
│   └── model.keras          # Trained model file (NOT included in this repo — see below)
└── uploads/                 # Created automatically at runtime for uploaded scans
```

## Setup

**1. Clone the repo**

```
git clone <your-repo-url>
cd TumorDetection
```

**2. Create and activate a virtual environment**

Python 3.11 is recommended (some dependencies don't yet have Windows wheels for 3.12).

```
py -3.11 -m venv .venv
.venv\Scripts\activate
```

**3. Install dependencies**

```
pip install -r requirements.txt
```

## Getting the Model File

`models/model.keras` is **not included in this repository** because it's too large for a normal GitHub push.

You have two options:

**Option A — Download the pre-trained model**
Download it from here: [Google Drive](https://drive.google.com/drive/folders/1EEADZfc25Ck9x40uij2pAQ_yS-8vqq1z?usp=sharing)
Click into the folder, then click `model.keras` and hit **Download** at the top. Once downloaded, place it at `models/model.keras` in the project folder.

**Option B — Train it yourself**
1. Download the training dataset (~7,200 MRI images) from here: [Google Drive](https://drive.google.com/drive/folders/1Pm831NN-PnxvSUv1bH8K_dfMk0vpZ5ay?usp=sharing) and place the images where `model_training.ipynb` expects them (check the notebook's data-loading cell for the exact folder path it looks for).
2. Open `model_training.ipynb` in Jupyter Notebook or JupyterLab (`jupyter notebook`, then open the file in your browser).
3. Run all cells in order. This trains the model on the dataset and, at the final cell, saves it out.
4. Make sure the notebook's save step points to `models/model.keras` — for example:
   ```python
   import os
   os.makedirs('models', exist_ok=True)
   model.save('models/model.keras')
   ```
5. Once it finishes, confirm `models/model.keras` exists before running the app.

## Running the App

```
python main.py
```

You should see `Running on http://127.0.0.1:5000` in the terminal. Open that address in your browser, upload an MRI scan, and it'll return a prediction with a confidence score.

## Troubleshooting

- **`No matching distribution found for tensorflow-io-gcs-filesystem`** — this package has no Windows wheel for Python 3.12. Either use Python 3.11 (see Setup above), or comment out that line in `requirements.txt` and reinstall.
- **`TemplateNotFound: index.html`** — `index.html` must be inside a folder literally named `templates`, not loose in the project root.
- **`GlorotUniform.__init__() got an unexpected keyword argument`** when loading the model — your installed Keras/TensorFlow version is older than the one the model was saved with. Run `pip install --upgrade tensorflow keras` and try again.
