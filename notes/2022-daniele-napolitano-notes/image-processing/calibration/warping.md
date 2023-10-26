\_After finding all camera parameters with Camera calibration#Zhang's method, we can **warp** the images without worrying of Image formation#Lens distortion._<br>
![](../../img/pasted-image-20230718140416.png)<br>
After applying warping function we get continuous coordinates. We can apply<br>
- truncation (easier)<br>
- nearest neighbor (i.e. rounding)<br>
![](../../img/pasted-image-20230718140531.png)<br>
But regardless of this choice, we may end up with<br>
- more than one pixel onto the same position -> **folds**<br>
- some pixels of the destination may not be hit and remain empty -> **holes**<br>
![](../../img/pasted-image-20230718140806.png)<br>
## Backward mapping<br>
To avoid those problems, we can take a value for each coordinate in the output image by using **inverse mapping**. Coordinates are still continuous, so we can:<br>
- truncate or nearest neighbor (as before)<br>
- **interpolate** between the 4 closest points (bilinear, bicubic, ...)<br>
![](../../img/pasted-image-20230718141855.png)<br>
# Stereo homographies<br>
In the case of multiple cameras in Pinhole Camera model#Stereo Geometry, the two (or more) images are related by an homography:<br>
![](../../img/pasted-image-20230718142808.png)<br>
This is also valid for a single camera, when we have multiple images of the same object but rotated, A is fixed and $[R,t]$ change<br>
![](../../img/pasted-image-20230718143224.png)<br>
Or if we take two exact photos in the same pose, but with two different cameras, $[R,t]$ are fixed and A changes:<br>
![](../../img/pasted-image-20230718143341.png)<br>
If we change both A and $[R,t]$, we end up with the first scenario above.<br>
<br>
This allows us to rectify two images  (Pinhole Camera model#Epipolar Geometry and Rectification), for the purpose of stereo matching (Pinhole Camera model#Stereo matching), by aligning intrinsic and extrinsic parameters.