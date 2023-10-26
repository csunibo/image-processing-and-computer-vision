![](../../img/multiple_channels_convolution_CNN.gif | 500 )<br>
In traditional Image Processing, for Image Filters we rely on Image Filters#Convolutions with handcrafted filters.<br>
We could build convolutional layers that have the objective to learn the composition of such filters from data.<br>

- Input and output are not flattened, preserving the spatial structure<br>
- The parameters associated with connections are the same for all output units, so **parameters are shared** and the convolution learns the same detector regardless of the input position.<br>
- A convolution only processes a small set of neighbor pixels at a time, like in the Visual cortex#Receptive field.<br>
- Image exhibit informative **local** patterns that may appear anywhere across an image, that's why convolutions are so effective with images.<br>
  ![](../../img/pasted-image-20230711163000.png)<br>
  In this case we need only 2 parameters, while before we used $H^2W^2$.<br>
  <br>
  Convolution can be seen as a matrix multiplication operation if we reshape inputs and outputs. It's a linear operator which:<br>
- **shares parameters** across nodes<br>
- It's **sparse**<br>
- Adapts to different input sizes<br>
- **Equivariant to translations** of input (not rotation and scale) -> $T(x)*K=T(x*K)$. Allows for better generalization and data efficiency, possible thanks to weights sharing.<br>
  ![](../../img/pasted-image-20230711163151.png)<br>

## Conv layer vs dense layer<br>

We can see below that Conv layers are just dense layers which aren't fully connected. the weights of the connections are the kernel values, and are shared between neurons, as seen below.<br>
![](../../img/pasted-image-20230712163416.png)<br>
To better understand how weight sharing works and how CNN differs from FC networks, look at this [useful website](https://adamharley.com/nn_vis/). Below we have a comparison of CNN vs FC network for MNIST dataset handwritten digits classification.<br>
**CNN example:**<br>
![](../../img/pasted-image-20230831155518.png)<br>
Notice that even if select another pixel, the weights will be the same, but they will be looking at a different part of the input image (like if we are applying a convolution)<br>
![](../../img/pasted-image-20230831155801.png)<br>
<br>
**FC example:**<br>
Here we have that each single node of the first FC layer looks at every pixel of the input image. We have many more parameters to compute.<br>
![](../../img/pasted-image-20230831155547.png)<br>

# Channels<br>

Colored images have 3 channels, corresponding to a 3D tensor $3\times H\times W$.<br>
We need to have 3 kernels: one for each channel ($3\times K \times K$).<br>
Each filter generates a single channel **feature map**. <br>
<br>
![](../../img/pasted-image-20230711165448.png)<br>
To get **more output channels**, we need to use multiple filters and stack the ouptuts.<br>
![](../../img/pasted-image-20230711165704.png)<br>
In general, we can have as many input and output channels as we want, as long we use the right kernel dimensions.<br>
<br>

# Stride<br>

How many pixels to shift each time we apply a convolution.<br>
Stride 1 vs stride 2:<br>
![](../../img/pasted-image-20230823154636.png)<br>
It is a useful way to reduce the spatial size of the output (downsampling).<br>
Most often, to downsample we use Max pooling with stride > 1<br>
Here is a video about [**padding and stride**](https://www.youtube.com/watch?v=3TdBtI9dh2I)<br>
