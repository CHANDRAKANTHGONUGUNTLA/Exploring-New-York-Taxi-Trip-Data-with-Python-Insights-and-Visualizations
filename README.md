# Big Data Inspection: NYC Taxi Trips
### This repository describes a dataset that includes details about taxi travel in New York City. The 'list of attributes' utilized in the data is as follows:
* ['medallion', ' hack_license',' vendor_id',' rate_code',' store_and_fwd_flag',' pickup_datetime',' dropoff_datetime',' passenger_count',' trip_time_in_secs',' trip_distance',' pickup_longitude',' pickup_latitude',' dropoff_longitude', 'dropoff_latitude']
### These attributes give the user a lot of options for in-depth analysis and decision-making on the data.
### I thoroughly analyzed the dataset using Python. Additionally, using various modules simplified my work. Some descriptive and analytical questions are as follows:
---
### **File used: trip_data_5.csv**
### Let's load the file into memory.
#### This code imports the csv module, reads the file into memory, and stores it in the "data" list.
```python
import csv
with open('trip_data_5.csv', 'r') as file:
    csv = csv.reader(file)
    data = list(csv)
```
#### The first element of the list 'data' provides the names of the attributes. You can see it in the code below.
```python
data[0]
```
#### Output:
['medallion',
 ' hack_license',
 ' vendor_id',
 ' rate_code',
 ' store_and_fwd_flag',
 ' pickup_datetime',
 ' dropoff_datetime',
 ' passenger_count',
 ' trip_time_in_secs',
 ' trip_distance',
 ' pickup_longitude',
 ' pickup_latitude',
 ' dropoff_longitude',
 ' dropoff_latitude']

---
### 1. What datetime range does your data cover?  How many rows are there in total?
#### This code displays the dataset's first pickup date and last dropoff date.
```python
from datetime import datetime
pickup_datetime=data[1][5]
dropoff_datetime=data[-1][6]

print('pickup_date: ',pickup_datetime)
print('dropoff_date: ',dropoff_datetime)
```
#### Output:
```
pickup_date:  2013-05-01 00:04:00
dropoff_date:  2013-05-26 22:17:53
```
#### Description:
* According to the code above, we have a dataset of New York City taxis from May 5 to May 26, 2013, more specifically from 2013-05-01 00:04:00 to 2013-05-26 22:17:53.
#### This code shows the total number of rows in the dataset.
```python
total_rows=len(data)
print('Total Number of rows in dataset (including attributes): ',total_rows)
total_data=total_rows-1
print('Total Number of rows in dataset (excluding attributes): ',total_data)
```
#### Output:
```
Total Number of rows in the dataset (including attributes):  15285050
Total Number of rows in the dataset (excluding attributes):  15285049
```
#### Description:
* The dataset contains 15285050 rows in total. Attribute names are the first item in the list. Therefore, the dataset has 15285049 rows overall, excluding the initial row.
* ---
### 2. What are the field names?  Give descriptions for each field.
#### This code prints the field names
```python
print('The field names are:  ')
print(data[0])
```
#### output:
```
The field names are:
['medallion', ' hack_license', ' vendor_id', ' rate_code', ' store_and_fwd_flag', ' pickup_datetime', ' dropoff_datetime', ' passenger_count', ' trip_time_in_secs', ' trip_distance', ' pickup_longitude', ' pickup_latitude', ' dropoff_longitude', ' dropoff_latitude']
```
#### Description:
* **medallion**: It is a unique identifier for the taxi cab
* **hack_license**: A unique license ID assigned for the taxi driver
* **vendor_id**: A unique identification provided to the taxi company
* **rate_code**: The rate code for the trip (e.g. 1=standard rate; 2=JFK airport rate; 3= Newark; 4=Nassau or Westchester; 5 =Negotiated fare; 6 =Group ride .)
* **store_and_fwd_flag**: A flag indicating whether the trip data was held in vehicle memory before sending to the vendor (Y=store and forward; N=not a store and forward trip)
* **pickup_datetime**: The date and time when the passengers were picked up
* **dropoff_datetime**: The date and time when the passengers were dropped off
* **passenger_coun**: Total number of passengers onboard the vehicle for each trip (driver entered value)
* **trip_time_in_secs**: Duration of each trip in seconds
* **trip_distance**: Each trip distance in miles
* **pickup_longitude**: Longitude coordinate of the pickup location
* **pickup_latitude**: Latitude coordinate of the pickup location
* **dropoff_longitude**: Longitude coordinate of the dropoff location
* **dropoff_latitude**: Latitude coordinate of the dropoff location
---
### 3. Give some sample data for each field.
```python
sample=data[:2]
print(sample[0])
print(sample[1])
```
#### output:
```
['medallion', ' hack_license', ' vendor_id', ' rate_code', ' store_and_fwd_flag', ' pickup_datetime', ' dropoff_datetime', ' passenger_count', ' trip_time_in_secs', ' trip_distance', ' pickup_longitude', ' pickup_latitude', ' dropoff_longitude', ' dropoff_latitude']

['3B1A31779BCE30367D00C6F7911573C0', 'AED0496C937E41C4515D64E851F873AB', 'VTS', '1', '', '2013-05-01 00:04:00', '2013-05-01 00:12:00', '1', '480', '1.34', '-73.982285', '40.772816', '-73.986214', '40.758743']
```
#### Description:
The attributes and their values are printed from the sample data (first two rows).
 
 ---
### 4. What MySQL data types / len would you need to store each of the fields?
Data type and length of each field:
* **medallion**: VARCHAR(32)
* **hack_license**: VARCHAR(32)
* **vendor_id**: VARCHAR(3)
* **rate_code**: INT(2)
* **store_and_fwd_flag**: VARCHAR(1)
* **pickup_datetime**: DATETIME, format: YYYY-MM-DD HH:MM:SS
* **dropoff_datetime**: DATETIME, format: YYYY-MM-DD HH:MM:SS
* **passenger_coun**: INT(2)
* **trip_time_in_secs**: INT(6)
* **trip_distance**: FLOAT(5,2)
* **pickup_longitude**: DECIMAL(8,6)
* **pickup_latitude**: DECIMAL(8,6)
* **dropoff_longitude**: DECIMAL(8,6)
* **dropoff_latitude**: DECIMAL(8,6)
  
### 5. What is the geographic range of your data (min/max - X/Y)?
Plot this (approximately on a map)
### 6. What is the average computed trip distance? (You should use Haversine Distance)
#### created a function 'haversine' which takes longitudes and latitudes values as input and returns the Average trip distance (haversine)
```python
from math import radians, cos, sin, asin, sqrt

def haversine(lon1, lat1, lon2, lat2):
   
    # convert decimal degrees to radians 
    lon1, lat1, lon2, lat2 = map(radians, [lon1, lat1, lon2, lat2])

    # haversine formula 
    dlon = lon2 - lon1 
    dlat = lat2 - lat1 
    a = sin(dlat/2)**2 + cos(lat1) * cos(lat2) * sin(dlon/2)**2
    c = 2 * asin(sqrt(a)) 
    r = 3958.8 # Radius of earth in kilometers. Use 3958.8 for miles.
    return c * r
```
##### removing the null values from the data, using the function "haversine," and saving all of the returned values in a new list called "average_distance"
```python
average_distance=[]
for row in range(1,len(data)):
    if data[row][10]=='' or data[row][11]=='' or data[row][12]=='' or data[row][13]=='':
        continue
    else:
        ans= haversine(float(data[row][10]),float(data[row][11]),float(data[row][12]),float(data[row][13]))
        
        average_distance.append(round(ans,ndigits=2))
```
###### Plotting the graph of average computed trip distance




### 7. What are the distinct values for each field? (If applicable)
#### For iterations, I used a "for" loop in the code below. Then save each value in a list titled "unique". I estimated the 'length' of the data after removing the repetitive values using 'set'.
```python
for column in range(len(data[0])):
    unique=[]
    for row in range(1,len(data)):
        unique.append(data[row][column])
    print('Number of distinct values in {}: {} '.format(data[0][column],len(set(unique))))
    if len(set(unique))>2:
        print('The distinct values are: ',list(set(unique))[:5])
    else:
        print('The distinct values are: ',list(set(unique)))
    print()
```
#### output:
```
Number of distinct values in medallion: 13476 
The distinct values are:  ['262E47307C21E83C554CBC776F162B6C', 'AEB71B5F990A29E335C76ECBC28F9DB8', 'F55993E6D22D458A8F4A69AB38FF48CE', '962CD9C2298CA815B9C8FC831E694BB3', '9409531456308578F69EA758905CA4F9']

Number of distinct values in  hack_license: 33280 
The distinct values are:  ['B4BF30B5C1F3FCDAFF1F730CB5A775B5', 'A9E021DF57613D5EC9EBBDFA2999981A', '238067D9B875E1A4153EE18CADFDF499', 'C8DAFE07427E4EBEE671CCB0F1329607', 'B0B1A680BE3F3B581E1F5AD2137FB87D']

Number of distinct values in  vendor_id: 2 
The distinct values are:  ['VTS', 'CMT']

Number of distinct values in  rate_code: 10 
The distinct values are:  ['210', '2', '65', '5', '0']

Number of distinct values in  store_and_fwd_flag: 3 
The distinct values are:  ['', 'Y', 'N']

Number of distinct values in  pickup_datetime: 2342601 
The distinct values are:  ['2013-05-13 12:22:53', '2013-05-27 20:18:30', '2013-05-26 03:28:34', '2013-05-26 14:41:31', '2013-05-09 14:22:43']

Number of distinct values in  dropoff_datetime: 2345029 
The distinct values are:  ['2013-05-13 12:22:53', '2013-05-27 20:18:30', '2013-05-26 03:28:34', '2013-05-07 18:10:16', '2013-05-09 14:22:43']

Number of distinct values in  passenger_count: 7 
The distinct values are:  ['2', '5', '0', '6', '3']

Number of distinct values in  trip_time_in_secs: 6772 
The distinct values are:  ['1359', '5079', '3052', '4643', '426']

Number of distinct values in  trip_distance: 4231 
The distinct values are:  ['.47', '34.23', '18.84', '36.22', '43.39']

Number of distinct values in  pickup_longitude: 75976 
The distinct values are:  ['40.811741', '-73.926247', '40.754044', '40.766403', '40.703217']

Number of distinct values in  pickup_latitude: 82863 
The distinct values are:  ['40.615849', '40.766403', '40.641212', '40.734966', '-73.934441']

Number of distinct values in  dropoff_longitude: 103103 
The distinct values are:  ['40.85635', '', '-74.076294', '40.766403', '-74.193481']

Number of distinct values in  dropoff_latitude: 114267 
The distinct values are:  ['40.85635', '', '40.603645', '40.615849', '41.019608']
```
---
### 8. For other numeric types besides lat and lon, what are the min and max values?
#### Fields with numeric datatypes other than latitudes, longitudes, and datetime are `rate_code`, `passenger_count`, `trip_time` and `trip_distance`.
```python
indicies=[data[0].index(' rate_code'),data[0].index(' passenger_count'),
          data[0].index(' trip_time_in_secs'),data[0].index(' trip_distance')]
print(indicies)
```
#### output:
```
[3, 7, 8, 9]
```
##### Inserting these index numbers in the 'for' loop
```python
for index_number in indicies:
    values=[]
    for row in range(1,len(data)):
        values.append(data[row][index_number])
    print('The minimum and maximum values in "'+data[0][index_number]+'" field are:')
    print('Minimum Value: ',min(values))
    print('Maximum Value: ',max(values))
    print()
 ```
#### output:
```
The minimum and maximum values in " rate_code" field are:
Minimum Value:  0
Maximum Value:  7

The minimum and maximum values in " passenger_count" field are:
Minimum Value:  0
Maximum Value:  6

The minimum and maximum values in " trip_time_in_secs" field are:
Minimum Value:  0
Maximum Value:  9995

The minimum and maximum values in " trip_distance" field are:
Minimum Value:  .00
Maximum Value:  98.00
```
#### Description:
*  The minimum value in rate_code is 0. which is may be incorrect(error).
*From the output above, we can deduce that each journey will include a minimum of '0' passengers and a maximum of '6' passengers. Additionally, there are a few errors in the data that explain how 0 passengers might travel. 
* The results show that the trip's Minimum time is "0 sec" and its Maximum is "9995 sec = 2h 46m 35 sec." There's a risk that the driver in this case entered the distance incorrectly.
* The minimum trip distance is `0.00 miles` which again accounts for incorrectly recorded data and the maximum is `99.10 miles` .
* The minimum trip distance is `0.00 miles` and the maximum is `98 miles`
* After analyzing all the information from the result above. It is believed that trips have occasionally been canceled after being recorded. Because of this, the values for "passenger_count," "trip_time_in_secs," and "trip_distance" are all zero.
* It also makes sense that the trip_time is "2h 46m 35 sec" because that is how long it took to travel 98 miles (the maximum figure for the trip_distance).
  
  ---
### 9. Create a chart which shows the average number of passengers each hour of the day. (X axis should have 24 hours)
#### Code to calculate the average number of passengers each hour of the day:
```python
from datetime import datetime
hour_pass={}
for i in range(24):
    hour_pass[i]=[]

for row in range(1,len(data)):
    time=datetime.strptime(data[row][5],'%Y-%m-%d %H:%M:%S').hour
    hour_pass[time].append(int(data[row][7]))
    
averages=[]
for i in range(len(hour_pass)):
    mean=sum(hour_pass[i])/len(hour_pass[i])
    averages.append(mean)
```
##### In the above code, the 'pickup_datetime' data was converted from 'str' to 'datetime.hour' using the 'datetime' library.
##### I created an empty dictionary called 'hour_pass' and added keys to it that ranged from 0 to 23. Therefore, the dictionary's keys stand in for the day's 24 hours, and each key's dictionary value is a list of the number of passengers present at that hour's corresponding key.
##### Using the entries in the dictionary named "hour_pass". A list called "averages" was created by calculating the average number of people who took a taxi at each hour. 

#### Plotting the chart:
```python
import matplotlib.pyplot as plt
import seaborn as sns
sns.barplot(x=list(hour_pass.keys()),y=averages)
plt.grid(axis='y')
plt.xlabel("Hour of the Day")
plt.ylabel("Average Passenger Count")
plt.title("Average Number of Passengers by Hour")
plt.show()
```
#### output:
![image](https://github.com/CHANDRAKANTHGONUGUNTLA/Big_Data_Inspection-NYC_Taxi_Trips/assets/97879005/3a90e7a5-4d22-4153-a24c-1ad8d001647b)
#### Description:
* According to the graph, the average number of passengers progressively declined from the fourth hour and reached its lowest point at the sixth hour. Thereafter, the number of passengers gradually began to rise until the fifteenth hour, with the bar being stationary between the fifteenth and twentieth hours.
* The highest average number of passengers was seen between the hours of 0–3 and 21–23.

  ---
  ### 10. Create a new CSV file which has only one out of every thousand rows.
  


