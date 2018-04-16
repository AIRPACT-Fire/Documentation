<p align="center">
  <img src="https://raw.githubusercontent.com/AIRPACT-Fire/design/master/banner/v1/png/banner_small.png"/>
</p>

Fueling air-quality research with ubiquitous mobile technology. In search of understanding the patterns of [Particulate Matter (PM)](https://www.epa.gov/pm-pollution) and weather by estimating [Visual Range](https://en.wikipedia.org/wiki/Runway_visual_range). This is all within a crowd-sourced framework.

_Insert platform diagram here_

## Air-Quality Estimation Algorithms

The AIRPACT-Fire platform uses the following (experimental) algorithms in estimating air quality of images:

* One For One
* Two In One

These algorithms are being experimentally validated on this site. Through this inquiry, more refined approaches are likely to surface. You will see them updated here.

### One For One (OFO)

_Insert diagram here_

_Also called Near-Far Contrast_

#### Process

Capture two images at varying distances of a single Anchor Point.

#### Motivation

Seeing an object at two distinct distances -- ideally with a separation of at least 50 feet -- gives good insight into how the chromatic intensity of this object varies with respect to distance. This variance in contrast is directly related to the 

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
def ofo(img):
	pass
```

See source code.


## Glossary

**Visual Scene**: image representation of a natural landscape, including sky, mountain ranges, trees, etc.  
**Target**: small interface element placed on screen by user.  
**Anchor Point**: large land mass within a Visual Scene (e.g., a mountain or a tree).  
**Background Point**: sky or clouded backdrop from any Anchor Point.
