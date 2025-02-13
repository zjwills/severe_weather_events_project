# ABOUT THE PROJECT
This is a personal project to analyze severe weather events in the U.S. The data comes from the National Oceanographic and Atmospheric Administration (NOAA) collects annual data on severe weather events, including date, type, intensity, financial impact, and loss of human life. This data is available as CSVs with annual data going back to 1954 from the ncei.noaa.gov website: https://www.ncei.noaa.gov/pub/data/swdi/stormevents/csvfiles/.


# FINDINGS
A summary of my findings can be found here including visualizations on Tableau Public.
- Personal Projects Portfolio: https://rectangular-thief-f3c.notion.site/Portfolio-192ad11e127d80b1ad03cf25a2dbdb21?pvs=4
- Tableau Public: URL to be added



# WORKFLOW & TOOLS
### Ingestion, Organization, and Cleaning of Initial Dataset
I used Python to:
- Bulk unzip, rename, and remove unnecessary columns in 50 CSV files
- Compile individual CSVs into batches under 100mbs (BigQuery file import limit is 100mbs)

Libraries & modules used:
- pandas
- os
- glob
- shutil


### Exploration and Analysis
I used BigQuery as my data warehouse to:
- Create a single table with data from 1975 to 2024: ~1.9M rows.
- Final cleaning and transformation
- Analysis


### FILES
I've added Python and SQL to this repository. I have also included the 4 compiled CSV files that I imported into BigQuery.

Below, I'll outline the dataset and schema:

**Event Details File (named StormEvents_details-ftp_v1.0_d2019_c20200219.csv)**:
Where d = data year and c = creation date

**begin_yearmonth Ex: 201212 (YYYYMM format)**
The year and month that the event began

**begin_day Ex: 31 (DD format)**
The day of the month that the event began

**begin_time Ex: 2359 (hhmm format)**
The time of day that the event began

**end_yearmonth Ex: Ex: 201301 (YYYYMM format)**
The year and month that the event ended

**end_day Ex: 01 (DD format)**
The day of the month that the event ended

**end_time Ex: 0001 (hhmm format)**
The time of day that the event ended

**episode_id Ex: 61280, 62777, 63250**
ID assigned by NWS to denote the storm episode; Episodes may contain multiple Events.
The occurrence of storms and other significant weather phenomena having sufficient intensity
to cause loss of life, injuries, significant property damage, and/or disruption to commerce.

**event_id Ex: 383097, 374427, 364175**
ID assigned by NWS for each individual storm event contained within a storm episode; links
the record with the same event in the storm_event_details, storm_event_locations and
storm_event_fatalities tables (Primary database key field).

**state Ex: GEORGIA, WYOMING, COLORADO**
The state name where the event occurred (no State ID’s are included here; State Name is
spelled out in ALL CAPS).

**year Ex: 2000, 2006, 2012**
The four digit year for the event in this record.

**month_name Ex: January, February, March**
The name of the month for the event in this record (spelled out; not abbreviated).

**event_type Ex: Hail, Thunderstorm Wind, Snow, Ice (spelled out; not abbreviated)**
The only events permitted in Storm Data are listed in Table 1 of Section 2.1.1 of NWS Directive
10-1605 at http://www.nws.noaa.gov/directives/sym/pd01016005curr.pdf.
The chosen event name should be the one that most accurately describes the meteorological
event leading to fatalities, injuries, damage, etc. However, significant events, such as tornadoes,
having no impact or causing no damage, should also be included in Storm Data.

**begin_date_time Ex: 04/1/2012 20:48:00**
MM/DD/YYYY hh:mm:ss (24 hour time usually in LST)

**end_date_time Ex: 04/1/2012 21:03:00**
MM/DD/YYYY hh:mm:ss (24 hour time usually in LST)

**injuries_direct Ex: 1, 0, 56**
The number of injuries directly caused by the weather event.

**injuries_indirect Ex: 0, 15, 87**
The number of injuries indirectly caused by the weather event.

**deaths_direct Ex: 0, 45, 23**
The number of deaths directly caused by the weather event.

**deaths_indirect Ex: 0, 4, 6**
The number of deaths indirectly caused by the weather event.

**damage_property Ex: 10.00K, 0.00K, 10.00M**
The estimated amount of damage to property incurred by the weather event (e.g. 10.00K =
$10,000; 10.00M = $10,000,000)

**damage_crops Ex: 0.00K, 500.00K, 15.00M**
The estimated amount of damage to crops incurred by the weather event (e.g. 10.00K =
$10,000; 10.00M = $10,000,000).

**magnitude Ex: 0.75, 60, 0.88, 2.75**
The measured extent of the magnitude type ~ only used for wind speeds (in knots) and hail size
(in inches to the hundredth).

**magnitude_type Ex: EG, MS, MG, ES**
EG = Wind Estimated Gust; ES = Estimated Sustained Wind; MS = Measured Sustained Wind;
MG = Measured Wind Gust (no magnitude is included for instances of hail).

**tor_f_scale Ex: EF0, EF1, EF2, EF3, EF4, EF5**
Enhanced Fujita Scale describes the strength of the tornado based on the amount and type of
damage caused by the tornado. The F-scale of damage will vary in the destruction area;
therefore, the highest value of the F-scale is recorded for each event.
  EF0 – Light Damage (40 – 72 mph)
  EF1 – Moderate Damage (73 – 112 mph)
  EF2 – Significant damage (113 – 157 mph)
  EF3 – Severe Damage (158 – 206 mph)
  EF4 – Devastating Damage (207 – 260 mph)
  EF5 – Incredible Damage (261 – 318 mph)

**tor_length Ex: 0.66, 1.05, 0.48**
Length of the tornado or tornado segment while on the ground (in miles to the tenth).

**tor_width Ex: 25, 50, 1760, 10**
Width of the tornado or tornado segment while on the ground (in whole yards).

**damage_property_values**
A calculated field based on damage_property where number-string combinations (i.e., '100k') were converted to integers (i.e., '100,000').

**damage_crops_values**
A calculated field based on damage_crops where number-string combinations (i.e., '100k') were converted to integers (i.e., '100,000').



