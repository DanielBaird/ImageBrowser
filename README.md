# ImageBrowser

A browser for viewing gigantic images using Leaflet to zoom/pan around.


Preparing a TIFF for use in the viewer
--------------------------------------

Install GDAL. [Have fun](https://trac.osgeo.org/gdal/wiki/DownloadingGdalBinaries#Windows)!

I thought I needed to do this enlarging step to fix a position reporting bug, but although it works, the bug still happens.  So this can/should be skipped!

```
# enlarge a tiff to be {MAX_X} by {MAX_Y} pixels
#  {PIC_X} = tiff's current x size
#  {PIC_Y} = tiff's current y size
#  {O_Y} = {PIC_Y} - {MAX_Y} (should be negative)
#
gdal_merge.py -init "255" -ul_lr 0 {O_Y} {MAX_X} {PIC_Y} -o mergeout.tif jcu.tif
```

This tile making is the main thing we're doing here.

```
# produce tiles from FG-94-UterusChoriocarcinoma.tif with seven zoom levels and putting them into a ./tiles/ directory
#
gdal2tiles.py --config GDAL_DATA="C:\Program Files\GDAL\gdal-data" --s_srs=null --profile=raster -z 0-7 -v -w none BigOriginalImage.tif tiles
```

Example code with fields filled out (starts with `fg94.tif`, dumps tiles into `./fg94tiles`).

```
# this merge is not necessary
gdal_merge.py -init "255" -ul_lr 0 -15768 32768 17000 -o fg94.tif 

gdal2tiles.py --config GDAL_DATA="C:\Program Files\GDAL\gdal-data" --s_srs=null --profile=raster -z 0-7 -v -w none fg94.tif fg94tiles
```

Once you have tiles, you'll want to copy them into `webpage/images/FG-94-UterusChoriocarcinoma` for them to show up in your viewer.
