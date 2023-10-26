_Stands for "Scale Invariant Feature Transform" **invariant to image scaling, translation, and rotation** (canonical orientation) and partially invariant to illumination changes and affine or 3D projection._<br>
# Detector<br>
Based on the DoG formula: $$DoG(x,y,\sigma)=L(x,y,k\sigma)-L(x, y, \sigma)=(G(x,y,k\sigma)-G(x,y,\sigma))*I(x,y)$$makes a scale space of the image with different $\sigma$ for the Gaussian kernels (within an octave). In an octave, we sample $s$ scales: $$2\sigma=k^{s}\sigma \rightarrow k=2^{\frac{1}{s}}$$Between each pair of layer, difference is performed, obtaining multiple  Detector#Difference of Gaussian (DoG) layers:<br>
![](../../img/pasted-image-20230407183220.png)<br>
(two additive layers are added on top and bottom of scale space to have more DoG layers)<br>
<br>
In each octave the size is the same, and shrinks in the successive one:<br>
![](../../img/pasted-image-20230714175834.png)<br>
<br>
We now look for extremas in each of the DoG layer, comparing with neighborhood, both on same layer, and on upper and lower layers<br>
![](../../img/pasted-image-20230407182643.png)<br>
The layer of the detected extremas define the **scale** of the blob feature: the blurrier the image, the bigger the feature! <br>
![](../../img/pasted-image-20230407182723.png)<br>
<br>
### Thresholds<br>
After finding the extremas, 2 thresholds are applied:<br>
1. 1st removes useless points (for example in the background)<br>
2. 2nd removes features along edges (useless since we can use Edge Detection)<br>
![](../../img/pasted-image-20230407183740.png)<br>
- **Hyperparameter#Tuning**:<br>
	according to Lowe, the best values are $s=3$ and $\sigma=1.6$<br>
![SIFT](https://www.youtube.com/watch?v=ram-jbLJjFg)<br>
# Descriptor<br>
**local feature descriptor** that is computed for each keypoint detected by the SIFT Detector.<br>
More specifically, a 16x16 neighborhood patch is considered.<br>
This kind of descriptor, based on orientation, is biologically inspired from the Complex cells in the Primary Visual cortex (Visual cortex#V1#Simple and Complex cells)<br>
<br>
## Canonical orientation <br>
_"By assigning a consistent orientation to each keypoint based on local image properties, the keypoint descriptor can be represented relative to this orientation and therefore achieve invariance to image rotation"_<br>
magnitude and orientations are computed for each pixel using partial derivatives (with **central differences**).<br>
![](../../img/pasted-image-20230407211525.png)<br>
Then build an **orientation histogram** for each keypoint (considering a neighboring window whose size depends on the keypoint size). Taking into account the direction and magnitude of each neighboring pixels (weighted by a 2D Gaussian), with a bin size of 10°, we count the occurrences of the orientations and build the histogram.<br>
The **characteristic orientation** is the highest peak of the histogram (and the other 80% highest values)<br>
![](../../img/pasted-image-20230716160117.png-|-400)<br>
## SIFT Descriptor<br>
Given m(x,y) and $\theta(x,y)$ for each pixel: <br>
- A 16x16 neighborhood patch is considered for every keypoint. <br>
- Divided into 16 regions (4x4)<br>
- A **gradient histogram** is created for each of those region<br>
- Each histogram has 8 bins.<br>
- pixels contribute to histogram according to:<br>
	- Gradient magnitude<br>
	- Gaussian weighting function<br>
![](../../img/pasted-image-20230407223245.png)<br>
Finally, to avoid **boundary effects**, a **trilinear interpolation** scheme is applied: the contribution of two adjacent bins is weighted by distance of the bin center (done also between different regions).<br>
Descriptor is **normalized** to be more robust to intensity variations (i.e. light)<br>
![descriptor](https://www.youtube.com/watch?v=IBcsS8_gPzE&t=216s)<br>
# Matching<br>
Can be used for **object recognition, image stitching, 3D modeling, gesture recognition, video tracking**, and other applications.<br>
Comparing descriptors from 2 different images (different views) of the same scene/object, to find corresponding keypoints.<br>
All the invariance properties are needed to have reliable matching across all kind of variable domains (for light, rotation and scale).<br>
<br>
SIFT descriptors are compared to find corresponding keypoints, using **Nearest Neighbor (KNN) Search** and **Euclidean Distance**. <br>
![](../../img/pasted-image-20230407224556.png)<br>
Distance is computed for each keypoint in T, and we count as matches the ones where $$\frac{d_nn}{d_{2-nn}}\le T $$ <br>
_("Lowe's Ratio")_<br>
<br>
Where T is a threshold of 0.8 (empirically the best value to reject 90% of wrong matches and miss 5% of right matches).<br>
$d_{2-nn}$ is the distance of the neighbor's neighbor. This works best since real matches also have near neighbors with small distances.<br>
<br>
### Efficient search<br>
This search method is computationally expensive. Faster alternatives are:<br>
- **k-d tree** -> uses a kind of binary tree search<br>
- **Best Bin First (BBF)** -> efficient in high dimensional spaces<br>
<br>
