```python

!pip install geemap

```

```python

import ee

# Initialize Earth Engine after OAuth2 authentication
ee.Initialize(project='lyra-teledetection')

# Coordonnées du site Haute-Loire (décimal)
point = ee.Geometry.Point([4.0966666, 45.0430555])

# ROI for NDVI calculation : très local (10 m)
ndvi_roi = point.buffer(10)

# ROI for NDVI display : plus grand, par exemple 1000 m pour un carré plus lisible
ndvi_display_roi = point.buffer(1000).bounds()  # bounds() for square

# ROI for LST calculation : plus large (5 km)
lst_roi = point.buffer(5000)

# NDVI Sentinel-2 (15 mai – 15 juin 2025)
s2 = ee.ImageCollection('COPERNICUS/S2_SR_HARMONIZED') \    .filterDate('2025-05-15', '2025-06-15') \    .filterBounds(point) \    .sort('CLOUDY_PIXEL_PERCENTAGE') \    .first()

ndvi = s2.normalizedDifference(['B8', 'B4']).rename('NDVI')

# Clip NDVI for display (évite un grand carré par défaut)
ndvi_clipped = ndvi.clip(ndvi_display_roi)

mean_ndvi = ndvi.reduceRegion(
    reducer=ee.Reducer.mean(),
    geometry=ndvi_roi,
    scale=10,
    maxPixels=1e9
)

# LST MODIS (15 mai – 15 juin 2025)
lst_collection = ee.ImageCollection('MODIS/061/MOD11A1') \    .filterDate('2025-05-15', '2025-06-15') \    .filterBounds(lst_roi)

lst_image = lst_collection.mean().select('LST_Day_1km').multiply(0.02).subtract(273.15).rename('LST_Celsius')

mean_lst = lst_image.reduceRegion(
    reducer=ee.Reducer.mean(),
    geometry=lst_roi,
    scale=1000,
    maxPixels=1e9
)

# Clean merge, NDVI to 2 decimals, LST to 1 decimal
combined = {
    'NDVI': round(mean_ndvi.getInfo().get('NDVI'), 2),
    'LST_Celsius': round(mean_lst.getInfo().get('LST_Celsius'), 1)
}

print('Combined NDVI + LST (rounded) :', combined)

```

```python

import geemap.foliumap as geemap

# Create Folium map centered on Haute-Loire site
Map = geemap.Map(center=[45.0430555, 4.0966666], zoom=12)

Map.add_basemap('HYBRID')

# NDVI VISUALIZATION — clipped version for tighter square
ndvi_vis = ndvi_clipped.visualize(min=0, max=1, palette=['white', 'green'])

# Add clipped NDVI, point, and buffers
Map.addLayer(ndvi_vis, {}, 'NDVI (clipped)')
Map.addLayer(point, {'color': 'red'}, 'Site Haute-Loire')
Map.addLayer(ndvi_display_roi, {'color': 'green'}, 'NDVI ROI display (1 km square)')
Map.addLayer(lst_roi, {'color': 'blue'}, 'Buffer LST (5 km)')

Map

```