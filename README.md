# RealView-Golf-Annotations
Course geometries and topo assets used by RealView Golf.

## Link to app listing
https://play.google.com/store/apps/details?id=com.googlemapsgolf.golfgamealpha&amp;hl=en_US

## Contributing
You are welcome to add new courses or suggest edits by filing a Pull Request.

### KMLs
Use Google Earth to work with the kml files. The Structure must match the following:

```
Folder: Course Name
  Folder: 1
    Description: par N
    [List of Placemarks]
  Folder: 2
  ...
  Folder: 18
  Folder: global
    [List of Placemarks]
```

There should be 19 subfolders in each course. One for each hole, named 1-18, and a folder named "global". 

#### Hole Folders
Each hole-folder should have a description that specify the par, e.g. "par 4". Each folder contains a list of Placemarks, which could be Polygons or Points. At an absolute minimum there must be Placemarks for Out-of-bounds (OB), the Teebox, the Tee, the Green, and the Cup. Additional Placemarks can be added to describe the following regions: fairway, bunker, fairway_hole, bunker_hole.

The OB polygon specifies the legal playing area for that hole. If a user's ball lands outside this boundary, it will incur an out-of-bounds penalty.  Note that any un-annotated areas are implicitly designated as rough.

The fairway_hole and bunker_hole are for removing interior sections of the respective polygon. For example, a fairway_hole will remove an interior region of the fairway (which makes it rough).


| name         | type      |   required  | maximum per hole |
| ------------ | --------- | ----------- | ---------------- |
| OB           | Polygon   | yes         | 1                |
| teebox       | Polygon   | yes         | 1                |
| tee          | Point     | yes         | 1                |
| green        | Polygon   | yes         | 1                |
| cup          | Point     | yes         | no limit         |
| fairway      | Polygon   | no          | no limit         |
| bunker       | Polygon   | no          | no limit         |
| fairway_hole | Polygon   | no          | no limit         |
| bunker_hole  | Polygon   | no          | no limit         |


#### global Folder
The global folder contains the geometries that may not necessarily associate with one particular hole in some cases. These include water, trees, asphalt, and water_hole.


| name       | type       |  required   | maximum  |
| ---------- | ---------- | ----------- | -------- |
| water      | Polygon    | no          | no limit |
| trees      | LineString | no          | no limit |
| asphalt    | Polygon    | no          | no limit |
| water_hole | Polygon    | no          | no limit |

### Topography

<b>NOTE: The easiest way to create these files is to use Christoph Hofstetter's [Countour Map Creator](https://contourmapcreator.urgr8.ch/). Enter the desired number of sampling points, click the map to set the grid boundary, click "get data", then copy & paste the samples from the  "Elevation Data" textbox.</b>  

Every course folder in the topo folder needs to contain 18 *.topo files, named 1.topo, 2.topo, ..., 18.topo.
They are plaintext files that contain a grid of elevation samples. The grid should be along the lat/lon axes and cover the extent of the green. It is not a bad idea to expand the coverage grid a few yards past the current extent of the green in case the green is modified in the future.

The first line of the topo file specifies the boundary of the coverage grid, separated with tabs.

```
minLat  minLon  maxLat  maxLon
```

After that, each row represents a latitudinal line of elevation samples. The samples do NOT need to be relative to sea level, but they must be in meters.
