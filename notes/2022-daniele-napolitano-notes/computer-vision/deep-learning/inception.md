![](../../img/pasted-image-20230823120442.png-|-400)<br>
_aka GoogLeNet <br>
With the aim of reducing computing resources to scale the model, exploiting "inception modules" and increasing depth and width while keeping the computational budget constant._<br>
It can process multiple activations in parallel<br>
![](../../img/pasted-image-20230712152358.png)<br>
Here is a video about [**Inception by Andrew
NG**](https://www.youtube.com/watch?v=KfV8CJh7hE0)<br>
<br>

# Stem layers<br>

Have the objective of strongly downsample the input from 224 to 28, with half the flops as VGG.<br>
LAYERS:<br>

- conv 7x7 stride 2<br>
- maxpooling 3x3 stride 2<br>
- conv 1x1 stride 1<br>
- conv 3x3 stride 1<br>
- maxpooling 3x3 stride 2<br>
  ![](../../img/pasted-image-20230712153019.png)<br>

# Inception module<br>

The main reason of the inception module is to try different approaches of layers altogether, and combine them (as if it were an Ensemble).<br>
Due to maxpooling, which doubles the n°of channels, the total number becomes prohibitively expensive with big convolutions<br>
![](../../img/pasted-image-20230712163802.png)<br>
Here is a video about [**inception reason by Andrew NG |
200**](https://www.youtube.com/watch?v=C86ZXvgpejM)<br>

## 1x1 convolution<br>

Introduced in the [Network in Network](https://arxiv.org/pdf/1312.4400v3.pdf) paper in 2013.<br>
Here is a video about [**1x1 conv by Andrew
NG**](https://www.youtube.com/watch?v=c1RBQzKsDCk)<br>
And here is one about [**short 1x1 conv**](https://www.youtube.com/watch?v=qVP574skyuM&t=58s)<br>
A good solution is using **1x1 convolutions**, which allows to shrink the activation depth (n° of channels) while preserving spatial size.<br>
It's like applying a linear fully connected layer at each spatial location (all the channels of a pixel)<br>
![](../../img/pasted-image-20230712164050.png)<br>
The final inception module exploits this property by applying 1x1 convolutions **before** large convolutions and after maxpooling.<br>

- Drastically reduces number of output channels:<br>
  - 16 instead of 192 for the 5x5<br>
  - 96 instead of 192 for the 3x3<br>
  - after maxpool we end up with 32 channels instead of 192<br>
  - **total output channel is 256, instead of 416**<br>
- Time complexity is reduced for convolutions<br>
- Less parameters<br>
  ![](../../img/pasted-image-20230712164333.png)<br>
  ![](../../img/pasted-image-20230824181028.png)<br>
  So the final inception module is composed of 4 parallel paths:<br>
- 1x1 conv + 5x5 conv<br>
- 1x1 conv + 3x3 conv<br>
- 1x1 conv <br>
- 3x3 max pooling + 1x1 conv (to drastically reduce the channels)<br>

## Global Average Pooling<br>

Instead of using expensive fully connected layers as classifiers, Inception obtains the same result by first applying average pooling (takes average of the neighborhood), requiring drastically less parameters.<br>
In the end only **one fully connected layer** with softmax activation is applied to predict the classes.<br>
![](../../img/pasted-image-20230831180642.png)
