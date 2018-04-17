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
class AlgorithmTwo(models.Model):
    '''This algorithm computes the visual range for two images, one target each.'''
    
    # Foreign key reference to picture

    picture = models.ForeignKey(Picture, unique=True,
                                on_delete=models.CASCADE)

    # The image for the far target

    image2 = models.ImageField(upload_to='pictures/')

    # Our calculated visual range

    calculatedVisualRange = models.FloatField(null=True, default=0)

    # X value for the far target

    farX = models.FloatField(null=False, default=0)

    # Y value for the far target

    farY = models.FloatField(null=False, default=0)

    # X value for the near target

    nearX = models.FloatField(null=False, default=0)

    # Y Value for the near target

    nearY = models.FloatField(null=False, default=0)

    farRadius = models.FloatField(null=True)

    nearRadius = models.FloatField(null=True)

    # Distance to far target

    farDistance = models.FloatField(null=True, default=0)

    # Distance to near target

    nearDistance = models.FloatField(null=True, default=0)

    # Must define

    def __str__(self):
        return 'AlgorithmTwo'

    # Find and assign the visual range based off the given picture object

    def findTwoTargetContrastVr(self):
        self.picture.image.seek(0)
        self.image2.seek(0)

        # Open images

        image1 = Image.open(StringIO(self.picture.image.read()))
        image2 = Image.open(StringIO(self.image2.read()))

        # Convert to RGB values for each pixel in the images

        nearPixelData = image1.convert('RGB')
        farPixelData = image2.convert('RGB')

        # Set up containers for red green and blue for each target

        farRed = []
        farGreen = []
        farBlue = []
        nearRed = []
        nearGreen = []
        nearBlue = []

        newFarX = int(self.farX)
        newFarY = int(self.farY)
        newNearX = int(self.nearX)
        newNearY = int(self.nearY)
        radius = int(self.nearRadius)

        # The radius must be the smaller of the two radiuss

        if int(self.farRadius) < radius:
            radius = int(self.farRadius)

        # Process far target first

        for x in range(newFarX, newFarX + radius * 2):
            for y in range(newFarY, newFarY + radius * 2):
                try:
                    (R, G, B) = farPixelData.getpixel((x, y))
                    farRed.append(R)
                    farGreen.append(G)
                    farBlue.append(B)
                except Exception:
                    print 'Out of bounds when getting pixel data'

        # Do the same for near target

        for x in range(newNearX, newNearX + radius * 2):
            for y in range(newNearY, newNearY + radius * 2):
                try:
                    (R, G, B) = nearPixelData.getpixel((x, y))
                    nearRed.append(R)
                    nearGreen.append(G)
                    nearBlue.append(B)
                except Exception:
                    print 'Out of bounds at x:' + str(x) + 'y:' + str(y)

        # Now we need to run the function 3 times one for each color band then
        # average them together

        vrR = TwoTargetContrast(farRed, nearRed, self.farDistance,
                                self.nearDistance)
        vrG = TwoTargetContrast(farGreen, nearGreen, self.farDistance,
                                self.nearDistance)
        vrB = TwoTargetContrast(farBlue, nearBlue, self.farDistance,
                                self.nearDistance)

        self.calculatedVisualRange = abs((vrR[0] + vrG[0] + vrB[0]) / 3)

    # Override the save function for AlgorithmOne

    def save(self):
        try:
            self.findTwoTargetContrastVr()
        except Exception, e:
            print 'ERROR CALCULATING VR: ' + e.message

        super(AlgorithmTwo, self).save()
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
# TODO: Simplify this function and adapt backend code.
class tio(models.Model):
    '''Computes VR for two Targets in one image.'''

    # Foreign key reference to picture

    picture = models.ForeignKey(Picture, unique=True,
                                on_delete=models.CASCADE)

    # Our calculated visual range

    calculatedVisualRange = models.FloatField(null=True, default=0)

    # X value for the far target

    farX = models.FloatField(null=False, default=0)

    # Y value for the far target

    farY = models.FloatField(null=False, default=0)

    # X value for the near target

    nearX = models.FloatField(null=False, default=0)

    # Y Value for the near target

    nearY = models.FloatField(null=False, default=0)

    farRadius = models.FloatField(null=True)

    nearRadius = models.FloatField(null=True)

    # Distance to far target

    farDistance = models.FloatField(null=True, default=0)

    # Distance to near target

    nearDistance = models.FloatField(null=True, default=0)

    # Must define

    def __str__(self):
        return 'AlgorithmOne'

    # Find and assign the visual range based off the given picture object

    def findTwoTargetContrastVr(self):
        self.picture.image.seek(0)

        # Open image

        image = Image.open(StringIO(self.picture.image.read()))

        # Convert to RGB values for each pixel

        pixelData = image.convert('RGB')

        # Set up containers for red green and blue for each target

        farRed = []
        farGreen = []
        farBlue = []
        nearRed = []
        nearGreen = []
        nearBlue = []

        newFarX = int(self.farX)
        newFarY = int(self.farY)
        newNearX = int(self.nearX)
        newNearY = int(self.nearY)
        radius = int(self.nearRadius)

        # The radius must be the smaller of the two radiuss

        if int(self.farRadius) < radius:
            radius = int(self.farRadius)

        # Process far target first

        for x in range(newFarX, newFarX + radius * 2):
            for y in range(newFarY, newFarY + radius * 2):
                try:
                    (R, G, B) = pixelData.getpixel((x, y))
                    farRed.append(R)
                    farGreen.append(G)
                    farBlue.append(B)
                except Exception:
                    print 'Out of bounds when getting pixel data'

        # Do the same for near target

        for x in range(newNearX, newNearX + radius * 2):
            for y in range(newNearY, newNearY + radius * 2):
                try:
                    (R, G, B) = pixelData.getpixel((x, y))
                    nearRed.append(R)
                    nearGreen.append(G)
                    nearBlue.append(B)
                except Exception:
                    print 'Out of bounds at x:' + str(x) + 'y:' + str(y)

        # Now we need to run the function 3 times one for each color band then
        # average them together

        vrR = TwoTargetContrast(farRed, nearRed, self.farDistance,
                                self.nearDistance)
        vrG = TwoTargetContrast(farGreen, nearGreen, self.farDistance,
                                self.nearDistance)
        vrB = TwoTargetContrast(farBlue, nearBlue, self.farDistance,
                                self.nearDistance)

        self.calculatedVisualRange = abs((vrR[0] + vrG[0] + vrB[0]) / 3)

    # Override the save function for AlgorithmOne

    def save(self):
        try:
            self.findTwoTargetContrastVr()
        except Exception, e:
            print 'ERROR CALCULATING VR: ' + e.message

        super(AlgorithmOne, self).save()
```

See source code.


## Glossary

1. **Particulate Matter (PM)**: (includes both PM2.5 and PM10) the term for a mixture of solid particles and liquid droplets found in the air. Some particles, such as dust, dirt, soot, or smoke, are large or dark enough to be seen with the naked eye. Others are so small they can only be detected using an electron microscope ([source](https://www.epa.gov/pm-pollution))  PM2.5 is the size fraction of particulate matter with an aerodynamic diameter of 2.5 micrometers and smaller.
2. **Visual Range (VR)**:  measure of distance at which two objects appear indisinct; worsened by inclimate weather and presence of Particulate Matter  in the surrounding air.
3. **Visual Scene**: image representation of a natural landscape, including sky, mountain ranges, trees, etc.  
4. **Target**: small interface element placed on screen by user.  

