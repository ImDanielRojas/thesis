## Remote Sensing Datasets
### Train and test data for model building
#### Data acquisition
The remote sensing data for building the model are ALI and Hyperion imagery. Both land imaging instruments are onboard the NASA EO-1, which partially collects data over the same area at the exact same time. ALI, the MS sensor, provides image data from 10 spectral visible NIR (VNIR) and SWIR discrete bands. Hyperion, the HS sensor, collects 242 continuous spectral channels ranging from 0.357 to 2.576 mm with a 10-nm bandwidth. Both sensors have a similar spatial resolution, pixel size, of about 30 meters.
ALI and Hyperion spectral, spatial and temporal domain overlapping makes the two sensors the best choice for building a solid model without the variance of external factors affecting the reliability of the correlation that we dealt to map.

The product scenes were acquired through USGS Earth Explorer, selecting the L1Gst level of correction, a terrain corrected product provided in 16-bit radiance values. A total of 31 scenes, during a span of 8 years (2008-2016), were collected from the surrounding area of Ciudad Real due to the high-density of land dedicated to crop soil, which produces a higher impact on surface reflectance of SWIR bands\cite{buscar}.  From the 10 ALI bands, 9 of them are MS and 1 is a panchromatic (PAN) band, which we will dismiss for our study due to the fact that it does not add relevant spectral information. From the 242 Hyperion bands, 170 bands (8--57, 79--120, 131--165, 181--223) will be used, the rest are uncalibrated or noisy and would cause a negative impact on the prediction. Hence, 170 will be the number of bands that we will predict from a 9-band input, resulting in a nearly 20x spectral super-resolution.

#### Data preprocessing
ALI and Hyperion have different product properties, they differ on height, width, pixel size, area covered and such more properties which require some preprocessing before doing any experiment.
Despite being both sensors onboard the same satellite, the pixel to pixel alignment and pixel size are not identical and thus they require to be geometrically corrected. For the correspondence of the scenes, ground control points (GCP) were manually set and ALI scenes were geometrically corrected using first-order polynomial interpolation and bilinear resampling to match Hyperion scenes. Also, their extents were clipped so they have the exact same size and shape and cover the exact same area.
All data is normalized to the [0--1] range and converted to 32-bit float data.
Satellite sensors sometimes deliver a bad behaviour by missing information in some pixels. During a study of the data, we noticed some spikes on the maximum values of different bands from a single image scene caused by these corrupted pixels. We corrected them by changing their value to the median of the same pixel from the neighbour bands.
The data that will be provided to the model will be in a set of multiple pair-identical patches extracted from the different product scenes, excluding the ones with clouds. The dataset will finally be split in half, 50\% of randomly chosen patches for the training set and the rest 50\% for the test set.

### Crop classification data
#### Data acquisition
The image data selected for crop classification is a crop-specific land use data, the Cropland Data Layer (CDL) product from the USDA NASS that covers the entire Contiguous United States (CONUS) at 30-meter spatial resolution with very high accuracy up to 95% for major crop types (i.e., grape, almond) in major crop area.

![USA NASS Crop Data Layer Classification](https://github.com/Rojas-D/thesis/blob/main/5.Appendix/images/CDL_2016_CropStats.png)
