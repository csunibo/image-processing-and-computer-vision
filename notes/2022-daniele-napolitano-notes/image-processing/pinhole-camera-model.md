_Simplest imaging device. Light goes through a very small pinhole and hits the image plane_<br>
![](../img/pasted-image-20230301160141.png)<br>
## Perspective Projection<br>
![](../img/pasted-image-20230301160300.png)<br>
![](../img/pasted-image-20230301160348.png)<br>
The Image plane is _(u,v)_, the scene plane is _(x,y)_. We're assuming that they are parallel.<br>
With this assumption, the relation between the two planes is **proportional**.<br>
![](../img/pasted-image-20230302103740.png)<br>
(note: signs are positive only if we invert the z axis, which means putting the image plane BEFORE the scene plane).<br>
From this relation, the coefficient of the proportion are:<br>
- **z (depth)**: the higher, the smaller the image<br>
- **f (focal length)**: the bigger, the smaller the FOV (the bigger the image)<br>
<br>
# Perspective projection<br>
With 2D representations (projections) of the 3D space, we lose one dimension. This leads to information loss<br>
![](../img/pasted-image-20230302104301.png)<br>
**RECOVERING 3D STRUCTURE FROM SINGLE IMAGE IS AN ILL-POSED PROBLEM**<br>
<br>
## Stereo Geometry<br>
If one image is not enough, 2 is just right to fully solve this problem:<br>
Stereo images (just like our eyes), allow to infer 3D thanks to **triangulation**.<br>
<br>
We now have 2 image planes (and of course one scene plane), that needs to be perfectly aligned:<br>
![](../img/pasted-image-20230302104625.png)<br>
- **b is the BASE LINE**, which is the distance between the two cameras.<br>
- **d is DISPARITY**: difference between the horizontal coordinates of the two images<br>
![](../img/pasted-image-20230302104935.png)<br>
if d is high, z is low (and viceversa).<br>
<br>
## Stereo matching<br>
since the two images are parallel, given a projection point $P_{L}$, if we want to find the respective $P_{R}$ point on the other image, we just need to follow the horizontal line that starts from Pl and search for a similar neighborhood of pixel on the right image (there are several algorithms to do so).<br>
![](../img/pasted-image-20230302105045.png)<br>
<br>
## Epipolar Geometry and Rectification<br>
But what if the cameras are not aligned (in reality in fact this never happens).<br>
![](../img/pasted-image-20230302110015.png)<br>
The **epipolar line** is the projection of the first image's line which connects $P_L$ to P, projected to the first plane.<br>
This method is hard and not efficient though.<br>
It's simpler to just **rectify** both images (warping them) to virtually align them (so we can use straight horizontal lines again).<br>
**BEFORE** (find correspondace point by following the<br>
epipolar line)<br>
![](../img/pasted-image-20230307114940.png)<br>
**AFTER** (we can look at the correspondance point by following the horizontal line)<br>
![](../img/pasted-image-20230307104642.png)<br>
This is possible thanks to Camera calibration#Homography.<br>
## Stereo correspondence<br>
When we look for the corresponding point on the left image, we can get an estimate of the distance from the camera<br>
![](../img/pasted-image-20230307104732.png)<br>
points with bigger disparity are closer (d1>d2).<br>
<br>
# Vanishing point<br>
In projections, ratios of lengths are not preserved.<br>
Also, parallel scene lines actually converge in a point in the projection<br>
![](../img/pasted-image-20230307104933.png)<br>
### vanishing point of a 3d line<br>
the vanishing point of a 3d line  is the image of the point at infinity of the line