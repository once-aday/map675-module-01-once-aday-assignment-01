# map675-module-01-once-aday-assignment-01
# NBA Bubble Hotel Locations outside of Orlando


## 3D Building Data

I am utilizing [Open Street Map Buildings](https://osmbuildings.org/) to render 3D models of the hotels where NBA players are staying during their restarted 2019-20 season.

*Data collections steps*:
- Confirmed that the buildings are available by panning to their locations with OSMB map tool at [https://osmbuildings.org/documentation/viewer/].
- Used https://overpass-turbo.eu/ to download all raw data of OSMB data within bounding box of Disney World and portions of outer Orlando as GeoJSON
- Filtered down the data into three distinct GeoJSON files of each of the three hotels
- Plan to use https://github.com/kekscom/osmbuildings to render the buildings and apply different stylings

*Rendering steps*:
- Used jquery .getJSON method to read in first hotel from file, took some effort to get the json formatted and read in correctly
- the properties for the building feature that control height and color settings seemed to be missing form my export so I will need to look that information up. The visualization properties are configured within the feature object like so:

`"properties": {
      "wallColor": "rgb(255,0,0)",
      "roofColor": "rgb(255,128,0)",
      "height": 500,
      "minHeight": 0
    }`

Next:

    - Dynamic Coloring
    - Pop up information and change color on mouse hover
