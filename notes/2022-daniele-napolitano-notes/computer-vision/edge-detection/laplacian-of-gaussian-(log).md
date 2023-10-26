_instead of using first order derivatives, another effective way to extract edges in Edge Detection, is using **second order derivatives** and the **Laplacian operator**._<br>
# Zero Crossing<br>
Instead of searching the peak of the 1st derivative, we now look for the zero crossing point of the 2nd.<br>
![](../../img/pasted-image-20230327103325.png)<br>
## Laplacian<br>
As second order derivative operator, we can use the Laplacian:<br>
![](../../img/pasted-image-20230327103455.png)<br>
# LOG<br>
LOG simply consist in applying a Gaussian filter BEFORE applying the Laplacian kernel to find zero-crossing points. In order, the operations are:<br>
1. applying Gaussian Filter<br>
2. applying Laplacian kernel<br>
3. extracting zero crossing points (the edges)<br>
<br>
Thanks to Gaussian Filter#Separability, LoG can be applied with four 1D convolutions, which makes the computations easier than applying a two 2D convolution, with a speed up of $\frac{1}{2}K$, where K is the dimension of the Gaussian kernel: $\nabla(I(x,y)*G(x,y))=I(x,y)*\nabla G(x,y)$<br>
which becomes: $$(I(x,y)*G''(x))*G(y) + (I(x,y)*G''(y))*G(x)$$<br>
Ideally, the Laplacian operator would return something like this: $[-10, 0, 20]$, where 0 is the zero-crossing point. This does not actually happen, so we get something like $[-10, 20]$. In this case, the pixel closest to zero is chosen as edge candidate (-10 in the previous example).<br>
<br>
## Example<br>
![](../../img/pasted-image-20230327111303.png)<br>
note: the higher the $\sigma$ of the Gaussian filter, the fewer edges are detected.<br>
Overall, it's still best to apply a small sigma (just for denoising), and then apply a threshold to filter out irrelevant edges.