# Project: Perception Pick & Place

[non-normalized]: ./misc/confusion_matrix_without_normalization.png
[normalized]: ./misc/confusion_matrix_with_normalization.png
[output1]: ./misc/output1.png
[output2]: ./misc/output2.png
[output3]: ./misc/output3.png

## Pipeline

### 1. Filtering and RANSAC plane fitting

Filtering and RANSAC plane fitting implemented inside the `segmentation` function in `project_template.py`, start from line 58.

### 2. Clustering for segmentation implemented

Segmentation part is implemented inside the `segmentation` function in `project_template.py`, start from line 88.

Clustering is implemented insdie the `clustering` function in `project_template.py`, start from line 103.

### 3. Features extracted and SVM trained,  object recognition implemented

This part is implemented inside the `classify` function

### 4. Result

#### SVM confusion matrix normalized

![matrix][normalized]

See `output_1.yaml`, `output_2.yaml` and `output_3.yaml` inside `./pr2_robot/scripts` folder.

Output image:

#### World 1

![world1][output1]

#### World 2

![world2][output2]

#### World 3

![world3][output3]
