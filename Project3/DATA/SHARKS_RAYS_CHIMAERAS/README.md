##Data Summary##

Our dataset contains 1,546 images of 15 shark species. The dataset is sourced from Roboflow’s Shark Species Dataset (/content/Shark-Species-Dataset-1). The dataset is partitioned into training, validation, and test directories following a conventional supervised image classification structure (train/, valid/, test/). Each directory contains subfolders that represent individual shark species classes. The dataset supports our goal of developing a Convolutional Neural Network (CNN) capable of classifying shark species from photographic data and integrating conservation status metadata for vulnerability analysis.  

##Provenance##

This dataset originates from Roboflow, a public platform for computer vision datasets. The original contributors collected and annotated images of shark species from open-access marine photography repositories and research collaborations. Roboflow standardizes datasets into YOLO- and TensorFlow-compatible formats, performing preprocessing steps such as image resizing, bounding box alignment, and train/validation/test splits. 

For this project, we used Roboflow’s pre-split dataset, ensuring reproducibility across training and evaluation phases. No manual relabeling was performed. The dataset provides balanced class representation across species, though real-world imbalances may persist. The images were loaded into TensorFlow/Keras using the image_dataset_from_directory() API, resized to 224×224 pixels, and normalized for CNN ingestion. 

##License##

##Ethical Statements##

##Data Dictionary##

##Explanatory Plots##
![alt text](https://github.com/ewunder32/Project3/blob/main/Project3/DATA/SHARKS_RAYS_CHIMAERAS/Distribution%20of%20Contrast%20per%20Shark%20Species.png)
![alt text](https://github.com/ewunder32/Project3/blob/main/Project3/DATA/SHARKS_RAYS_CHIMAERAS/Distribution%20of%20Image%20Brightness%20per%20Shark%20Species.png)
![alt text](

