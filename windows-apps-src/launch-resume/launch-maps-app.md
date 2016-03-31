---
Launch the Windows Maps app
Learn how to launch the Windows Maps app from your app.
ms.assetid: E363490A-C886-4D92-9A64-52E3C24F1D98
---

# Launch the Windows Maps app


\[ Updated for UWP apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Learn how to launch the Windows Maps app from your app. This topic describes the **bingmaps:**, **ms-drive-to:**, and **ms-walk-to:** Uniform Resource Identifier (URI) schemes. Use these URI schemes to launch the Windows Maps app to specific maps, directions, and search results.

**Tip** To learn more about launching the Windows Maps app from your app, download the [Universal Windows Platform (UWP) map sample](http://go.microsoft.com/fwlink/p/?LinkId=619977) from the [Windows-universal-samples repo](http://go.microsoft.com/fwlink/p/?LinkId=619979) on GitHub.

## Introducing URIs


URI schemes let you open apps by clicking hyperlinks (or programmatically, in your app). Just as you can start a new email using **mailto:** or open a web browser using **http:**, you can open the Windows maps app using **bingmaps:**, **ms-drive-to:**, and **ms-walk-to:**.

-   The **bingmaps:** URI provides maps for locations, search results, directions, and traffic.
-   The **ms-drive-to:** URI provides turn-by-turn driving directions from your current location.
-   The **ms-walk-to:** URI provides turn-by-turn walking directions from your current location.

For example, the following URI opens the Windows Maps app and displays a map centered over New York City.

```xml
<bingmaps:?cp=40.726966~-74.006076>
```

![a map centered over new york city.](images/mapnyc.png)

Here is a description of the URI scheme:

**bingmaps://?query**

In this URI scheme, *query* is a series of parameter name/value pairs:

**&param1=value1&param2=value2 …**

For a full list of the available parameters, see the [bingmaps:](#bingmaps), [ms-drive-to:](#msdriveto), and [ms-walk-to:](#mswalkto) parameter reference. There are also examples later in this topic.

## Launch a URI from your app


To launch the Windows Maps app from your app, call the [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) method with a **bingmaps:**, **ms-drive-to:**, or **ms-walk-to:** URI. The following example launches the same URI from the previous example. For more info about launching apps via URI, see [Launch the default app for a URI](launch-default-app.md).

```cs
// Center on New York City
var uriNewYork = new Uri(@"bingmaps:?cp=40.726966~-74.006076");

// Launch the Windows Maps app
var launcherOptions = new Windows.System.LauncherOptions();
launcherOptions.TargetApplicationPackageFamilyName = "Microsoft.WindowsMaps_8wekyb3d8bbwe";
var success = await Windows.System.Launcher.LaunchUriAsync(uriNewYork, launcherOptions);
```

In this example, the [**LauncherOptions**](https://msdn.microsoft.com/library/windows/apps/hh701435) class is used to help ensure the Windows Maps app is launched.

## Display known locations


There are several ways to control the map center point and the zoom level. Using *cp* (center point) and *lvl* (zoom level) parameters are the most straightforward methods and they produce predictable results. Using *bb* parameter (specifies an area bounded by latitude and longitude values) is less predictable because it takes into account the screen resolution and determines the map center point and zoom level based on the coordinates provided. The *bb* parameter is ignored when all three parameters (*bb*, *cp*, and *lvl*) are present.

To control the type of view, use the *ss* (Streetside) and *sty* (style) and parameters. The *ss* parameter puts the map into a Streetside view. The *sty* parameter lets you switch between road, aerial, and 3D views. When using the 3D style, the *hdg*, *pit*, and *rad* parameters can be used to specify the 3D view. *hdg* specifies the heading of the view, *pit* specifies the pitch of the view, and *rad* specifies the distance from the center point to show in view. For more info about these and other parameters, see the [bingmaps: parameter reference](#bingmaps).

| Sample Uri                                                                 | Results                                                                                                                                                                                                   |
|----------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| bingmaps:                                                                  | Opens the Maps app.                                                                                                                                                                                       |
| bingmaps:?cp=40.726966~-74.006076                                          | Displays a map centered over New York City.                                                                                                                                                               |
| bingmaps:?cp=40.726966~-74.006076&lvl=10                                   | Displays a map centered over New York City with a zoom level of 10.                                                                                                                                       |
| bingmaps:?bb=39.719\_-74.52~41.71\_-73.5                                   | Displays a map of New York City with the size of the screen as the bounding box.                                                                                                                          |
| bingmaps:?bb=39.719\_-74.52~41.71\_-73.5&cp=47~-122                        | Displays a map of New York City, which is the area specified in the bounding box argument. The center point for Seattle specified in the **cp** argument is ignored.                                      |
| bingmaps:?bb=39.719\_-74.52~41.71\_-73.5&cp=47~-122&lvl=8                  | Displays a map of New York, which is the area specified in the **bb** argument. The **cp** argument, which specifies Seattle, is ignored because **cp** and **lvl** are ignored when **bb** is specified. |
| bingmaps:?collection=point.36.116584\_-115.176753\_Caesars%20Palace&lvl=16 | Displays a map with a point named Caesars Palace (in Las Vegas) and sets the zoom level to 16.                                                                                                            |
| bingmaps:?collection=point.40.726966\_-74.006076\_Some%255FBusiness        | Displays a map with a point named Some\_Business (in Las Vegas).                                                                                                                                          |
| bingmaps:?cp=40.726966~-74.006076&trfc=1&sty=a                             | Displays a map of NYC with traffic on and aerial map style.                                                                                                                                               |
| bingmaps:?cp=47.6204~-122.3491&sty=3d                                      | Displays a 3D view of the Space Needle.                                                                                                                                                                   |
| bingmaps:?cp=47.6204~-122.3491&sty=3d&rad=200&pit=75&hdg=165               | Displays a 3D view of the Space Needle with a radius of 200m, a pitch or 75 degrees, and a heading of 165 degrees.                                                                                        |
| bingmaps:?cp=47.6204~-122.3491&ss=1                                        | Displays a Streetside view of the Space Needle.                                                                                                                                                           |

 

## Display search results


We recommend when doing a business search using the *q* parameter, make the terms specific as possible and use it in conjunction with either the *cp* or the *where* parameter to specify a location. If the user has not given the Maps app permission to use their location and you do not specify a location for a business search, the search may be performed at the country level and not return meaningful results. Search results are displayed in the most appropriate map view, so unless there is a need to set the *lvl* (zoom level), we recommend to allow the Maps app to decide. For more info about these and other parameters, see the [bingmaps: parameter reference](#bingmaps).

| Sample Uri                                                    | Results                                                                                                                                         |
|---------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------|
| bingmaps:?where=1600%20Pennsylvania%20Ave,%20Washington,%20DC | Displays a map and searches for the address of the White House in Washington, D.C.                                                              |
| bingmaps:?cp=40.726966~-74.006076&lvl=10&where=New%20York     | Searches for New York near the specified center point, displays the results on a map, and sets the zoom level to 10.                            |
| bingmaps:?lvl=10&where=New%20York                             | Searches for New York and shows the result at zoom level 10.                                                                                    |
| bingmaps:?cp=40.726966~-74.006076&lvl=14.5&q=pizza            | Searches for pizza near the specified center point (that is, in New York City), displays the results on a map, and sets the zoom level to 14.5. |
| bingmaps:?q=coffee&where=Seattle                              | Searches for coffee in Seattle.                                                                                                                 |

 

## Display multiple points


Use the *collection* parameter to show a custom set of points on the map. If there is more than one point, a list of points is displayed. There can be up to 25 points in a collection and they are listed in the order provided. The collection takes precedence over search and directions requests. For more info about this parameter and others, see the [bingmaps: parameter reference](#bingmaps).

| Sample Uri                                                                                                                                                         | Results                                                                                                                   |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------|
| bingmaps:?collection=point.36.116584\_-115.176753\_Caesars%20Palace                                                                                                | Searches for Caesar's Palace in Las Vegas and displays the results on a map in the best map view.                         |
| bingmaps:?collection=point.36.116584\_-115.176753\_Caesars%20Palace&lvl=16                                                                                         | Displays a pushpin named Caesars Palace in Las Vegas and zooms to level 16.                                               |
| bingmaps:?collection=point.36.116584\_-115.176753\_Caesars%20Palace~point.36.113126\_-115.175188\_The%20Bellagio&lvl=16&cp=36.114902~-115.176669                   | Displays a pushpin named Caesars Palace and a pushpin named The Bellagio in Las Vegas and zooms to level 16.              |
| bingmaps:?collection=point.40.726966\_-74.006076\_Fake%255FBusiness%255Fwith%255FUnderscore                                                                        | Displays New York with a pushpin named Fake\_Business\_with\_Underscore.                                                  |
| bingmaps:?collection=name.Hotel%20List~point.36.116584\_-115.176753\_Caesars%20Palace~point.36.113126\_-115.175188\_The%20Bellagio&lvl=16&cp=36.114902~-115.176669 | Displays a list named Hotel List and two pushpins for Caesars Palace and The Bellagio in Las Vegas and zooms to level 16. |

 

## Display directions and traffic


You can display directions between two points using the *rtp* parameter; those points can be either an address or latitude and longitude coordinates. Use the *trfc* parameter to show traffic information. To specify the type of directions: driving, walking, or transit, use the *mode* parameter. If *mode* isn't specified, directions will be provided using the user's mode of transportation preference. For more info about these parameters and others, see the [bingmaps: parameter reference](#bingmaps).

![an example of directions](images/windowsmapgcdirections.png)

| Sample Uri                                                                                                              | Results                                                                                                                                                         |
|-------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| bingmaps:?rtp=pos.44.9160\_-110.4158~pos.45.0475\_-109.4187                                                             | Displays a map with point-to-point directions. Because *mode* is not specified, directions will be provided using the user's mode of transportation preference. |
| bingmaps:?cp=43.0332~-87.9167&trfc=1                                                                                    | Displays a map centered over Milwaukee, WI with traffic.                                                                                                        |
| bingmaps:?rtp=adr.One Microsoft Way, Redmond, WA 98052~pos.39.0731\_-108.7238                                           | Displays a map with directions from the specified address to the specified location.                                                                            |
| bingmaps:?rtp=adr.1%20Microsoft%20Way,%20Redmond,%20WA,%2098052~pos.36.1223\_-111.9495\_Grand%20Canyon%20northern%20rim | Displays directions from 1 Microsoft Way, Redmond, WA, 98052 to the Grand Canyon's northern rim.                                                                |
| bingmaps:?rtp=adr.Davenport, CA~adr.Yosemite Village                                                                    | Displays a map with driving directions from the specified location to the specified landmark.                                                                   |
| bingmaps:?rtp=adr.Mountain%20View,%20CA~adr.San%20Francisco%20International%20Airport,%20CA&mode=d                      | Displays driving directions from Mountain View, CA to San Francisco International Airport, CA.                                                                  |
| bingmaps:?rtp=adr.Mountain%20View,%20CA~adr.San%20Francisco%20International%20Airport,%20CA&mode=w                      | Displays walking directions from Mountain View, CA to San Francisco International Airport, CA.                                                                  |
| bingmaps:?rtp=adr.Mountain%20View,%20CA~adr.San%20Francisco%20International%20Airport,%20CA&mode=t                      | Displays transit directions from Mountain View, CA to San Francisco International Airport, CA.                                                                  |

 

## Display turn-by-turn directions


The **ms-drive-to:** and **ms-walk-to:** URI schemes let you launch directly into a turn-by-turn view of a route. These URI schemes can only provide directions from the user's current location. If you must provide directions between points that do not include the user's current location, use the **binmaps:** URI scheme as described in the previous section. For more info about these URI schemes, see the [ms-drive-to:](#msdriveto) and [ms-walk-to:](#mswalkto) parameter reference.

> **Important**  When the **ms-drive-to:** or **ms-walk-to:** URI schemes are launched, the Maps app will check to see if the device has ever had a GPS location fix. If it has, then the Maps app will proceed to turn-by-turn directions. If it hasn't, the app will display the route overview, as described in [Display directions and traffic](#directions).

 

![an example of turn-by-turn directions](images/windowsmapsappdirections.png)

| Sample Uri                                                                                                | Results                                                                                       |
|-----------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------|
| ms-drive-to:?destination.latitude=47.680504&destination.longitude=-122.328262&destination.name=Green Lake | Displays a map with turn-by-turn driving directions to Green Lake from your current location. |
| ms-walk-to:?destination.latitude=47.680504&destination.longitude=-122.328262&destination.name=Green Lake  | Displays a map with turn-by-turn walking directions to Green Lake from your current location. |

 

## bingmaps: parameter reference


The syntax for each parameter in this table is shown by using Augmented Backus–Naur Form (ABNF).

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Parameter</th>
<th align="left">Definition</th>
<th align="left">ABNF Definition and Example</th>
<th align="left">Details</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>**cp**</p></td>
<td align="left"><p>Center point</p></td>
<td align="left"><p>cp = "cp=" cpval</p>
<p>cpval = degreeslat "~" degreeslon</p>
<p>degreeslat = ["-"] 1*3DIGIT ["." 1*7DIGIT]</p>
<p>degreeslon = ["-"] 1*2DIGIT ["." 1*7DIGIT]</p>
<p>Example:</p>
<p>cp=40.726966~-74.006076</p></td>
<td align="left"><p>Both values must be expressed in decimal degrees and separated by a tilde(**~**).</p>
<p>Valid longitude values are between -180 and +180 inclusive.</p>
<p>Valid latitude values are between -90 and +90 inclusive.</p></td>
</tr>
<tr class="even">
<td align="left"><p>**bb**</p></td>
<td align="left"><p>Bounding box</p></td>
<td align="left"><p>bb = "bb=" southlatitude "_" westlongitude "~" northlatitude "_" eastlongitude</p>
<p>southlatitude = degreeslat</p>
<p>northlatitude = degreeslat</p>
<p>westlongitude = degreeslon</p>
<p>eastlongitude = degreeslon</p>
<p>Example:</p>
<p>bb=39.719_-74.52~41.71_-73.5</p></td>
<td align="left"><p>A rectangular area that specifies the bounding box expressed in decimal degrees, using a tilde (**~**) to separate the lower left corner from the upper right corner. Latitude and longitude for each are separated with an underscore (**_**).</p>
<p>Valid longitude values are between -180 and +180 inclusive.</p>
<p>Valid latitude values are between -90 and +90 inclusive.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>**where**</p></td>
<td align="left"><p>Location</p></td>
<td align="left"><p>where = "where=" whereval</p>
<p>whereval = 1*( ALPHA / DIGIT / "-" / "." / "_" / pct-encoded / "!" / "$" / "'" / "(" / ")" / "*" / "+" / "," / ";" / ":" / "@" / "/" / "?")</p>
<p>Example:</p>
<p>where=1600%20Pennsylvania%20Ave,%20Washington,%20DC</p></td>
<td align="left"><p>Search term which is a location, landmark or place.</p></td>
</tr>
<tr class="even">
<td align="left"><p>**q**</p></td>
<td align="left"><p>Query Term</p></td>
<td align="left"><p>q = "q=" qval</p>
<p>qval = whereval / "~"</p>
<p>Example:</p>
<p>q=mexican%20restaurants</p></td>
<td align="left"><p>Search term for local business or category of businesses.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>**lvl**</p></td>
<td align="left"><p>Zoom Level</p></td>
<td align="left"><p>lvl = "lvl=" 1*2DIGIT ["." 1*2DIGIT]</p>
<p>Example:</p>
l
<p>lvl=10.50</p></td>
<td align="left"><p>Defines the zoom level of the map view. Valid values are 1-20 where 1 is zoomed all the way out.</p></td>
</tr>
<tr class="even">
<td align="left"><p>**sty**</p></td>
<td align="left"><p>Style</p></td>
<td align="left"><p>sty = "sty=" ("a" / "r"/"3d")</p>
<p>Example:</p>
<p>sty=a</p></td>
<td align="left"><p>Defines the map style. Valid values for this parameter include:</p>
<ul>
<li>**a**: Display an aerial view of the map.</li>
<li>**r**: Display a road view of the map.</li>
<li>**3d**: Display a 3D view of the map. Use in conjunction with the **cp** parameter and optionally with the **rad** parameter.</li>
</ul>
<div class="alert">
> **Note**  Omitting the **sty** parameter produces the same results as sty=r.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>**rad**</p></td>
<td align="left"><p>Radius</p></td>
<td align="left"><p>rad = "rad=" 1*8DIGIT</p>
<p>Example:</p>
<p>rad=1000</p></td>
<td align="left"><p>A circular area that specifies the desired map view. The radius value is measured in meters.</p></td>
</tr>
<tr class="even">
<td align="left"><p>**pit**</p></td>
<td align="left"><p>Pitch</p></td>
<td align="left"><p>pit = "pit=" pitch</p>
<p>Example:</p>
<p>pit=60</p></td>
<td align="left"><p>Indicates the angle that the map is viewed at, where 90 is looking out at the horizon (maximum) and 0 is looking straight down (minimum).</p></td>
</tr>
<tr class="odd">
<td align="left"><p>**hdg**</p></td>
<td align="left"><p>Heading</p></td>
<td align="left"><p>hdg = "hdg=" heading</p>
<p>Example:</p>
<p>hdg=180</p></td>
<td align="left"><p>Indicates the direction the map is heading in degrees, where 0 or 360 = North, 90 = East, 180 = South, and 270 = West.</p></td>
</tr>
<tr class="even">
<td align="left"><p>**ss**</p></td>
<td align="left"><p>Streetside</p></td>
<td align="left"><p>ss = "ss=" BIT</p>
<p>Example:</p>
<p>ss=1</p></td>
<td align="left"><p>Indicates that street-level imagery is shown when <code>ss=1</code>. Omitting the **ss** parameter produces the same result as <code>ss=0</code>. Use in conjunction with the **cp** parameter to specify the location of the street-level view.</p>
<div class="alert">
> **Note**  Street-level imagery is not available in all regions.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>**trfc**</p></td>
<td align="left"><p>Traffic</p></td>
<td align="left"><p>trfc = "trfc=" BIT</p>
<p>Example:</p>
<p>trfc=1</p></td>
<td align="left"><p>Specifies whether traffic information is included on the map. Omitting the trfc parameter produces the same results as <code>trfc=0</code>.</p>
<div class="alert">
> **Note**  Traffic data is not available in all regions.
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><p>**rtp**</p></td>
<td align="left"><p>Route</p></td>
<td align="left"><p>rtp = "rtp=" (waypoint "~" [waypoint]) / ("~" waypoint)</p>
<p>waypoint = (("pos." cpval ["_" whereval]) / ("adr." whereval))</p>
<p>Examples:</p>
<p>rtp=adr.Mountain%20View,%20CA~adr.SFO</p>
<p>rtp=adr.One%20Microsoft%20Way,%20Redmond,%20WA~pos.45.23423_-122.1232 _My%20Picnic%20Spot</p></td>
<td align="left"><p>Defines the start and end of a route to draw on the map, separated by a tilde (**~**). Each of the waypoints is defined by either a position using latitude and longitude (**cp**) or an address identifier (**where**).</p>
<p>A complete route contains exactly two waypoints. For example, a route with two waypoints is defined by <code>rtp="A"~"B"</code>.</p>
<p>It's also acceptable to specify an incomplete route. For example, you can define only the start of a route with <code>rtp="A"~</code>. In this case, the driving directions input panel is displayed with the provided waypoint in the **From:** field and the **To:** field that has focus.</p>
<p>If only the end of a route is specified, as with <code>rtp=~"B"</code>, the driving directions panel is displayed with the provided waypoint in the **To:** field. If available, the current location is pre-populated in the **From:** field with focus.</p>
<p>No route line is drawn when an incomplete route is given.</p>
<p>Use in conjunction with the **mode** parameter to specify the mode of transportation (driving, transit, or walking). If **mode** isn't specified, directions will be provided using the user's mode of transportation preference.</p>
<div class="alert">
**Note**  A title can be used for a location if the location is specified by the **pos** parameter value. Rather than showing the latitude and longitude, the title will be displayed.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>**mode**</p></td>
<td align="left"><p>Route mode</p></td>
<td align="left"><p>mode = "mode=" ("d" / "t" / "w")</p>
<p>Example:</p>
<p>mode=d</p></td>
<td align="left"><p>Defines the route mode. Valid values for this parameter include:</p>
<ul>
<li>**d**: Displays route overview for driving directions</li>
<li>**t**: Displays route overview for transit directions</li>
<li>**w**: Displays route overview for walking directions</li>
</ul>
<p>Use in conjunction with the **rtp** parameter for route directions. If **mode** isn't specified, directions will be provided using the user's mode of transportation preference.</p></td>
</tr>
<tr class="even">
<td align="left"><p>**collection**</p></td>
<td align="left"><p>Collection</p></td>
<td align="left"><p>collection = "collection="</p>
<p>Example:</p>
<p>collection=name.Custom%20List</p></td>
<td align="left"><p>Collection of entities to be added to the map and list.</p>
<p>Supported Entities are:</p>
<ul>
<li>point</li>
</ul>
<p>Separate multiple collections editor items with tildes (**~**).</p>
<p>If the item you specify contains a tilde, make sure the tilde is encoded as <code>%7E</code>. If not accompanied by Center point and Zoom Level parameters, the collection will provide the best map view.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>**point**</p></td>
<td align="left"><p>Point</p></td>
<td align="left"><p>point = "point." pointval_title_notes_link URL_photo URL</p>
<p>pointval = degreeslat "_" degreeslon</p>
<p>title = whereval / _</p>
<p>Example:</p>
<p>collection=point.40.726966_-74.006076_Pin%20Title</p></td>
<td align="left"><p>Specifies a point to add by using latitude and longitude. For points, the value includes the latitude, longitude, title, notes, link URL, and photo URL to display, each separated by an underscore (**_**):</p>
<p>If the item you specify contains an underscore, make sure the underscore is double encoded as <code>%255F</code>.</p>
<p>Max length of the title and notes parameters is 255 chars.</p>
<p>If a point is defined without a name, the title will be "Custom pin."</p></td>
</tr>
<tr class="even">
<td align="left"><p>**name**</p></td>
<td align="left"><p>Name</p></td>
<td align="left"><p>name = "name." whereval</p>
<p>Example:</p>
<p>collection=name.Hotel%20List</p></td>
<td align="left"><p>Name of the collection. If a name is not provided and there is more than one entity in the collection, the default name is "Custom pins."</p></td>
</tr>
</tbody>
</table>

 

## ms-drive-to: parameter reference


The Uri to launch a request for turn-by-turn driving directions has the following format.

> **Note**  You don’t specify the starting point in this URI scheme. The starting point is always assumed to be the current location. If you need to specify a starting point other than the current location, see [Display directions and traffic](#directions).

 

| Parameter | Definition | Example | Details |
|------------|-----------|---------|---------|
| **destination.latitude** | Destination latitude | Example: destination.latitude=47.6451413797194 | The latitude of the destination. |
| **destination.longitude** | Destination longitude | Example: destination.longitude=-122.141964733601 | The longitude of the destination. |
| **name** | Name of the destination | Example: destination.name=Redmond, WA | Then name of the destination. You do not have to encode the Uri or the **name** value. |

 

## ms-walk-to: parameter reference


The Uri to launch a request for turn-by-turn walking directions has the following format.

> **Note**  You don’t specify the starting point in this URI scheme. The starting point is always assumed to be the current location. If you need to specify a starting point other than the current location, see [Display directions and traffic](#directions).

 

| Parameter | Definition | Example | Details |
|-----------|------------|---------|----------|
| **destination.latitude** | Destination latitude | Example: destination.latitude=47.6451413797194 | The latitude of the destination. |
| **destination.longitude** | Destination longitude | Example: destination.longitude=-122.141964733601 | The longitude of the destination. |
| **name** | Name of the destination | Example: destination.name=Redmond, WA | Then name of the destination. You do not have to encode the Uri or the **name** value. |

 

 

 



<!--HONumber=Mar16_HO1-->
