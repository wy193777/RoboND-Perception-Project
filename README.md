# Project: Perception Pick & Place

[non-normalized]: ./misc/confusion_matrix_without_normalization.png
[normalized]: ./misc/confusion_matrix_with_normalization.png
[output1]: ./misc/output1.png
[output2]: ./misc/output2.png
[output3]: ./misc/output3.png
[after-ransac]: ./misc/after-ransac.png
[clustered]: ./misc/clustered.png

## Pipeline



### 1. Filtering and RANSAC plane fitting

The purpose of this step is to seperate objects from the table. There are 3 steps to do it. Filtering and RANSAC plane fitting implemented inside the `segmentation` function in `project_template.py`, start from line 58.

#### Step 1: Voxel Grid Filter

Because the object we want to identify aren't so small and the point cloud provided has a very high resolution. We can down sample the the cloud to save some computation power.

A voxel grid filter allows you to downsample the data by taking a spatial average of the points in the cloud confined by each voxel. We can adjust the sampling size by setting the voxel size along each dimension. The set of points which lie within the bounds of a voxel are assigned to that voxel and statistically combined into one output point.

#### Step 2: Pass Through Filter

Pass through filter is used to remove useless data from the point cloud. If we have some prior information about the location of our target in the scene.

The Pass Through Filter works much like a cropping tool, which allows you to crop any given 3D point cloud by specifying an axis with cut-off values along that axis. The region you allow to pass through, is often referred to as region of interest.

We use 0.6 to 1.3 on `z` axis, -0.5 to 0.5 on `y` axis and 0.3 to 1.0 on `x` axis to limit area of the point clouds.

#### Step 3: RANSAC Plane Segmentation

Next we need to remove the table itself from the scene. We did this by using a  technique known as Random Sample Consensus or "RANSAC". RANSAC is an algorithm, that we can use to identify points in our dataset that belong to a particular model. In the case of the 3D scene we're working with here, the model you choose could be a plane, a cylinder, a box, or any other common shape.

The RANSAC algorithm assumes that all of the data in a dataset is composed of both inliers and outliers, where inliers can be defined by a particular model with a specific set of parameters, while outliers do not fit that model and hence can be discarded. Like in the example below, we can extract the outliners that are not good fits for the model.

In our pipeline we fit the table plane and let outliers be those objects we want to grab.

Expected result after Filtering and RANSAC plane fitting:

![objects][after-ransac]

### 2. Clustering for segmentation implemented

This step is to cluster point cloud points into different objects. We use euclidean cluster to seperate points into different objects.

First we need to transfor points into a k dimensional tree. Then we can apply euclidean clustering to this k dimensional tree.

We need to specify cluster tolerance, min cluster size and max cluster size.

Clustering is implemented insdie the `clustering` function in `project_template.py`, start from line 103.

Result after clustering:

![clustered][clustered]

### 3. Features extracted and SVM trained,  object recognition implemented

We use RGB and HSV histogram of points of classified objects to train SVM and use the trained SVM identify objects.

Training of the SVM is implemented in `train_svm.py` file. And the part that classify objects is implemented inside the `classify` function

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
