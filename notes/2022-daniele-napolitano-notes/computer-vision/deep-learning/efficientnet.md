_Scales ResNet in depth, width and resolution in an optimal way._<br>
## Scaling ResNet<br>
ResNet allowed to scale depth.<br>
Model scaling can also increase width (n° of channels) or resolution:<br>
![](../../img/pasted-image-20230718225812.png)<br>
## Compound scaling<br>
The best solution, is scaling everything together: **compound scaling**<br>
In fact, by only scaling one dimension (inevitably increasing flops), we reach a plateau around 80% accuracy<br>
![](../../img/pasted-image-20230718225926.png)<br>
In the graph below, width is increased. The other two parameters variation changes depending on the color of the line.<br>
The red line represent variation in all dimensions, and is able to reach the highest score, while also using less flops than other ones:<br>
![](../../img/pasted-image-20230718230119.png)<br>
### Compound scaling coefficient<br>
Flops increase linearly for depth, but quadratically for width and resolution. we want that the product of all 3 coefficients to be around 2:<br>
![](../../img/pasted-image-20230718230417.png)<br>
It is like a linear programming problem, performing grid search to find the optimal combination of parameters.<br>
# EfficientNet-B0<br>
The baseline network for EfficientNet was searched using **Neural Architecture Search** (a neural network which optimizes ResNet architectures), letting it perturb a model similar to MobileNet V2.<br>
It ended up alternating apparently random convolution sizes (3x3 and 5x5), while always using Squeeze and Excitation modules (from ResNet#SENet).<br>
![](../../img/pasted-image-20230718230837.png)<br>
Indeed EfficientNet managed to score the highest accuracy with moderate flops compared to its competitors.<br>
<br>
![efficientnet](https://www.youtube.com/watch?v=3svIm5UC94I)