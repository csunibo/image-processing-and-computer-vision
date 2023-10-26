_A fast ResNet thought to be implemented in smartphone.<br>
It uses **inverted residual blocks** and **Depthwise separable convolutions**._<br>

## Inverted residual blocks<br>

MobileNet solves this issue by using **inverted residual blocks**, where <br>

- the first 1x1 layer expands the channels<br>
- second compresses back again<br>
- expansion works according to an **expansion ratio t**<br>
- There are no ReLUs between residual blocks<br>
  ![](../../img/pasted-image-20230718183635.png-|-400)<br>
  ![](../../img/pasted-image-20230824181133.png)<br>

# Depthwise separable convolution<br>

The inner 3x3 conv layer is implemented as a depthwise separable convolution:<br>
Instead of doing one big convolution, the matrix is split in 2, and we just do two smaller convolutions. The result is the same, but the complexity is **much** smaller.<br>
Splits the computation into two steps: <br>

- **Depthwise convolution** is used to create a linear combination of the output of the depthwise convolution. Uses grouped convolutions to have filter of only one channel. (3x3 filters of channel 1 in the gif below)<br>
- **Pointwise convolution** is used to create a linear combination of the output of the depthwise convolution (1x1 conv with n channels in the gif)<br>
  ![](../../img/depthwise-separable-convolution-animation-3x3-kernel.gif)<br>
  Using this kind of convolution allows to deal with more channel, while keeping the cost down. The groups are computed in parallel, allowing a speedup up to 9x!!!<br>
  ![](../../img/pasted-image-20230719120536.png)<br>
  <br>
  Here is a video about [**Depthwise separable convolution**](https://www.youtube.com/watch?v=vVaRhZXovbw)<br>

## Architecture<br>

MobileNet V2 is a stack of inverted residual blocks:<br>
![](../../img/pasted-image-20230718225409.png)<br>
Has few parameters since stem layer does not have to make heavy downsampling, and channels grow slowly
