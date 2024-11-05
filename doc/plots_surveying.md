In order to generate a path for the robot, the survey of the agricultural plots is available in
different file formats in the `data` directory of the `hackathon_bringup` package:

* `field_surveying.json`
* `field_surveying.geojson`
* `field_surveying.csv`

One of this file should be automatically loaded in your program to compute the path.
They contain lists of points which, if taken in pairs, correspond to the rows of crops to be
treated with the robot implement.
However, the line of crops are not straight, so you cannot use it to generate the exacy trajectory
to follow.


## Description of the JSON file

Here is an example of a survey file:
```json
{
  "origin": [ 46.339159, 3.433923 ],
  "fields": {
    "mixed_field": [
      [ 46.340248, 3.435536, 124.242, 121.087 ],
      [ 46.340156, 3.435170, 96.017, 110.921 ],
      [ 46.340131, 3.435183, 97.033, 108.099 ],
      [ 46.340222, 3.435549, 125.258, 118.265 ],
      [ 46.340197, 3.435563, 126.274, 115.442 ],
      [ 46.340106, 3.435196, 98.050, 105.277 ]
    ],
    "sloping_field": [
      [ 46.340913, 3.433783, -10.755, 195.028 ],
      [ 46.341092, 3.434376, 34.940, 214.967 ]
    ]
  }
}
```

Description of the fields of this file:
* __`origin`__: the `[latitude, longitude]` (in WGS84) of the (0,0) point of the world in the local
East-North-Up (ENU) coordinates system used by the gazebo simulator and the localization node (its
frame ID is `map`)
* __`fields`__: the list of fields that the robot have to cover. Each field is an object where the
  key is name of the field and the value is a list of points. Each point is 4D element corresponding
  to `[latitude, longitude, x, y]` (in degrees and meters). If taken in pairs, the points correspond
  to the crops row to treat.


## Description of the GeoJSON file

The GeoJSON file contains a set of feature of type _MultiLineString_.
Each feature corresponds to a field and each segment of the _MultiLineString_ corresponds to a row
of crops to follow.
The feature also contains properties to store the name of the field and the coordinates of the
points in the (x,y) coordinates of the world.
Here is an example of feature:
```json
{
  "type": "Feature",
  "geometry": {
    "type": "MultiLineString",
    "coordinates": [
      [ [ 3.433783, 46.340 ], [ 3.434376, 46.341 ] ]
    ]
  },
  "properties": {
    "field_name": "sloping_field",
    "xy": [
      [ [ -10.755, 195.028 ], [ 34.940, 214.967 ] ]
    ]
  }
}
```

## Description of the CSV file

The CSV file contains the points of the row of crops to follow.
A point corresponds to the following columns:
* __`field_name`__: the name of the field
* __`latitude`__: latitude coordinate (WGS84)
* __`longitude`__: longitude coordinate (WGS84)
* __`x`__: _x_ coordinate in the simulator (frame ID: `map`)
* __`y`__: _z_ coordinate in the simulator (frame ID: `map`)
