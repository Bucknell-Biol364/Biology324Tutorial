Tutorial
================
Go Ogata, Will Snyder

Importing Brain scan and Brain fold tracings
--------------------------------------------

the oro.nifiti package provides us a way to load the nifti files to R using the command readNIfTI. Additionally this command is able to read from g zipped (.gz) files too.

In this tutorial we will be using the brain scan file 'bias\_corrected.nii.gz' and Brain fold tracings file 'full\_orbital\_sulcus\_t1dim.nii.gz' located in the same file as the rmd file.

``` r
#scan <- readNIfTI(<nifti file location>)
scan <- readNIfTI('bias_corrected.nii.gz')
sulci <- readNIfTI('full_orbital_sulcus_t1dim.nii.gz')
```

Displaying the Data
-------------------

With the orthographic command from the previous package we are able to dispaly the brain scan in R. The image produced by the scan shows the

``` r
orthographic(scan)
```

![](Tutorial_files/figure-markdown_github/Displaying%20the%20Brain%20Scans-1.png)

``` r
#or
oro.nifti::image(scan[,,75])
```

![](Tutorial_files/figure-markdown_github/Displaying%20the%20Brain%20Scans-2.png)

But in most case the default setting of centered in the middle file isn't enough. To move the access you can use the add the flag xyz = (x-axis,y-axis,z-axis). The x-plane is shown on the top right, y-plane is shown on the top-left and the z-plane is shown on the bottom left and the red lines shows the location of the plane in the other two planes

``` r
# Editing the location of the Cross Section
orthographic(scan,xyz = c(125,125,125))
```

![](Tutorial_files/figure-markdown_github/Displaying%20the%20Brain%20Scans%20with%20Settings-1.png)

``` r
image(scan[,,130])
```

![](Tutorial_files/figure-markdown_github/Displaying%20the%20Brain%20Scans%20with%20Settings-2.png)

``` r
image(scan[,130,])
```

![](Tutorial_files/figure-markdown_github/Displaying%20the%20Brain%20Scans%20with%20Settings-3.png)

``` r
image(scan[130,,])
```

![](Tutorial_files/figure-markdown_github/Displaying%20the%20Brain%20Scans%20with%20Settings-4.png)

``` r
# Editing the settings for the Cross Hairs
orthographic(scan,crosshairs = F)
```

![](Tutorial_files/figure-markdown_github/Displaying%20the%20Brain%20Scans%20with%20Settings-5.png)

``` r
orthographic(scan,col.crosshairs = "blue")
```

![](Tutorial_files/figure-markdown_github/Displaying%20the%20Brain%20Scans%20with%20Settings-6.png)

``` r
oro.nifti::image(scan[,,130], col = gray(0:64/64))
```

![](Tutorial_files/figure-markdown_github/Displaying%20the%20Brain%20Scans%20with%20Settings-7.png)

Once we learned how to display the brain scans, and we can use the the command for orthographic or the overlay command to see one specific cross section for a better look.

``` r
orthographic(scan,y = sulci, col.y = "green", col.crosshairs = "red", c(125,225,75))
```

![](Tutorial_files/figure-markdown_github/Overlaying%20the%20Brain%20Fold%20Tracings%20on%20the%20Scan-1.png)

``` r
overlay(x = scan, y = sulci, z = 75, plot.type = "single", col.y = "green")
```

![](Tutorial_files/figure-markdown_github/Overlaying%20the%20Brain%20Fold%20Tracings%20on%20the%20Scan-2.png)

However, we forgot a step when editing the Brain Fold Tracing file. The NIfTI files are essentially 3-Dimensianal matrixes called Voxels and as of right now are large portions have a value of zero and the overlay is showing it with the solid green color. To fix this problem, you can edit the file so that every spot with a value of zero is changed to contain a NA so that the value becomes invisable for the overlay.

``` r
sulci[sulci == 0] = NA

orthographic(scan,y = sulci, col.y = "green", col.crosshairs = "red", c(125,225,75))
```

![](Tutorial_files/figure-markdown_github/with%20the%20fixed%20value-1.png)

``` r
overlay(x = scan, y = sulci, z = 85, plot.type = "single", col.y = "green")
```

![](Tutorial_files/figure-markdown_github/with%20the%20fixed%20value-2.png)

``` r
image(scan[,,130])
```

![](Tutorial_files/figure-markdown_github/unnamed-chunk-1-1.png)

``` r
vec <- as.vector(scan)
```

``` r
scan[scan>80] = 3
scan[scan>80] = 1
image(scan[,,130])
```

![](Tutorial_files/figure-markdown_github/Extra%20information-1.png)

``` r
ggplot() +
  geom_density( aes(x = vec)) +
  theme_cowplot()
```

![](Tutorial_files/figure-markdown_github/unnamed-chunk-2-1.png)

Acknowledgements
================

1.<https://cran.r-project.org/web/packages/oro.nifti/oro.nifti.pdf> 2.
