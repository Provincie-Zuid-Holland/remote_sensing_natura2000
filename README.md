# Introduction 

We as Province South Holland want to use satellite images in order to detect different surface area's of vegetation types so can we can monitor nature protected area's over time for different climate purposes.

To do this we have made a pixel based prediction model which iterates and predicts over 50cm pixels from the Superview Satellite provided by us for free by the Netherlands Space Office (NSO).

<!--- ( A [DALLE-2](https://openai.com/dall-e-2/), artistic representation of this:

![alt text](DALL_E_2023-01-04_16_23_38_satellite_oilpainting.png))-->


We had to make a lot of custom python code ourselves for this project which hopefully can be used for other remote sensing projects at other companies or institutions as well.

We decided to split different functionalities of the python project code into different repositories in order to reduce complexity 

This repository is a consolidation of all the different python repositories used the remote sensing/ objection detection in satellite images for Natura 2000 regions by Province South Holland.

Thus this repository gives a overview of all how they all come together.

# Overview.

A picture is worth more than a thousand words, so here this architecture drawing of the different components:

![alt text](RS_Architecture_2022.png "Title")

I will be explaining from up to down and then left to right.

## Annotations

We have tried different unsupervised models since we did not have any annotations on thus no training data to for a model.
But after several experiments we have found that with annotations and thus training data seem to lead to the best results.

And such we have made handmade annotations on satellite images ourselves for training data for the model.

They can be found here, if want to try it yourself:
https://github.com/Provincie-Zuid-Holland/satellite-images-nso-datascience/tree/main/data/annotations


## satellite_images_nso_extractor

We have found that during this project data engineering/data preprocessing took the most time and thus this package is made to easily extract satellite images from NSO.

NSO provides free satellite images from the Netherlands, but a downside is that the NSO does not provide satellite images fitted to a specific area but only a large overlapping region. This leads to a unnecessary large amount of data especially if you only want to study a smaller specific region and as such cropping is needed, which is what this package provides.

So check this repository if you want to easily download NSO satellite images:
https://github.com/Provincie-Zuid-Holland/satellite_images_nso_extractor

## vdwh_ahn_processing

A second data input we use is AHN lidar data, this is 3D height data from the Netherlands.
This can be very good to predict height in vegetation so trees for example are easy to see in this data.

We have to transform this data from 3D to 2D in order to merge it with the satellite data from the NSO so it both can be used in a model.
This repository uses Apache Spark on Databricks to transform the data from 3D to 2D, we have to spark because these computations are very expensive.

It can be found here:
https://github.com/Provincie-Zuid-Holland/vdwh_ahn_processing


## satellite_images_nso_datascience

This is the repository which holds all the different models, both old unsupervised models and the final chosen randomforest model, for the remote sensing project.

It also holds the annotation data and the code to extract the RGBIH values from satellite images based on these annotations thus creating training data for the model.

Find it on the link here:
https://github.com/Provincie-Zuid-Holland/satellite-images-nso-datascience

## pixel_based_tif_model_iterator

Since a average satellite image that we use contains around 3 billion pixels that we have to iterate over we have written custom python code to run the model in a distributed way. 

3 Billion pixels is too much data to hold in memory so we divide the satellite images into multiple parts, these parts in turn get distributed across the cluster.

It contains pyspark code to iterate the model on a disturbed cluster in a data bricks environment.

Or using python multiprocessing, which uses up all your cpu's and thus freezes your pc, when running locally


Find it here:
https://github.com/Provincie-Zuid-Holland/satellite_images_nso_tif_model_iterator

## Arcgis pipeline

We have a custom pipeline which deploys the models outcomes to our arcgis dashboard environment.


# Author
Michael de Winter

# Contact

Contact us at vdwh@pzh.nl







