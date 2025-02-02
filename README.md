# Dark Sky Network
## DSN
The Dark Sky Network (DSN): monitoring the night sky brightness over Southern Arizona 
for the next ten years, starting in 2025. We use SQM and TESS units, labeled DSNnnn,
where nnn runs from 000 to 037 as of 3 February 2025. Several units are extant, and have
been running for up to 7 years. Their data are being incorporated in our data space. 
We started acquiring SQM and TESS units in late January 2025. As of 02 Feb 2025, 6 extant units 
form the DSN.
> [!CAUTION]
> This code is a work in progress.

# The Process
The GitHub workflow [**DSN-process_data**](https://github.com/soazcomms/soazcomms.github.io/blob/main/.github/workflows/DSN-process_data.V02.yml) runs every day at 17:00 UTC (in production mode; currently runs manually).
- ### Step 1
SQM/TESS raw data are uploaded to DSNdata/NEW. This process may be manual
(e.g. SQMs w/o internet) or automatic (we are working on this step). The files are labeled with the sensor
name, e.g. DSN001_SiteName_yy.dat where yy is the year when the data are obtained. We add SiteName to make it easy to identify each site.
* ### Step 2
**DSN-process_data** looks for data in DSNdata/NEW. If it finds data there, 
it runs [DSN_python](https://github.com/soazcomms/soazcomms.github.io/blob/main/DSN_V03.py) on each file to calculate chisquared, moonalt and LST. 
1. For each file, **DSN_python** writes a .csv file in DSNdata/INFLUX, e.g. INF-DSNnnn_SiteName_yy.csv.
2. For each file, **DSN_python** writes a .csv file with UTC, SQM, lum, chisquared, moonalt and LST to DSNdata/BOX.
These files are an archive of processed data.
* ### Step 3
The .csv format is appropriate for input to **influxDB**, which 
feeds into **Grafana** for visualization. Each .csv file is uploaded into
influxDB, and then deleted from DSNdata/INFLUX.
* ### Step 4
Once each .dat file in DSNdata/NEW is processed it is deleted. 
* ### Step 5
Each file in DSNdata/BOX is uploaded to the Box repository, in the DSNdata/ARCHIVE
folder, and is deleted from DSNdata/BOX. This is intended as a permanent archive of the processed data.
+ ### Step 6
A record of the file operations above is written to a running [LOG](https://github.com/soazcomms/soazcomms.github.io/blob/main/DSNdata/RUN_LOG).
# Visualizing data
The processed data may be visualized with 
<a href="https://soazcomms.github.io/DSNweb.v03.html" target="_blank">
  DSNweb
</a> (Use SHIFT-click to open in a new window.)
