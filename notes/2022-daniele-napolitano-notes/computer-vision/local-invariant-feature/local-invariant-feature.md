_**An image pattern which differs from its immediate neighborhood**, aka keypoint.<br>
Detecting those features is an Image Processing task._<br>
![](../../img/pasted-image-20230412140808.png)<br>
Finding Correspondence points between 2 or more images is crucial in Pinhole Camera model#Stereo correspondence, to estimate depth in images, but also for many other applications in Computer Vision, such as Panorama Stitching (need 4 corr. points), object detection, AR, robot navigation and odometry (SLAM), 3D reconstruction, and many more.<br>
<br>
# Paradigm<br>
There are 3 main steps and components:<br>
1. **Detector** -> finds **salient points** (aka feature points or keypoints)<br>
	- **Repeatability**: should find same keypoints in different views, despite any transformation<br>
	- **Salience**: find keypoints surrounded by informative patterns (more discriminative when matching)<br>
2. **SIFT#Descriptor** -> computes a **descriptor**, based on neighbor pixels. <br>
	- **invariant** to any type of transformation. <br>
	- **Distinctiveness vs Robustness**: description algorithm should capture salient information, and disregard changes due to noise or light.<br>
	- **Compactness**: concise descriptions (minimize memory, efficient matching)<br>
3. **Matching** -> descriptors between images<br>
<br>
# Interesting Points<br>
<br>
## Corners<br>
Edges can be hardly told apart since they look very similar along the direction of the gradient.<br>
Pixels having large variations along **all directions** are more distinguishable, hence better for matching -> CORNERS<br>
![](../../img/pasted-image-20230331114240.png)<br>
## Blob<br>
Corners are indeed good interesting points, but they don't show up much often in images, and they're not very unique. We need another kind of features: blobs.<br>
![](../../img/pasted-image-20230404193840.png)<br>
They also have the very useful property to be fixed in position and definite in size.