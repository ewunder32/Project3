# Shark Week! Using a CNN to Classify Shark Species

This repository documents our full workflow for building, evaluating, and interpreting a convolutional neural network (CNN) that classifies shark species from images and links predictions to IUCN Red List conservation status.

---

## Repository Contents

This repository is organized following a simplified TIER-style structure to make the project easy to understand and fully reproducible:

- **DATA/** – Raw and processed data plus data documentation (metadata).
- **SCRIPTS/** – All analysis code, organized in the order needed to reproduce results.
- **OUTPUT/** – Saved figures, tables, and any derived artifacts used in our final presentation.
- **LICENSE.txt** – License for using and citing this repository.
- **README.md** – You are here: orientation, software/platform info, project map, and reproduction instructions.

---

## Section 1 – Software and Platform

This section lists the software, libraries, and platforms required to run the analysis.

### 1.1 Core Software

- **Programming language:** Python 3.11  
- **Interactive environment:** Google Colab

### 1.2 Python Packages

The analysis uses the following Python packages:

- `tensorflow` / `keras` – build and train the CNN
- `numpy` – numerical operations
- `pandas` – tabular data handling
- `matplotlib` – plotting
- `seaborn` – statistical visualization (EDA plots)
- `scikit-learn` – metrics (accuracy, precision, recall, F1-score), potential utilities
- `os`, `pathlib`, `glob` – file and path handling

> If a `requirements.txt` file is present in the repository, you can install everything at once using:
>
> ```bash
> pip install -r requirements.txt
> ```

### 1.3 Platform

- Development and testing were performed in a **Jupyter notebook environment** (Google Colab, Linux backend).  
- The code should run on **any OS** (Linux, macOS, or Windows) with Python 3.11 and the above packages installed.

---

## Section 2 – Map of the Project Folder

This section provides a tree-style map of the repository and a brief description of each folder/file.

```
Project3/
├── DATA/SHARKS_RAYS_CHIMAERAS/
│   ├──  README.md # Data summary, provenance, license, ethical statements, data dictionary, and explanatory plots description.
│   │   
│   └── SHARKS_RAYS_CHIMAERAS.dbf # Tabular dataset with image-level metadata (e.g., label,     width, height, contrast/brightness summaries, IUCN status).
│                                  
│
├── OUTPUT/
│   ├── Data Dictionary.png # Visual table summarizing variables, types, and example values.
│   │   
│   ├── Distribution of Contrast per Shark Species.png # Boxplot of image contrast (std. dev. of pixel intensities) stratified by shark species
│   │   
│   │  
│   ├── Distribution of Image Brightness per Shark Species.png # Boxplot of mean pixel intensity (brightness) per shark species.
│   │   
│   └── Distribution of Images Across All Unique Shark Species.png # Bar chart showing class counts (number of images) for each species.
│      
│
├── SCRIPTS/
│    └── Project3.ipynb # Full dataset creation + analysis pipeline  
│               
│
├── LICENSE.txt # MIT or other selected license for reuse and citation of this repository.
│   
│
└── README.md # Top-level orientation and reproduction instructions (this file).
    
```


## Section 3 – Instructions for Reproducing Results

This section explains, step-by-step, how someone can use this repository to reproduce our exploratory data analysis, CNN training, and IUCN-conservation integration results. The instructions assume a Google Colab or Jupyter environment, but the same steps work locally with minor path changes.

### 3.1 Set Up the Environment

1. **Clone or download the repository**
   - Clone via Git:
     ```bash
     git clone <URL-of-this-repo>.git
     cd Project3
     ```
   - Or download the ZIP from GitHub and unzip it so that the folder structure matches the map in Section 2.

2. **Open a Python environment**
   - Recommended: **Google Colab** with GPU enabled  
     - In Colab: *Runtime → Change runtime type → Hardware accelerator → GPU*.
   - Alternative: local Jupyter Notebook with Python 3.x.

3. **Install required packages**
   - If you are running locally, open a terminal in the project folder and install:
     ```bash
     pip install tensorflow numpy pandas matplotlib seaborn scikit-learn roboflow dbfread
     ```
   - In Colab, these installations are handled by the first cells of the main notebook (see Step 4).

### 3.2 Download the Shark Image Dataset from Roboflow

4. **Open the main analysis notebook**
   - In the `SCRIPTS/` folder, open the primary notebook (e.g., `Project3.ipynb`).

5. **Get a Roboflow API key**
   - Create a free Roboflow account and obtain an API key from your account settings.
   - In the first code cell of the notebook (the one that runs `!pip install roboflow` and initializes `Roboflow`), replace the placeholder key with **your own**:
     ```python
     from roboflow import Roboflow
     rf = Roboflow(api_key="YOUR_API_KEY_HERE")
     project = rf.workspace("project3-zhzbg").project("shark-species-dataset-818gy")
     version = project.version(1)
     dataset = version.download("tensorflow")
     ```
   - Run this cell. Roboflow will download the dataset into a directory named something like:
     ```text
     /content/Shark-Species-Dataset-1
     ```
     with the standard `train/`, `valid/`, and `test/` subfolders and `_annotations.csv` files.

6. **Confirm paths**
   - Run the next cells that set:
     ```python
     dataset_path = "/content/Shark-Species-Dataset-1"
     train_dir = os.path.join(dataset_path, "train")
     valid_dir = os.path.join(dataset_path, "valid")
     test_dir  = os.path.join(dataset_path, "test")
     ```
   - If you are not using Colab, update `dataset_path` so it points to the location where Roboflow downloaded the dataset on your machine.

### 3.3 Reproduce the CNN Training and Evaluation

7. **Create image data generators**
   - Run the cell that:
     - Reads `_annotations.csv` into `train_df`, `valid_df`, `test_df` with `pandas`.
     - Defines `ImageDataGenerator` objects with rescaling and augmentation (rotation, zoom, horizontal flip).
     - Builds `train_generator`, `valid_generator`, and `test_generator` via `.flow_from_dataframe(...)` using:
       - `target_size = (128, 128)`
       - `batch_size = 32`
       - `class_mode = 'categorical'`.
   - This step links each image file to its shark species label and prepares batches for the CNN.

8. **Define the CNN architecture**
   - Run the cell under “Model for classification” that builds a `Sequential` model with:
     - Three `Conv2D` layers (filters 32, 64, 128) with ReLU activations and `MaxPooling2D` between them.
     - `Flatten()` followed by a dense hidden layer with 128 units.
     - Output dense layer with `num_classes = len(train_generator.class_indices)` and `softmax` activation.
   - The cell also compiles the model (e.g., with `categorical_crossentropy` loss and `adam` optimizer).

9. **Handle common-class filtering**
   - Run the cells that:
     - Identify the set of classes common to both training and test splits.
     - Create filtered generators `train_generator_filtered`, `valid_generator_filtered`, and `test_generator_filtered` that only include these common classes.
   - This step ensures that evaluation is well-defined even if some species appear only in one split.

10. **Train the model**
    - Run the training cell:
      ```python
      history = model.fit(
          train_generator_filtered,
          steps_per_epoch=train_generator_filtered.samples // batch_size,
          epochs=EPOCHS,                      # e.g., 10–20
          validation_data=valid_generator_filtered,
          validation_steps=valid_generator_filtered.samples // batch_size
      )
      ```
    - Keep `EPOCHS` as in the notebook to reproduce our results (or note any changes).
    - This cell will print training and validation accuracy/loss per epoch.

11. **Evaluate and generate quantitative results**
    - Run the evaluation cell that:
      - Uses `model.evaluate(...)` on `test_generator_filtered` to compute test loss and accuracy.
      - Uses `model.predict(...)` plus `np.argmax` to obtain predicted class indices.
      - Computes a `classification_report` and `confusion_matrix` from `sklearn.metrics`.
    - Save or screenshot these outputs; they reproduce the core numeric results we report for model performance.

### 3.4 Reproduce Conservation-Status Integration

12. **Inspect or load the IUCN metadata**
    - Our repository includes `DATA/SHARKS_RAYS_CHIMAERAS/SHARKS_RAYS_CHIMAERAS.dbf`, which contains species-level metadata (including IUCN status).
    - In the notebook, run the cell that uses the `dbf` library to open this file (or adjust the path to match where you mounted the repository in Colab). This is mainly for inspection and validation of metadata.

13. **Link model predictions to IUCN Red List categories**
    - Run the cells under “more conservation connection” that:
      - Define the `conservation_info` dictionary mapping common shark species names to:
        - `status` (e.g., “Endangered”, “Near Threatened”)
        - `threats` (short text description)
      - Loop over the filtered test filenames and their predicted classes.
      - For each predicted species, look up its IUCN status and threats and print a readable summary.
    - These cells reproduce the conservation-oriented interpretation of the model’s outputs.

### 3.5 Reproduce Exploratory Data Analysis (Brightness, Contrast, and Class Counts)

14. **Compute image-level brightness and contrast**
    - Run the cells that:
      - Iterate through each image in the Roboflow dataset.
      - Convert the image to a NumPy array and compute:
        - **Brightness**: mean pixel intensity.
        - **Contrast**: standard deviation of pixel intensities.
      - Store results in an `image_metrics` dictionary keyed by species.

15. **Aggregate metrics by species**
    - Run the cell that builds `metrics_df` from `image_metrics`, which contains one row per image with:
      - Species name
      - Brightness
      - Contrast
    - This DataFrame is the basis for the EDA plots.

16. **Generate EDA plots and save them to `OUTPUT/`**
    - Run the plotting cells that use `matplotlib` and `seaborn` to create:
      - A bar chart for the **Distribution of Images Across All Unique Shark Species**.
      - A boxplot of **Brightness per Shark Species**.
      - A boxplot of **Contrast per Shark Species**.
    - To reproduce the figures stored in `OUTPUT/`, either:
      - Add `plt.savefig("OUTPUT/<exact-file-name>.png", dpi=300, bbox_inches="tight")` right before each `plt.show()` call, **or**
      - Use your notebook interface’s “Save image” option and manually save the PNGs into the `OUTPUT/` folder with the following filenames:
        - `OUTPUT/Distribution of Images Across All Unique Shark Species.png`
        - `OUTPUT/Distribution of Image Brightness per Shark Species.png`
        - `OUTPUT/Distribution of Contrast per Shark Species.png`

### 3.6 What You Should See if Everything Worked

If you follow the steps above, you should be able to:

- **Recreate the CNN performance metrics**  
  - Test loss and accuracy printed by `model.evaluate(...)`.  
  - A classification report with per-species precision, recall, and F1-scores.  
  - A confusion matrix summarizing misclassifications.

- **Recreate the conservation summaries**  
  - Printed lines linking predicted species to IUCN Red List status and typical threats.

- **Recreate the EDA figures in `OUTPUT/`**  
  - Bar chart of images per species.  
  - Boxplots of brightness and contrast per species that match (up to minor visual differences) the PNGs included in the repository.

Following these steps allows an interested user to independently rebuild the dataset, rerun the CNN training and evaluation, and regenerate all tables and figures used in our MI3 analysis.

