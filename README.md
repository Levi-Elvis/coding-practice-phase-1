 
 Student name: Elvis Oduor
* Student pace:  part time 
* Scheduled project review date/time: 
* Instructor name: Sameul G. Mwangi
* Blog post URL: https://public.tableau.com/app/profile/elvis.oduor/viz/Project1_17459477359280/Dashboard2?publish=yes


# Phase 1 Project Description

For this project, you will use data cleaning, imputation, analysis, and visualization to generate insights for a business stakeholder.

## Project Overview

For this project, you will use data cleaning, imputation, analysis, and visualization to generate insights for a business stakeholder.

### Business Problem

Your company is expanding in to new industries to diversify its portfolio. Specifically, they are interested in purchasing and operating airplanes for commercial and private enterprises, but do not know anything about the potential risks of aircraft. You are charged with determining which aircraft are the lowest risk for the company to start this new business endeavor. You must then translate your findings into actionable insights that the head of the new aviation division can use to help decide which aircraft to purchase.

### The Data
1. Data Cleaning with pandas

-Importing pandas as pd helps to improve the code readerbility in complex data analysis scripts
-It also helps in avoiding potential conflicts with built-in Python functions or functions from other libraries that might have the same name as pandas functions

import pandas as pd

2. Loading the data into the coding environment. 
The data has to be in a readerbale format i.e cvs
We use the following formula to load the data then run the it 

df = pd.read_csv('data\AviationData.csv', encoding='latin1')

3. Then we inspecting the the data to show first few rows , data types and missing values (Nan), statistics summary
The following code is used to do the inspection of the data
print(df.head()) # Show first few rows
print(df.info())  # Data types and missing values
print(df.describe())  # Statistics summary    

These would give the following outcome
Event.Id Investigation.Type Accident.Number  Event.Date  \
0  20001218X45444           Accident      SEA87LA080  1948-10-24   
1  20001218X45447           Accident      LAX94LA336  1962-07-19   
2  20061025X01555           Accident      NYC07LA005  1974-08-30   
3  20001218X45448           Accident      LAX96LA321  1977-06-19   
4  20041105X01764           Accident      CHI79FA064  1979-08-02   

          Location        Country Latitude Longitude Airport.Code  \
0  MOOSE CREEK, ID  United States      NaN       NaN          NaN   
1   BRIDGEPORT, CA  United States      NaN       NaN          NaN   
2    Saltville, VA  United States  36.9222  -81.8781          NaN   
3       EUREKA, CA  United States      NaN       NaN          NaN   
4       Canton, OH  United States      NaN       NaN          NaN   

  Airport.Name  ... Purpose.of.flight Air.carrier Total.Fatal.Injuries  \
0          NaN  ...          Personal         NaN                  2.0   
1          NaN  ...          Personal         NaN                  4.0   
2          NaN  ...          Personal         NaN                  3.0   
3          NaN  ...          Personal         NaN                  2.0   
4          NaN  ...          Personal         NaN                  1.0   

  Total.Serious.Injuries Total.Minor.Injuries Total.Uninjured  \
0                    0.0                  0.0             0.0   
1                    0.0                  0.0             0.0   
2                    NaN                  NaN             NaN   
...
25%                0.000000         0.000000  
50%                0.000000         1.000000  
75%                0.000000         2.000000  
max              380.000000       699.000000  
Output is truncated. View as a scrollable element or open in a text editor. Adjust cell output settings...

4. The next step is to Handle the Missing Data 
This is important step in data preprocessing, as it can significantly affect the quality and accuracy of the analysis we are dealing with.

a) Checking for missing values
To do this, we apply and run the following formula
df[df.isnull().any(axis=1)] # Show rows with missing values
df.info()  # Data types and missing values
print(df.isnull().sum())  # Checking for missing values

The outcome of the above yields the following:
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 88889 entries, 0 to 88888
Data columns (total 31 columns):
 #   Column                  Non-Null Count  Dtype  
---  ------                  --------------  -----  
 0   Event.Id                88889 non-null  object 
 1   Investigation.Type      88889 non-null  object 
 2   Accident.Number         88889 non-null  object 
 3   Event.Date              88889 non-null  object 
 4   Location                88837 non-null  object 
 5   Country                 88663 non-null  object 
 6   Latitude                34382 non-null  object 
 7   Longitude               34373 non-null  object 
 8   Airport.Code            50249 non-null  object 
 9   Airport.Name            52790 non-null  object 
 10  Injury.Severity         87889 non-null  object 
 11  Aircraft.damage         85695 non-null  object 
 12  Aircraft.Category       32287 non-null  object 
 13  Registration.Number     87572 non-null  object 
 14  Make                    88826 non-null  object 
 15  Model                   88797 non-null  object 
 16  Amateur.Built           88787 non-null  object 
 17  Number.of.Engines       82805 non-null  float64
 18  Engine.Type             81812 non-null  object 
 19  FAR.Description         32023 non-null  object 
...
Broad.phase.of.flight     27165
Report.Status              6381
Publication.Date          13771
dtype: int64
Output is truncated. View as a scrollable element or open in a text editor. Adjust cell output settings...


b) The next step is removing missing values
This because incomplete data can negatively affect the accuracy, reliability, and performance of data analysis models
Depending on the intended outcome and identified areas where we want to expunge, we apply specifi code aligned to the areas
For this case, our interest is to drop NaN values in Purpose of flight, Public Date, Aircraft. Category, Report. Status.
To do these, we run the following codes:

df = df.dropna(axis=0, how='all') 
df = df.dropna(axis=1, how='all')  
df = df.dropna(subset=['Purpose.of.flight'], how='any')  
df = df.dropna(subset=['Publication.Date'], how='any')  
df = df.dropna(subset=['Aircraft.Category'], how='any') 
df = df.dropna(subset=['Report.Status'], how='any')

The outcome of this would be:

Event.Id	Investigation.Type	Accident.Number	Event.Date	Location	Country	Latitude	Longitude	Airport.Code	Airport.Name	...	Purpose.of.flight	Air.carrier	Total.Fatal.Injuries	Total.Serious.Injuries	Total.Minor.Injuries	Total.Uninjured	Weather.Condition	Broad.phase.of.flight	Report.Status	Publication.Date
7	20020909X01562	Accident	SEA82DA022	1982-01-01	PULLMAN, WA	United States	NaN	NaN	NaN	BLACKBURN AG STRIP	...	Personal	NaN	0.0	0.0	0.0	2.0	VMC	Takeoff	Probable Cause	01-01-1982
8	20020909X01561	Accident	NYC82DA015	1982-01-01	EAST HANOVER, NJ	United States	NaN	NaN	N58	HANOVER	...	Business	NaN	0.0	0.0	0.0	2.0	IMC	Landing	Probable Cause	01-01-1982
12	20020917X02148	Accident	FTW82FRJ07	1982-01-02	HOMER, LA	United States	NaN	NaN	NaN	NaN	...	Personal	NaN	0.0	0.0	1.0	0.0	IMC	Cruise	Probable Cause	02-01-1983
13	20020917X02134	Accident	FTW82FRA14	1982-01-02	HEARNE, TX	United States	NaN	NaN	T72	HEARNE MUNICIPAL	...	Personal	NaN	1.0	0.0	0.0	0.0	IMC	Takeoff	Probable Cause	02-01-1983
14	20020917X02119	Accident	FTW82FPJ10	1982-01-02	CHICKASHA, OK	United States	NaN	NaN	NaN	NaN	...	Personal	NaN	1.0	0.0	0.0	0.0	IMC	Cruise	Probable Cause	02-01-1983
...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...
88639	20221011106092	Accident	CEN23LA008	2022-10-06	Iola, TX	United States	304354N	0096752W	PVT	Private	...	Personal	NaN	0.0	0.0	0.0	1.0	VMC	NaN	The pilots failure to maintain control of the...	20-12-2022
88647	20221011106098	Accident	ERA23LA014	2022-10-08	Dacula, GA	United States	034055N	0835224W	LZU	GWINNETT COUNTY - BRISCOE FLD	...	Personal	NaN	0.0	0.0	0.0	2.0	VMC	NaN	An in-flight collision with a bird while on ap...	20-12-2022
88661	20221018106153	Accident	CEN23LA015	2022-10-13	Ardmore, OK	United States	034849N	0097722W	1F0	Ardmore Downtown Executive Air	...	Personal	NaN	0.0	0.0	0.0	1.0	VMC	NaN	The pilot did not ensure adequate clearance fr...	20-12-2022
88735	20221031106231	Accident	CEN23LA023	2022-10-29	Houston, TX	United States	293620N	0095959W	EFD	ELLINGTON	...	ASHO	NaN	0.0	1.0	0.0	0.0	VMC	NaN	The pilots failure to secure the magneto swit...	20-12-2022
88767	20221109106272	Accident	CEN23LA033	2022-11-09	Bridgeport, TX	United States	331026N	0974943W	KXBP	Bridgeport Municipal	...	Personal	NaN	0.0	0.0	0.0	2.0	VMC	

