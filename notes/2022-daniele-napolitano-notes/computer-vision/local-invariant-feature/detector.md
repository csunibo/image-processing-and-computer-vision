_Algorithms that detect Local Invariant Feature points._<br>
## Moravec Interesting Point Detector<br>
_**cornerness** of a pixel p is a measure of the likelihood of that point being a corner:_<br>
$$C(p)=min_{q\in neigh(p)} ||N(p)-N(q)||^{2}$$<br>
![](../../img/pasted-image-20230331121152.png)<br>
If this number is sufficiently high, it means that on every neighboring direction the change is high, meaning that the point p is a corner.<br>
## Harris Corner Detector<br>
Continuous formulation of #Moravec Interesting Point Detector:<br>
shifting the image on infinitesimal scale ($\Delta x , \Delta y$), being able to deploy Taylor's expansion at (x,y): $$f(x+\Delta x)=f(x)+f'(x)\Delta x$$<br>
![](../../img/pasted-image-20230831111336.png-|-600)<br>
_(we only need the first degree approximation)_<br>
<br>
so the 2D image with infinitesimal shift is written as: $$I(x+\Delta x, y+ \Delta y)= I(x,y)+I_{x}\Delta x + I_{y}\Delta y$$<br>
We consider a window function (usually of +size 1): $w(x,y)$ which is 1 inside the window, 0 outside<br>
![](../../img/pasted-image-20230714174736.png)<br>
In the end you end up with a weighted sum of derivatives:<br>
![](../../img/pasted-image-20230331121623.png)<br>
This matrix M encodes the local image structure around the pixel p. If M is a diagonal matrix, it becomes the **matrix of eigenvalues**:<br>
![](../../img/pasted-image-20230331121737.png)<br>
![](../../img/pasted-image-20230331121746.png)<br>
<br>
But M can always be diagonalized with rotations:<br>
![](../../img/pasted-image-20230331121835.png)<br>
That's why this method is **invariant to rotations.**<br>
<br>
Computing eigenvalues for every pixel is very expensive though, so a better solution is using an approximation that exploits determinant and trace.<br>
![](../../img/pasted-image-20230331122530.png)<br>
## Steps<br>
Harris Corner Detector follow usually 3 steps:<br>
<br>
**1. Compute C for each pixel**<br>
![](../../img/pasted-image-20230331122835.png)<br>
**2. Select only pixels where C is higher than a threshold T**<br>
![](../../img/pasted-image-20230331122848.png)<br>
**3. Detect as corners only those pixels that are _local maxima of C_**<br>
using Non-Maxima Suppression (NMS)<br>
![](../../img/pasted-image-20230331122924.png)<br>
### Properties<br>
- **Rotation invariant**<br>
- **NOT** illumination invariant (as seen above)<br>
- **NOT** scale invariant, since the neighboring window is fixed (usually 1, thus it's detailed)<br>
![](../../img/pasted-image-20230331124515.png)<br>
<br>
# Scale-Space<br>
Depending on distance and focal-length, objects may appear differently in the image, and may exhibit less/more details (features).<br>
Scaling changes the amount of features detectable<br>
![](../../img/pasted-image-20230331131220.png)<br>
Solution: applying a fixed-sized tool on different scaled and blurrier versions of the same image -> "SCALE SPACE"<br>
The blurrier the image, means the bigger the feature detected is. The more detailed the image, it means that the features is small, since it was filtered out in the other layers of the scale space.<br>
![](../../img/pasted-image-20230331131322.png)<br>
Scale space must be realized with Gaussian Filter smoothing:<br>
$L(x,y,\sigma)=G(x,y,\sigma)*I(x,y)$, increasing the kernel size when shrinking the image.<br>
<br>
# Scale-Normalized LOG<br>
_Useful for detecting **Local Invariant Feature#Blob features** in images.<br>
Uses the scale-normalized Laplacian of Gaussian (LOG), which uses second order derivatives._<br>
Convolves image with LOG filter at multiple scales:<br>
$F(x,y,\sigma)=\sigma^{2}(\nabla^{2}G(x,y,\sigma)*I(x,y))$ <br>
the $\sigma^{2}$ does normalization, making the filter invariant to scale changes. <br>
<br>
![](../../img/pasted-image-20230331132119.png)<br>
The idea is to find extrema (min, max) across x, y, $\sigma$. because these points correspond to regions of the image that have **high curvature or intensity variation in multiple directions** (detected by 2nd derivative) -> **BLOBS**<br>
<br>
![](../../img/pasted-image-20230331132735.png)<br>
We can see from the different sizes of the circles, that the **detected blobs are associated to different scales.**<br>
# Difference of Gaussian (DoG)<br>
$DoG(x,y,\sigma)=L(x,y,k\sigma)-L(x, y, \sigma)$ <br>
Where **k** is a constant Hyperparameter, which **controls the scale** of the filter.<br>
It is a difference of smoothed images, where $\sigma$ of the Gaussian Filter slightly changes.<br>
<br>
It gives similar result to #Scale-Normalized LOG, but it's more efficient (**approximation**)<br>
![](../../img/pasted-image-20230331151842.png)<br>
![](../../img/pasted-image-20230331151937.png)<br>
Can also detect blob-like features.