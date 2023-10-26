_Process whereby all parameters defining the Image formation#Complete camera model are estimated for a **specific camera device**_.<br>
<br>
## Calibration patterns<br>
Checkerboard patterns useful for being easy to process.<br>
Two categories:<br>
- Single image of a 3D calibration object<br>
- Multiple images of the same planar pattern (different orientations)<br>
We know **number of internal corners** (has to be odd in one side for rotation ambiguities) and **size of each square** (cm).<br>
<br>
![](../../img/pasted-image-20230504104831.png)<br>
<br>
# Zhang's method<br>
![](../../img/pasted-image-20230504104901.png)<br>
### World Reference Frame<br>
The WRF can be defined on the calibration pattern as follows:<br>
- origin on the same corner (e.g. top left one)<br>
- X=shorter side<br>
- Y=longer side<br>
- Z=0 (planar image), direction perpendicular to X and Y.<br>
Given these reference coordinates, we can define 3D coordinates for any point of the plane.<br>
![](../../img/pasted-image-20230504105748.png)<br>
WRF is different for each calibration image. We estimate as many extrinsic matrices as the number of images used for calibration.<br>
<br>
## Homography<br>
We consider only 3D points with z=0. For this reason we can remove a column from the PPM matrix (so it becomes 3x3):<br>
![](../../img/pasted-image-20230504105955.png)<br>
The **H matrix** is known as **homography** (simplification of P).<br>
<br>
Given a pattern with m corners, we can write 3 linear equations for each corner j.<br>
We know already 2D and 3D cooridinates, but the unknowns are the 9 variables in $H_{i}$ of the i-th image $(p_{1,1}, ... , p_{3,4})$.<br>
H* is a point in projection space that lies on the same line of $\tilde m_j$, meaning that the are parallel, hence $\tilde m_{j}\times H\tilde w_j=0$. <br>
![](../../img/pasted-image-20230718105311.png)<br>
![](../../img/pasted-image-20230713155259.png)<br>
We get this linear system of equations for all $w_1, ..., w_m$:<br>
![](../../img/pasted-image-20230504110943.png)<br>
To remove the trivial (useless) solution with h=0, we also impose that $||h||=1$.<br>
Solution h* is found by minimizing the norm of Lh. <br>
![](../../img/pasted-image-20230504111117.png)<br>
This is a **least squares problem**, solvable with **Singular Value Decomposition (SVD)**<br>
![](../../img/pasted-image-20230504111212.png)<br>
h* is the matrix V (orthonormal), more precisely its last column.<br>
<br>
### Refining H <br>
H at this point is just an approximation. In fact the projection of the corner $H_{i}w_{j}$ won't probably concide with the real corner point $m_{j}$ -> **geometric error**<br>
![](../../img/pasted-image-20230505175418.png)<br>
Using the distance (error) between those two, we can get a better approximation for $H_{i}$.<br>
This is an iterative minimization problem:<br>
![](../../img/pasted-image-20230505175644.png)<br>
We take $H_{i}$ as a first guess value, then using a method similar to SGD and Newton's method, we iteratively try to minimize the distance for all corners in the image i.<br>
<br>
## Estimatic Intrinsic parameters (A)<br>
All images share the same intrinsic parameters.<br>
At this point we have a fairly good approximation of H for every image.<br>
![](../../img/pasted-image-20230505181329.png)<br>
Since they have the same length, the dot product is zero. Hence we can write<br>
![](../../img/pasted-image-20230505183203.png)<br>
By solving these constraint for each image, we get a homogeneous sistem of equations with many equation as much as images.<br>
This can be solved again by **SVD** (that's why we need more than 3 images)<br>
<br>
## Estimating Extrinsic parameters (R and t)<br>
$R_{i}$ and $t_{i}$ are different for each image. Given A and H, we can compute them as follows:<br>
![](../../img/pasted-image-20230505183718.png)<br>
Though R will not be orthonormal since this is an approximation.<br>
To get an orthonormal R, we use again SVD, sobstituting D ($\Sigma$) with the identity matrix I.<br>
<br>
## Distorsion parameters (k1, k2 and p)<br>
The coordinates predicted by the Homography H correspond to the undistorted image ($m_{undist})$, so we should consider the non-linear effect of Image formation#Lens distortion, thus estimating the two parameters $k_{1}$ and $k_{2}$ of radial distortion.<br>
![](../../img/pasted-image-20230516175745.png)<br>
<br>
To do this, we need to go back to metric image coordinates (continuous):<br>
![](../../img/pasted-image-20230516175917.png)<br>
![](../../img/pasted-image-20230516175934.png)<br>
We thus get a non-homogeneous system of linear equations $Dk=d$ in the unknowns $k=[k_{1},k_{2}]^{T}$. This can be solved as a least-squares problem, minimizing $||Dk-d||_{2}$ exploiting the pseudoinverse of D:<br>
![](../../img/pasted-image-20230516180245.png)<br>
<br>
## Refinement of all parameters<br>
We now have all the parameters, but there still might be some algebraic error .<br>
To refine and align our final results, we use Maximum Likelyhood Estimator (MLE), using the current param values as initial guesses.<br>
![](../../img/pasted-image-20230831190624.png)