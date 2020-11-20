# Dissertation
##### special thanks to both Paul Bates and Jeison Sosa for their help on this project so far:

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
Save your DTM as a GeoTIFF (.tif) file, this is important for the next step!

 Step 4: Resample the DTM into a coarser resolution.
 This is accomplished using the LFPtools package (https://github.com/jsosa/LFPtools) on Python 
 
 An example piece of code for a resampling method is as such:
 --------------------
 #!/usr/bin/env python

from subprocess import call
import lfptools as lfp

# Resampling using LFPtools (allows removing outliers)
lfp.rasterresample(nproc=4,
                   outlier='yes',
                   method='mean',
                   hrnodata=-9999,
                   thresh=0.00416,
                   demf='one_m_clipped.tif',
                   netf='two_m_clipped_tmp.tif',
                   output='two_m_clipped.tif')

 ---------------------------
 where;
  nproc = # of computer cores to use
  outlier = do you want to detect outliers in the model? 
  method = which reduction method do you want to use? mean, min, meanmin?
  hrnodata = the highresolution NODATA value- if you're unsure as to what this is then you can open your DTM file in a text reader, and it will show you.
  thresh = the searching windows threshold
  demf = the file name of your high resolution DTM
  netf = the target mask file path
  output = the output file name
 
 ------------------------------

