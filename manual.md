
# Manual on Prediction of Future Land Cover Changes using QGIS MOLUSCE Plugin**

## Table of Contents
[1. QGIS Installation](#1-qgis-installation)

[2. MOLUSCE Plugin Installation](#2-molusce-plugin-installation)

[3. Study Area Selection](#3-study-area-selection)

[4. Dataset Preparation](#4-dataset-preparation)

    [4.1 Land Cover Dataset](#41-land-cover-dataset)

    [4.2 Driving Factors](#42-driving-factors)

[5. Steps in QGIS MOLUSCE Plugin](#5-steps-in-qgis-molusce-plugin)

    [5.1 Step 1: Inputs](#51-step-1-inputs)

    [5.2 Step 2: Evaluating Correlation](#52-step-2-Evaluating-Correlation)

    [5.3 Step 3: Area Changes](#53-step-3-area-changes)

    [5.4 Step 4: Transition Potential Modeling](#54-step-4-transition-potential-modeling)

    [5.5 Step 5: Cellular Automata Simulation](#55-step-5-cellular-automata-simulation)

    [5.6 Step 6: Validation](#56-step-6-validation)

    [5.7 Step 7: Prediction](#57-step-7-prediction)


## 1. QGIS Installation

Download QGIS setup file for windows from this website. The MOLUSCE plugin is
    supported only in QGIS version 2. Thus, QGIS version 2.18.0 is recommended.
  
  <img width="408" alt="image" src="https://github.com/user-attachments/assets/3cc2c7ff-0e69-4d24-bb1d-9482a9f61b05" />



## 2. MOLUSCE Plugin Installation

Open QGIS application and create new empty project. In the menu bar, click the
    **_Plugins_** tab and click on **_Manage and Install Plugins_** option to open **_Plugins_** window as shown in Figure below.
  
  <img width="415" alt="image" src="https://github.com/user-attachments/assets/e0e12430-4ae6-49ac-9fc9-1ebfca4cb9fa" />

Search plugin by writing MOLUSCE keyword in search bar. Then, select MOLUSCE
    and click **_Install plugin_** button to install the plugin.

<img width="700" alt="image" src="https://github.com/user-attachments/assets/093074c3-7cae-4aff-a9c8-7c209951c0be" />

After the successful installation of plugin, you would be able to see the **_MOLUSCE_** option listed in the Raster menu.

<img width="700" alt="image" src="https://github.com/user-attachments/assets/b181315d-eab4-45d1-9914-5880bbdc5695" />

## 3. Study Area Selection
a. First of all, select the area where you would like to carry out case study. The extent
of study area could be at national, regional, municipal, ward or any spatial unit
depending upon requirements.

b. In this case study, Ben Cat district of Vietnam was chosen. The district level
administrative layer in shapefile format was downloaded from OpenDevelopment
Mekong data portal (link).

c. Layer was loaded in QGIS and only the BenCat district was exported.

d. As projected coordinate system is preferred, it is essential to know to the UTM
zone of the selected study area. To find the UTM zone, use the link1 or link2.

e. The Ben Cat district lies in WGS UTM zone 48N (EPSG: 32648). Hence, the layer
was projected using Reproject Layer tool.

- Find the UTM zone using this link1 or link
- Find and open the “Reproject Layer” tool from processing toolbox
  
  <img width="225" alt="image" src="https://github.com/user-attachments/assets/8a1cc659-5cb7-4ed2-94f9-0ec638d1c8c6" />

- Select the layer you want to reproject as an **Input layer**. Find and set Target
    CRS as per the required (WGS84/ UTM ZONE 48N in this case study)

    <img width="450" alt="image" src="https://github.com/user-attachments/assets/2becd7b4-3457-4d5b-aae4-91f053f69915" />
    <img width="200" alt="image" src="https://github.com/user-attachments/assets/f06d750e-2d4b-4a76-8ece-6c0b0f63932f" />

- Run and save the reprojected layer


## 4. Dataset Preparation

### 4.1 Land Cover Dataset

a. First of all, select the years you need/want to map the land cover. If you want to
prepare land cover map from scratch, start by collecting high-resolution satellite
or aerial imagery and ground truth data. Preprocess the data by correcting for
radiometric and geometric distortions and remove clouds if necessary. Annotate
the imagery by defining land cover classes and either manually digitizing or using
semi-automatic methods with machine learning tools. Validate the dataset
through comparison with ground truth and accuracy assessments, then refine as
needed.
Most of the countries or researchers have mapped the land cover in dicerent
levels which are publicly available. You can also find global land cover map
datasets in dicerent sources. Find the datasets and download the dataset and
analyze its quality and reliability for the selected study area. While selecting the
years, there should be at least three consecutive years for land cover prediction
and should be of equal interval.

b. For the Ben Cat district case study, data were downloaded from Global Land
Analysis and Discovery website (link). This dataset can be found in 10*10 degree
tiles and has a spatial resolution of 0.00025° per pixel (~30 meters at the equator).
The annual land cover and land use maps of years 2000, 2005, 2010, 2015 and
2020 can be found.

- First select the grid, where the area of interest falls. Since Ben Cat district lies in 20N 100E grid, the grid was selected.
- After the grid selection, the data download links will be listed below.
- Click the links to download the data for respective years.
  
<img width="700" alt="image" src="https://github.com/user-attachments/assets/928f8325-1089-4f84-8ca4-04f460f52b89" />
    
<img width="700" alt="image" src="https://github.com/user-attachments/assets/e42b2b00-cf37-486e-bf1d-ae9f28b18c31" />


c. Import downloaded data into QGIS, you will see the gradient legend in layer panel.
Refer to this [legend](https://storage.googleapis.com/earthenginepartners-hansen/GLCLU2000-2020/legend.xlsx) on which land cover mapping is based on.
Since we are not working on these all these classes, reclassification is required.
In this case study, land cover classes were reclassified as follows:
| Pixel Value Range     | New Pixel Value | Land Cover Class |
|----------------------|----------------|------------------|
| 0 - 48, 100 - 148   | 1              | Forest          |
| 200 - 207          | 2              | Water Bodies    |
| 250                | 3              | Built-up        |
| 244                | 4              | Cropland        |


Since snow/ice and ocean land cover are not in the study area, they are not
mentioned in above reclassification table.

<img width="700" alt="image" src="https://github.com/user-attachments/assets/b0e7925f-c5e0-42ea-92ec-844d1cf75560" />


d. First clip the tentative study area using Clipper tool available in extraction option
of Raster menu.

<img width="550" alt="image" src="https://github.com/user-attachments/assets/6328c35f-39bc-4e9c-b2e4-ccede515ee51" />


e. For the reclassification in QGIS, open **_Reclassify values (simple)_** tool.

- Select the layer you want to reclassify as grid
- Set the replace condition
- Define lookup table as shown in figure below
- Run the reclassification tool.
  
<img width="350" alt="image" src="https://github.com/user-attachments/assets/f4e567a5-60a8-491e-a069-6d2604bf12a0" />

<img width="550" alt="image" src="https://github.com/user-attachments/assets/23235044-b83c-4855-a7ec-c56f670fbed6" />


f. To work in projected coordinate system, open the Warp (reproject) tool to project
the dataset

- Find the UTM zone using this [link1](https://www.apsalin.com/utm-zone-finder/) or [link2](https://www.apsalin.com/utm-zone-finder/).

  <img width="350" alt="image" src="https://github.com/user-attachments/assets/259255ab-2c6b-4c74-82ed-54601897b961" />

- Select the layer you want to reproject as an input layer.
- Select the destination CRS as required (WGS UTM zone 48N in this case
    study).
- In this case study, the spatial resolution of dataset is 0.00025 degree.
    1 degree= 111.11 km
    0.00025 degree=0.00025111.11*1000 m
    0.00025 degree=27.75m
    Therefore, output resolution is set as 25m is this case study and “nearest
    neighbourhood” is selected as resampling method.
    You can also set output resolution as 30m or 27.75m but it should be same
    when to prepare driving factors datasets as well.
- Define the output file path in Reprojected

- Run the tool
  
    <img width="400" alt="image" src="https://github.com/user-attachments/assets/c5da488d-a2fa-4b98-85c3-31484fb00f81" />
    <img width="300" alt="image" src="https://github.com/user-attachments/assets/fbaa59ae-edb1-46e1-b21c-99986ca8d144" />


g. Finally, clip the raster layer within the study area administrative boundary.

<img width="400" alt="image" src="https://github.com/user-attachments/assets/e13330ee-c81f-45be-bd03-a347c261cf36" />

<img width="600" alt="image" src="https://github.com/user-attachments/assets/bc8d7311-417c-4cf3-82b9-eb1b84791ef1" />

- Select the reprojected land cover layer as an input layer
- Select study area admin as a mask layer layer (Ben Cat District boundary data
    in this case study)
- Select the options crop the extent of the target dataset to the extent of the
    cutline and keep resolution of output raster.
- Define the output file path in Clipped (mask)
- Run the tool
- The final properties of land cover map of this case study are as follows:
    No of classes: 4
    Pixel size:25, - 25
    Columns:
    Rows: 1559
    Coordinate Reference System (CRS): EPSG 32648, WGS 84/UTM ZONE 48N
h. Repeat the process to prepare the land cover datasets for other selected years.


### 4.2 Driving Factors

MOLUSCE Plugin only accepts the raster format datasets.

In this case study, population density, slope, hillshade, digital elevation model, distance from the road, distance from the rivers were considered as the driving factors based on the data availability. The preparation procedure involved in preparation of each dataset to feed into the MOLUSCE plugin is described below in detail.

a) Digital Elevation Model and Hillshade
- Clip the tentative study area using **_Clipper_** tool available in extraction option
    of Raster menu [refer to 4.1 (d)]
- Reproject data into the selected projected coordinate system and set
    preferred spatial resolution which should be same as the land cover dataset.
    Select the bilinear convolution as a resampling method. [refer to 4.1 (f)]
- Finally, clip the layer within the study area administrative boundary [refer to 4.
    (g)]

b) Population density
- Clip the tentative study area using **_Clipper_** tool available in extraction option
of Raster menu [refer to 4.1 (d)]
- Reproject data into the selected projected coordinate system and set
preferred spatial resolution which should be same as the land cover dataset.
Select the cubic convolution as a resampling method. [refer to 4.1 (f)]
- Finally, clip the raster layer within the study area administrative boundary [refer
to 4.1 (g)]

c) Slope
Slope data is derived from the digital elevation dataset. Use the projected digital
elevation data.
- Open GDAL based slope analysis tool.
  
    <img width="500" alt="image" src="https://github.com/user-attachments/assets/ee9fe100-4b96-4846-b6a3-af7f6fdf8117" />

- Check the Slope expressed as percent (instead of degrees) option

  <img width="600" alt="image" src="https://github.com/user-attachments/assets/7765f30d-91bd-460f-b0df-672f8bc7af43" />
- Run the tool
- Finally, clip the slope layer within the study area administrative boundary [refer
    to 4.1 (g)]
  
d) Distance from the Road and Distance from the River
Road network and river network datasets are vector datasets which are mostly
found in .shp or .geojson format.

- Clip the tentative study area using **Clip vectors by extent** geoprocessing tool.
Click the Select extent on canvas option and draw tentative extent.

    <img width="319" alt="image" src="https://github.com/user-attachments/assets/a315e00f-2bd1-4a13-b0c3-2536b9e3e758" />
    <img width="341" alt="image" src="https://github.com/user-attachments/assets/3488c574-4e5a-4d02-8997-316dedc834a2" />


- Reproject data into the selected projected coordinate system [Refer 3 (e)].

- Open the attribute table window, toggle on the editing mode, add new field (Eg:‘id’ in this case) and populate the field with value 0.

    <img width="600" alt="image" src="https://github.com/user-attachments/assets/7816c4ea-cba5-456a-88d4-74991ab68135" />

- Using the **Rasterize (vector to raster)** tool, the river and road datasets were converted into raster datasets.

    <img width="500" alt="image" src="https://github.com/user-attachments/assets/d565409d-8a67-4c18-849a-422ceef6e60b" />

- Select the newly created field (‘id’ in this case) as an **Attribute field**. Set output resolution as per required (25*25 in this case). Define **Nodata value** in advanced parameters as -9999. Run the tool and save the rasterized dataset.

    <img width="700" alt="image" src="https://github.com/user-attachments/assets/0e57e798-3815-411e-95e2-b934ec53e8c5" />

- Use raster tool named “ Proximity raster” to generate the distance from the road/river layer.

    <img width="450" alt="image" src="https://github.com/user-attachments/assets/47dd06b8-262d-4a6f-a877-e4e93cbbc71c" />

- Select the rasterized layer generated from previous layer, run the tool and save the result.

    <img width="600" alt="image" src="https://github.com/user-attachments/assets/0ffd3674-4a3a-49c0-a49e-d484a6b3ca56" />

- Finally, clip the raster layer within the study area administrative boundary [refer
    to 4.1 (g)]


## 5. Steps in QGIS MOLUSCE Plugin

### 5.1 Step 1: Inputs

All the layers which have been added in QGIS project are listed in left panel in Inputs tab.

- Firstly, select the layer you want to set as an initial data and then click **_Initial>>_** button to set the initial data. The year box is filled automatically based on layer’s name you have defined for selected layer, if not you can define/edit layer name as well as year. Similarly, select and set the final layer as well. In this case study, land cover of year 2000 and 2010 are set as initial and final layer respectively.
  
- In a similar way, select the layers you want to define as Spatial variables and set in spatial variables window by clicking **_Add>>_** button.

- Click on **_Check geometry_** button to check whether the inputs dataset fulfill all the requirement that includes same coordinate system, same resolution and same extent. If all the requirements are fulfilled, the prompt window is displayed with message “Geometries of raster’s are matched” and all other remaining tabs will be activated.

- Click **_ok_** to close the prompt window.

- If the geometries of raster’s do not match, error message will be displayed. Then, correct the dataset and try again.

    <img width="550" alt="image" src="https://github.com/user-attachments/assets/7b858afc-143c-40ad-be07-09bb20ba14cd" />

### 5.2 Step 2: Evaluating Correlation

This tab aims for evaluating the correlation between spatial variables that are defined in
the previous input tab. The three methods that are available for calculating correlations
among spatial variables are Pearson’s correlation, Crammer’s correlation and Joint
Information Uncertainty**.**


**_Check all rasters_** option must be selected to calculate the correlation among all
variables.

In our case, since all the spatial variables are continuous type, Pearson’s correlation
method has been selected.

In general, if the correlation between two drivers (or variables) is greater than 0.75, it
suggests a strong linear relationship between them. In such cases, one of the drivers may
be neglected to avoid multicollinearity implications such as redundancy, overfitting, and
diciculty in training the models.

<img width="550" alt="image" src="https://github.com/user-attachments/assets/2baba8c1-05b6-4e33-af29-9906286113ed" />


### 5.3 Step 3: Area Changes

- Click on **_Update tables_** button to compute land use/cover area changes and transition probabilities table.The land use/cover area units can be expressed in raster unit, square kilometer (sq. km) and hectare (ha) as shown in below Figure.

- Generate and save the land use/cover change map by clicking the **Create_changes map_** button.

    <img width="550" alt="image" src="https://github.com/user-attachments/assets/13182a58-6ef9-4a4f-98ab-3b21c2c5c31b" />


### 5.4 Step 4: Transition Potential Modeling

After successful completion of the previous steps, transition potential modelling is
carried out. The modeling is carried out by defining samples, modeling method and
parameters. Firstly, the available three sampling mode options are All, Random and
Stratified and selection of sampling mode depends on the class’s distribution of input
dataset. In this case study, stratified sampling method is selected since precise class
representation is crucial and input dataset classes distribution are also not balanced.

Secondly, there are four methods that encompass artificial neural networks (ANN),
weights of evidence (WoE), logistic regression (LR), and multi-criteria evaluation for
constructing potential transition maps. Each method has its own advantages and
disadvantages. In this case study, the ANN method has been selected due to its
capability to handle complex (non-linear) factors.

Thirdly, other miscellaneous parameters such as number of samples, neighborhood,
learning rate, maximum iterations, hidden layers, and momentum are input.

After giving all the inputs, there is a button ‘ **Train neural network** ’ which needs to be clicked to start the modelling process. There is also an option to ‘ **Stop’** button which is used to terminate the model.

After modelling is completed, overall accuracy values appeared on the interface
determines the quality of the modelling. In most cases, desirable overall accuracy might
not be obtained in the first step. Hence, other miscellaneous parameters need to be
adjusted until the desirable overall accuracy is obtained based on the hit and trial
method.

In this very crucial step, neural network learning curve (training vs validation datasets)
need to be reviewed and diagnosed properly to prevent from the learning problems such
as an underfit or overfit model, and unrepresentative training or validation datasets.
[Reference: https://machinelearningmastery.com/learning-curves-for-diagnosing-
machine-learning-model-performance/ ]

The input values used in this case study are shown in the Figure below. The interface also
has dicerent functions as shown below.


After successful completion of the training the model **click** on the save and save the
graph into the designated folder.

In this case sudy, decrease in both training and validation loss to a point of stability with a minimal gap between the two final loss values can be witnessed in the learning curve which represents the well fitted model for this case study.

<img width="550" alt="image" src="https://github.com/user-attachments/assets/83065fbf-361d-4836-83c5-35042d20667a" />

### 5.5 Step 5: Cellular Automata Simulation

This is the second last step. The main task that needs to be carried out is to save the file path for prefix of transition potential maps, certainty function, and simulation results as shown in below Figure. In step 1, the year dicerence between initial (2000) and final (2010) land cover map is 10. Thus, Number of simulation iterations defined as 1 in this step will simulates the land cover map of year 2020.

<img width="550" alt="image" src="https://github.com/user-attachments/assets/c3692784-cbb4-445d-9735-a217d1442336" />


### 5.6 Step 6: Validation

This is the last step of the MOLUSCE. As shown in the Figure below there are many
functions on validation steps.

- Firstly, reference map needs to be imported. To do this, click on **Browse** and select the reference land cover which will act as validation data against the simulated map. To
import the simulated map, adopt similar process using the browsing tool.

- Ensure that you have checked the validation map and check persistent classes.

- The number of validation iterations should be imported based on the results obtained on
the % of correctness, kappa (overall), kappa (histo), and kappa (loc). If these values are satisfying (i.e. >0.6) the simulated map can be accepted else hit and trials method should be adopted by changing the parameters used in the transition potential modelling step.
    <img width="550" alt="image" src="https://github.com/user-attachments/assets/f60ec979-e28e-4b94-a661-369c2b695e53" />


### 5.7 Step 7: Prediction
Once acceptable kappa statistics are obtained in previous step, inputs need to be given to predict the future land cover map. As shown in the figure all the land cover maps and driving are given as input. The below figure shows the input for this case study. Land cover map of year 2010 and 2020 are set as initial and final respectively to predict the land cover for year 2030 and so on.
  
<img width="550" alt="image" src="https://github.com/user-attachments/assets/a0a9f19c-86b5-4d51-a285-8f203a1354f1" />

Once the inputs are defined, proceed directly to step 5 (Cellular Automata Simulation). The number of simulation iterations must be entered in a specific way. In this case study, with an initial year of 2010 and a final year of 2020, setting the number of simulation iterations to 1 will predict outcomes for 2030, 2 will predict for 2040, 3 for 2050, and so forth. Therefore, each iteration represents one interval between the initial and final years.

<img width="550" alt="image" src="https://github.com/user-attachments/assets/ffafc086-f0a3-4c03-9ced-6dc68f53065f" />

Simulate and save the predicted land cover map


