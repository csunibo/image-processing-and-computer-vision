_Linear filter.<br>
Replaces each pixel intensity by the average intensity of a chosen neighborhood (k neighbors)._<br>
Works as a **low-pass filter** (image smoothing)<br>
![](../../img/pasted-image-20230313100249.png)<br>
All the values are the same: $\frac{1}{k}$<br>
The bigger the kernel, the smoother (and blurrier) the image, removing Image Noise#Gaussian Noise, but loosing details and edges.<br>
Mean filters cannot solve **impulse noise**.<br>
