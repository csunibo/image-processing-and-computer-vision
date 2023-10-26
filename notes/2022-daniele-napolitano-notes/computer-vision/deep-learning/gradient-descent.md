_The algorithm used in Neural Networks (MLP) to optimize the parameters. (and to perform training)._<br>
# Optimization<br>
To solve the curse of dimensionality problem, we convert it to a similar, simpler problem: minimizing a **loss function** which is related to the actual problem.<br>
This objective function is much simpler to optimize, and if chosen correctly, will result in a good outcome for the original problem.<br>
- if the loss is high, the classifier is bad -> LOW ACCURACY<br>
- if the loss is low, the classifier is good -> HIGH ACCURACY<br>
$$\theta^*=argmin_{\theta}L(\theta,D^{train})$$<br>
The parameters to optimize ($\theta$) are those of the loss function, and the input of the latter is the training data (pixels of the images).<br>
## Loss functions<br>
The kind of loss function we want to use depends on how we want to represent the label.<br>
### RMSE loss<br>
Uses the **one-hot encoding** for the label representation:<br>
$$L(\theta,(x^i,y^y)))=L(Wx^{i}+b, y^i)=||Wx^{i}+b-1hot(y^i)||_2$$<br>
![](../../img/pasted-image-20230820202558.png-|-600)<br>
<br>
### Softmax activation function<br>
There are practical reason to prefer a loss that transforms the scores computed by the classifier into **probabilities**, then perform MLE of $\theta$.<br>
Softmax is an **activation function** (NOT a loss), which represents the label as a mass probability vector<br>
$$p_{model}(Y=j|X=x^{i};\theta)=softmax_{j}(s)=\frac{exp(s_j)}{\sum\limits_{k=1}^{C}exp(s_k)}$$<br>
![](../../img/pasted-image-20230820202847.png-|-600)<br>
### Cross-entropy loss<br>
Cross-entropy loss relies on the probability mass vector given by the Softmax activation function, and performs MLE estimation to find the optimal parameters $\theta$:<br>
$$\theta^{*}=argmax_{\theta}p_{model}(y^{1}, \dots , y^{N}|x^{1}, \dots,x^{N}, \theta)=\dots=argmin_{theta}\sum\limits_{i=1}^{N}-log(p_{model}(Y=y^{i}|X=x^{i}; \theta))$$<br>
Given an example of predicted label, with a softmax activation:<br>
![](../../img/pasted-image-20230820203708.png)<br>
- If the true class is bird (hence the model was correct), the cross-entropy loss computed is 0.1 (=-log(0.9)),<br>
- If the true class is car, the loss is 2.4 (=-log(0.09)), which is a much higher value, that has much more impact when updating the parameters.<br>
<br>
# Gradient descent<br>
The algorithm that updates the parameters of Neural Networks (MLP), thus performing the actual learning. <br>
It kinda resembles the natural way in which Neurons change their connections with Surprise#Prediction error.<br>
<br>
1. **Forward pass** -> compute prediction and its loss<br>
2. **Backward pass** -> compute the gradient for all nodes, starting from the output going back to the input.<br>
	It is a non trivial operation, which can be performed:<br>
	- numerically, applying the derivation formula<br>
	- analytically, applying the chain rule <br>
	- algorithmically, with **backpropagation**<br>
1. **Update parameters** -> update values of each node according to gradient values: $$\theta^{n}=\theta^{n-1}-\alpha g$$Where $\alpha$ is the learning rate and $g$ the gradient (similar to Surprise#Rescolra-Wagner model of Pavlovian conditioning)