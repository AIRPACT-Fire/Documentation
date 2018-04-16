<p align="center">
  <img src="https://raw.githubusercontent.com/AIRPACT-Fire/design/master/banner/v1/png/banner_small.png"/>
</p>

Fueling air-quality research with ubiquitous mobile technology. In search of understanding the patterns of [Particulate Matter (PM)](https://www.epa.gov/pm-pollution) and weather by estimating [Visual Range (VR)](https://en.wikipedia.org/wiki/Runway_visual_range). This is all within a crowd-sourced framework for research.

_Insert platform diagram here_

## Air-Quality Estimation Algorithms

The AIRPACT-Fire platform uses the following (experimental) algorithms in estimating air quality of images:

* One For One
* Two In One

Each is designed to take an image (2-dimensional array of pixels) and output an estimate of VR (in meters or miles). The underlying assumption of the AIRPACT-Fire platform is that visual contrast is a reasonable determinant of VR, where VR is also a reasonable determinant of PM. Given these assumptions, we can estimate the air-quality of some natural space given just a picture!

This is a research platform. Consequently, these algorithms are subject to adaptation given evidence, being experimentally validated on this site. We currently require expert knowledge to validate and improve our VR algorithms. Through this inquiry, more refined approaches are likely to surface. You will see them updated here.

### One For One (OFO)

_Insert diagram here_

_Also called Near-Far Contrast_

#### Process

Capture two images at varying distances of a single Anchor Point.

#### Motivation

Seeing an object at two distinct distances -- ideally with a separation of at least 50 feet -- gives good insight into how the chromatic intensity of this object varies with respect to distance. This variance in contrast is directly related to the VR in the user's vicinity.

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

Place two Targets in the same Visual Scene. One must rest on an Anchor Point, such as a mountain, and the other must rest on a Background Point, like the sky.

#### Motivation



#### How It's Done

```python
def tio(img):
	pass
```

See source code.


## Glossary

1. **Particulate Matter (PM)**: (PM_2.5 and PM_10) the term for a mixture of solid particles and liquid droplets found in the air. Some particles, such as dust, dirt, soot, or smoke, are large or dark enough to be seen with the naked eye. Others are so small they can only be detected using an electron microscope ([source](https://www.epa.gov/pm-pollution))
2. **Visual Range (VR)**:  measure of distance at which two objects appear indisinct; worsened by inclimate weather and presence of Particulate Matter  in the surrounding air.
3. **Visual Scene**: image representation of a natural landscape, including sky, mountain ranges, trees, etc.  
4. **Target**: small interface element placed on screen by user.  
5. **Anchor Point**: large land mass within a Visual Scene (e.g., a mountain or a tree).  
6. **Background Point**: sky or clouded backdrop from any Anchor Point.
