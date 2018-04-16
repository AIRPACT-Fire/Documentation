<p align="center">
  <img src="https://raw.githubusercontent.com/AIRPACT-Fire/design/master/banner/v1/png/banner_small.png"/>
</p>

Empowering air-quality research through ubiquitous mobile technology: To support better understanding how estimation of visual range can contribute to useful quantificatoin of air quality, and app has been developed to assist in measuring [Visual Range (VR)] (https://en.wikipedia.org/wiki/Runway_visual_range) as a means of characterizing PM2.5 [Particulate Matter (PM)](https://www.epa.gov/pm-pollution). This is all within a crowd-sourced framework for research.

_Insert platform diagram here_

## Air-Quality Estimation Algorithms

The AIRPACT-Fire platform experimentally applies the following algorithms, both for loss of contrast with distance effect, in estimating visual range of images:

* One For One  (two images taken at different distances are analyzed for Visual Range)
* Two In One  (one image containing two targets at known distances ise analyzed for Visual Range)

Each is designed to capture for analysis one or more images, and process such images for contrast within use-defined targets, to reportt an estimate of VR (in meters or miles). The underlying assumption is that loss of contrast with distance is a reasonable determinant of VR, and VR is also a reasonable determinant of PM2.5. Given these assumptions, we can estimate the air-quality of some natural space given just a picture or two!  In addition, the app build a gallery of images submitted by users and offers the possibility of  testing additonal image analysis algorithms  or techniques to establish VR or other Air-quality measures.

This is a research platform. Consequently, these algorithms are subject to adaptation given evidence, being experimentally validated on this site. We currently require expert knowledge to validate and improve our VR algorithms. Through this inquiry, more refined approaches are likely to surface. You will see them updated here.

### One For One (OFO)

_Insert diagram here_

_Also called Near-Far Contrast_

#### Process

Capture two images containing the same target from two locations with the observer at known distances from the target. 

#### Motivation

Capturing images of an object at two distinct distances -- ideally with a separation of at least ?? km ( ?? miles) -- captures information on how the percieved contrast of this object varies with respect to distance. This difference in contrast is directly related to the VR in the user's vicinity.

#### Critical Assumptions

Both images are captured within a short period of time such that prevailing air quality and angles of sun illumination and viewer angle (azimuth) are all essentailly invariant.

#### How It's Done

```python
def ofo(img):
	pass
```

See source code.

### Two In One (TIO)

_Insert diagram here_

_Also called Near-Sky Contrast_

#### Process

Capture one image containing two Targets in the same Visual Scene.  The two targets must be a known distances from the observer. 

#### Motivation

Capturing images of two objects,  assumed to have equal inherent contrast, at two distinct distances -- ideally with a separation of at least ?? km ( ?? miles) -- captures information on how the percieved contrast varies with respect to distance. This difference in contrast is directly related to the VR in the user's vicinity.

#### Critical Assumptions

Both targets withing the image have equal inherent contrast and angles of sun illumination and viewer angle (azimuth) are both effectively equal.

#### How It's Done

```python
def tio(img):
	pass
```

See source code.


## Glossary

1. **Particulate Matter (PM)**: (includes both PM2.5 and PM10) the term for a mixture of solid particles and liquid droplets found in the air. Some particles, such as dust, dirt, soot, or smoke, are large or dark enough to be seen with the naked eye. Others are so small they can only be detected using an electron microscope ([source](https://www.epa.gov/pm-pollution))  PM2.5 is the size fraction of particulate matter with an aerodynamic diameter of 2.5 micrometers and smaller.
2. **Visual Range (VR)**:  measure of distance at which two objects appear indisinct; worsened by inclimate weather and presence of Particulate Matter  in the surrounding air.
3. **Visual Scene**: image representation of a natural landscape, including sky, mountain ranges, trees, etc.  
4. **Target**: small interface element placed on screen by user.  

