_Very deep network that uses residual connections and Batch normalization, easy to optimize._<br>
<br>
It has been observed that when scaling Convolutional Neural Networks, not always the performance increases, due to training problems, since optimizing very big networks is hard.<br>
They are easy to optimize, making it possible to train deeper networks (which are also more accurate)<br>
<br>
In standard networks (green), having more layers also means having more **train** error. ResNet aims to solves this problem with a heavy use of Batch normalization.<br>
![](../../img/pasted-image-20230829154244.png)<br>
Here is a video about [**ResNet**](https://www.youtube.com/watch?v=o_3mboe1jYI)<br>

## Residual block<br>

The solution is changing the network, by allowing to learn identity functions thanks to **residual blocks**, implemented by adding **skip connections** between input and last activation.<br>
A skip connection is just a concatenation:<br>
![](../../img/pasted-image-20230712165635.png)<br>
This network makes a heavy use of Batch normalization, using it after any convolution operation (even 1x1). This allows to have really deep networks without having bad consequences on the learning.<br>
![](../../img/pasted-image-20230712181541.png)<br>
We can see below that in ResNet (middle graph), increasing the layer count does not affect as much the training and test error, as opposed to the plain network (Same resnet architecture, without skip connections).<br>
![](../../img/pasted-image-20230829173751.png)<br>

# Residual Network<br>

They are inspired by VGG's regular design, stacking together fixed stages:<br>

- stages are stacks of residual blocks<br>
- **each residual block is a stack of two 3x3 conv with batch norm and ReLU in between**<br>
- First block (conv layer) of each stage halves the resolution and doubles channels by using stride 2<br>
- uses an initial stem layer to first reduce the resolution<br>
- end with a global average pooling layer like Inception<br>
  ![](../../img/pasted-image-20230712175132.png)<br>

## Stem layer<br>

- convolution 7x7 stride 2 <br>
- maxpool 3x3 stride 2<br>
  ![](../../img/pasted-image-20230712175232.png)<br>

## Skip connections<br>

- ResNet blocks never use maxpooling to downsample, instead the first 3x3 convolution will have stride 2 and double the channels<br>
- Since the size halves (for the stride=2) and the number of channels doubles **after** the first conv layer of a stage (since we use 2C filters), the skip input would not match with the stage output. <br>
- To solve this, it's best to apply a 1x1 convolution with stride 2 and 2C filters (followed by batch norm), so we can successfully apply the sum.<br>
  _(remember: the number of filters determine the number of output channels. The number of filter channel must be the same of the number of input channels)_<br>
  ![](../../img/pasted-image-20230712180235.png)<br>
  ![](../../img/pasted-image-20230831175055.png)<br>

## Bottleneck residual block<br>

- When designing very deep residual nets, it's best to use these kind of block, which enable faster depth increase without altering computational budget.<br>
- Adds a third 1x1 conv layer before adding the skip, with 4 times the number of channels. Also the first conv layer is 1x1. Only one 3x3 layer in the middle.<br>
- The 3x3 conv layer operates in a compressed domain (might lead to information loss)<br>
  ![](../../img/pasted-image-20230829174940.png-|-500)<br>

# Inception-ResNet<br>

Inception modules with skip connections<br>
![](../../img/pasted-image-20230718180657.png)<br>
The whole network has the same structure of vanilla ResNet, but uses Inception-resnet blocks instead.<br>

# ResNeXt<br>

Decomposes #Bottleneck residual blocks into G parallel branches, though the complexity remains similar to a standard ResNet block.<br>
Only one skip connection is present for all the G branches:<br>
![](../../img/pasted-image-20230713100243.png)<br>
Follow the **split-transform-merge** paradigm.<br>

## Grouped convolutions<br>

A technique to split the input and output channels into G groups, obtaining the same result but with less filter channels, thus reducing parameters and flops, allowing parallel computation with multiple GPUs<br>
![](../../img/pasted-image-20230713110733.png)<br>
ResNext uses grouped convolutions in residual blocks, so it can have more paths while keeping the n° of parameters small.<br>
<br>
The ResNext block is approximately an #Inception-ResNet block<br>
![](../../img/pasted-image-20230713121418.png)<br>

## SENet<br>

Uses a **squeeze and excitation** module to capture global context ant to reweight channels in each block.<br>
![](../../img/pasted-image-20230718182801.png)<br>

- squeeze = global average pooling<br>
- excitation = FC+ReLU+FC+sigmoid<br>
  There are 2 skip connections: <br>

1. vanilla between input and last relu<br>
2. between residual block and scale after the S-E block<br>
   <br>
