# Dissertation
##### special thanks to Paul Bates for their help on this project so far:

Investigating the impact of resolution, particularly at building-resolving levels, on modelled inundation levels in the urban area of Carlisle, UK.

My dissertation will include the comparison of two elevation models: a bare-earth, and a surface model.


### Step 1: 
Find the terrain files for the area of choice: Carlisle.
This was accomplished by gaining access to digimap's 1m lidar composite Digital Terrain Model (DTM) and Digital Surface Model (DSM) grid.
This provides 1x1km (1000x1000 cells) grids of Carlisle.

### Step 2:
Merge the files for each digital model together to create two large grids.
This was accomplished by using the raster and rgdal libraries on Rstudio, an open-source coding language.
The code is as follows, I suggest creating a .Rmd (R Markdown) file to copy this into:

``` {r}
install.packages("rgdal")
install.packages("raster")
library(rgdal)  
library(raster) 
 
 # The list of file names derived from the Digimap download:  
 #note that this will merge all files in your working directory with the .asc pattern. Be wary of merging files not intended for the DTM!
 f <- list.files(pattern = ".asc")  
 
 # turn these into a list of RasterLayer objects  
 r <- lapply(f, raster) 

 # as you have the arguments as a list call 'merge' with 'do.call'  
 x <- do.call("merge",r) 

 #output a raster grid   
 writeRaster(x,"<output_file_name>.asc")
 ```
 
 
 ### Step 3:
Clip the Digital Models to the original model's boundaries.
This was accomplished on QGIS, an open-source geographic information systems application. 

Firstly, load in your original model grid and your merged DTM grid from step 2.
Secondly, via. the 'Raster' tab at the top of the page, navigate to the 'extension' subset and click 'clip raster by extexnt'
Lastly, load in the merged  as your input layer, and clip its extent according to that of the original model DTM. 
Save your DTM as a GeoTIFF (.tif) file, this is important for the next step!
Repeat for the DSM

 ### Step 4:
 Resample the DTM + DSM into a coarser resolution.
 This is accomplished using ArcMap (developed by Esri): unfortunately for the public, this software is private- however, you may have access to it if you are enrolled at a university in the UK.
 
 Firstly, load in your clipped Digital Models. You can simply drag the file into the workplace on the left-hand-side of the screen.
 Secondly, access and open 'Resample (Data Management)' via the search tab.
 Thirdly, input your clipped Digital Models as the input rasters and select which cell sizes you would like the model to resample to (change the numbers in the X and Y boxes)
 Here you also have the option of changing the resampling technique from a nearest neighbour approach to bilinear, cubic, or majority approach.
 
 
