# Dissertation
Investigating the impact of resolution, particularly at building-resolving levels, on modelled inundation levels in the urban area of Carlisle, UK.  



Step 1: Find the terrain files for the area of choice: Carlisle.
This was accomplished by gaining access to digimap's 1m lidar composite Digital Terrain Model grid.
This provides 1x1km (1000x1000 cells) grids of Carlisle.

Step 2: Merge the files together to create one DTM.
This was accomplished by using the raster and rgdal libraries on Rstudio, an open-source coding language.
The code is as follows:

``` {r}
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
 
 Step 3: Clip the DTM to the original model's boundaries.
 This was accomplished on QGIS, an open-source geographic information systems application. 

Firstly, load in your original model DTM and your merged-DTM from step 2.
Secondly, via. the 'Raster' tab at the top of the page, navigate to the 'extension' subset and click 'clip raster by extexnt'
Lastly, load in the merged-DTM as your input layer, and clip its extent according to that of the original model DTM. 

 Step 4: Resample the DTM into a coarser resolution.
 This is accomplished using the LFPtools package (https://github.com/jsosa/LFPtools) on Python 
 
 
 

