### Depth of Field (DOF)<br>
Defined as _the distance between the nearest and the furthest objects that are in acceptably sharp focus_.<br>
The pinhole camera model has _infinite DOF_ (everything is on focus on every distance), but it allows little lights in: requires long exposure times, and that leads to motion blur.<br>
To avoid motion blur we can use a larger hole (more light in), but we lose the infinite DOF, making blurry images. the solution is using lenses.<br>
# Thin lenses model<br>
Gather more light from scene point and focus it on a single image point. Enables small exposure times with much more light. DOF is not infinite.<br>
<br>
![](../img/pasted-image-20230307105433.png)<br>
f, u and v are no longer the same as the Pinhole Camera Model.<br>
**THIN LENSES EQUATION:** $\frac{1}{u}+\frac{1}{v}=\frac{1}{f}$ <br>
if f is fixed, when u increases, v decreases.<br>
<br>
**PROPERTIES**:<br>
1. Rays parallel to optical axis are deflected to pass through F<br>
2. Rays that pass through C are undeflected<br>
## Circles of confusion<br>
Scene points behind/in front of the focusing plane will be out of focus<br>
![](../img/pasted-image-20230307105657.png)<br>
### Diaphragm<br>
Cameras ave adjustable diaphragms ti control the amount of light gathered through the hole of the lens.<br>
- small aperture= less light= smaller blur circle (motion blur)<br>
- big aperture= more light = larger blur circle (less motion blur)<br>
<br>
## Focusing mechanism<br>
In cameras, the lens can shift along the optical axis.<br>
![](../img/pasted-image-20230307114524.png)<br>
if v=f, u=$\infty$ but u can only get so big. the maximum u defines the minimum distance at which an object can be on focus  <br>
<br>
