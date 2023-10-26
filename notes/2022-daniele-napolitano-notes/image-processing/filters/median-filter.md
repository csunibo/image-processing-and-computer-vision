_**non-linear** filter where each pixel intensity is replaced by the **median** over a given neighborhood_<br>
![](../../img/pasted-image-20230319165559.png)<br>
The kernel is not fixed, and needs to be computed for each pixel (though it is an easy computation)<br>
It is **very effective** to contrast Image Noise#Impulse Noise, while also preserving sharp edges (**no blurring**)<br>
![](../../img/pasted-image-20230319165715.png)<br>
To remove further the impulse noise, the filter can be applied multiple times.<br>
It cannot deal with Gaussian noise: The best choice would be to combine them: first using a median and then the Gaussian.<br>
<br>
