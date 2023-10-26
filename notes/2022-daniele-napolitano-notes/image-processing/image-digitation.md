_Process in which the light is turned into discrete electric signals, and the image becomes a tensor of pixels_<br>
<br>
### Steps<br>
1. **SAMPLING** -> the light sensors convert irradiance into electric quantity<br>
	The planar _continuous_ image is sampled into both axis to a 2D matrix (NxM), but with continues values (discretization of space)<br>
![](../img/pasted-image-20230307115721.png)<br>
<br>
2. **QUANTIZATION** -> the electric values are quantized into $2^{m}$ discrete values. m is the number of bits used to represent a pixel. The final size will be NxMxm (discretization of values)<br>
![](../img/pasted-image-20230307115625.png)<br>
<br>
![](../img/pasted-image-20230307120028.png)<br>
# Camera Sensors<br>
Pinhole Camera models are made of 2D arrays of **photodetectors**. During exposure, the light is converted into electric charge. Digital cameras have ADC circuit integrated.<br>
![](../img/pasted-image-20230307120226.png)<br>
There are 2 main sensor technologies:<br>
- **CCD** (better but expensive)<br>
- **CMOS** (most used, allows to integrate the circuit with the sensors in the same chip)<br>
They both only sense intensity and not colour(Greyscale images)<br>
**OPTICAL FILTERS** are placed in front of photodetectors to render each pixel sensitive to a specific wavelength range.<br>
![](../img/pasted-image-20230307120826.png)<br>
## Signal to Noise Ratio (SNR)<br>
The intensity measured under perfectly static conditions varies due to random Image Noise. The higher the better.<br>
<br>
![](../img/pasted-image-20230307121425.png)<br>
## Dynamic Range (DR)<br>
If the sensed amount of light is too small, the true signal cannot be distinguished from noise.<br>
- $E_{min}$ = minimum detectable irradiation<br>
- $E_{max}$ = saturation irradiation (amount of lights that fills the capacity of a photodetector)<br>
$DR=\frac{E_{max}}{E_{min}}$<br>
The higher the DR, the better is the ability to simultaneously capture both dark and bright structures of the scene (higher the better)<br>
<br>
### High Dynamic Range (HDR)<br>
images obtaining by combining together a sequence of images of the same subject taken under different exposure times. This result in a very high DR.<br>
[example of HDR](https://www.hdrsoft.com/index.html)