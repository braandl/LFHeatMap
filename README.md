# LFHeatMap

iOS heat map package

![LFHeatMap](lfheatmap_screenshot.png)

## Features
* extremely fast heat map generation from point/weight data pairs
* generates UIImage objects that can be overlaid as needed
* variable boost/bleed

## Anti-Features
LFHeatMap is a simple `UIImage` generator. The resulting object can be used like any other `UIImage`, standalone or in a `UIImageView`. While it can be overlaid on top of a `MKMapView`, it is not strongly tied to this specific component and hence does not offer the benefits that come with a more complex implementation of `MKOverlayRenderer`.

## Demo
This demo plots the measured magnitudes of the [2011 Virginia Earthquake](http://en.wikipedia.org/wiki/2011_Virginia_earthquake).

### Running
1. Open and launch the LFHeatMapDemo XCode project. 
2. Move the slider on the bottom to adjust the boost.

### Explanation

The data is stored in `quake.plist` which is a simple plist storing the latitude, longitude, and magnitude of each measurement. The points (locations) and weights (magnitudes) are stored in two `NSArray` objects in `viewDidLoad` of `LFHeadMapDemoViewController`.

The main action takes place in the `sliderChanged:` function. Moving the slider determines the new boost value and generates a new heat map image. The image's dimensions are the same as the `self.mapView` object, with the points and weights supplied by the two data arrays. The image is then passed to the overlaying `UIImageView` that sits on top of the map.


## LFHeatMap

This class contains the three basic static functions used to generate the heat maps.

### 1. Basic Heat Map

Supply the desired image dimensions and boost, as well as the point/value arrays. There should be a 1:1 mapping between these two arrays, that is each index in the *points* array should have a corresponding index in the *weights* array.

```
@params
rect: region frame
boost: heat boost value
points: array of NSValue CGPoint objects representing the data points
weights: array of NSNumber integer objects representing the weight of each point
 
@returns
UIImage object representing the heatmap for the specified region.
 
+ (UIImage *)heatMapWithRect:boost:points:weights:
```

### 2. Advanced Heat Map

Works generally the same as the basic heat map, but allows to tweak two additional parameters to control the "bleed" of heat rendering.

```
@params
rect: region frame
boost: heat boost value
points: array of NSValue CGPoint objects representing the data points
weights: array of NSNumber integer objects representing the weight of each point
weightsAdjustmentEnabled: set YES for weight balancing and normalization
groupingEnabled: set YES for tighter visual grouping of dense areas
 
@returns
UIImage object representing the heat map for the specified region.
 
+ (UIImage *)heatMapWithRect:boost:points:weights:weightsAdjustmentEnabled:groupingEnabled:
```

### 3. MKMapView Helper

Works the same as the basic heat map, but allows you to supply map-specific parameters. Pass an `MKMapView` object (typically the target you want to overlay), and an array of `CLLocation` objects corresponding to coordinates on the specified `MKMapView` object.

The function will convert these to the required CGRect/CGPoint values as needed.

```
@params 
mapView: Map view representing the heat map area.
boost: heat boost value
locations: array of CLLocation objects representing the data points
weights: array of NSNumber integer objects representing the weight of each point
 
@returns
UIImage object representing the heatmap for the map region.

+ (UIImage *)heatMapForMapView:boost:locations:weights:
```
