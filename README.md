# Research Archive
### by Inan Bostanci
This archive contains the code to reproduce the results from my study on traffic demand modeling. The study combines traffic loop sensor data and administrative data to validate and calibrate traffic estimations on a nationwide level. It was done in collaboration with [CBS](https://www.cbs.nl/en-gb), the Dutch Central Agency for Statistics. CBS has a larger project on traffic demand estimation in the Netherlands called DaCiMob, and this study is part of that project. The study is also my thesis project, which was supervised by [Dr. Peter Lugtig](https://www.uu.nl/staff/plugtig) from Utrecht University and Yvonne Gootzen from CBS. It was approved by the Ethical Review Board of the Faculty of Social and Behavioural Sciences of Utrecht University under file number 21-2133. The archive is located in [this repository on GitHub](https://github.com/iebos/dacimob). 
</br> 
</br>
In this repository, there are three jupyter notebook scripts, a csv file and a license. The license contains all information you need if you want to use this repository. Understanding the content and structure is easier if you have read the thesis.
Below is a table of contents. Following that, I explain the structure of the scripts and the csv files, as well as other data that was used in this project and cannot be published openly. I describe alternatives to generate synthetic data. 
| File                                                | Purpose                                                                                                                                              | Type             |
|-----------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------|------------------|
| load_preprocess_sensors.ipynb                       | Preprocessing traffic loop sensor data                                                                                                               | Jupyter notebook |
| link_obs_exp.ipynb                                  | Linking observed to expected traffic counts and creating figures for section 4 of thesis                                                             | Jupyter notebook |
| inspect_model_c.ipynb                               | Inspecting and modeling the calibration factor and creating figures and tables for section 5 and appendix of thesis                                  | Jupyter notebook |
| edges_intensities.csv                               | Aggregated and preprocessed observed traffic counts from traffic loop sensors                                                                        | Data             |
| LICENSE                                             | License of this repository                                                                                                                           | License          |

## Scripts
At the beginning of each script, I import all packages that are needed. If you do not have one of the packages, you can install them with !pip install PACKAGE. I worked in Jupyter notebook scripts with python v3.8.5. The main packages used are pandas v1.3.4, NumPy v.1.21.4, GeoPandas v0.10.1 and OSMnx v1.1.2. Output is stored within the Jupyter notebook script, but I included optional "checkpoints", which you can uncomment to save data frames or figures to csv or pdf for later usage (note: It was necessary for me to work in Jupyter notebook due to the internal structure at CBS).
</br>The scripts should be run in the following order and are structured as follows:
<ol>
  <li><b>load_preprocess_sensors.ipynb</b></li>
  <ol>
     This notebook contains the method that I used to load and preprocess the sensor data from CBS. This is not as trivial as you would expect, due to the size of this data set. If you do not have access to the internal CBS server, you cannot run this script.
    </ol>
  <li><b>link_obs_exp.ipynb</b></li>
   <ol>
  First, the sensor data, that resulted in the previous script, is aggregated. 
  If you do not have access to CBS, it is indicated at which point you can start using edges_intensites.csv
  I also load the road network data from OpenStreetMap. This takes a while (an hour or more), because it loads the entire Dutch road network.
  I inspect and visualize the observed traffic counts.
  Then, I load the expected counts that were provided by CBS. 
  If you do not have access to CBS data, you will find a chunk that you can uncomment to generate synthetic data.
  I inspect and visualize the expected counts and link them to the observed counts. 
  To make sure you don't have to reload the entire road network from OSM again for the next script, I included chunks that store the desired objects in the environment, which then can be called in another script.
     </ol>
  <li><b>inspect_model_c.ipynb</b></li>
   <ol>
     To make sure you don't have to load the entire road network from OSM again, I call the stored objects at the beginning of this script.
     After that, I inspect the calibration factor and the quality of the expected counts as an estimate for the observed counts visually and numerically.
     I then run multiple "calibration models" that I compare, to predict the calibration factor on road segments that do not have observed counts.
     Finally, I inspect whether the calibration on the basis of the calibration model improved the expected counts both visually and numerically.
     </ol>
</ol>

## Data
<ol>
  <li><b>edges_intensities.csv</b></li>
  <ol>
This csv contains preprocessed and aggregated traffic sensor data. Observed traffic counts are provided by the <a href=https://www.ndw.nu/>Nationaal Dataportaal</a>. I used a reformatted version of this data that is stored on a CBS server. If you do not have access to CBS, you can load the preprocessed and aggregated data that is provided in this csv.
  </ol>
  <li><b>Regional data</b></li>
  <ol>
    In script 3, I use a csv file that contains the population density of each municipality in 2019. The source of this data is the open data portal of CBS, you can directly access it <a href=https://opendata.cbs.nl/statline/#CBS/nl/dataset/70072ned/table?dl=5A35F">here</a>. I do not hold the license to this data, so you have to fetch the data and create your own csv. The population density is used as a predictor to model the calibration factor in script 3. Unfortunately, you also need to get the municipality that the sensor is located in. I did this using a shapefile from CBS, which I cannot upload publically because it is not mine. If you do not have access to the internal CBS server, you can either discard the population density as a predictor, which will not have a large effect on the result. Or you can look for other public shapefiles for the Netherlands, which should be available on the internet. 
  </ol>
  <li><b>Infrastructure data</b></li>
  <ol>
    Infrastructure data on the road network is directly loaded from OpenStreetMap using the package osmnx as a graph with nodes and edges. 
  </ol>
  <li><b>Expected traffic counts</b></li>
  <ol>
  Expected counts were obtained by CBS using administrative data (see [this paper by <a href=https://www.cbs.nl/-/media/innovatie/combining-data-sources-to-gain-new-insights-in-mobility-v2.pdf>Gootzen et al. 2020</a> for more details), 
and survey data from the ODiN survey (see this paper by <a href=https://www.cbs.nl/-/media/_pdf/2021/44/mobility-trends-2021-report-v3.pdf>Boonstra et al. 2021</a> for more info on the ODiN survey). CBS provided me with the resulting expected count for each road segment. These data sources are sensitive, and therefore I can neither publish them, nor the resulting expected counts, in this repository. Get in touch with the <a href=https://www.cbs.nl/en-gb/about-us/contact/infoservice>CBS infoservice</a> if you are an interested researcher that wants to work with this data. To reproduce my results without access to the data, I included a code chunk in script 2 to generate synthetic data. This creates a data frame with edges and an expected count for each edge that was randomly sampled from a gamma distribution.  
  </ol>
  </ol>
