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

<h2>corner-buttons branch</h2>

  I began finding additional data for the teams staying at the 3 hotels. I used css to scale them into individual columns which will serve as buttons. These buttons are a key feature of this web map and will be the primary user interaction to direct the map to pan to the different hotels.

  Next I plan to match the color of the buildings to the color of the buttons. After that I will link the buttons to trigger a map pan action to the given team's hotel. That will complete the feature and I will merge this branch into the main project.

  I realized I needed to update to the NEW Osmbuildings to allow for tilting and other map options.
  This is osmbuildings 4.1.1

  Now I can use:
  https://osmbuildings.org/documentation/viewer/api/#method-addGeoJSON

I am going to use the rotate feature as a small button.

Icon attribution html: Icons made by <a href="https://www.flaticon.com/authors/pixel-perfect" title="Pixel perfect">Pixel perfect</a> from <a href="https://www.flaticon.com/" title="Flaticon"> www.flaticon.com</a>

<h2>espn-wwos-dataset branch</h2>
*ESPN Wide World of Sports OSMBuildings GeoJSON*

I have exported a geojson of the ESPN WWoS Complex of buildings, these objects are missing the height and color attributes
that tell the OSMB API to give the buildings 3D height (in meters) and color them when rendered on the map.
I would like to read this file into command line and add the height and color attributes to the feature objects.

This would be the final part of the project, I think having the ESPN Facility on the map will give more context to the
hotels and their geographic position. How long for instance, each team has to travel to reach the basketball courts to play their games.

Some sort of an animation/guided tour would be nice when moving from hotel to hotel, but there may be limitations on the OSMB map API.

What I have noticed is that the OSMB API uses it's own MAP object, not the traditional leaflet. Leaflet apparently doesn't support animating maps/tilting perspectives on it's own. Mapbox offers this, and so does this OSMB map object. However the OSMB map seems to be more primitive that mapbox's version.

Reducing the large dataset exported from https://overpass-turbo.eu/ (This was the dataset of ALL of outter Orlando including Disneyworld and more)


**Creating a GeoJSON of just the ESPN Wide World of Sports Complex:**

Using ogrinfo and ogr2ogr

Get ogrinfo on large raw export of buildings:

`ogrinfo DisneyWorldandSurrounding.geojson`

 `INFO: Open of 'DisneyWorldandSurrounding.geojson'
      using driver 'GeoJSON' successful.
1: DisneyWorldandSurrounding`

*Get the boundingbox xmin ymin xmax ymax.*

 I went to google maps and created pins in the bottom left and top right around the ESPN complex in order to grab the x,y values needed for a bounding box.

`ogr2ogr -spat -81.562182 28.333004 -81.550569 28.341744 -f GeoJSON espn_wide_world_sports_clip.geojson DisneyWorldandSurrounding.geojson`


Check ogrinfo for new GeoJSON file that was clipped to the ESPN WWoS bounding box.
'ogrinfo espn_wide_world_sports_clip.geojson'

`INFO: Open of `espn_wide_world_sports_clip.geojson'
      using driver `GeoJSON' successful.
1: DisneyWorldandSurrounding`

**Get Info WITH layer name - I forgot you need to specify the layer name within the layer to get the full details..**

`ogrinfo espn_wide_world_sports.geojson espn_wide_world_sports`

Selection of Return:
```covered (String) = (null)
  intermittent (String) = (null)
  bicycle (String) = (null)
  foot (String) = (null)
  horse (String) = (null)

  motor_vehicle (String) = (null)
  enterance (String) = (null)
  POLYGON ((-81.5569006 28.3344511,-81.5569006 28.334115,-81.5566731 28.334115,-81.5566731 28.3344511,-81.5569006 28.3344511))
```

`ogrinfo -so espn_wide_world_sports.geojson espn_wide_world_sports`

```INFO: Open of 'espn_wide_world_sports.geojson'
      using driver 'GeoJSON' successful.

Layer name: espn_wide_world_sports
Geometry: Polygon
Feature Count: 41

Extent: (-81.560919, 28.333377) - (-81.553140, 28.340443)
Layer SRS WKT:
GEOGCRS["WGS 84",
    DATUM["World Geodetic System 1984",
        ELLIPSOID["WGS 84",6378137,298.257223563,
            LENGTHUNIT["metre",1]]],
    PRIMEM["Greenwich",0,
        ANGLEUNIT["degree",0.0174532925199433]],
    CS[ellipsoidal,2],
        AXIS["geodetic latitude (Lat)",north,
            ORDER[1],
            ANGLEUNIT["degree",0.0174532925199433]],
        AXIS["geodetic longitude (Lon)",east,
            ORDER[2],
            ANGLEUNIT["degree",0.0174532925199433]],
    ID["EPSG",4326]]
Data axis to CRS axis mapping: 2,1
id: String (0.0)
@id: String (0.0)
building: String (0.0)
name: String (0.0)
type: String (0.0)
addr:state: String (0.0)
sport: String (0.0)
note: String (0.0)```


**Use ogrinfo and SQL to Insert a new Column, wallColor, roofColor**

* https://gis.stackexchange.com/questions/59705/gdal-sql-syntax-to-add-field-an-put-values

* https://gdal.org/user/sql_sqlite_dialect.html


`ogrinfo [ds_name] -sql "ALTER TABLE [layer_name] ADD [new_column_name] [new_column_type]"`

`ogrinfo -sql "alter table espn_wide_world_sports add wallColor text" espn_wide_world_sports.geojson`

**convert geojson to shapefile**

`ogr2ogr -f "ESRI Shapefile" espn_wide_world_sports.shp espn_wide_world_sports.geojson`

The above commands weren't actually adding the new column, now I am trying the same command with a shapefile version

`ogrinfo -sql "alter table espn_wide_world_sports add wallColor text" espn_wide_world_sports.shp`

**It did work as SHAPEFILE (instead of GeoJSON):**

`ogrinfo -sql "alter table espn_wide_world_sports add wallColor text" espn_wide_world_sports.shp`


`'ogrinfo -so espn_wide_world_sports.shp espn_wide_world_sports'`


```bicycle: String (254.0)
foot: String (254.0)
horse: String (254.0)

motor_vehi: String (254.0)
enterance: String (254.0)
wallColor: String (80.0)```


**Add roofColor**

`ogrinfo -sql "alter table espn_wide_world_sports add roofColor text" espn_wide_world_sports.shp`

`ogrinfo -so espn_wide_world_sports.shp espn_wide_world_sports`

```horse: String (254.0)
motor_vehi: String (254.0)

enterance: String (254.0)
wallColor: String (80.0)
roofColor: String (80.0)```

**Use ogrinfo and SQL to update Height property to default to 25m**

`ogrinfo espn_wide_world_sports.shp -dialect SQLite -sql "UPDATE espn_wide_world_sports SET height = 25"`

**Set wallColor and roofColor to rgb(240, 90, 108)**

`ogrinfo espn_wide_world_sports.shp -dialect SQLite -sql "UPDATE espn_wide_world_sports SET wallColor = 'rgb(240, 90, 108)'"`


`ogrinfo espn_wide_world_sports.shp -dialect SQLite -sql "UPDATE espn_wide_world_sports SET roofColor = 'rgb(240, 90, 108)'"`



**Convert Shapefile to GeoJSON**


`ogr2ogr -f "GeoJSON" espn_wide_world_sports_val_updates.geojson espn_wide_world_sports.shp`


Now the data prep process is complete for the ESPN Complex Building dataset.

If nothing went wrong and all the new values are accurate I can add this geojson file as a fourth dataset to my webmap..
