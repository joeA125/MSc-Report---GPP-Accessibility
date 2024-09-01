# MSc-Report---GPP-Accessibility
My master thesis revolved around developing a non-complex remote-sourced model for GPP estimation. The aim was to maintain reasonable accuracy when evaluated against Fluxnet data, while only using remote-sourced variables either from satellites or publicly available reanalysis datasets.

# Pre-requisites 
## Package Installations (If not already completed)
Full list of all used packages is incuded at the bottom of this file. These are the packages I needed to install to my environment. For reference I used Jupyter notebook via Ananconda Navigator.

### For importing 
netCDF4    
xarray    
pyhdf    
pygrib    

#### If using CDS API for ERA5 Land    
cdsapi   

Pease see below for further instructions reagrding this.     

### For preprocessing 
pykrige    

### For modelling 
cartopy    
networkx    
xgboost    
lightgbm    
shap    

## Notebook Running Order 
1 - FluxNet Sampling    
(2 to 6 are interchangeable as independent of each other)     
2 - FluxNet Data  
3 - MODIS  
4 - SIF  
5 - CO2  
6 - ERA5_Land  
7 - Thesis Report  
  Thesis Report is the final culmination of all data and includes modelling.  

## Sites used in this analysis, locations (lat, long) and PFTs (plant function types)  
### Variable Analysis  
AU_CPR - Austraila, Calperum, (-34.0021, 140.5891), Savanna  
AU_TTE - Australia, Ti Tree East, (-22.2870, 133.6400), Grassland  
CH_CHA - Switzerland, Chamau, (47.2102, 8.4104), Grassland  
DE_RUS - Germany, Selhausen Juelich, (50.8659, 6.4471), Cropland  
DE_SFN - Germany, Schechenfilz, (47.8064, 11.3275), Permanent Wetland  
GF_GUY - French Guiana, Guyaflux, (5.2788, -52.9249), Evergreen Broadleaf Forest  
GH_ANK - Ghana, Ankasa, (5.2685, -2.6942), Evergreen Broadleaf Forest  
IT_COL - Italy, Collelongo, (41.8494, 13.5881), Deciduous Broadleaf Forest  
IT_NOE - Italy, Arca di Noe, (40.6062, 8.1517), Closed Shrubland  
US_PFA - USA, Park Falls, (45.9459, -90.2723), Mixed Forest  
US_SRM - USA, Santa Rita Mesquite, (31.8214, -110.8660), Woody Savanna  
US_TW3 - USA, Twitchell Alfalfa (38.1152, -121.6470), Cropland  
US_WCR - USA, Willow Creek, (45.8059, -90.0799). Deciduous Broadleaf Forest  
US_WHS - USA, Walnut Gulch Lucky Hills, (31.7438, -110.0520), Open Shrubland  
US_WKG - USA, Walnut Gulch Grasslands, (31.7365, -109.9420), Grassland  

### Removed for modelling due to lacking dates 
AU_TTE, DE_SFN, US_WHS, US_TW3  

### Added for final modelling
AU_GIN - Australia, Gingin, (-31.3764, 115.7138), Woody Savanna  
FR_PUE - France, Puechabon, (43.7413, 3.5957), Evergreen Broadleaf Forest  
NL_LOO - Netherlands, Loobos, (52.1666, 5.7436), Evergreen Needleleaf Forest  
US_NR1 - USA, Niwot Ridge Forest, (40.0329, -105.5464), Evergreen Needleleaf Forest  

## Dates used - for file importing where dates are needed (ERA5, MODIS, OCO-2)
### Variable Analysis (All 2014) (DD/MM)
06/09, 07/09, 14/09, 17/09, 18/09, 21/09, 23/09, 25/09   
10/10, 11/10, 17/10, 18/10, 28/10, 31/10   
03/11, 05/11, 08/11    
12/12, 21/12, 22/12, 23/12, 26/12, 28/12, 30/12  

### Modelling (DD)
Same dates for all months for all years    
11, 12, 13, 14, 15, 16, 18, 21, 26, 28   

## File Importing for all 5 Sources   
### Fluxnet  
Fluxnet Data can be downloaded from: https://fluxnet.org/   
  Requires account creation first.   

#### Sampling 
FluxNet Sampling Notebook uses the 'Fluxnet Site Descs' excel to create a site sample for further analysis. Sample used in this analysis is located as 'Fluxnet_Site_Sample.xlsx' for variable identification and for final modelling - the 4 added sites are found at end of FluxNet Sampling Notebook. The 4 removed sites are mentioned in the Thesis Report notebook, and also if all steps are followed in the FluxNet Data notebook the the variable investigation dataset and validation datases for final modelling will be made correctly. Also please see above for site information. 

##### Downloading Process 
After account creation see Data -> Download Data on the webpage above. For this project, FLUXNET2015 CC-BY-4.0 Data from FLUXNET2015 Product was used. Ensure FULLSET product is selected and choose the 19 specific sites used in the analysis. (Can be found in 'Fluxnet_Site_Sample.xlsx' for variable analysis, the 4 added files are shown at the end of FluxNet Sampling Notebook). After downloading, unzip folders and use the 'FULLSET_DD' files. These are the daily averaged files. 

### ERA5-Land Reanalysis 

#### File Creation 
This can be done automatically, however, in this instance, automatic download seems to take a while so it was completed manually. Please ensure you have created an account and accepted the terms of use prior to attempting any imports manual or automatic.   

Manual download can be done from here, may require account login / creation first:   https://cds-beta.climate.copernicus.eu/datasets/reanalysis-era5-land?tab=download    

For automatic downloading, please see the ERA5_Land Auto notebook for example code.  
In order to automatically download files, you must save a '.cdsapirc' file inside your C:/users/ filepath ($HOME/) in your computer with the following contents:  
url: https://cds.climate.copernicus.eu/api/v2   
key: {UID}:{API KEY}  

You must also have installed the cdsapi package to your python environment, this is described in the below file as well. 

Please see: https://cds.climate.copernicus.eu/api-how-to for more information on CDS API use.  
Your UID and API KEY can be found in your account information.   

##### Variable Analysis
See dates used above.    
Variables needed can be seen in ERA5_Land Notebook. Select required dates and all 24 hours. Use a sub-region, sub-region is 1 significant figures around desired centre locoations.  

Example:   
AU_CPR location = (-34.0021, 140.5891). Sub-region = (-34.0 North, 140.6 East, -34.1 South, 140.5 west).

###### Naming conventions
All files here are per site per month, for efficient importing please name using following format, otherwise import functions will not run.   
{Site}_{Month}.grib.   

Example for AU_CPR in December:   
AU_CPR_December.grib

##### Modelling 
See dates used above.    
Required variables in this scenario are as follows: '10 metre U wind component', '2 metre dewpoint temperature', '2 metre temperature', 'Evaporation from vegetation transpiration', 'Forecast albedo', 'Leaf area index, high vegetation', 'Leaf area index, low vegetation', 'Soil temperature level 3', 'Evaporation', 'Surface pressure', 'Volumetric soil water layer 3', 'Volumetric soil water layer 4'  

However, if running only analysis use any features seen fit.   

Follow same sub-region convention as previous.    

###### Naming conventions
All files here are per site per month per year, for efficient importing please name using following format, otherwise import functions will not run.   
{Site}_{Month}_{Year}.grib.   

Example for AU_CPR in December 2014:   
AU_CPR_Dec_14.grib

### MODIS

#### File Creation 
Requires account creation. 
Must ensure following applications are authorised for use of this and OCO-2 products.   
Earthdata Feedback Module	   
Earthdata Code Collaborative	   
Earthdata Website	  
LP DAAC Cumulus PROD	   
CMR SSO APP for EDL in PROD	  
Metadata Management Tool	   
LAADS Web	     
NASA GESDISC DATA ARCHIVE	    
Earthdata Search PROD (Serverless)   

MODIS 'MOD09GQ' Product used and can be found at: https://search.earthdata.nasa.gov/search/granules?p=C2343115666-LPCLOUD&pg[0][v]=f&pg[0][gsk]=-start_date&q=MOD09GQ&tl=1721824457!3!!   

Can use date filters on left hand side and spatial coordinate search on left to locate desired datasets. These again required manually downloaded. Certain sites can be downloaded using same dataset due to geographical proximity, see below.    

CH_CHA, IT_NOE, DE_SFN    
US_WHS, US_WKG, US_SRM    
US_WCR, US_PFA    

##### Naming Conventions 
Datasets are downloaded as per site per day with following naming.    
MOD09GQ.{Date}.{Site}.NIR.hdf   

Example For AU_CPR 30/12/2014:    
MOD09GQ.20141230.AUCpr.NIR.hdf

### OCO-2 CO2
OCO-2 CO2 Data can be found at:    
https://search.earthdata.nasa.gov/search/granules?p=C2716248872-GES_DISC&pg[0][v]=f&pg[0][gsk]=-start_date&g=G2717666226-GES_DISC&fpj=OCO-2&tl=1721910511!3!!&zoom=0   
These files are daily therefore only need to be downloaded once per day for all sites, can use date filters on left to find correct files.    

#### Naming conventions 
oco2_LtCO2_{Date}.nc4    

Example For 30/12/2014:  
oco2_LtCO2_20141230.nc4    

### OCO-2 SIF
OCO-2 SIF Data can be found at:    
https://search.earthdata.nasa.gov/search/granules?p=C2248652649-GES_DISC&pg[0][v]=f&pg[0][gsk]=-start_date&q=SIF&fpj=OCO-2&tl=1721910511!3!!&zoom=0
These files are daily therefore only need to be downloaded once per day for all sites, can use date filters on left to find correct files.    

#### Naming conventions 
oco2_LtSIF_{Date}.nc4    

Example For 30/12/2014:  
oco2_LtSIF_20141230.nc4  

## All packages used:
pandas 
numpy 
seaborn  
matplotlib.pyplot 
datetime   
time   
random   

pyhdf    
pygrib     
netCDF4    
xarray    
pykrige    

networkx as nx    
scipy    
sklearn     

matplotlib.colors     
cartopy      
  
xgboost     
lightgbm        

itertools    
math    
shap    

# Running 
Once all above downloading is completed successfully, which does take time! Follow the order shown above, ensure FluxNet Sampling is first and Thesis Report is last.     
Please experiment with different sites and different timeframes, my timeframe is short due to tge timeliness of submission, and using different datasets for feature combination identification and then modelling is sub-optimal but required under this scenario. If you find anything interesting please report it back to me!   
