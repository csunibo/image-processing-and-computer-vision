_Edges are local features of images that capture important **semantic information**.<br>
They are big variations of intensity between pixels in a particular image area:_<br>
![](../../img/pasted-image-20230319175005.png)<br>
![](../../img/pasted-image-20230404193459.png)<br>
edges are pixels that lie exactly in between two or more different image regions (can be phisical objects).<br>
<br>
<br>
# 1D Step-Edge<br>
Sharp chamge of a 1D signal (line).<br>
Those changes can be detected using (first order) **derivatives**.<br>
The simplest edge detector uses a static threshold that is compared with the absolute value of the derivative in the given point. If it's over, it's considered as an edge.<br>
![](../../img/pasted-image-20230319175316.png)<br>
The absolute values is needed since in 1D the edge can have 2 **directions**:<br>
- Low to High (positive derivative)<br>
- High to Low (negative derivative)<br>
![](../../img/pasted-image-20230319175259.png)<br>
# 2D Step-Edge<br>
Images are in 2D, so we are interested in 2D edge detection.<br>
In this case, there are infinite possible directions:<br>
![](../../img/pasted-image-20230319175650.png)<br>
In this case, the **gradient** is used (vector of the two partial derivatives).<br>
Gradient's direction corresponds to the edge's direction. Again, a threshold is used.<br>
![](../../img/pasted-image-20230319175848.png)<br>
<br>
## Discrete gradient approximation<br>
Since pixel values are discrete, the following approximation eases the work:<br>
We can use both backward and forward differences (both on x or y axes):<br>
![](../../img/pasted-image-20230319190203.png)<br>
![](../../img/pasted-image-20230320182826.png)<br>
![](../../img/pasted-image-20230319190125.png)<br>
The **magnitude** can be estimated with different kind of norms, but the best is the **inf norm**. This is because it is **invariant with respect to edge direction** (as seen on table below).<br>
![](../../img/pasted-image-20230320183009.png)<br>
# Noise<br>
Real images have noisy edges:<br>
![](../../img/pasted-image-20230320183141.png)<br>
The problem is that **derivatives amplify Noise**<br>
Thus, the edge detector must be **robust to Image Noise** <br>
A solution could be applying denoising techniques before edge detection, but blurring the images makes the detection much more difficult (also blurs edges).<br>
### Difference of averages<br>
Another solution is **difference of averages**, rather than averaging the image first and then computing the differences.<br>
![](../../img/pasted-image-20230320192236.png)<br>
![](../../img/pasted-image-20230320192314.png)<br>
# Prewitt and Sobel<br>
The Prewitt operator allows to use difference of averages with central differences, while the Sobel operator allows to weight more the central pixel.<br>
![](../../img/pasted-image-20230320192414.png)<br>
![computerphile video on sobel operator](https://www.youtube.com/watch?v=uihBwtPIBxM)<br>
<br>
![](../../img/pasted-image-20230714152336.png)