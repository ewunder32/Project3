## Data Summary

Our dataset contains 1,546 images of 15 shark species. The dataset is sourced from Roboflow’s Shark Species Dataset (/content/Shark-Species-Dataset-1). The dataset is partitioned into training, validation, and test directories following a conventional supervised image classification structure (train/, valid/, test/). Each directory contains subfolders that represent individual shark species classes. The dataset supports our goal of developing a Convolutional Neural Network (CNN) capable of classifying shark species from photographic data and integrating conservation status metadata for vulnerability analysis. The shark species dataset was pulled through the RoboFlow API, and the conservation status was downloaded from ​The IUCN Red List and is entitled SHARKS_RAYS_CHIMAERAS.dbf.

## Provenance

This dataset originates from Roboflow, a public platform for computer vision datasets. The original contributors collected and annotated images of shark species from open-access marine photography repositories and research collaborations. Roboflow standardizes datasets into YOLO- and TensorFlow-compatible formats, performing preprocessing steps such as image resizing, bounding box alignment, and train/validation/test splits. 

For this project, we used Roboflow’s pre-split dataset, ensuring reproducibility across training and evaluation phases. No manual relabeling was performed. The dataset provides balanced class representation across species, though real-world imbalances may persist. The images were loaded into TensorFlow/Keras using the image_dataset_from_directory() API, resized to 128×128 pixels, and normalized for CNN ingestion. 

## License

MIT License

Copyright (c) 2025 ewunder32

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

## Ethical Statements

This dataset contains no personal or identifying information about individuals. Ethical considerations primarily relate to species misclassification and dataset bias. Some shark species have limited photographic data, potentially leading to class imbalance and uneven model performance. Misclassifying threatened species as common ones could affect conservation prioritization if applied at a scale. 

To mitigate these issues, we document class distributions, apply data augmentation for underrepresented species, and cross-check model predictions against IUCN Red List statuses. Our project is strictly educational, focusing on transparent evaluation rather than field deployment. 

## Data Dictionary
![alt text](https://github.com/ewunder32/Project3/blob/main/Project3/OUTPUT/Data%20Dictionary.png)

## Explanatory Plots

![alt text](https://github.com/ewunder32/Project3/blob/main/Project3/OUTPUT/Distribution%20of%20Images%20Across%20All%20Unique%20Shark%20Species.png)

![alt text](https://github.com/ewunder32/Project3/blob/main/Project3/OUTPUT/Distribution%20of%20Image%20Brightness%20per%20Shark%20Species.png)

![alt text](https://github.com/ewunder32/Project3/blob/main/Project3/OUTPUT/Distribution%20of%20Contrast%20per%20Shark%20Species.png)


