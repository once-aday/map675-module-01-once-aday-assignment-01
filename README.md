# NBA Bubble Hotel Locations outside of Orlando
map675-module-01-once-aday-assignment-01

## 3D Building Data

I am utilizing [Open Street Map Buildings](https://osmbuildings.org/) to render 3D models of the hotels where NBA players are staying during their restarted 2019-20 season.

*Data collections steps*:
- Confirmed that the buildings are available by panning to their locations with OSMB map tool at [https://osmbuildings.org/documentation/viewer/].
- Used https://overpass-turbo.eu/ to download all raw data of OSMB data within bounding box of Disney World and portions of outer Orlando as GeoJSON
- Filtered down the data into three distinct GeoJSON files of each of the three hotels
- Plan to use https://github.com/kekscom/osmbuildings to render the buildings and apply different stylings

*Rendering steps*:
- Used jquery .getJSON method to read in first hotel from file, took some effort to get the json formatted and read in correctly
- the properties for the building feature that control height and color settings seemed to be missing from my original export so I will need to look that information up. The visualization properties are configured within the feature object like so:

`"properties": {
      "wallColor": "rgb(255,0,0)",
      "roofColor": "rgb(255,128,0)",
      "height": 500,
      "minHeight": 0
    }`

Next:

    - Dynamic Coloring
    - Pop up information and change color on mouse hover

*corner-buttons branch*

  I began finding additional data for the teams staying at the 3 hotels. I used css to scale them into individual columns which will serve as buttons. These buttons are a key feature of this web map and will be the primary user interaction to direct the map to pan to the different hotels.

  Next I plan to match the color of the buildings to the color of the buttons. After that I will link the buttons to trigger a map pan action to the given team's hotel. That will complete the feature and I will merge this branch into the main project.

  I realized I needed to update to the NEW Osmbuildings to allow for tilting and other map options.
  This is osmbuildings 4.1.1

  Now I can use:
  https://osmbuildings.org/documentation/viewer/api/#method-addGeoJSON
