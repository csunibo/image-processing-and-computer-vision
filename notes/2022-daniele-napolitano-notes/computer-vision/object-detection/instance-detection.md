_ Given a **template image** of a specific **instance object**, determine if it is present in the **target image** under analysis. In case of detection, also estimate the pose._<br>
The pose can be:<br>
- Translation<br>
- Roto-translation<br>
- Similarity (roto-translation + scaling)<br>
<br>
Other nuisances to be dealt with are intensity changes, occlusions, and clutter.<br>
There are different methods that can be applied.<br>
<br>
# Template matching<br>
Model image is slid across target image to be compared at each position to an equally sized window. **Based on similarity functions.**<br>
It's a slow technique: $O(MN \cdot WH)$ -> size of template (MN) $\cdot$ size of target (WH)<br>
<br>
![](../../img/pasted-image-20230706115320.png-|-600)<br>
## Similarity functions<br>
The matching is performed according to similarity functions, that compute how much the current grid is dissimilar to the template.<br>
![](../../img/pasted-image-20230716184305.png)<br>
Left: template grid -> size MN<br>
Right: template grid sliding onto the Image -> size HW. <br>
(i,j) is top left pixel corner coordinates of target grid<br>
<br>
- **Sum of Squared Differences:** $$SSD(i,j)=\sum_{m=0}^{M-1} \sum_{n=0}^{N-1}(I(i+m,j+n)-T(m,n))^2$$<br>
- **Sum of Absolute Differences:** $$SAD(i,j)=\sum_{m=0}^{M-1} \sum_{n=0}^{N-1}|I(i+m,j+n)-T(m,n)|$$<br>
Normalization is essential, since change in intensity would mess up the matching process (a bright area would have high correlation than the actual target just for the intensity)<br>
- **Normalized Cross Correlation:** $$NCC(i,j)=\frac{\sum_{m=0}^{M-1} \sum_{n=0}^{N-1}I(i+m,j+n)\cdot T(m,n)}{\sqrt{\sum_{m=0}^{M-1} \sum_{n=0}^{N-1}I(i+m,j+n)^{2}} \cdot \sqrt{\sum_{m=0}^{M-1} \sum_{n=0}^{N-1}T(m,n)^{2}}}$$<br>
	It is invariant to linear intensity changes<br>
- **Zero-mean Normalize Cross correlation:** <br>
	It is invariant to affine intensity changes (linear + bias)<br>
	First we compute the means: $$\mu(\tilde I)=\frac{1}{MN} \sum_{m=0}^{M-1} \sum_{n=0}^{N-1} I(i+m, j+n)$$<br>
	$$\mu(T)=\frac{1}{MN} \sum_{m=0}^{M-1} \sum_{n=0}^{N-1} T(m,n)$$<br>
Then the ZNCC is just the NCC there those means are subtracted<br>
	$$ZNCC(i,j)=\frac{\sum_{m=0}^{M-1} \sum_{n=0}^{N-1}(I(i+m,j+n)-\mu(\tilde I)) (T(m,n)-\mu(T))}{\sqrt{\sum_{m=0}^{M-1} \sum_{n=0}^{N-1}(I(i+m,j+n)-\mu(\tilde I))^{2}} \cdot \sqrt{\sum_{m=0}^{M-1} \sum_{n=0}^{N-1}(T(m,n)-\mu(T))^{2}}}$$<br>
It's invariant to affine intensity changes: $\tilde I(i,j)=\alpha \cdot T + \beta$<br>
<br>
![](../../img/pasted-image-20230706131918.png)<br>
![template matching video](https://www.youtube.com/watch?v=1_hwFc8PXVE)<br>
## Fast template matching<br>
To speed up the slow template matching, we can use an image pyramid, similar to the Detector#Scale-Space.<br>
Each level is a smoothed and sub-sampled<br>
![](../../img/pasted-image-20230706132219.png)<br>
Full search at top (smallest) level, then local refinements traversing down the pyramid.<br>
<br>
# Shape-based Matching<br>
**Edge-based** #Template matching approach.<br>
1) A set of **control points $P_k$** is extracted from the model image by an edge detector, also gradient direction is computed and stored for each $P_k$.<br>
2) At each position $(i,j)$ of the target image, the recorded gradient directions associated with control points are compared to their corresponding image points $\tilde P_{k}(i,j)$ to compute a similarity function.<br>
![](../../img/pasted-image-20230706132643.png-|-600)<br>
**Edge detection is only computed on the target (once).**<br>
So basically the comparison is done between control points, which lays on edges.<br>
### Similarity functions for shape-based matching<br>
![](../../img/pasted-image-20230706132735.png)<br>
This one uses cosine similarity <br>
![](../../img/pasted-image-20230706133013.png)<br>
<br>
# Hough Transform<br>
Detects objects with shapes that can be expressed by an equation, based on projection of the input into **Hough space** (or parameter space).<br>
- It's applied after Edge Detection. <br>
- Robust to noise, and partially to occlusion.<br>
- Generalized Hough Transform -> advanced version that recognizes any shape<br>
<br>
### Hough space<br>
Given the linear equation $y-mx-c=0$, one can fix y and x (instead of m and c), and find all possible lines through the specific fixed point at $(x,y)$.<br>
If we consider the intercepting line between two points $P_1$ and $P_2$ in parameter space, we end up with two lines in Hough space that intercept in one point, which represents the image line through $P_1$ and $P_2$.<br>
![](../../img/pasted-image-20230706155005.png)<br>
Moreover, given n colinear image points in image space, projected in Hough space, the all the lines will merge in a single point<br>
![](../../img/pasted-image-20230706155125.png)<br>
**Therefore, rather than looking at extended shapes in images, we look for interceptions in the parameter space of lines!**<br>
<br>
Below, an example where we got blue points from the same orange line, and a green point that does not belong to it. Its line in Hough space does not pass through the orange point.<br>
![](../../img/pasted-image-20230830162437.png-|-400)<br>
### Implementation<br>
The actual Hough space needs to be quantized and allocated as a matrix, known as the **Accumulator Array (AA)<br>
![](../../img/pasted-image-20230706163741.png)<br>
<br>
![hough transform](https://www.youtube.com/watch?v=XRBc_xkZREg)<br>
