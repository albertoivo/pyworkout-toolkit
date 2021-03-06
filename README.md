# pyworkout-toolkit
pyworkout-toolkit: Python tools to process workout data and telemetry

[![Build Status](https://dev.azure.com/triskadecaepyon/pyworkout/_apis/build/status/triskadecaepyon.pyworkout-toolkit?branchName=master)](https://dev.azure.com/triskadecaepyon/pyworkout/_build/latest?definitionId=1&branchName=master)
[![noarch](https://img.shields.io/circleci/project/github/conda-forge/pyworkout-toolkit-feedstock/master.svg?label=noarch)](https://circleci.com/gh/conda-forge/pyworkout-toolkit-feedstock)
[![Conda Version](https://img.shields.io/conda/vn/conda-forge/pyworkout-toolkit.svg)](https://anaconda.org/conda-forge/pyworkout-toolkit) [![Conda Platforms](https://img.shields.io/conda/pn/conda-forge/pyworkout-toolkit.svg)](https://anaconda.org/conda-forge/pyworkout-toolkit)
## Summary
The pyworkout-toolkit is a Python package that provides tools for post-workout analysis of data or telemetry.  The majority of the tools cater to coaches and invidividuals who wish to utilize the data to generate metrics or exercise machine learning/data mining.  The toolkit provides parsing of the popular .TCX and .GPX formats, along with some general purpose functions that help preprocess the data for metrics, visualization, or machine learning.  

## Features
- Parsing of .TCX files
- Caters to the Pandas DataFrame for analysis flexibility and use in Scikit-Learn and other frameworks
- Helper functions to correct sport-specific errors in recording
- Handling of missing data
- Exporting to popular formats such as CSV and HDF5 via Pandas
### Planned Features
- Parsing of .GPX files
- Conversion/correction of GPS units

## Documentation
- [Documentation](https://pyworkout-toolkit.readthedocs.io/en/latest/index.html) is hosted by readthedocs

## Examples
With pyworkout-toolkit, parsing of TCX files is simple:
```
from pyworkout.parsers import tcxtools
workout_data = tcxtools.TCXPandas('pyworkout/tests/data/test_dataset_1.tcx') # Create the Class Object
workout_data.parse() # Returns a dataframe
```
Other details about the TCX file can be found as well:
```
workout_data.get_sport()
'Biking'

workout_data.get_workout_startime()
'2016-10-20T22:01:26.000Z'
```
If opening multiple TCX files for large-scale reporting, it is recommended that [Dask and Dask Delayed](https://dask.pydata.org/en/latest/) be used:
```
import dask.dataframe as dd
from dask import delayed

tcx1 = delayed(tcxtools.TCXPandas('workout_1.tcx').parse()) # Delay these calculations
tcx2 = delayed(tcxtools.TCXPandas('workout_2.tcx').parse()) # Use as many as needed

total = dd.from_delayed([tc1, tc2]) # However many files you need
total.visualize() # Visualize the task graph
total.compute() # Compute it
# This returns a dataframe with all the files
```


## Getting your data
In order to get your data in TCX format, you will need to export the files from the given service.
- Instructions for [Strava](https://support.strava.com/hc/en-us/articles/216918437-Exporting-your-Data-and-Bulk-Export)
- Instructions for [Garmin](https://support.garmin.com/en-US/?faq=W1TvTPW8JZ6LfJSfK512Q8&searchType=noProduct)

Please note that the TCX format will make certain workouts absent of important metadata, such as those used to identify swim workouts.  In these cases, specifying workout type upon class instantiation is recommended.  

In addition to this, the new Run Dynamics data which debuted a few years ago is not exported via TCX format.  You must have a _relatively expensive_ [Garmin Connect API License](http://developer.garmin.com/garmin-connect-api/overview/) for this.  A bit more info on this is viewable on [stackoverflow](https://stackoverflow.com/questions/35082776/how-to-retrieve-activities-from-garmin-fenix3).  It is unknown whether Suunto's [MovesCount](http://www.movescount.com) allows for this data export.  More info on exporting data from MovesCount [here](https://github.com/cpfair/tapiriik/issues/68) and [here](https://github.com/openambitproject/openambit)

## Dependencies
- NumPy
- Pandas
- lxml
- Python 3 (developed originally on 3.5.2)

## Installation
Local installation is supported, with pip and conda-build files included.  Currently available on pip and conda.

### pip installation:
```
pip install pyworkout-toolkit
```
### conda installation:

pyworkout-toolkit has recently been added to conda-forge.  Use the command below to install:
```
conda install pyworkout-toolkit
```
If one's conda installation needs the conda-forge channel, use the following command:
```
conda config --add channels conda-forge
```


## License
BSD

## Scope and goals
The pyworkout-toolkit aims to assist in the furthering of research in the health/wearables area by providing the tools necessary to process, correct, and analyze collected data.  The project was created to fill the gap between data aquisition on the device to the end-developer, allowing for algorithm creation, data mining, and visualization once the data has been converted.  Eventual integration with graphing libraries have been planned, with Matplotlib, Bokeh, and Datashader on the list.
