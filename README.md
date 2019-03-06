# CAlifornia natural and working LANDs carbon and greenhouse gas model (CALAND), diagnostics, and data pre-processing 
### Version 3

## Authors 
Alan Di Vittorio  
Maegen Simmonds  
Lawrence Berkeley National Laboratory  
  
## Funding 
The California Natural Resources Agency  
Copyright (c) 2016-2019


## Motivation
The CALAND model was developed to quantify the impacts of various suites of State-funded land use and land management strategies on landscape carbon and greenhouse gas emissions relative to a baseline scenario for the California Natural and Working Lands Implementation Plan. 

## R Files
These are the five R files in the caland/ directory, which contain scripts (commands) for you to do the following:  

1. **write\_caland\_inputs.r**: Contains the `write_caland_inputs` function, which writes the detailed input files (carbon\_input.xls and xls files for each scenario) that you need to run the `CALAND` function. `write_caland_inputs` will create the input files by reformatting and merging a suite of data files found in the caland\raw directory. These data files define the management scenarios, initial land category areas and carbon densities; annual area changes; ecosystem carbon fluxes; management parameters; parameters for land conversion to cultivated or developed lands; wildfire parameters; wildfire areas; and climate change scalars.  There are several options for each of these categories that you choose when running  `write_caland_inputs`.

2. **CALAND.r**: Contains the `CALAND` function, which is the carbon and greenhouse gas accounting model. It uses the input files generated by `write_caland_inputs`. One scenario file is simulated each time  `CALAND` is run, producing a single output xls file that summarizes the annual outputs for 214 variables, each in an individual Excel worksheet.  There are several options that you select when running  `CALAND`, such as which carbon values to use from the carbon inputs file (i.e. mean, mean+sd, mean-sd, min, or max) and what level of non-regeneration to assume post-wildfire.

3. **plot_caland.r**: Contains the `plot_caland` function that plots a subset of the outputs from `CALAND`. `plot_caland` generates many diagnostic output plots (pdf files) and corresoponding spreadhseets (csv files) by reading two or more `CALAND` scenario output xls files. It is designed to compare any number of scenarios to a single reference baseline scenario. 

4. **plot\_scen\_types.r**: Contains the `plot_scen_types` function, which plots land types onto a single graph for a designated scenario.  `plot_scen_types` uses the csv outputs from  `plot_caland`.  

5. **plot\_uncertainty.r**: Contains the `plot_uncertainty` function, which plots shaded uncertainty bounds around a single output variable from `plot_caland` in individual plots for each scenario as well as combined in a single plot for comparison. Three sets of  `plot_caland` outputs are required per scenario that represent the mean, lower uncertainty bounds, and upper uncertainty bounds. Both `plot_caland` and `CALAND` must be run 3 times per scenario to produce these.

These files are designed to be used in the order above. However all desired scenarios must be completed with CALAND before plotting figures. Either `plot_scen_types` or `plot_uncertainty` can be run after `plot_caland`.

## Input files
### Carbon input file 
The initial carbon state (carbon densities) and all the carbon flow parameters (fluxes and scalars) are in an xls file in the inputs/ directory. This file is created by `write_caland_inputs`, and it does not change unless the raw land carbon input file lc_params.xls is modified. The default name is **carbon\_input\_nwl.xls**.  

### Scenario input files
There are five scenario input xls files in the caland/inputs/ directory that were created using `write_caland_inputs` for the draft Natural and Working Lands Implementation Plan (Dec 2018). These scenarios incorporate RCP8.5 climate effects on carbon exchange and wildfire area. 'Default' in the filename means that the scenarios include doubled forest mortality from 2015 to 2024 to emulate recent and ongoing die-off due to insects and drought.  

1. **Historical Baseline Scenario** **NWL\_Historical\_v6\_default\_RCP85.xls** is the reference scenario used to compare the changes in management that are prescribed in the following alternative scenarios. It does not include any California State-funded management or increases in the forest fraction of urban lands. Note that this scenario was intended to run in  `CALAND` with the setting for maximum non-regeneration (i.e., forest conversion to shrubland) in forest areas burned by high-severity wildfire.  

2. **Alternative A Scenario**  
**NWL\_Alt\_A\_v6\_default\_RCP85.xls**: adds desired, *low levels* of California State-funded management to the Historical Baseline Scenario. Note that agricultural management is limited in scope, and only represents California Natural Resources Agency-funded areas for soil conservation in cultivated lands. Note that this scenario was intended to run in  `CALAND` with the setting for full regeneration post-wildfire to represent maximum reforestation of forest areas that would not otherwise recover fully following high-severity wildfire.  

3. **Alternative B Scenario**  
**NWL\_Alt\_B\_v6\_default\_RCP85.xls**: adds desired, *high levels* of California State-funded management to the Historical Baseline Scenario. Note that agricultural management is limited in scope, and only represents California Natural Resources Agency-funded areas for soil conservation in cultivated lands. Note that this scenario was intended to run in  `CALAND` with the setting for full regeneration post-wildfire to represent maximum reforestation of forest areas that would not otherwise recover fully following high-severity wildfire.  

4. **Alternative A Scenario without Avoided Conversion**  
**NWL\_Alt\_A\_v6\_NoAC\_default\_RCP85.xls**: mimics the *low levels* of state-funded management in Alternative A, but *without avoided conversion*.

B. **Alternative B Scenario without Avoided Conversion**  
**NWL\_Alt\_B\_v6\_NoAC\_default\_RCP85.xls**: mimics the *high levels* of state-funded management in Alternative B, but *without avoided conversion*.

## Instructions for downloading R and CALAND
###Downloading R Studio 
(in progress)

### Downloading CALAND from github  
1. Sign up for a free [github account] (github.com)  

2. Go to the [CALAND github repository] (https://github.com/aldivi/caland)  

3. Under repository name, click Clone or download  

4. In the Clone with HTTPs section, click the clipboard icon to copy the clone URL for the repository

5. Open Terminal (MAC and Linux), or Git Bash (Windows) 

6. Change the current working directory to the location where you want the cloned directory to be made  

7. Type `git clone` and then paste the URL you copied in Step 4  

8. Press Enter. Your local clone will be created

## Instructions for your first test runs with CALAND

Once you have completed downloading CALAND, you will have a local copy of all the R scripts, four example input scenario files (described above), a carbon input file, and all the raw files for creating new input files, on your local computer. Now you can use the carbon and scenario input files that were already created with `write_caland_inputs` to do a test run with  `CALAND` and the three plotting functions: `plot_caland`, `plot_scen_types` , and  `plot_uncertainty`.

#### Load the functions in R
1. Open the four R files in R.  
2. Make sure that the working directory in R is caland/ (add instructions)  
3. To load the functions in each file, click the Source button in the top-right corner of the editor window or choose Edit→Source.  
4. Run `CALAND` for at least two scenarios by typing `CALAND(<settings here>). See below for instructions with settings (arguments).  
5. Make some plots using `plot_caland` and `plot_scen_types`

**Read the details below and use the examples to run the functions successfully**

(updates below this line are in progress) 

############################
# CALAND() #

# Inputs
# carbon_input_nwl.xls
#	The initial carbon density, carbon fluxes, and management/fire/conversion carbon adjustments
# <scenario_name>.xls
#	The initial land area, managed area, fire area, and annual changes to these areas; also the annual mortality rates and climate scalar effects for vegetation and soil
#	Name the file something appropriate
# These files have matching formats, including the number of preceding rows, the land types, the ownership, the management, the fire

# Outputs
# <scenario_name>_otpt_<tags>.xlsx
# "_otpt_" is appended to the input scenario name, then tags to denote which input values were used (e.g., the default <tags> = "mean")
#	Tags key:
#		sd = standard deviation; mean, max, min are self explanatory
#		D=density, A=accumulation, S=soil conservation
#		+ means add, - means substract; applied to standard deviation only
#		BC1 = black carbon treated like CO2; BC900 = black carbon segregated as GHG
#		NR# = non-regeneration (i.e., conversion to shrubland) of burned forest area greater than # meters from high severity patch edge
# output precision is to the integer (for ha and Mg C and their ratios)

# This model follows basic density (stock) and flow guidelines similar to IPCC Tier 3 protocols
# The initial year area data are used for the first year for carbon operations
# The area data for each year are operated on by the carbon adjustments for management, flux, and fire within that year
#	first eco fluxes are applied, then management, then fire
# Land conversion area changes are applied after the carbon flux operations, and conversion carbon fluxes assigned to that same year
# Each subsequent step uses the updated carbon values from the previous step
# The new carbon density and area are assigned to the beginning of next year

# Output positive flux values are land uptake; negative values are losses to the atmosphere

# all wood products are lumped together and labeled as "wood"
# wood product emissions are based on landfill decay of discarded products

# This R script is in the caland directory, which should be the working directory
#	Open R by opening this R script, or set the working the directory to caland/
# setwd("<your_path>/caland/")

# The input excel files are in caland/inputs/ (unless a sub-directory <indir> is specified as different from the default "")
# Output excel file is written to caland/outputs/ (unless a sub-directory <outdir> is specifed as different from the default "")

# CALAND() has 15 arguments:
#	scen_file_arg		name of the scenario file; assumed to be in caland/inptus/<indir>
#	c_file_arg			name of the carbon parameter input file; assumed to be in caland/inputs/<outdir>
#	indir				name only of directory in caland/inputs/ that contains scen_file and c_file; do not include "/" character at the end
#	outdir				name only of directory in caland/outputs/ that contains scen_file and c_file; do not include "/" character at the end
#	start_year		  	simulation begins at the beginning of this year
#	end_year		    simulation ends at the beginning of this year (so the simulation goes through the end of end_year - 1)
#	value_col_dens		select which carbon density values to use; 5 = min, 6 = max, 7 = mean, 8 = std dev
# 	value_col_accum 	select which carbon accumulation values to use; 5 = min, 6 = max, 7 = mean, 8 = std dev
# value_col_soilcon select which carbon accumulation values to use for cultivated soil conservation; 6 = min, 7 = max, 8 = mean, 9 = std dev
#	ADD_dens			for use with value_col_dens ==8: TRUE= add the std dev to the mean; FALSE= subtract the std dev from the mean
#	ADD_accum			for use with value_col_accum ==8: TRUE= add the std dev to the mean; FALSE= subtract the std dev from the mean
# ADD_soilcon   for use with value_col_soilcon ==9: TRUE= add the std dev to the mean; FALSE= subtract the std dev from the mean
#	NR_Dist			for adjusting the amount of non-regenerating forest after high severity fire (-1 = full regeneration, 120m is the minimum)
#	WRITE_OUT_FILE	TRUE= write the output file; FALSE= do not write the output file
# blackC      TRUE = GWP of black C is 900, FALSE = GWP of black C is 1 (default)

# notes:
# carbon calcs occur in start_year up to end_year-1
# end_year denotes the final area after the changes in end_year-1
# density values are the stats of the total pixel population within each land type id
# accumulation values are stats of literature values

# example:
# historical baseline without black carbon and with non-regeneration:
CALAND(scen_file=“NWL_Historical_v6_default_RCP85.xls”, c_file_arg = "carbon_input_nwl.xls", indir = "", outdir = "", start_year = 2010, end_year = 2101, value_col_dens = 7, ADD_dens = TRUE, value_col_accum = 7, ADD_accum = TRUE, value_col_soilcon=8, ADD_soilcon = TRUE, NR_Dist = 120, WRITE_OUT_FILE = TRUE, blackC = FALSE)
# Alternative A without black carbon and with full reforestation of non-regenerated areas:
CALAND(scen_file=“NWL_Alt_A_v6_default_RCP85.xls”, c_file_arg = "carbon_input_nwl.xls", indir = "", outdir = "", start_year = 2010, end_year = 2101, value_col_dens = 7, ADD_dens = TRUE, value_col_accum = 7, ADD_accum = TRUE, value_col_soilcon=8, ADD_soilcon = TRUE, NR_Dist = -1, WRITE_OUT_FILE = TRUE, blackC = FALSE)


#############################
# plot_caland() #

# make diagnostic plots (.pdf files) and associated text files (.csv files) of two or more CALAND scenario outputs

# this script loops over all the regions and land types
#  the ownerships will be aggregated
#  note that Seagrass has non-zero values only for all c stock == soil c stock, and cum and ann eco c gain

# get only the year columns (not the change column)

# make sure that the working directory is caland/
# This R script is in the caland directory, which should be the working directory
#	Open R by opening this R script, or set the working the directory to caland/
# setwd("<your_path>/caland/")

# the model output files for plotting are in caland/<data_dir>
# the plots are put into caland/<data_dir>/<figdir>/ within each region, land type, and ownership directory
#	where <data_dir> and <figdir> is an argument to this function

# plot_caland() has 12 arguments:
#	scen_fnames		array of scenario output file names; assumed to be in data_dir; scen_lnames is automatically determined from this by extracting text before "_output_..."
#	scen_snames		array of scenario short lables associated with scen_fnames (up to 8 character labels for bar graphs)
#	data_dir		the path to the directory containing the caland output files; do not include the "/" character at the end; default is "./outputs"
#	reg				array of regions to plot; can be any number of available regions (all are below as default)
#	lt				array of land types to plot; can be any number of available types (all are below as default)
#	own				array of ownerships to plot; can be any number of available types (only "All_own" is the default)
#	figdir			the directory within data_dir to write the figures; do not include the "/" character at the end
#	INDIVIDUAL		TRUE = output per area effects of individual practices based on model runs configured for this purpose
# units_ha         TRUE = output units in "ha", FALSE = "ac". Default is "ac".
# blackC        TRUE = black GWP equal to 900, FALSE = black GWP equal to 1 (default)
# blackC_plot   TRUE = plot BC, CO2, and CH4, FALSE = plot only CO2 and CH4 (BC added to CO2 which is only valid if black C is FALSE) (default)
# last_year			the last year to plot; this should be the final run year + 1 because cumulative and area outputs reflect previous years
#						e.g., 2050 is the final run year, but everything is defined and output to 2051, so last_year = 2051

# notes:
# need at least two scenarios for this to work
# the first scenario in the array is assumed to be the baseline
# scen_fnames, scen_lnames, and scen_snames all need to be equal length vectors
# this takes several hours for 5 scenarios
# it is faster to run four separate instances at once via the command line (assuming you have at least 4 cores)
#	this took about 2.5 hours to complete
#		three instancs each with three of the land regions
#		one instance with the Ocean and All_region regions
# When using All_region and All_land, there is only the All_own ownership avaialable in the output files
#	so it is best to not use these when plotting individual ownerships
#	otherwise you may get several outputs labelled with different ownerships, but that are identical and are the All_own outputs

#### the indivudual per area outputs are only valid when comparing:
#	a static run (only ecosystem exchange enabled, no lulcc, no practices, no fire (except when evaluating effects on fire emissions))
#	with a run with a single practice enabled
#	for any runs with restoration/reforestation/afforestation/lulcc, the land type must be "All_land" in order to capture the dynamics of lulcc and wildfire; any other land type will not give valid results
#	these diagnostics are designed to estimate effects of a practice in isolation
#		these effects change over time because most practices have effects beyond year of implementation
#	one exception is for evaluating avoided conversion cases, in which case lulcc has to be on in the baseline so that urban growth rate can be reduced in the scenario
#	one consideration is that restoration activities have secondary effects on land use/cover change related to forcing conversion of other land types and the proportion of land types burned by wildfire
#	example scenarios can be found in /caland/raw_data/individual_proposed_sims_aug2018.xls and corresponding /caland/raw_data/individual_proposed_sims_control_lulcc_wildfire_aug2018.csv

#### the output files do not include the carbon transfered into and out of a land category due to lcc
#### so the component diagnostics include only land-atmosphere c exchange
#### which means that the carbon going between land categories is not represented in these diagnostics

# available regions:
#	"All_region", "Central_Coast", "Central_Valley", "Delta", "Deserts", "Eastside", "Klamath", "North_Coast", "Sierra_Cascades", "South_Coast", "Ocean"

# available land types:
#	"All_land", "Water", "Ice", "Barren", "Sparse", "Desert", "Shrubland", "Grassland", "Savanna", "Woodland", "Forest", "Meadow", "Coastal_marsh", "Fresh_marsh", "Cultivated", "Developed_all", "Seagrass"

# available ownerhsips:
#	"All_own", "BLM", "DoD", "Easement", "Local_gov", "NPS", "Other_fed", "Private", "State_gov", "USFS_nonwild"

# example:
# plot Alternative A in reference to the historical baseline, but only out to 2051
plot_caland(scen_fnames = c(“NWL_Historical_v6_default_RCP85_output_mean_BC1_NR120.xls”, “NWL_Alt_A_v6_default_RCP85_output_mean_BC1.xls”), scen_lnames = c(“Baseline”, “Alt_A”), scen_snames = c(“BASE”, “AltA”), data_dir = "./outputs", reg = c("All_region", "Central_Coast", "Central_Valley",
"Delta", "Deserts", "Eastside", "Klamath", "North_Coast", "Sierra_Cascades", "South_Coast", "Ocean"),
lt = c("All_land", "Water", "Ice", "Barren", "Sparse", "Desert", "Shrubland", "Grassland", "Savanna", "Woodland", "Forest",
"Meadow", "Coastal_marsh", "Fresh_marsh", "Cultivated",  "Developed_all", "Seagrass"),
own = c("All_own"), figdir = "figures", INDIVIDUAL = FALSE, units_ha=FALSE, blackC = FALSE, blackC_plot = FALSE, last_year = 2051)


#############################
# plot_scen_types() #

# put a variable for specified land types within a region and ownership on the same plot, for each scenario in the diagnostic output file

# this script reads the csv files produced by plot_caland()

# make sure that the working directory is caland/
# the csv files are assumed to be in <data_dir>/<figdir>, in the appropriate land type and ownership directories
# resulting output will go directly into <data_dir>/<figdir>/<region>/<own>, which should be the same as used in plot_caland()
# setwd("<your_path>/caland/")

# plot_scen_types() has 8 arguments:
#	varname			name of variable to plot (see the outputs from plot_caland)
#						this name is between the land type and "_output" in these file names; do not include the surrounding "_" characters
#	ylabel			y label for the plot; this indicates the units and whether it is a difference from baseline
#	data_dir		the path to the directory containing the caland output files; do not include the "/" character at the end; default is "./outputs"
#	file_tag		additional tag to file name to note what regions/lts/ownerships are included; default is "" (nothing added)
#	reg				array of region names to plot (see below)
#	lt				array of land types to plot; can be any number of available types (all but Seagrass are below as default; All_land is excluded)
#	own				array of ownerships to plot; can be any number of available types (only "All_own" is the default)
#	figdir			the directory within data_dir containing the data to plot, and where to write the figures; do not include the "/" character at the end

# available regions:
#	"Central_Coast", "Central_Valley", "Delta", "Deserts", "Eastside", "Klamath", "North_Coast", "Sierra_Cascades", "South_Coast", "All_region"

# available land types:
#	"Water", "Ice", "Barren", "Sparse", "Desert", "Shrubland", "Grassland", "Savanna", "Woodland", "Forest", "Meadow", "Coastal_marsh", "Fresh_marsh", "Cultivated", "Developed_all", "Seagrass", "All_land"

# available ownerhsips:
#	"All_own", "BLM", "DoD", "Easement", "Local_gov", "NPS", "Other_fed", "Private", "State_gov", "USFS_nonwild"

# Note: plotting the ocean region doesn't provide any comparison with land types because only seagrass exists in the ocean

# Note: Seagrass has only a subset of the output files, so if including for All_region make sure that the desired varname is available
#	Seagrass is not in the default land type list

# example:
# plot the total organic c stock for the individual land types in one figure, for the Alternative A and Baseline scenarios, for all regions, and "All_own"
plot_scen_types("All_orgC_stock", "MMTC", data_dir = "./outputs", file_tag = “”, reg = c("Central_Coast", "Central_Valley", "Delta", "Deserts", "Eastside", "Klamath", "North_Coast", "Sierra_Cascades", "South_Coast", "All_region"), lt = c(Water", "Ice", "Barren", "Sparse", "Desert", "Shrubland", "Grassland", "Savanna", "Woodland", "Forest", "Meadow", "Coastal_marsh", "Fresh_marsh", "Cultivated", "Developed_all"), own = c("All_own"), figdir = "figures")


#############################
# plot_uncertainty() #

# plot the mean output with shaded uncertainty region for a given variable
# the inputs to this function are diagnostic output csv files from plot_caland()
# this script includes 2 functions: wrapper and plot_uncertainty
#	wrapper manipulates text for the plots

# this is currently set up to require 2 scenarios and a baseline

# plot_uncertainty() takes 14 arguments:

##	start_year  year to start plotting
##  end_year    year to end plotting
##  varname		name of variable to plot (see the outputs from plot_caland)
                # this name is between the land type and "_output" in these file names; do not include the surrounding "_" characters
##	ylabel		y label for the plot (assign value 1 to 4); this indicates the units and whether it is a difference from baseline. Current options:
                # 1 = "Change from Baseline (MMT CO2e)"
                # 2 = "MMT CO2e"
                # 3 = "Change from Baseline (MMT C)"
                # 4 = "MMT C"
##	data_dir		the path to the directory containing the three folders of plot_caland outputs (mean, low, and high); do not include the "/" character at 
                # the end; default is "./outputs"
##	figdirs		the three folders within data_dir containing the data to plot. These must be assigned in order of mean, low, and high. The figures will
                # be written to the folder representing the mean; do not include the "/" character at the end
                # the csv files are assumed to be in <data_dir>/<figdir>, in the appropriate region and land type and ownership directories
##  scen_labs   scenario labels; must include three, end with the reference baseline, and correspond to files assigned to scen_a, scen_b, and base arguments;
                # default = c("A", "B", "Baseline")
##  scen_a      filename corresponding to first scenario in scen_labs
##  scen_b      filename corresponding to second scenario in scen_labs
##  base        filename corresponding to third scenario in scen_labs; must be the reference baseline scenario
##	file_tag		tag to add to end of new file names (e.g., to note what time period is plotted); default is "" (nothing added)
##	reg			array of region names to plot; default = "All_region", but can be any number of available types:
                #	"All_region", "Central_Coast", "Central_Valley", "Delta", "Deserts", "Eastside", "Klamath", "North_Coast", "Sierra_Cascades", "South_Coast", 
                # Note: plotting the ocean region doesn't provide any comparison with land types because only seagrass exists in the ocean
##	lt			array of land types to plot;  default = "All_land", but can be any number of available types 
                # Note: Seagrass has only a subset of the output files, so if including for All_region make sure that the desired varname is available
##	own			array of ownerships to plot;  default = "All_own", but can be any number of available types 


#############################
# write_caland_inputs() #

# this function generates the detailed input files from specific area and carbon density and other data (see below), and more general C parameters and management scenarios
# this is necessary because there are 940 land categories, all of which have at least one record in the suite of input tables
#  and there are not input data (except for the area and carbon density data), nor management activities, for each of these categories

# using this function to create new input files is a complicated task
# all of the input data to this function have to be carefully assembled
# this is not recommended unless you have a good understanding of the required data and how the model works

# write_caland_inputs has 20 arguments
#	scen_tag:			(optional) a scenario file name tag; scen files are already named based on the tables in scenarios_file
#	c_file:				the carbon density and parameter file created for input to caland (needs to be .xls)
#	start_year:			this is the initial year of the simulation, one of the area files needs to be for this year
#	end_year:			this is the final year output from the caland simulation (matches the caland end_year argument)
#	CLIMATE:				projected climate or BAU climate; "HIST" or "PROJ"; affects wildfire and veg and soil carbon accum values;
#		the projected scenario is determined by the fire and climate input files; HIST uses RCP85 fire for historical average burn area, and sets all climate scalars to 1
# 	land_change_method: "Landcover" will use original method of remote sensing landcover change from 2001 to 2010. "Landuse_Avg_Annual" will 
#    use avg annual area change from 2010 to 2050 based on projected land use change of cultivated and developed lands (USGS data). 

#	parameter_file:		carbon accumulation and transfer parameters for the original 45 land categories and seagrass (xls file)
#	scenarios_file:		generic scenarios to be expanded to the actual scenario files (xls file with one scenario per table)
# 	units_scenario: 	specify units for input areas in scenarios_file - must be ac or ha.
#	inputs_dir:			the directory within "./inputs" to put the generated input files; the default is "" so they go into "./inputs"
#	climate_c_file:		the climate scalars for vegetation and soil carbon accumulation; future scenario must match the fire input file; needs to be a valid proj file for CLIMATE="HIST"
#	fire_area_file:		annual burned area by region-ownership; future scenario must match the climate c file; needs to be rcp85 for CLIMATE="HIST"
#	mortality_file:		mortality rate as annual fraction of above ground biomass; includes values for all woody land types that have c accumulation in vegetation
#	area_gis_files_orig:		vector of two csv file names of gis stats area by land category (sq m)
#	area_gis_files_new:		vector of one csv file name of gis stats area by land category (sq m)
#	carbon_gis_files:	vector of 13 csv file names of gis stats carbon density by land category (t C per ha)
#	forest_mort_fact:		the value by which to adjust the forest mortality during the period specified by forest_mort_adj_first/last; default is 2
#	forest_mort_adj_first:	the first year to adjust forest mortality; default is 2015
#	forest_mort_adj_last:	the last year to adjust forest mortality; default is 2024
# 	control_wildfire_lulcc:	if TRUE, read control_wildfire_lulcc_file to shut off wildfire, or lulcc, or both; default is FALSE
#	control_wildfire_lulcc_file: file with flags for each scenario in scenarios_file to control wildfire and lulcc

# NOTE:
#	The default simulation includes 2X forest mortality from 2015 through 2024 to emulate the recent and ongoing die off due to beetles and drought
#	Individual practice sims also include this doubled mortality (should check without it to find out how much difference it makes)

# parameter_file
#	8 tables
#	vegc_uptake, soilc_accum, conversion2ag_urban, forest_manage, dev_manage, grass_manage, ag_manage, wildfire
#	column headers are on row 12
#	only non-zero values are included; more land type, ownership, management, or severity rows can be added as appropriate (if the column exists)
#	this xls file is a subset of the original ca_carbon_input.xlsx file, as a starting point for filling in the new parameter tables

# scenarios_file
#	one scenario per table; can have many scenarios defined in this file
#	the table names determine the output scenario file names
#	defines the scenarios fairly concisely, as given
#	10 columns each: Region, Land_Type, Ownership, Management, start_year, end_year, start_area, end_area, start_frac, end_frac
#	either area or frac values are used; the other two columns are NA
# units can be in ha or ac, which must be scecified by the "units_scenario" argument 
#	Region and Ownership can be "All" or a specific name
#	Each record defines management for a specific time period; outside of the defined time periods the management values are zero

# climate_c_file
#	One file contains both vegetation and soil scalars
#	Three ID columns: Region, Ownership, Component; all possible Region-Ownership combos are included (81), even if they don't exist
#	Then one column for each year, starting with 2010
#	The component is either "Vegetation" or "Soil"
#	These values directly scale the vegetation and soil c accumulation values in CALAND
# 	Must be a valid projected climate file, even for CLIMATE=HIST that sets all climate scalars to 1

# fire_area_file
#	Two ID columns: Region, Ownership; all possible Region-Ownership combos are included (81), even if they don't exist
#	Then one column for each year, starting with 2010
#	The initial year fire area is an average from 2001-2015
#	The BAU fire is the initial fire area extrapolated through the sim (same value, no trend)
#		Are using the RCP85 file for the historical average; it is ~10,000 ha per year less than the RCP45 file average
#	Fire severity is estimated as fractions of the annual totals, and high sev frac increases at a historical rate
#	Non-regen area is parameterized as a function of the high severity fraction and a threshold distance from burn edge
#	Projected climate fire is directly from the area file, and is currently area only, so other aspects are estimated as for BAU fire

# mortality_file
#	Three ID columns: Region, Land_Type, Ownership
#	One data column: Mortality_frac
#	this is the annual mortality rate as a fraction of above ground biomass
#	includes values for all woody land types that have c accumulation in vegetation
#	all valid land types must be explicitly specified: Shrubland, Savanna, Woodland, Forest, Developed_all
#	region and/or ownership can be "All"

# area_gis_files (sq m)
#	7 columns
#	region code, region name, land type code, land type name, ownership code, ownership name, area
#	these two csv files contain the text labels and integer codes for Region, Land_type, and Ownership
#	the Land_cat integer codes will be calculated and then matched with the carbon_gis_files integer codes
#	the year has to be in the name of the file; currently assumes that the years are 2001 and 2010

# carbon_gis_files (t C per ha)
#	14 columns
#	land category code, label (blank), valid cell count, null cell count, min, max, range, mean, mean of absolute values, sdddev, variance, coeff_var, sum, sum_abs
#	use the min, max, mean, and stddev
#	these are the density and standard error csv files (13) for the seven carbon pools, with values by land category
#	above ground, below ground, downed dead, standing dead, litter, understory are from ARB inventory
#	soil organic c is from gSSURGO, and we assume that any zero values in the original data set are non-valid when calculating the land category averages
#		there is 12236544 ha (mostly desert) with no data that is converted to zero when the raster is created, and only 17601 ha (mostly rivers) with zeros in the gssurgo data

# control_wildfire_lulcc_file
#	flags to dtermine whether wildfire and/or lulcc will happen
#	three columns, in order: Scenario, Wildfire, LULCC
#		Scenario: matches the worksheet names in the scenarios_file; these scenario names MUST MATCH between these two files
#		Wildfire: 1=wildfire; 0=no wildfire
#		LULCC: 1=lulcc; 0=no lulcc

# mortality
# the recent and expected forest mortality due to insects and drought is emulated by a doubled forest mortality rate for 10 years (2015-2024)
# mortality applies only to woody systems with veg c accumulation
# these types are: shrubland, savanna, woodland, forest, and developed_all
# developed_all mortality is processed differently from the others
#  the morality from the scenario is transferred to the above ground harvest of the dead_removal management
#  this is because the urban system is highly managed, and allows for more control of what happens to the dead biomass

# the only land categories available throughout the sim are those that are included in the input files

# restoration
# fresh marsh comes out of only private and state land in the delta, so make sure that these are included (for now, all existing cultivate delta land categories are included as potential fresh marsh)
#	      so far, this is the only land type for which land categories need to be added (except for seagrass)
# coastal marsh comes out of only private and state land in the coastal regions, so make sure these are available, from cultivated
# meadow restoration is only in the sierra cascades, and in private, state, and usfs nonwilderness, from shrub, grass, savanna, and woodland
# woodland comes from cultivated and grassland
# afforestation is forest area expansion and comes from shrubland and grassland
# reforestation is forest area expansion that comes from shrubland only to match non-regeneration of forest

# output files
# areas are in ha (scenario files)
# carbon file:
#  densities are in Mg C per ha (t C per ha)
#  factors are fractions

# example to generate input files for NWL scenarios (for which the raw data are in acres) with RCP 8.5 climate:
write_caland_inputs <- function(scen_tag = "default", c_file = "carbon_input_nwl.xls", start_year = 2010, end_year = 2101, 
                                CLIMATE = "PROJ", parameter_file = "lc_params.xls", scenarios_file = "nwl_scenarios_v6_ac.xls",
                                units_scenario = "ac",
                                inputs_dir = "",
                                climate_c_file = "climate_c_scalars_iesm_rcp85.csv",
                                fire_area_file = "fire_area_canESM2_85_bau_2001_2100.csv",
                                mortality_file = "mortality_annual_july_2018.csv",
                                area_gis_files_new = "CALAND_Area_Changes_2010_to_2051.csv", land_change_method = "Landuse_Avg_Annual",
                                area_gis_files_orig = c("area_lab_sp9_own9_2001lt15_sqm_stats.csv", "area_lab_sp9_own9_2010lt15_sqm_stats.csv"), 
                                carbon_gis_files = c("gss_soc_tpha_sp9_own9_2010lt15_stats.csv", "lfc_agc_se_tpha_sp9_own9_2010lt15_stats.csv", 
                                                     "lfc_agc_tpha_sp9_own9_2010lt15_stats.csv", "lfc_bgc_se_tpha_sp9_own9_2010lt15_stats.csv", 
                                                     "lfc_bgc_tpha_sp9_own9_2010lt15_stats.csv", "lfc_ddc_se_tpha_sp9_own9_2010lt15_stats.csv", 
                                                     "lfc_ddc_tpha_sp9_own9_2010lt15_stats.csv", "lfc_dsc_se_tpha_sp9_own9_2010lt15_stats.csv", 
                                                     "lfc_dsc_tpha_sp9_own9_2010lt15_stats.csv", "lfc_ltc_se_tpha_sp9_own9_2010lt15_stats.csv", 
                                                     "lfc_ltc_tpha_sp9_own9_2010lt15_stats.csv", "lfc_usc_se_tpha_sp9_own9_2010lt15_stats.csv", 
                                                     "lfc_usc_tpha_sp9_own9_2010lt15_stats.csv"),
								forest_mort_fact = 2,
								forest_mort_adj_first = 2015,
								forest_mort_adj_last = 2024,
								control_wildfire_lulcc = FALSE,
								control_wildfire_lulcc_file = "individual_proposed_sims_control_lulcc_wildfire_aug2018.csv")
