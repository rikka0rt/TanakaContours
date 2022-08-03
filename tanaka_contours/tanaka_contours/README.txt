
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ TanakaContours ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The Plugin is designed to automatically generate illuminated contours from a digital elevation model.

The aim is to automate the cumbersome process, therefore some extra options were implemented to further 
increase the representation of the Tanaka Contours.



------------------------------------------ Instruction -------------------------------------------------
******************************************************
1. Generate Contours
******************************************************

Please provide a proper DEM !


Further the Plugin needs a value for the interval of the contours, so please choose one. Wisely!
If you are in need of an offset, you have to add a value in the 'tanaka_contours.py' file. 


Used function here:	gdal_contour
'The gdal_contour generates a vector contour file from the input raster elevation model (DEM).
The contour line-strings are oriented consistently and the high side will be on the right, 
i.e. a line string goes clockwise around a top.'

******************************************************
2. Delete values by value of length (optional)
******************************************************

If you want every single contour, please click the checkBox in the according section.

All values that are under the entered threshold going to be discarded and the programm passes
a vector layer containing all geometries with the requested length.



Used function here:	extract by attribute
'This algorithm creates a new vector layer that only contains matching features from an input layer.
The criteria for adding features to the resulting layer is defined based on the values of an 
attribute from the input layer.'

******************************************************
3. Smoothing of contours (optional)
******************************************************
In the case you want to smooth your lines, just provide three values for the corresponding fields.

Iteration:
The iterations parameter dictates how many smoothing iterations will be applied to each geometry.
A higher number of iterations results in smoother geometries with the cost of greater number of 
nodes in the geometries.

Offset:
The offset parameter controls how "tightly" the smoothed geometries follow the original geometries.
Smaller values results in a tighter fit, and larger values will create a looser fit.

Maximum angle:
The maximum angle parameter can be used to prevent smoothing of nodes with large angles.
Any node where the angle of the segments to either side is larger than this will not be smoothed.
For example, setting the maximum angle to 90 degrees or lower would preserve right angles in the geometry.


Used function here:	smooth
'This algorithm smooths the geometries in a line or polygon layer. 
It creates a new layer with the same features as the ones in the input layer, 
but with geometries containing a higher number of vertices and corners in the geometries smoothed out.'

******************************************************
4. Styling contours (partially optional)
******************************************************
The Plugin will always generate Tanaka Contours after a sucessful run. Every contour will be seperated on
the nodes, the chosen QGIS Function is: explode

'This algorithm takes a lines layer and creates a new one in which each line is replaced by a set of 
lines representing the segments in the original line. Each line in the resulting layer contains only 
a start and an end point, with no intermediate nodes between them.'

The detached lines getting a new attribute: 'azimuth' via the Field Calculator and the follwing expression:

--- degrees(azimuth(start_point($geometry),end_point($geometry))) ---


All the lines getting a specific grey scale depending on their 'azimuth' value. The maximum value herefore
is the northwest and minimum is in the southeast. To achieve this the follwing formaula was used:

--- color_hsl( 0,0,
  scale_linear( abs(
    ( CASE WHEN "azimuth"-45 < 0
      THEN "azimuth"-45+360
      ELSE "azimuth"-45
    END )
  -180), 0, 180, 0, 100)
  ) ---

If you want to scale the width of the Tanaka contours, just click the necessary checkBox. After the process
you will receive a lines from a width of 0.2 to 1.0mm. To change the width, you need to change the value 
in the code. Following formula was used:

--- scale_linear(
abs( abs(
  ( CASE WHEN "azimuth"-45 < 0
    THEN "azimuth"-45+360
    ELSE "azimuth"-45
  END )
-180) -90),
0, 90, 0.2, 1) ---


The last option is to change the appearance of the input DEM, therfore just click the checkBox and choose 
any colorramp you prefer. The programm will change the 
interpolation - linear and the
classification - equal interval.
Moreover it will prepare some classes for the colors depending on the chosen interval and the maximum and 
minimum value of your DEM.

e.g.   min 5.4, max 308.7, interval 50  -> number of classes: 7 
	(0-50, 50-100, 100-150, 150-200, 200-250, 250-300, 300-350)




-----------------------------
	About
-----------------------------
I created this Plugin as part of my bachelor thesis with little to no precognition. You might find some bugs
and face incorrect behaviour of the program. I am sorry, if you do so. Please do not hesitate and send me a 
report, if you face such a problem.
It would be my pleasure to implement more functions in the future, so if you have something in mind, 
please contact me.


Richard Moritz
richi.moritz.rm@gmail.com