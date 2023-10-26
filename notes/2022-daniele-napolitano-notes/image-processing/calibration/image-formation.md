_A more realistic mathematical model than the Pinhole Camera model, useful for Camera calibration_<br>
# From 3D to 2D<br>
The 3D **World Reference Model (WRF)** has to be projected in the **Camera Reference Model (CRF)**, and then in a 2D plane (the image plane).<br>
![](../../img/pasted-image-20230501172900.png)<br>
## Extrinsic parameters<br>
To pass from WRF to CRF we need to change the origin, performing **rototranslation**.<br>
6 PARAMETERS NEEDED:<br>
- 3 for translation<br>
- 3 for rotation (they are actually 9, but only 3 are linearly independent)<br>
![](../../img/pasted-image-20230501173018.png)<br>
## Intrinsic parameters<br>
4 PARAMETERS NEEDED:<br>
$f_{u}=\frac{f}{\Delta u}$, $f_{v}=\frac{f}{\Delta v}$, $u_0$ and $v_0$ -> **camera geometry**.<br>
![](../../img/pasted-image-20230501173142.png)<br>
<br>
# Projective space<br>
The final model is non-linear:<br>
![](../../img/pasted-image-20230501173341.png)<br>
We can make it linear by considering a virtual **Projective space**, equivalent of the euclidean space of the WRF.<br>
<br>
The generic vector $[x, y, z] ∈ R^{3}$ becomes $[x, y, z, 1] ∈ P^{3}$ , i.e. a fourth dimension is added to the vector. Moreover, the new vector can be multiplied by a constant k 6= 0 and can still represent the same original point from $R^{3}$ . In general, if a n-dimensional projective space vector is given, this will just be represented as a vector of n + 1 elements: $[x1, x2, . . . , xn+1]$. <br>
To shift from P3 to R3 it is necessary to divide every element by xn+1 (to get the 1 in the additional dimension) and than remove the last dimension.<br>
In projective space, points are represented as lines. This allows the representation of points at infinity, i.e. when xn+1 = 0, while in R3 this is not possible.<br>
### Intrinsic parametric matrix<br>
We can rewrite the linear equation of the intrinsic parameters in matrix form in projective space, as follows:<br>
![](../../img/pasted-image-20230501181004.png)<br>
<br>
<br>
![](../../img/pasted-image-20230501181054.png)<br>
The left 4x3 matrix $P_{int}$ is called **intrinsic parametric matrix**, often referred as $[A|0]$.<br>
A is always upper right triangular.<br>
Realistic models also include a 5th parameter, called **skew**, to take into account non orthogonalities between axes of the sensor.<br>
<br>
### Extrinsic parameter matrix<br>
We just take the already known parameter matrices and project in projective space<br>
![](../../img/pasted-image-20230501181331.png)<br>
This matrix is also referred as **G**.<br>
G is defined by R and t<br>
<br>
## Perspective projection matrix<br>
Getting the two matrices together, we get the PPM, also referred as **P**:<br>
![](../../img/pasted-image-20230501181404.png)<br>
P is a 3x4 full-rank matrix = $P_{int}G$<br>
<br>
# Lens distortion<br>
PPM is based on Pinhole Camera model, but real Lenses introduce distortions to it.<br>
We need to take into account their effect:<br>
![](../../img/pasted-image-20230501182600.png)<br>
<br>
**KINDS OF DISTORTIONS:**<br>
- **Radial distortion** -> lens curvature (making straight lines curve)<br>
	- **Barrel distortion** -> straight lines bend outwards ("botte")<br>
	- **Pincushion distortion** -> straight line bend inwards ("puntaspilli")<br>
![](../../img/pasted-image-20230501183300.png)<br>
- **Tangential distortion** -> misalignment and defects of the camera components<br>
<br>
Lens distortion is modeled through a non-linear transformation.<br>
![](../../img/pasted-image-20230501192607.png)<br>
L(r) is the **radial distortion function**, proportional to the radius r (distance to the center of image in (0,0)).<br>
![](../../img/pasted-image-20230501193602.png)<br>
This adds k new parameters to the complete camera model<br>
<br>
# Complete camera model<br>
![](../../img/pasted-image-20230501193654.png)