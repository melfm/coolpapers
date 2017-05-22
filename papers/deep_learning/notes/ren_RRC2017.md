# Accurate Single Stage Detector Using Recurrent Rolling Convolution

## Missing piece of state of the art methods
- Faster R-CNN relies on large receptive field of each overlapping 3x3 area of last conv layer to detect both small and large objects.
- Since multiple pooling layers are used, resulting resolution is much smaller than the input.
- SSD method uses higher res feature maps to detect small objects and lower res for big.

## Recurrent Rolling Convolution
-
