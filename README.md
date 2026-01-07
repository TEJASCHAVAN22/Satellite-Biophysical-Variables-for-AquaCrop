# Satellite Biophysical Variables for AquaCrop 🌾🛰️

✨ Attractive, ready-to-run Google Earth Engine (GEE) script that computes a multi-source AquaCrop assimilation index for Silage Maize. Combines Sentinel-2, MODIS and CHIRPS to produce NDVI, Canopy Cover, LAI, ET, Rainfall and a final AquaCrop Index — visualized and ready for zonal statistics or export.

---

## 🚀 Quick Overview
- Input: Sentinel-2 (NDVI, Canopy Cover), MODIS (LAI, ET), CHIRPS (Rainfall)  
- Output: Per-pixel AquaCrop assimilation index (0–1) and component layers  
- Use: Paste into the GEE Code Editor and run — change AOI and dates as needed.

---

## 🧭 Features
- ✅ NDVI from Sentinel-2 (cloud filter)  
- ✅ Canopy Cover (derived from NDVI)  
- ✅ LAI from MODIS (MOD15A2H)  
- ✅ Cumulative ET from MODIS (MOD16A2)  
- ✅ Cumulative Rainfall from CHIRPS  
- ✅ Normalization to 0–1 and weighted AquaCrop index  
- ✅ Map layers and a UI legend for easy interpretation  
- ✅ Zonal mean calculation for AquaCrop inputs

---

## 🔧 How to run (GEE Code Editor)
1. Open the Google Earth Engine Code Editor: https://code.earthengine.google.com/  
2. Create a new script and paste the provided script.  
3. Edit these variables at the top:
   - `var aoi = ee.FeatureCollection('FAO/GAUL/2015/level2').filter(ee.Filter.eq('ADM2_NAME', 'Sangli'));` ← change AOI (or use your own geometry)
   - `var startDate = '2023-06-01';`
   - `var endDate   = '2023-10-31';`
4. Run the script. Map layers and the legend will appear.  
5. Check the Console for printed outputs (FinalStack, AquaCrop Index mean, etc.).

---

## 🧮 AquaCrop Index — Formula & Weights
AquaCrop Index = 0–1 normalized weighted sum:
- NDVI: 25% 🟢  
- Canopy Cover (CC): 20% 🌿  
- LAI: 25% 🌱  
- ET: 15% 💧  
- Rainfall: 15% ☔

In code:
```js
var AquaCropIndex = NDVI_n.multiply(0.25)
  .add(CC_n.multiply(0.20))
  .add(LAI_n.multiply(0.25))
  .add(ET_n.multiply(0.15))
  .add(R_n.multiply(0.15))
  .rename('AquaCrop_Index')
  .clip(aoi);
```

---

## 🎨 Visualization
Default palettes used in the script:
- NDVI: ['brown','yellow','green']  
- CC: ['white','green']  
- LAI: ['yellow','darkgreen']  
- ET: ['white','blue']  
- Rainfall: ['white','cyan','blue']  

Final composite palette (AquaCrop Index):
- ['#8c510a', '#d8b365', '#f6e8c3', '#c7eae5', '#5ab4ac', '#01665e']
  - Very Low → Very High (brown → green)

Legend UI is created and added to the map by the script (bottom-left).

---

## ⬇️ Export examples
Copy these examples into the GEE script (after the final stack / index are created) to export to Drive or Assets.

Export AquaCrop Index to Drive (GeoTIFF):
```js
Export.image.toDrive({
  image: AquaCropIndex,
  description: 'AquaCropIndex_Sangli_2023',
  folder: 'GEE-Exports',
  fileNamePrefix: 'AquaCropIndex_Sangli_2023',
  region: aoi.geometry(),
  scale: 500,
  maxPixels: 1e13,
  crs: 'EPSG:4326'
});
```

Export FinalStack (multi-band) to Drive:
```js
Export.image.toDrive({
  image: FinalStack,
  description: 'FinalStack_Sangli_2023',
  folder: 'GEE-Exports',
  fileNamePrefix: 'FinalStack_Sangli_2023',
  region: aoi.geometry(),
  scale: 500,
  maxPixels: 1e13,
  crs: 'EPSG:4326'
});
```

Export to an asset (if you prefer Assets):
```js
Export.image.toAsset({
  image: AquaCropIndex,
  description: 'AquaCropIndex_Sangli_2023_asset',
  assetId: 'users/your_username/AquaCropIndex_Sangli_2023',
  region: aoi.geometry(),
  scale: 500,
  maxPixels: 1e13
});
```

---

## 📈 Output & Use Cases
- Input for AquaCrop model (zonal mean available from script)  
- Field monitoring and seasonal crop vigor maps  
- Yield forecasting support and drought stress detection  
- Decision support for irrigation & management

---

## 🛠️ Tips & Improvements
- Adjust cloud filter threshold if you need more images (e.g., 30%). ☁️  
- Refine AOI to a farm polygon for field-scale outputs. 🧭  
- Temporal aggregation: change `mean()` / `sum()` logic for different windows. ⏱️  
- Try alternative weights to reflect local expert knowledge. ⚖️

---

## 🧾 Attribution & Data Sources
- Sentinel-2 L2A: COPERNICUS/S2_SR  
- MODIS LAI: MODIS/061/MOD15A2H  
- MODIS ET: MODIS/061/MOD16A2  
- CHIRPS Daily Precipitation: UCSB-CHG/CHIRPS/DAILY

---

## 📝 License
MIT License — feel free to reuse and adapt. 🙌

---
## 🏆 Author

Tejas Chavan  
* GIS Expert at Government Of Maharashtra Revenue & Forest Department  
* tejaskchavan22@gmail.com  
* +91 7028338510  

