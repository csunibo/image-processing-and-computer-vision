_Edge detection algorithm, and most cited paper on Image Processing and Edge Detection. Defines **three criteria** that a good edge detector should have._<br>
<br>

# Criteria<br>

<br>
- **GOOD DETECTION** -> must correctly extract edges also in noisy images<br>
- **GOOD LOCALIZATION** -> the distance between true and found edge should be minimal<br>
- **ONE RESPONSE TO ONE EDGE** -> Filter should detect **one** single edge pixel  at each true edge (sharp edges)<br>
<br>
## Simple implementation<br>
Canny's edge detectors can be implemented using:<br>
- Gaussian Filter (denoising)<br>
- Gradient computation (edge detection)<br>
- Non-Maxima Suppression (NMS) along gradient direction<br>
<br>
# Hysteresis threshold<br>
![](../../img/pasted-image-20230320194240.png)<br>
Canny proposes a hysteresis threshold relying on higher $T_{h}$ and lower $T_{l}$:<br>
<br>
A pixel will be considered as edge if the gradient magnitude  $| \nabla I |$ is either:<br>
-  higher than $T_{h}$ <br>
**-OR-**<br>
- higher than $T_{l}$  **AND** the pixel is neighbor of an already accepted edge<br>
<br>
In fact it is usually carried out by tracking edge pixels along contours<br>
![](../../img/pasted-image-20230320211813.png)<br>
In the example below, A and B are sure-edges as they are above ‘High’ threshold. Similarly, D is a sure non-edge. Both ‘E’ and ‘C’ are weak edges but since ‘C’ is connected to ‘B’ which is a sure edge, ‘C’ is also considered as a strong edge. Using the same logic ‘E’ is discarded. This way we will get only the strong edges in the image.<br>
![](../../img/pasted-image-20230831110648.png)<br>
![](../../img/pasted-image-20230320211848.png)<br>
<br>
<br>
## Additional material <br>
Here is a link to [**Computerphile video on Canny edge
detector**](https://www.youtube.com/watch?v=sRFM5IEqR2w)<br>
- Canny vs Sobel comparison:<br>
![](../../img/pasted-image-20230714152658.png)
