Style quickstart
================

Mapbox Studio uses a language called CartoCSS to determine the look of a map. Colors, sizes, and shapes can all be manipulated by applying their specific CartoCSS parameters in the stylesheet panel to the right of the map. Read the [CartoCSS manual](https://www.mapbox.com/carto/) for a more detailed introduction to the language.

In this tutorial we’ll create a custom style by writing CartoCSS for buildings, roads, and parks.

Create a style project
----------------------

Create a new style by clicking on __Projects__ in the lower left, and click __New style__. Select the __Basic__ style project. The basic project includes basic styling for for roads, city names, admin borders, rivers, and so on.

![Create style](https://cloud.githubusercontent.com/assets/83384/3870270/d16352ac-20c5-11e4-9728-072b14213f79.png)

Inspecting the vector source
----------------------------

Center the map preview around New York City and increase the zoom level of the map to z17. Click the <span class='icon inspect'></span> icon on the map preview pane. Although the map currently looks empty, there are many features available for styling in this area.

Mapbox Studio includes a vector source inspector that will show you all layers and features for the vector source. Like the DOM inspector in web browsers, the vector source inspector shows you details about the fields and features in vector tiles that can be styled. Click a __building polygon__ on the map to see its properties.

![Inspector](https://cloud.githubusercontent.com/assets/3952537/4005631/de34440e-2995-11e4-8907-33905a36a73b.png)


Toggle the <span class='icon inspect'></span> icon again to return to the normal map preview.

Add a stylesheet tab
--------------------

Click on the __+__ button on the top right of the style editor to add a new tab. Name your tab _custom_.

![Custom](https://cloud.githubusercontent.com/assets/83384/3870301/4cd8b908-20c7-11e4-8fea-b12665003556.png)

Styling buildings
-----------------

Add the following CartoCSS to your _custom_ stylesheet and then click __Save__.

    #building[zoom>=14] {
      polygon-fill:#eee;
      line-width:0.5;
      line-color:#ddd;
    }

![Styling buildings](https://cloud.githubusercontent.com/assets/83384/3870305/ba0d0a6a-20c7-11e4-9454-a751319ca7e2.png)

- `#building` selects the building layer as the one that will be styled.
- `[zoom>=14]` restricts the styles to zoom level 14 or greater.
- `polygon-fill: #eee` fills the building polygons with a light grey color.

To add depth to our buildings at higher zoom levels let's add another set of rules that use the `building` symbolizer to render polygons as block-like shapes. Add the following CartoCSS to your _custom_ stylesheet and then click __Save__.

    #building[zoom>=16] {
      building-fill:#eee;
      building-fill-opacity:0.9;
      building-height:4;
    }

![Building symbolizer](https://cloud.githubusercontent.com/assets/83384/3870329/bceff796-20c8-11e4-8ff2-23bf7b374bff.png)

Styling parks
-------------

Add the following CartoCSS to your _custom_ stylesheet and then click __Save__.

    #landuse[class='park'] {
      polygon-fill:#dec;
    }

    #poi_label[maki='park'][scalerank<=3][zoom>=15] {
      text-name:@name;
      text-face-name:@sans;
      text-size:10;
      text-wrap-width: 60;
      text-fill:#686;
      text-halo-fill:#fff;
      text-halo-radius:1;
      text-halo-rasterizer:fast;
    }

![Styling parks](https://cloud.githubusercontent.com/assets/83384/3870363/c7b51674-20c9-11e4-8393-9da2f75b5d67.png)

- `#landuse` selects features from the landuse layer.
- `[class='park']` restricts the landuse layer to features where the `class` attribute is `park`.
- `#poi_label` selects the poi_label layer for labelling parks.
- `[maki='park'][scalerank<=3][zoom>=15]` restricts the poi_label layer to prominent park labels and restricts their visibility to zoom level 15 or greater.
- `text-name: @name` sets the field that label contents will use for their text. It references the existing `@name` variable defined in the `style` tab.
- `text-face-name: @sans` sets the font to use for displaying labels. It references the existing `@sans` variable defined in the `style` tab.
- `text-wrap-width: 60` sets a maximum width for a single line of text.
- `text-halo-rasterizer: fast` uses an alternative optimized algorithm for drawing halos around text that improves rendering speed.

Labelling roads
---------------

Add the following CartoCSS to your _custom_ stylesheet and then click __Save__.

    #road_label[zoom>=13] {
      text-name:@name;
      text-face-name:@sans;
      text-size:10;
      text-placement:line;
      text-avoid-edges:true;
      text-fill:#68a;
      text-halo-fill:#fff;
      text-halo-radius:1;
      text-halo-rasterizer:fast;
    }

![Labelling roads](https://cloud.githubusercontent.com/assets/83384/3870380/23717e70-20cb-11e4-99f5-68a80914a0ce.png)

- `#road_label` selects features from the road_label layer.
- `[zoom>=13]` restricts the road_label layer to zoom level 13 or greater.
- `text-placement: line` sets labels to follow the orientation of lines rather than horizontally.
- `text-avoid-edges: true` forces labels to be placed away from tile edges to avoid being clipped.

Add custom fonts
----------------

Download fonts [Junction](https://www.theleagueofmoveabletype.com/junction) and [Chunk](https://www.theleagueofmoveabletype.com/chunk) from open-source type collective, [League of Moveable Type](https://github.com/theleagueof). Create a new *fonts* folder in your `.tm2` style folder and copy the `.otf` files from both fonts there. 

![new fonts folder](https://cloud.githubusercontent.com/assets/4587826/6070586/d5bccdec-ad5b-11e4-9c21-77db8c320e8f.png)

_Note: .woff, .ttf, and .otf are all acceptable font formats in Mapbox Studio, however only use one format to reduce file size of your map on upload._

Set the font directory reference in the Map element in your `style.mss` file:

	font-directory: url("fonts/");

You will now see your custom fonts listed in Studio. You can use your fonts by name anywhere in your style and they will be packaged with the style when uploaded to Mapbox or exported as a `.tm2z`.

![fonts tab updated](https://cloud.githubusercontent.com/assets/4587826/6064078/a2565f4a-ad29-11e4-8836-5c8526efc467.png)

Change font variables in your `style.mss` file from Source Sans Pro to ChunkFive Regular, Junction Light, and Junction Bold. 

	// Fonts //
	@sans: 'ChunkFive Regular';
	@sans_italic: 'Junction Light';
	@sans_bold: 'Junction Bold';

**Save** and admire your new font!

![new map with custom fonts](https://cloud.githubusercontent.com/assets/4587826/6064219/89f30434-ad2a-11e4-872e-6be9582cfebe.png)

UTFGrid interactivity
---------------------

Let's add interactivity to the POI labels in our map. First, quit and close Mapbox Studio and open the `project.yml` file from your `.tm2` style folder in a text editor. Remove the single quotes (`' '`) to the right of `interactivity_layer` and replace like so:

	interactivity_layer: poi_label

Now remove the single quotes (`' '`) to the right of `template` field and replace with the following [html/mustache template](https://github.com/mapbox/utfgrid-spec/blob/master/1.3/interaction.md#template) code:

![Template code](https://cloud.githubusercontent.com/assets/4587826/6071384/49926afe-ad63-11e4-8628-0ada1fd85d38.png)

Restart Studio and hover over a park location to see your layer in action. 

![UTFGrid layer](https://cloud.githubusercontent.com/assets/4587826/6071164/e7200b58-ad60-11e4-933e-f97bf06e2fdb.png)

_Note: After making edits to the `project.yml` file in a text editor you should quit and restart Mapbox Studio to see your changes. Studio loads up your project into memory and currently does not detect changes from other text editors._


Uploading
---------

Upload your project by click on the __Settings__ button, then __Upload to Mapbox__. Publishing custom styles requires a [Mapbox Standard plan](https://www.mapbox.com/plans/). You may be prompted if you aren't yet on one.

![Upload style](https://cloud.githubusercontent.com/assets/4587826/6052186/ce39d394-ac9d-11e4-80cc-5042f6a3cb87.png)


Mission complete
----------------

Your map style is now deployed to Mapbox and has a __Map ID__. You can use this map with any of the Mapbox Developer APIs to integrate into your apps and sites.

<div class='clearfix'>
    <a class='button rcon next margin3 col6' href='https://www.mapbox.com/developers/'>Developer docs</a>
</div>

