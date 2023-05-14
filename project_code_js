//--------------------------------------------------
//            DEFINE ANALYSIS INPUTS                |
//--------------------------------------------------
//
// Define AOIs for the project.
//--------------------------------------------------
//
// AOIs were created in geojson.io based on coordinates from other sources, and added below.
//
// GERD AOI:
var gerd_aoi = ee.Geometry.Polygon([[[35.024611679049045,11.260294010304435],
                                 [35.019555717852818,10.510826970848381],
                                 [35.348063498469116,10.508565684007843],
                                 [35.353940159854261,11.257867557175205],
                                 [35.024611679049045,11.260294010304435]]]);
//
// Roseires Dam AOI:
var roseires_aoi = ee.Geometry.Polygon([[[34.296163209371613,11.82262609030745],
                                    [34.293718268834859,11.290253593662358],
                                    [34.736302716189861,11.287991873227917],
                                    [34.739583181182397,11.820254820898015],
                                    [34.296163209371613,11.822626090307455]]]);
//
// Regional AOI (in case this is needed for visualization):
var region_aoi = ee.Geometry.Polygon([[[33.29319909707098,13.719582349467302],
                                   [33.29319909707098,10.113663943110666],
                                   [35.76696068532152,10.113663943110666],
                                   [35.76696068532152,13.719582349467302],
                                   [33.29319909707098,13.719582349467302]]]);
                                   
//
print('Creation of ROIs is complete.');
//
//
// Query and select imagery inputs for analysis.
//--------------------------------------------------
//
// Center the GEE map on the Regional AOI for context.
Map.centerObject(region_aoi);
//
//
// Import the Sentinel-1 Image Collection from GEE.
//  Source: https://developers.google.com/earth-engine/datasets/catalog/COPERNICUS_S1_GRD#description 
var s1 = ee.ImageCollection("COPERNICUS/S1_GRD");
//
//
// Query the Sentinel-1 Image Collection for each AOI and each date.
//    GERD 2021:
var s1_gerd_21 = s1.filterBounds(gerd_aoi).filterDate('2021-11-10','2021-11-30').first().clip(gerd_aoi);
//    GERD 2022:
var s1_gerd_22 = s1.filterBounds(gerd_aoi).filterDate('2022-11-15','2022-11-30').first().clip(gerd_aoi);
//    Roseires Dam 2021:
var s1_roseires_21 = s1.filterBounds(roseires_aoi).filterDate('2021-11-10','2021-11-30').first().clip(roseires_aoi);
//    Roseires Dam 2022:
var s1_roseires_22 = s1.filterBounds(roseires_aoi).filterDate('2022-11-15','2022-11-30').first().clip(roseires_aoi);
//
//
// Select the best Sentinel-1 images.
//    Define variable for best Sentinel-1 SAR image for each AOI.
//    Print image date for each best image.
//    Source: https://stackoverflow.com/questions/4631928/convert-utc-epoch-to-local-date
//    GERD 2021:
var s1_gerd_21_imgdate = s1_gerd_21.get('system:time_start').getInfo();
print("The GERD 2021 image date is:", new Date(s1_gerd_21_imgdate));
//    GERD 2022:
var s1_gerd_22_imgdate = s1_gerd_22.get('system:time_start').getInfo();
print("The GERD 2022 image date is:", new Date(s1_gerd_22_imgdate));
//    Roseires Dam 2021:
var s1_roseires_21_imgdate = s1_roseires_21.get('system:time_start').getInfo();
print("The Roseires 2021 image date is:", new Date(s1_roseires_21_imgdate));
//    Roseires Dam 2022:
var s1_roseires_22_imgdate = s1_roseires_22.get('system:time_start').getInfo();
print("The Roseires 2022 image date is:", new Date(s1_roseires_22_imgdate));
//
//
// Select VV polarization for each image.
//    GERD 2021:
var s1_gerd_21_vv = s1_gerd_21.select('VV');
//    GERD 2022:
var s1_gerd_22_vv = s1_gerd_22.select('VV');
//    Roseires Dam 2021:
var s1_roseires_21_vv = s1_roseires_21.select('VV');
//    Roseires Dam 2022:
var s1_roseires_22_vv = s1_roseires_22.select('VV');
//
//
// Select VH polarization for each image.
//    GERD 2021:
var s1_gerd_21_vh = s1_gerd_21.select('VH');
//    GERD 2022:
var s1_gerd_22_vh = s1_gerd_22.select('VH');
//    Roseires Dam 2021:
var s1_roseires_21_vh = s1_roseires_21.select('VH');
//    Roseires Dam 2022:
var s1_roseires_22_vh = s1_roseires_22.select('VH');
//
//
// Select VV and VH polarizations for each image.
//    GERD 2021:
var s1_gerd_21_vvvh = s1_gerd_21.select('VV','VH');
//    GERD 2022:
var s1_gerd_22_vvvh = s1_gerd_22.select('VV','VH');
//    Roseires Dam 2021:
var s1_roseires_21_vvvh = s1_roseires_21.select('VV','VH');
//    Roseires Dam 2022:
var s1_roseires_22_vvvh = s1_roseires_22.select('VV','VH');
//
//
// Create and display histograms of Sentinel-1 VV pixel values to determine appropriate contrast stretch for each image display.
//    Source: https://developers.google.com/earth-engine/guides/charts_image#uichartimagehistogram
//    GERD 2021:
var chart_s1_gerd_21_vv =
    ui.Chart.image.histogram({image: s1_gerd_21_vv, region: gerd_aoi, scale: 500})
        .setSeriesNames(['VV'])
        .setOptions({
          title: 'GERD 2021: VV Histogram',
          hAxis: {
            title: 'Backscatter (db)',
            titleTextStyle: {italic: false, bold: true},
          },
          vAxis:
              {title: 'Count', titleTextStyle: {italic: false, bold: true}},
          colors: ['cf513e']
        });
print(chart_s1_gerd_21_vv);
//
//    GERD 2022:
var chart_s1_gerd_22_vv =
    ui.Chart.image.histogram({image: s1_gerd_21_vv, region: gerd_aoi, scale: 500})
        .setSeriesNames(['VV'])
        .setOptions({
          title: 'GERD 2022: VV Histogram',
          hAxis: {
            title: 'Backscatter (db)',
            titleTextStyle: {italic: false, bold: true},
          },
          vAxis:
              {title: 'Count', titleTextStyle: {italic: false, bold: true}},
          colors: ['cf513e']
        });
print(chart_s1_gerd_22_vv);
//
//    Roseires Dam 2021:
var chart_s1_roseires_21_vv =
    ui.Chart.image.histogram({image: s1_gerd_21_vv, region: gerd_aoi, scale: 500})
        .setSeriesNames(['VV'])
        .setOptions({
          title: 'Roseires 2021: VV Histogram',
          hAxis: {
            title: 'Backscatter (db)',
            titleTextStyle: {italic: false, bold: true},
          },
          vAxis:
              {title: 'Count', titleTextStyle: {italic: false, bold: true}},
          colors: ['4287f5']
        });
print(chart_s1_roseires_21_vv);
//
//    Roseires Dam 2022:
var chart_s1_roseires_21_vv =
    ui.Chart.image.histogram({image: s1_gerd_21_vv, region: gerd_aoi, scale: 500})
        .setSeriesNames(['VV'])
        .setOptions({
          title: 'Roseires 2022: VV Histogram',
          hAxis: {
            title: 'Backscatter (db)',
            titleTextStyle: {italic: false, bold: true},
          },
          vAxis:
              {title: 'Count', titleTextStyle: {italic: false, bold: true}},
          colors: ['4287f5']
        });
print(chart_s1_roseires_21_vv);
//
//
// Create and display histograms of Sentinel-1 VH pixel values to determine appropriate contrast stretch for each image display.
//    Source: https://developers.google.com/earth-engine/guides/charts_image#uichartimagehistogram
//    GERD 2021:
var chart_s1_gerd_21_vh =
    ui.Chart.image.histogram({image: s1_gerd_21_vh, region: gerd_aoi, scale: 500})
        .setSeriesNames(['VH'])
        .setOptions({
          title: 'GERD 2021: VH Histogram',
          hAxis: {
            title: 'Backscatter (db)',
            titleTextStyle: {italic: false, bold: true},
          },
          vAxis:
              {title: 'Count', titleTextStyle: {italic: false, bold: true}},
          colors: ['cf513e']
        });
print(chart_s1_gerd_21_vh);
//
//    GERD 2022:
var chart_s1_gerd_22_vh =
    ui.Chart.image.histogram({image: s1_gerd_21_vh, region: gerd_aoi, scale: 500})
        .setSeriesNames(['VH'])
        .setOptions({
          title: 'GERD 2022: VH Histogram',
          hAxis: {
            title: 'Backscatter (db)',
            titleTextStyle: {italic: false, bold: true},
          },
          vAxis:
              {title: 'Count', titleTextStyle: {italic: false, bold: true}},
          colors: ['cf513e']
        });
print(chart_s1_gerd_22_vh);
//
//    Roseires Dam 2021:
var chart_s1_roseires_21_vh =
    ui.Chart.image.histogram({image: s1_gerd_21_vh, region: gerd_aoi, scale: 500})
        .setSeriesNames(['VH'])
        .setOptions({
          title: 'Roseires 2021: VH Histogram',
          hAxis: {
            title: 'Backscatter (db)',
            titleTextStyle: {italic: false, bold: true},
          },
          vAxis:
              {title: 'Count', titleTextStyle: {italic: false, bold: true}},
          colors: ['4287f5']
        });
print(chart_s1_roseires_21_vh);
//
//    Roseires Dam 2022:
var chart_s1_roseires_21_vh =
    ui.Chart.image.histogram({image: s1_gerd_21_vh, region: gerd_aoi, scale: 500})
        .setSeriesNames(['VH'])
        .setOptions({
          title: 'Roseires 2022: VH Histogram',
          hAxis: {
            title: 'Backscatter (db)',
            titleTextStyle: {italic: false, bold: true},
          },
          vAxis:
              {title: 'Count', titleTextStyle: {italic: false, bold: true}},
          colors: ['4287f5']
        });
print(chart_s1_roseires_21_vh);
//
//
// Create and display histograms of Sentinel-1 VV-VH pixel values to determine appropriate contrast stretch for each image display.
//    Source: https://developers.google.com/earth-engine/guides/charts_image#uichartimagehistogram
//    GERD 2021:
var chart_s1_gerd_21_vvvh =
    ui.Chart.image.histogram({image: s1_gerd_21_vvvh, region: gerd_aoi, scale: 500})
        .setSeriesNames(['VV-VH'])
        .setOptions({
          title: 'GERD 2021: VV-VH Histogram',
          hAxis: {
            title: 'Backscatter (db)',
            titleTextStyle: {italic: false, bold: true},
          },
          vAxis:
              {title: 'Count', titleTextStyle: {italic: false, bold: true}},
          colors: ['cf513e']
        });
print(chart_s1_gerd_21_vvvh);
//
//    GERD 2022:
var chart_s1_gerd_22_vvvh =
    ui.Chart.image.histogram({image: s1_gerd_21_vvvh, region: gerd_aoi, scale: 500})
        .setSeriesNames(['VV-VH'])
        .setOptions({
          title: 'GERD 2022: VV-VH Histogram',
          hAxis: {
            title: 'Backscatter (db)',
            titleTextStyle: {italic: false, bold: true},
          },
          vAxis:
              {title: 'Count', titleTextStyle: {italic: false, bold: true}},
          colors: ['cf513e']
        });
print(chart_s1_gerd_22_vvvh);
//
//    Roseires Dam 2021:
var chart_s1_roseires_21_vvvh =
    ui.Chart.image.histogram({image: s1_gerd_21_vvvh, region: gerd_aoi, scale: 500})
        .setSeriesNames(['VV-VH'])
        .setOptions({
          title: 'Roseires 2021: VV-VH Histogram',
          hAxis: {
            title: 'Backscatter (db)',
            titleTextStyle: {italic: false, bold: true},
          },
          vAxis:
              {title: 'Count', titleTextStyle: {italic: false, bold: true}},
          colors: ['4287f5']
        });
print(chart_s1_roseires_21_vvvh);
//
//    Roseires Dam 2022:
var chart_s1_roseires_21_vvvh =
    ui.Chart.image.histogram({image: s1_gerd_21_vvvh, region: gerd_aoi, scale: 500})
        .setSeriesNames(['VV-VH'])
        .setOptions({
          title: 'Roseires 2022: VV-VH Histogram',
          hAxis: {
            title: 'Backscatter (db)',
            titleTextStyle: {italic: false, bold: true},
          },
          vAxis:
              {title: 'Count', titleTextStyle: {italic: false, bold: true}},
          colors: ['4287f5']
        });
print(chart_s1_roseires_21_vvvh);
//
//
// Display each Sentinel-1 VV SAR Image on GEE map.
//    Scale each VV polarization SAR image based on image value histograms. 
//    Add each Sentinel-1 SAR image to the GEE map.
//    GERD 2021:
Map.addLayer(s1_gerd_21_vv, {min: -20, max: 0}, 'Sentinel-1 GERD 2021 (VV)');
//    GERD 2022:
Map.addLayer(s1_gerd_22_vv, {min: -20, max: 0}, 'Sentinel-1 GERD 2022 (VV)');
//    Roseires Dam 2021:
Map.addLayer(s1_roseires_21_vv, {min: -20, max: 0}, 'Sentinel-1 Roseires 2021 (VV)');
//    Roseires Dam 2022:
Map.addLayer(s1_roseires_22_vv, {min: -20, max: 0}, 'Sentinel-1 Roseires 2022 (VV)');
//
//
// Display each Sentinel-1 VH SAR Image on GEE map.
//    Scale each VH polarization SAR image based on image value histograms. 
//    Add each Sentinel-1 SAR image to the GEE map.
//    GERD 2021:
Map.addLayer(s1_gerd_21_vh, {min: -25, max: -10}, 'Sentinel-1 GERD 2021 (VH)');
//    GERD 2022:
Map.addLayer(s1_gerd_22_vh, {min: -25, max: -10}, 'Sentinel-1 GERD 2022 (VH)');
//    Roseires Dam 2021:
Map.addLayer(s1_roseires_21_vh, {min: -25, max: -10}, 'Sentinel-1 Roseires 2021 (VH)');
//    Roseires Dam 2022:
Map.addLayer(s1_roseires_22_vh, {min: -25, max: -10}, 'Sentinel-1 Roseires 2022 (VH)');
//
//
// Display each Sentinel-1 VV-VH SAR Image on GEE map.
//    Scale each VV-VH polarization SAR image based on image value histograms. 
//    Add each Sentinel-1 SAR image to the GEE map.
//    GERD 2021:
Map.addLayer(s1_gerd_21_vvvh, {min: -20, max: 0}, 'Sentinel-1 GERD 2021 (VV-VH)');
//    GERD 2022:
Map.addLayer(s1_gerd_22_vvvh, {min: -20, max: 0}, 'Sentinel-1 GERD 2022 (VV-VH)');
//    Roseires Dam 2021:
Map.addLayer(s1_roseires_21_vvvh, {min: -20, max: 0}, 'Sentinel-1 Roseires 2021 (VV-VH)');
//    Roseires Dam 2022:
Map.addLayer(s1_roseires_22_vvvh, {min: -20, max: 0}, 'Sentinel-1 Roseires 2022 (VV-VH)');
//
//
// Below code section is commented out as this only needs to be run once.
//    Export of rasters from GEE can be time-consuming.
//    To reduce export time, be sure to define a region to be exported.
//
//  /*
//
// Export Sentinel-1 images from GEE to Google Drive for visualization and maps.
//    Source 1: https://developers.google.com/earth-engine/apidocs/export-image-todrive
//    Parameters:
//      CRS needs to be defined on export.N
//      Sentinel-1 scale is 10 m per documentation.
//
// Select VV for export of 2021 GERD Sentinel-1 image.
Export.image.toDrive({
  image: s1_gerd_21_vv,
  description: 's1_gerd_21_vv',
  folder: 'image_outputs',
  crs: 'EPSG:32636',
  scale: 10,
  region: gerd_aoi
});
//
// Select VH for export of 2021 GERD Sentinel-1 image.
Export.image.toDrive({
  image: s1_gerd_21_vh,
  description: 's1_gerd_21_vh',
  folder: 'image_outputs',
  crs: 'EPSG:32636',
  scale: 10,
  region: gerd_aoi
});
//
// Select VV-VH for export of 2021 GERD Sentinel-1 image.
Export.image.toDrive({
  image: s1_gerd_21_vvvh,
  description: 's1_gerd_21_vvvh',
  folder: 'image_outputs',
  crs: 'EPSG:32636',
  scale: 10,
  region: gerd_aoi
});
//
// Select VV for export of 2021 Roseires Dam Sentinel-1 image.
Export.image.toDrive({
  image: s1_roseires_21_vv,
  description: 's1_roseires_21_vv',
  folder: 'image_outputs',
  crs: 'EPSG:32636',
  scale: 10,
  region: roseires_aoi
});
//
// Select VH for export of 2021 Roseires Dam Sentinel-1 image.
Export.image.toDrive({
  image: s1_roseires_21_vh,
  description: 's1_roseires_21_vh',
  folder: 'image_outputs',
  crs: 'EPSG:32636',
  scale: 10,
  region: roseires_aoi
});
//
// Select VV-VH for export of 2021 Roseires Dam Sentinel-1 image.
Export.image.toDrive({
  image: s1_roseires_21_vvvh,
  description: 's1_roseires_21_vvvh',
  folder: 'image_outputs',
  crs: 'EPSG:32636',
  scale: 10,
  region: roseires_aoi
});
//
//  Select VV for export of 2022 GERD Sentinel-1 image.
Export.image.toDrive({
  image: s1_gerd_22_vv,
  description: 's1_gerd_22_vv',
  folder: 'image_outputs',
  crs: 'EPSG:32636',
  scale: 10,
  region: gerd_aoi
});
//
//  Select VH for export of 2022 GERD Sentinel-1 image.
Export.image.toDrive({
  image: s1_gerd_22_vh,
  description: 's1_gerd_22_vh',
  folder: 'image_outputs',
  crs: 'EPSG:32636',
  scale: 10,
  region: gerd_aoi
});
//
//  Select VV-VH for export of 2022 GERD Sentinel-1 image.
Export.image.toDrive({
  image: s1_gerd_22_vvvh,
  description: 's1_gerd_22_vvvh',
  folder: 'image_outputs',
  crs: 'EPSG:32636',
  scale: 10,
  region: gerd_aoi
});
//
//  Select VV for export of 2022 Roseires Dam Sentinel-1 image.
Export.image.toDrive({
  image: s1_roseires_22_vv,
  description: 's1_roseires_22_vv',
  folder: 'image_outputs',
  crs: 'EPSG:32636',
  scale: 10,
  region: roseires_aoi
});
//
//  Select VH for export of 2022 Roseires Dam Sentinel-1 image.
Export.image.toDrive({
  image: s1_roseires_22_vh,
  description: 's1_roseires_22_vh',
  folder: 'image_outputs',
  crs: 'EPSG:32636',
  scale: 10,
  region: roseires_aoi
});
//
//  Select VV-VH for export of 2022 Roseires Dam Sentinel-1 image.
Export.image.toDrive({
  image: s1_roseires_22_vvvh,
  description: 's1_roseires_22_vvvh',
  folder: 'image_outputs',
  crs: 'EPSG:32636',
  scale: 10,
  region: roseires_aoi
});
//
print('Export of SAR images for visualization and mapping is complete.');
//
// */
//
//
// Import Sentinel-2 Image Collection:
//   This is used for training data definition and for mapping/visualization.
var s2 = ee.ImageCollection("COPERNICUS/S2_SR_HARMONIZED");
//
//
// Query Sentinel-2 Image Collection for each AOI, each date range, and lowest cloud cover (CC).
//    Filter to AOI, filter by date range.
//    Sort by least cloudy and choose the first, least cloudy image.
//    Print the image date for the best image(s).
//    Define variable for the final, best image. 
//      The best image has been filtered by the AOI,
//      the date of the least cloudy image(s),
//      mosaicked (if more than one image footrint falls within AOI,
//      and clipped to the AOI.
//    GERD 2021:
var s2_gerd_21 = s2.filterBounds(gerd_aoi).filterDate('2021-10-01','2021-11-30');
var s2_gerd_21 = s2_gerd_21.filter(ee.Filter.lt('CLOUDY_PIXEL_PERCENTAGE',1)).first();
var s2_gerd_21_imgdate = s2_gerd_21.get('system:time_start').getInfo();
print(new Date(s2_gerd_21_imgdate));
var s2_gerd_21 = s2.filterBounds(gerd_aoi).filterDate('2021-10-20','2021-10-22').mosaic().clip(gerd_aoi);
//
//    GERD 2022:
var s2_gerd_22 = s2.filterBounds(gerd_aoi).filterDate('2022-11-01','2022-11-30');
var s2_gerd_22 = s2_gerd_22.filter(ee.Filter.lt('CLOUDY_PIXEL_PERCENTAGE',1)).first();
var s2_gerd_22_imgdate = s2_gerd_22.get('system:time_start').getInfo();
print(new Date(s2_gerd_22_imgdate));
var s2_gerd_22 = s2.filterBounds(gerd_aoi).filterDate('2022-11-04','2022-11-06').mosaic().clip(gerd_aoi);
//
//    Roseires Dam 2021:
var s2_roseires_21 = s2.filterBounds(roseires_aoi).filterDate('2021-10-01','2021-11-30');
var s2_roseires_21 = s2_roseires_21.filter(ee.Filter.lt('CLOUDY_PIXEL_PERCENTAGE',2)).first();
var s2_roseires_21_imgdate = s2_roseires_21.get('system:time_start').getInfo();
print(new Date(s2_roseires_21_imgdate));
var s2_roseires_21 = s2.filterBounds(roseires_aoi).filterDate('2021-11-19','2021-11-21').mosaic().clip(roseires_aoi);
//
//    Roseires Dam 2022:
var s2_roseires_22 = s2.filterBounds(roseires_aoi).filterDate('2022-10-01','2022-11-30');
var s2_roseires_22 = s2_roseires_22.filter(ee.Filter.lt('CLOUDY_PIXEL_PERCENTAGE',1)).first();
var s2_roseires_22_imgdate = s2_roseires_22.get('system:time_start').getInfo();
print(new Date(s2_roseires_22_imgdate));
var s2_roseires_22 = s2.filterBounds(roseires_aoi).filterDate('2022-11-04','2022-11-06').mosaic().clip(roseires_aoi);
//
//
// Display each Sentinel-2 MSI image on the GEE map.
//    Source: https://developers.google.com/earth-engine/apidocs/map-addlayer
//    Select true-color band combination of RGB (map channels to: B4,B3,B2) for each image.
//    GERD 2021:
Map.addLayer(s2_gerd_21, {bands:['B4','B3','B2'],min:0,max:3000,gamma:1.5}, 'Sentinel-2 GERD 2021');
//    GERD 2022:
Map.addLayer(s2_gerd_22, {bands:['B4','B3','B2'],min:0,max:1200,gamma:1.5}, 'Sentinel-2 GERD 2022');
//    Roseires Dam 2021:
Map.addLayer(s2_roseires_21, {bands:['B4','B3','B2'],min:0,max:1200,gamma:1.5}, 'Sentinel-2 Roseires 2021');
//    Roseires Dam 2022:
Map.addLayer(s2_roseires_22, {bands:['B4','B3','B2'],min:0,max:1200,gamma:1.5}, 'Sentinel-2 Roseires 2022');
//
//
// Below code section is commented out as this only needs to be run once.
//    Export of rasters from GEE can be time-consuming.
//    To reduce export time, define a region to be exported.
//
/*
//
// Export images for visualization and mapping.
//--------------------------------------------------
//
// Export 2022 Sentinel-2 images from GEE to Google Drive for visualization and maps.
//    Source 1: https://developers.google.com/earth-engine/apidocs/export-image-todrive
//    Source 2: https://sentinels.copernicus.eu/web/sentinel/user-guides/sentinel-2-msi/resolutions/spatial
//    Parameters:
//      CRS needs to be redefined on export to EPSG:32636, WGS 84 / UTM zone 36N
//      Sentinel-2 scale is 10 m in Bands 4-2.
//    Select bands for export of 2022 GERD Sentinel-2 RGB image:
var s2_gerd_22_rgb = s2_gerd_21.select(['B4', 'B3', 'B2']);
//
//  Export image to Google Drive for download and use in QGIS desktop software:
Export.image.toDrive({
  image: s2_gerd_22_rgb,
  description: 's2_gerd_22_rgb',
  folder: 'image_outputs',
  crs: 'EPSG:32636',
  scale: 10,
  region: gerd_aoi
});
//
//  Select bands for export of 2022 Roseires Dam Sentinel-2 RGB image:
var s2_roseires_22_rgb = s2_roseires_22.select(['B4', 'B3', 'B2']);
//  Export image to Google Drive for download and use in QGIS:
Export.image.toDrive({
  image: s2_roseires_22_rgb,
  description: 's2_roseires_22_rgb',
  folder: 'image_outputs',
  crs: 'EPSG:32636',
  scale: 10,
  region: roseires_aoi
});
//
print('Export of RGB images for visualization and mapping is complete.');
//
*/
//
//
//--------------------------------------------------
//            2021 GERD CLASSIFICATION              |
//--------------------------------------------------
//
// Create training and validation datasets.
//--------------------------------------------------
//
// Create ROIs for supervised classification training.
//    Source: https://developers.google.com/earth-engine/guides/classification
//    NOTE: CREATING ROIS IN GEE ADDS THOUSANDS OF LINES OF CODE TO A CODE DOCUMENT. This can
//      quickly increase the size of the code document to greater than GEE's limie of 512 KB.
//      For best results, follow the below steps in separate GEE code documents, export the 
//      resulting ROIs (FeatureCollections) to CSVs (small file size), and then import each
//      ROI CSV to this main code document as a CSV/table to a FeatureCollection.
//    Create ROIs in GEE:
//      Define and collect training data manually within the GEE interface as a FeatureCollection.
//      Name the ROI set for water, 'Import as' FeatureCollection, add a Propery of 'landcover' 
//        and give it a value of 1. Name the ROI set for non-water, 'Import as' FeatureCollection, 
//        add a Propery of 'landcover' and give it a value of 2.
//      Collect points for each class using the 2021 GERD True-Color Sentinel-2 image & the 2021 
//        GERD Sentinel-1 SAR VV polarization image as references.
//      Create one FeatureCollection of the two ROI FeatureCollections defined manually.
//      Merge the water and the nonwater classes.
//      'waterg21' is the name of the manually-defined water points FeatureCollection.
//      'nonwaterg21' is the name of the manually-defined non-water points FeatureCollection.
//      var gerd_21_rois = waterg21.merge(nonwaterg21);
//
// Import ROIs from Google Drive.
//    These will be the ROIs created manually in a separate GEE code document per above steps.
//    When prompted by GEE, convert to an Import Record for easier access/data management.
// var gerd_21_rois = ee.FeatureCollection('projects/ee-ktradke/assets/gerd_21_rois_FC');
//
// Split the ROI FeatureCollection into Training and Validation data.
//    This enables a validation of the accuracy and precision of the classification image.
//    Add a random column to the ROI FeatureCollection to hold the split values:
var gerd_21_rois = gerd_21_rois.randomColumn();
//    Randomly split 70% of the FeatureCollection ROIs into a Training ROI dataset:
//      Sources vary on the recommended % split of training vs. validation.
//      Especially as RandomForest 'protects against overfitting'.
//      For more information, see Source: shorturl.at/bdipU
var gerd_21_roi_train = gerd_21_rois.filter(ee.Filter.lt('random',0.7));
//    Randomly split 30% of the FeatureCollection ROIs into a Validation ROI dataset:
var gerd_21_roi_val = gerd_21_rois.filter(ee.Filter.gte('random',0.3));
// Define the band(s) to be used in the supervised classification.
//     Select VV polarization of the Sentinel-1 SAR imagery.
var gerd_21_bands = ['VV','VH'];
//
// Populate the Training ROIs with pixel values (SAR backscatter value, in decibels).
//   Note: 'landcover' was created when manually defining the ROIs above.
var gerd_21_training = s1_gerd_21.select(gerd_21_bands).sampleRegions({
  collection: gerd_21_roi_train,
  properties: ['landcover'],
  scale: 10});
//
// Train and run the classification.
//--------------------------------------------------
//  
// Train the RandomForest classification algorithm with the Training ROIs.
//    Use 50 trees for the algorithm.
//    Use the the pixel value populated Training ROIs to train.
//    Again, select the VV polarization of the Sentinel-1 SAR imagery.
var gerd_21_classalgo = ee.Classifier.smileRandomForest(50).train({
  features: gerd_21_training, 
  classProperty: 'landcover',
  inputProperties: gerd_21_bands});
//  
// Run the RandomForest classification algorithm.
var gerd_21_classified = s1_gerd_21.select(gerd_21_bands).classify(gerd_21_classalgo);
//
// Display the resulting classification image in GEE:
//   Blue = Water class (1)
//   White = Non-water class (2)
Map.addLayer(gerd_21_classified, {min: 1, max: 2, palette: ['blue', 'white']}, '2021 GERD Classification Image');
//
// Assess the classification results.
//--------------------------------------------------
//
// Assess the classification algorithm and training data.
//    Source: https://developers.google.com/earth-engine/guides/classification
//    Print metadata about the trained RandomForest classification algorithm.
print('2021 GERD: RandomForest Classification Results', gerd_21_classalgo.explain());
//    Create and print a confusion matrix for the Training ROIs dataset.
var gerd_21_confmat = gerd_21_classalgo.confusionMatrix();
print('2021 GERD Training ROIs Confusion Matrix', gerd_21_confmat);
//    Create and print the accuracy of the Training ROIs dataset.
print('2021 GERD Training Accuracy', gerd_21_confmat.accuracy());
// Assess the validation data.
//    Source: https://developers.google.com/earth-engine/apidocs/ee-classifier-confusionmatrix
//    Use the Validation ROIs.
//    Use the output water/non-water classification image.
//    This pulls values from the output classification image and compares them to valudation values.
var gerd_21_valid = gerd_21_classified.sampleRegions({
  collection: gerd_21_roi_val,
  properties: ['landcover'],
  scale: 10});
// Create and print an error matrix for the Validation ROIs dataset.
var gerd_21_errormat = gerd_21_valid.errorMatrix('landcover','classification');
print('2021 GERD Validation Error Matrix', gerd_21_errormat);
// Create and print the accuraacy of the Validation ROIs dataset.
print('2021 GERD Validation Overall Accuracy', gerd_21_errormat.accuracy());
//
// Export results for visualization and final paper.
//--------------------------------------------------
//
// Export 2021 GERD classification image to Google Drive.
Export.image.toDrive({
  image: gerd_21_classified,
  description: 'gerd_21_class_final',
  folder: 'image_outputs',
  crs: 'EPSG:32636',
  scale: 10,
  region: gerd_aoi
});
//
//
//--------------------------------------------------
//        2021 ROSEIRES DAM CLASSIFICATION          |
//--------------------------------------------------
// Create training and validation datasets.
//--------------------------------------------------
// Create ROIs for supervised classification training.
//    Define and collect training data manually within the GEE interface as a FeatureCollection.
//    Name the ROI set for water, 'Import as' FeatureCollection, add a Propery of 'landcover' and give it a value of 1.
//    Name the ROI set for non-water, 'Import as' FeatureCollection, add a Propery of 'landcover' and give it a value of 2.
//    Collect points for each class using the 2021 Roseires True-Color Sentinel-2 image & the 2021 GERD Sentinel-1 SAR VV polarization image as references.
// Create one FeatureCollection of the two ROI FeatureCollections defined manually.
//    Merge the water and the nonwater classes.
//    'waterR21' is the name of the manually-defined water points FeatureCollection.
//    'nonwaterR21' is the name of the manually-defined non-water points FeatureCollection.
// Import ROIs from Google Drive.
//    These will be the ROIs created manually in a separate GEE code document per above steps.
//    When prompted by GEE, convert to an Import Record for easier access/data management.
// var rose_21_rois = ee.FeatureCollection('projects/ee-ktradke/assets/rose_21_rois_FC');
// var rose_21_rois = ee.FeatureCollection('projects/ee-ktradke/assets/rose_21_rois_FC2');
var rose_21_rois = rose_21_rois2;
//
// Split the ROI FeatureCollection into Training and Validation data.
//    This enables a validation of the accuracy and precision of the classification image.
//    Add a random column to the ROI FeatureCollection to hold the split values:
var rose_21_rois = rose_21_rois.randomColumn();
//    Randomly split 70% of the FeatureCollection ROIs into a Training ROI dataset:
//      Sources vary on the recommended % split of training vs. validation.
//      Especially as RandomForest 'protects against overfitting'.
//      For more information, see Source: shorturl.at/bdipU
var rose_21_roi_train = rose_21_rois.filter(ee.Filter.lt('random',0.7));
//    Randomly split 30% of the FeatureCollection ROIs into a Validation ROI dataset:
var rose_21_roi_val = rose_21_rois.filter(ee.Filter.gte('random',0.3));
//
// Define the band(s) to be used in the supervised classification.
//     Select VV polarization of the Sentinel-1 SAR imagery.
var rose_21_bands = ['VV','VH'];
//
// Populate the Training ROIs with pixel values (SAR backscatter value, in decibels).
//   Note: 'landcover' was created when manually defining the ROIs above.
var rose_21_training = s1_roseires_21.select(rose_21_bands).sampleRegions({
  collection: rose_21_roi_train,
  properties: ['landcover'],
  scale: 30});
//
// Train and run the classification.
//--------------------------------------------------
//  
// Train the RandomForest classification algorithm with the Training ROIs.
//    Use 25 trees for the algorithm.
//    Use the the pixel value populated Training ROIs to train.
//    Again, select the VV polarization of the Sentinel-1 SAR imagery.
var rose_21_classalgo = ee.Classifier.smileRandomForest(25).train({
  features: rose_21_training, 
  classProperty: 'landcover',
  inputProperties: rose_21_bands});
// 
// Run the RandomForest classification algorithm.
var rose_21_classified = s1_roseires_21.select(rose_21_bands).classify(rose_21_classalgo);
//
// Display the resulting classification image in GEE:
//   Blue = Water class (1)
//   White = Non-water class (2)
Map.addLayer(rose_21_classified, {min: 1, max: 2, palette: ['blue', 'white']}, '2021 Roseires Dam Classification Image');
//
// Assess the classification results.
//--------------------------------------------------
//
// Assess the classification algorithm and training data.
//    Source: https://developers.google.com/earth-engine/guides/classification
//    Print metadata about the trained RandomForest classification algorithm.
print('2021 Roseires Dam RandomForest Classification Results', rose_21_classalgo.explain());
//    Create and print a confusion matrix for the Training ROIs dataset.
var rose_21_confmat = rose_21_classalgo.confusionMatrix();
print('2021 Roseires Dam Training ROIs Confusion Matrix', rose_21_confmat);
//    Create and print the accuracy of the Training ROIs dataset.
print('2021 Roseires Dam Training Accuracy', rose_21_confmat.accuracy());
// Assess the validation data.
//    Source: https://developers.google.com/earth-engine/apidocs/ee-classifier-confusionmatrix
//    Use the Validation ROIs.
//    Use the output water/non-water classification image.
//    This pulls values from the output classification image and compares them to valudation values.
var rose_21_valid = rose_21_classified.sampleRegions({
  collection: rose_21_roi_val,
  properties: ['landcover'],
  scale: 30});
// Create and print an error matrix for the Validation ROIs dataset.
var rose_21_errormat = rose_21_valid.errorMatrix('landcover','classification');
print('2021 Roseires Dam Validation Error Matrix', rose_21_errormat);
// Create and print the accuraacy of the Validation ROIs dataset.
print('2021 Roseires Dam Validation Overall Accuracy', rose_21_errormat.accuracy());
//
// Export results for visualization and final paper.
//--------------------------------------------------
//
// Export 2021 Roseires Dam classification image to Google Drive.
Export.image.toDrive({
  image: rose_21_classified,
  description: 'rose_21_class_final',
  folder: 'image_outputs',
  crs: 'EPSG:32636',
  scale: 10,
  region: roseires_aoi
});
//
//
//--------------------------------------------------
//            2022 GERD CLASSIFICATION              |
//--------------------------------------------------
//
// Create training and validation datasets.
//--------------------------------------------------
//
// Create ROIs for supervised classification training.
//    Define and collect training data manually within the GEE interface as a FeatureCollection.
//    Name the ROI set for water, 'Import as' FeatureCollection, add a Propery of 'landcover' and give it a value of 1.
//    Name the ROI set for non-water, 'Import as' FeatureCollection, add a Propery of 'landcover' and give it a value of 2.
//    Collect points for each class using the 2021 Roseires True-Color Sentinel-2 image & the 2021 GERD Sentinel-1 SAR VV polarization image as references.
//
// Create one FeatureCollection of the two ROI FeatureCollections defined manually.
//    Merge the water and the nonwater classes.
//    'waterg22' is the name of the manually-defined water points FeatureCollection.
//    'nonwaterg22' is the name of the manually-defined non-water points FeatureCollection.
//
// Import ROIs from Google Drive.
//    These will be the ROIs created manually in a separate GEE code document per above steps.
//    When prompted by GEE, convert to an Import Record for easier access/data management.
// var gerd_22_rois = ee.FeatureCollection('projects/ee-ktradke/assets/gerd_22_rois_FC');
//
// Split the ROI FeatureCollection into Training and Validation data.
//    This enables a validation of the accuracy and precision of the classification image.
//    Add a random column to the ROI FeatureCollection to hold the split values:
var gerd_22_rois = gerd_22_rois.randomColumn();
//    Randomly split 70% of the FeatureCollection ROIs into a Training ROI dataset:
//      Sources vary on the recommended % split of training vs. validation.
//      Especially as RandomForest 'protects against overfitting'.
//      For more information, see Source: shorturl.at/bdipU
var gerd_22_roi_train = gerd_22_rois.filter(ee.Filter.lt('random',0.7));
//    Randomly split 30% of the FeatureCollection ROIs into a Validation ROI dataset:
var gerd_22_roi_val = gerd_22_rois.filter(ee.Filter.gte('random',0.3));
//
// Define the band(s) to be used in the supervised classification.
//     Select VV polarization of the Sentinel-1 SAR imagery.
var gerd_22_bands = ['VV','VH'];
//
// Populate the Training ROIs with pixel values (SAR backscatter value, in decibels).
//   Note: 'landcover' was created when manually defining the ROIs above.
var gerd_22_training = s1_gerd_22.select(gerd_22_bands).sampleRegions({
  collection: gerd_22_roi_train,
  properties: ['landcover'],
  scale: 30});
//
// Train and run the classification.
//--------------------------------------------------
//  
// Train the RandomForest classification algorithm with the Training ROIs.
//    Use 200 trees for the algorithm.
//    Use the the pixel value populated Training ROIs to train.
//    Again, select the VV polarization of the Sentinel-1 SAR imagery.
var gerd_22_classalgo = ee.Classifier.smileRandomForest(200).train({
  features: gerd_22_training, 
  classProperty: 'landcover',
  inputProperties: gerd_22_bands});
//  
// Run the RandomForest classification algorithm.
var gerd_22_classified = s1_gerd_22.select(gerd_22_bands).classify(gerd_22_classalgo);
//
// Display the resulting classification image in GEE:
//   Blue = Water class (1)
//   White = Non-water class (2)
Map.addLayer(gerd_22_classified, {min: 1, max: 2, palette: ['blue', 'white']}, '2022 GERD Classification Image');
//
// Assess the classification results.
//--------------------------------------------------
//
// Assess the classification algorithm and training data.
//    Source: https://developers.google.com/earth-engine/guides/classification
//    Print metadata about the trained RandomForest classification algorithm.
print('2022 GERD RandomForest Classification Results', gerd_22_classalgo.explain());
//    Create and print a confusion matrix for the Training ROIs dataset.
var gerd_22_confmat = gerd_22_classalgo.confusionMatrix();
print('2022 GERD Training ROIs Confusion Matrix', gerd_22_confmat);
//    Create and print the accuracy of the Training ROIs dataset.
print('2022 GERD Training Accuracy', gerd_22_confmat.accuracy());
// Assess the validation data.
//    Source: https://developers.google.com/earth-engine/apidocs/ee-classifier-confusionmatrix
//    Use the Validation ROIs.
//    Use the output water/non-water classification image.
//    This pulls values from the output classification image and compares them to valudation values.
var gerd_22_valid = gerd_22_classified.sampleRegions({
  collection: gerd_22_roi_val,
  properties: ['landcover'],
  scale: 30});
// Create and print an error matrix for the Validation ROIs dataset.
var gerd_22_errormat = gerd_22_valid.errorMatrix('landcover','classification');
print('2022 GERD Validation Error Matrix', gerd_22_errormat);
// Create and print the accuraacy of the Validation ROIs dataset.
print('2022 GERD Validation Overall Accuracy', gerd_22_errormat.accuracy());
//
// Export results for visualization and final paper.
//--------------------------------------------------
//
// Export 2022 GERD classification image to Google Drive.
Export.image.toDrive({
  image: gerd_22_classified,
  description: 'gerd_22_class_final',
  folder: 'image_outputs',
  crs: 'EPSG:32636',
  scale: 10,
  region: gerd_aoi
});
//
//
//--------------------------------------------------
//          2022 ROSEIRES CLASSIFICATION            |
//--------------------------------------------------
//
// Create training and validation datasets.
//--------------------------------------------------
//
// Create ROIs for supervised classification training.
//    Define and collect training data manually within the GEE interface as a FeatureCollection.
//    Name the ROI set for water, 'Import as' FeatureCollection, add a Propery of 'landcover' and give it a value of 1.
//    Name the ROI set for non-water, 'Import as' FeatureCollection, add a Propery of 'landcover' and give it a value of 2.
//    Collect points for each class using the 2021 Roseires True-Color Sentinel-2 image & the 2021 GERD Sentinel-1 SAR VV polarization image as references.
//
// Create one FeatureCollection of the two ROI FeatureCollections defined manually.
//    Merge the water and the nonwater classes.
//    'waterR21' is the name of the manually-defined water points FeatureCollection.
//    'nonwaterR21' is the name of the manually-defined non-water points FeatureCollection.
//
// Import ROIs from Google Drive.
//    These will be the ROIs created manually in a separate GEE code document per above steps.
//    When prompted by GEE, convert to an Import Record for easier access/data management.
// var rose_22_rois = ee.FeatureCollection('projects/ee-ktradke/assets/rose_22_rois_FC');
//
// Split the ROI FeatureCollection into Training and Validation data.
//    This enables a validation of the accuracy and precision of the classification image.
//    Add a random column to the ROI FeatureCollection to hold the split values:
var rose_22_rois = rose_22_rois.randomColumn();
//    Randomly split 70% of the FeatureCollection ROIs into a Training ROI dataset:
//      Sources vary on the recommended % split of training vs. validation.
//      Especially as RandomForest 'protects against overfitting'.
//      For more information, see Source: shorturl.at/bdipU
var rose_22_roi_train = rose_22_rois.filter(ee.Filter.lt('random',0.7));
//    Randomly split 30% of the FeatureCollection ROIs into a Validation ROI dataset:
var rose_22_roi_val = rose_22_rois.filter(ee.Filter.gte('random',0.3));
//
// Define the band(s) to be used in the supervised classification.
//     Select VV polarization of the Sentinel-1 SAR imagery.
var rose_22_bands = ['VV','VH'];
//
// Populate the Training ROIs with pixel values (SAR backscatter value, in decibels).
//   Note: 'landcover' was created when manually defining the ROIs above.
var rose_22_training = s1_roseires_22.select(rose_22_bands).sampleRegions({
  collection: rose_22_roi_train,
  properties: ['landcover'],
  scale: 30});
//
// Train and run the classification.
//--------------------------------------------------
//  
// Train the RandomForest classification algorithm with the Training ROIs.
//    Use 50 trees for the algorithm.
//    Use the the pixel value populated Training ROIs to train.
//    Again, select the VV polarization of the Sentinel-1 SAR imagery.
var rose_22_classalgo = ee.Classifier.smileRandomForest(50).train({
  features: rose_22_training, 
  classProperty: 'landcover',
  inputProperties: rose_22_bands});
//  
// Run the RandomForest classification algorithm.
var rose_22_classified = s1_roseires_22.select(rose_22_bands).classify(rose_22_classalgo);
//
// Display the resulting classification image in GEE:
//   Blue = Water class (1)
//   White = Non-water class (2)
Map.addLayer(rose_22_classified, {min: 1, max: 2, palette: ['blue', 'white']}, '2022 Roseires Dam Classification Image');
//
// Assess the classification results.
//--------------------------------------------------
//
// Assess the classification algorithm and training data.
//    Source: https://developers.google.com/earth-engine/guides/classification
//    Print metadata about the trained RandomForest classification algorithm.
print('2022 Roseires Dam RandomForest Classification Results', rose_22_classalgo.explain());
//    Create and print a confusion matrix for the Training ROIs dataset.
var rose_22_confmat = rose_22_classalgo.confusionMatrix();
print('2022 Roseires Dam Training ROIs Confusion Matrix', rose_22_confmat);
//    Create and print the accuracy of the Training ROIs dataset.
print('2022 Roseires Dam Training Accuracy', rose_22_confmat.accuracy());
// Assess the validation data.
//    Source: https://developers.google.com/earth-engine/apidocs/ee-classifier-confusionmatrix
//    Use the Validation ROIs.
//    Use the output water/non-water classification image.
//    This pulls values from the output classification image and compares them to valudation values.
var rose_22_valid = rose_22_classified.sampleRegions({
  collection: rose_22_roi_val,
  properties: ['landcover'],
  scale: 30});
// Create and print an error matrix for the Validation ROIs dataset.
var rose_22_errormat = rose_22_valid.errorMatrix('landcover','classification');
print('2022 Roseires Dam Validation Error Matrix', rose_22_errormat);
// Create and print the accuraacy of the Validation ROIs dataset.
print('2022 Roseires Dam Validation Overall Accuracy', rose_22_errormat.accuracy());
//
// Export results for visualization and final paper.
//--------------------------------------------------
//
// Export 2021 Roseires Dam classification image to Google Drive.
Export.image.toDrive({
  image: rose_22_classified,
  description: 'rose_22_class_final',
  folder: 'image_outputs',
  crs: 'EPSG:32636',
  scale: 10,
  region: roseires_aoi
});
//
//
//--------------------------------------------------
//       ANALYSIS OF CHANGE BETWEEN 2021 - 2022     |
//--------------------------------------------------
//
// Calculate km2 of water area for each AOI by year.
//--------------------------------------------------
// Source 1: https://developers.google.com/earth-engine/apidocs/ee-image-pixelarea
// Source 2: https://developers.google.com/earth-engine/tutorials/tutorial_forest_03
// 2021 GERD AOI total km2 water.
//    Select the water class (value = 1) from the 2021 GERD classification image:
var gerd_21_water = gerd_21_classified.eq(1);
//    Calculate the area of each classification image pixel with water.
//      Divide by 1000*1000 (1e6) to get area in km2 vs. m2.
var gerd_21_water_area_image = gerd_21_water.multiply(ee.Image.pixelArea()).divide(1e6);
//      Sum the total water pixel area across the entire image.
var gerd_21_water_area = gerd_21_water_area_image.reduceRegion({
  reducer: ee.Reducer.sum(),
  scale: 10,
  bestEffort: true,
  crs: 'EPSG:32636'
});
//    Print the result.
print('2021 GERD total water area in km2', gerd_21_water_area);
//
//
// 2021 Roseires Dam AOI total km2 water.
//    Select the water class (value = 1) from the 2021 Roseires Dam classification image:
var rose_21_water = rose_21_classified.eq(1);
//    Calculate the area of each classification image pixel with water.
//      Divide by 1000*1000 (1e6) to get area in km2 vs. m2.
var rose_21_water_area_image = rose_21_water.multiply(ee.Image.pixelArea()).divide(1e6);
//      Sum the total water pixel area across the entire image.
var rose_21_water_area = rose_21_water_area_image.reduceRegion({
  reducer: ee.Reducer.sum(),
  scale: 10,
  bestEffort: true,
  crs: 'EPSG:32636'
});
//    Print the result.
print('2021 Roseires Dam total water area in km2', rose_21_water_area);

// 2022 GERD AOI total km2 water.
//    Select the water class (value = 1) from the 2022 GERD classification image:
var gerd_22_water = gerd_22_classified.eq(1);
//    Calculate the area of each classification image pixel with water.
//      Divide by 1000*1000 (1e6) to get area in km2 vs. m2.
var gerd_22_water_area_image = gerd_22_water.multiply(ee.Image.pixelArea()).divide(1e6);
//      Sum the total water pixel area across the entire image.
var gerd_22_water_area = gerd_22_water_area_image.reduceRegion({
  reducer: ee.Reducer.sum(),
  scale: 10,
  bestEffort: true,
  crs: 'EPSG:32636'
});
//    Print the result.
print('2022 GERD total water area in km2', gerd_22_water_area);
//
// 2022 Roseires Dam AOI total km2 water.
//    Select the water class (value = 1) from the 2022 Roseires Dam classification image:
var rose_22_water = rose_22_classified.eq(1);
//    Calculate the area of each classification image pixel with water.
//      Divide by 1000*1000 (1e6) to get area in km2 vs. m2.
var rose_22_water_area_image = rose_22_water.multiply(ee.Image.pixelArea()).divide(1e6);
//      Sum the total water pixel area across the entire image.
var rose_22_water_area = rose_22_water_area_image.reduceRegion({
  reducer: ee.Reducer.sum(),
  scale: 10,
  bestEffort: true,
  crs: 'EPSG:32636'
});
//    Print the result.
print('2022 Roseires Dam total water area in km2', rose_22_water_area);
//
print('Calculation of water class area in km2 is complete for all AOIs.');
//
print('Entirety of code is complete.');
