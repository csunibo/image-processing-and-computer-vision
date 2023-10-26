![](../../img/convolution_arithmetic_-_padding_strides.gif-)<br>
_Image processing operators that compute the new intensity of a pixel $p$ based on the intensity of its neighborhood (aka **support**)._<br>
# Linear Translation Equivariant (LTE) Operator<br>
Image Noise#Denoising over space technique, that uses 2D convolutions between the input raw image and a **kernel** (impulse response function).<br>
![](../../img/pasted-image-20230830114916.png)<br>
- A filter is **linear** iff: $$T\{ai_1(x,y)+bi_2(x,y)\}=aT\{i_1\}+bT\{i_2\}$$<br>
- A filter is **translation-equivariant** iff: $$T\{i(x-x_{0}, y-y_{0})\}=o(x-x_{0},y-y_{0})$$<br>
	In general, it means that the position of an object in an image should not matter during the filtering process.<br>
## Convolution <br>
**Linear and Translation equivariant** filters apply a linear operator T to an image i(x,y).<br>
The output is given by the **convolution** between the input and the **impulse response function (KERNEL)** of the operator T.<br>
![](../../img/pasted-image-20230311105720.png-|-500)<br>
It is equivalent to reflect in both axis then translate the kernel.<br>
<br>
### Properties of convolution<br>
![](../../img/pasted-image-20230311105923.png)<br>
## Correlation<br>
Similar to convolution, but there is no reflection.<br>
![](../../img/pasted-image-20230311110618.png)<br>
Its properties are different (NOT commutative)<br>
![](../../img/pasted-image-20230311110652.png)<br>
if h is even, then i * h=h * i =h o i, but still, h o i != i o h (not commutative)<br>
<br>
## Discrete convolution<br>
same as continuous convolution, but with summatories. The same properties still hold. <br>
![](../../img/pasted-image-20230311161026.png)<br>
<br>
# Practical Implementation<br>
The kernel has a dimension much smaller than the image. Applying the discrete convolution formula, the kernel will slide across the whole input image to compute the convolution to each pixel.<br>
What abut edge pixels? Two options:<br>
- CROP (cut away 4k pixels, k is dimension of kernel)<br>
![](../../img/pasted-image-20230313092552.png)<br>
- PAD (e.g. zero-padding, replicate, reflect, ...), to fill the 4k border pixels with data that will be replaced by convolution.<br>
<br>
<br>
<br>
