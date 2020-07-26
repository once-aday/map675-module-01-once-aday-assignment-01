# map675-module-01-once-aday-assignment-01
# NBA Bubble Hotel Locations outside of Orlando


## 3D Building Data

I am utilizing [Open Street Map Buildings](https://osmbuildings.org/) to render 3D models of the hotels where NBA players are staying during their restarted 2019-20 season.

*Data collections steps*:
- Confirmed that the buildings are available by panning to their locations with OSMB map tool at [https://osmbuildings.org/documentation/viewer/].
- Used https://overpass-turbo.eu/ to download all raw data of OSMB data within bounding box of Disneyworld and portions of outter Orlando as GeoJSON
- Filtered down the data into three distinct GeoJSON files of each of the three hotels
- Plan to use https://github.com/kekscom/osmbuildings to render the buildings and apply different stylings
