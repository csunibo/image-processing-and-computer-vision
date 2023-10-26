_Very **simple and schematic** network, divided into **stages** of fixed components. _<br>
<br>
Uses a combination of:<br>

- **3x3 conv** with stride 1, P 1<br>
- 2x2 max-poolig stride 2, P<br>
  and doubling n° of channels after each pool<br>
  finally 3 fully connected layers and a softmax activation.<br>
  <br>
  PROS:<br>
- simple and schematic = easy to code<br>
  CONS:<br>
- It has a lot of parameters (many channels) = slow<br>
  <br>
  pre-initialization of weight was still necessary since batchnorm was not invented yet<br>
  ![](../../img/pasted-image-20230712123256.png)<br>
- There is a repetition of "**stages**"<br>
  a stage for example is $conv+conv*2+pool$, or $conv+conv+conv+pool$.<br>
- One stage ha same receptive field of larger convolutions, but requires less parameters and computation, introduces non-linearities.<br>
- After each downsampling of the image size (with maxpool), the number of channels is doubled, to keep the computational cost per layer constant, and still having a bigger receptive field.<br>
  <br>
  <br>

## 3x3 convolutions<br>

With VGG came the understanding that we don't need large convolutions, and that multiple 3x3 convs can have the same effect of a bigger one. <br>
Smaller convolutions = less parameters = less computation. <br>
Although, in the first layers (stem), it's best to use bigger convolutions to rapidly squeeze the image.<br>
Here is a video about [**3x3 conv**](https://www.youtube.com/watch?v=V9ZYDCnItr0)<br>

### VGG-16<br>

![](../../img/pasted-image-20230712151831.png)<br>
<br>
Here is a video about [**VGG**](https://www.youtube.com/watch?v=YcmNIOyfdZQ)<br>
