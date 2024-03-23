
<p align="center"><strong><h1>NYC Taxi Trip Data</h1></strong></p>

## This project is completely done and analyzed in Python language. I also analyzed the same NYC taxi Data using Tableau. Here is the link to the repository of the Tableau project
- https://github.com/CHANDRAKANTHGONUGUNTLA/NYC-Taxi-Insights-Visualizing-Tableau-Dashboard
## For a detailed overview of the Python analysis and code implementation, please explore the contents of this repository.

#### This repository describes a dataset that includes details about taxi travel in New York City. The 'list of attributes' utilized in the data is as follows:
* ['medallion', ' hack_license',' vendor_id',' rate_code',' store_and_fwd_flag',' pickup_datetime',' dropoff_datetime',' passenger_count',' trip_time_in_secs',' trip_distance',' pickup_longitude',' pickup_latitude',' dropoff_longitude', 'dropoff_latitude']
#### These attributes give the user a lot of options for in-depth analysis and decision-making on the data.
#### I thoroughly analyzed the dataset using Python. Additionally, using various modules simplified my work. Some descriptive and analytical questions are as follows:
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
 ---
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
---  
### 5. What is the geographic range of your data (min/max - X/Y)?
Plot this (approximately on a map)
```python
import csv
pickup_lat_min = 90
pickup_lat_max = -90
pickup_long_min = 180
pickup_long_max = -180
dropoff_lat_min = 90
dropoff_lat_max = -90
dropoff_long_min = 180
dropoff_long_max = -180
n = 0
with open('trip_data_5.csv', 'r') as file:
    r = csv.DictReader(file)
    for row in r:
        if n > 0:
            try:
                pickup_lat = float(row[' pickup_latitude'])
                pickup_long = float(row[' pickup_longitude'])
                dropoff_lat = float(row[' dropoff_latitude'])
                dropoff_long = float(row[' dropoff_longitude'])
                if (-74.4 <= pickup_long <= -72.05 and 40.4 <= pickup_lat<= 41.02):
                    pickup_lat_min = min(pickup_lat_min, pickup_lat)
                    pickup_lat_max = max(pickup_lat_max, pickup_lat)
                    pickup_long_min = min(pickup_long_min, pickup_long)
                    pickup_long_max = max(pickup_long_max, pickup_long)
                if dropoff_long is not None and (-74.5 <= dropoff_long <= -72.02 and 40.75 <= dropoff_lat<= 41):
                    dropoff_lat_min = min(dropoff_lat_min, dropoff_lat)
                    dropoff_lat_max = max(dropoff_lat_max, dropoff_lat)
                    dropoff_long_min = min(dropoff_long_min, dropoff_long)
                    dropoff_long_max = max(dropoff_long_max, dropoff_long)
            except ValueError:
                continue
        n+=1
        if n > 1000000000:
            break
print("pickup_latitude_min: " ,pickup_lat_min)
print("pickup_longitude_min: ",pickup_long_min)

print("pickup_latitude_max: ",pickup_lat_max)
print("pickup_longitude_max: ",pickup_long_max)

print("dropoff_latitude_min: ",dropoff_lat_min)
print("dropoff_longitude_min: ",dropoff_long_min)

print("dropoff_latitude_max: ",dropoff_lat_max)
print("dropoff_longitude_max: ",dropoff_long_max)
```
#### output:
```
pickup_latitude_min:  40.400002
pickup_longitude_min:  -74.399635
pickup_latitude_max:  41.019974
pickup_longitude_max:  -72.199997
dropoff_latitude_min:  40.75
dropoff_longitude_min:  -74.491753
dropoff_latitude_max:  40.99995
dropoff_longitude_max:  -72.066666
```

#### Geographical Map:
<img width="571" alt="image" src="https://github.com/CHANDRAKANTHGONUGUNTLA/Big_Data_Inspection-NYC_Taxi_Trips/assets/97879005/ba2e4309-1441-4732-961f-12a14f70e29e">

#### Description:
* The longitude and latitude section of the dataset has a lot of irrelevant data (Null values and Zeros) that is confusing the decision.
* Since it is nearly impossible for a taxi to travel to those coordinates for pickup or drop-off due to the vast disparity in the coordinates, we can assume and fix the maximum radius of New York City.


---
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
###### The data has a lot of outliers that can easily fool the graph. So, initially, using the statistical procedure known as "INTER QUARTILE RANGE," remove the outliers.
```python
average_distance=sorted(average_distance)

mean=sum(average_distance)/len(average_distance)

median=average_distance[int(len(average_distance)/2)]

Q1=average_distance[len(average_distance)//4]

Q3=average_distance[(len(average_distance)//4)*3]

IQR= Q3-Q1

lower = Q1-1.5*IQR
higher= Q3+1.5*IQR
````
```python
cleaned_data=[]
for value in average_distance:
    if value <= higher or value >= lower:
        cleaned_data.append(value)
```
#### Description:
* Calculate the average distance's mean and median first, followed by the values for Quartile 1 and Quartile 3.
* IQR = Quartile 3 - Quartile 1
* Determined the data's lowest and upper limits. And then I eliminated the outliers from the list of "average_distance" values.
* The code is not supported by my PC and takes a long time to run. I therefore constructed a new list and added the data that is within the boundary as an alternative to this procedure.
* There are no outliers (data) below the lower limits.
* The length of the data decreased from 15285010 to 13964486.
##### Plotting the graph of average computed trip distance in two ways
###### I) Using the matplotlib library
```python
import matplotlib.pyplot as plt

num_bins = 50  # Experimenting with this value
plt.hist(cleaned_data, bins=num_bins, edgecolor='k', range=(0.0, 5.02))
plt.xlabel('Average Distance')
plt.ylabel('Frequency')
plt.title('Histogram Plot')
plt.show()
```
#### output:
![image](https://github.com/CHANDRAKANTHGONUGUNTLA/Big_Data_Inspection-NYC_Taxi_Trips/assets/97879005/7d6334ea-3636-4c2c-9e41-bdde8357e2a8)

######  II) Using the Seaborn library 
```python
import seaborn as sns

sns.histplot(data=cleaned_data)
plt.xlabel('Average Distance')
plt.ylabel('Frequency')
plt.title('Histogram Plot')
plt.show()
```
#### output:
![image](https://github.com/CHANDRAKANTHGONUGUNTLA/Big_Data_Inspection-NYC_Taxi_Trips/assets/97879005/19d5a88f-cf9b-484c-a378-48bd4488bada)
#### Description:
* In the first chart, the range and bins were specified. Consequently, only 5.02 is possible for the x values. There are no bins or ranges in the second graph.
* After viewing the two charts, we may conclude that the majority of the data falls between 0 and 1.
* The data is right skewed
---
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
```python
  import csv
file ='trip_data_5.csv'
f = open(file,'r')
reader = csv.reader(f)
with open('subset_data.csv','w') as f1:
    f1.write('')

with open('subset_data.csv','a') as f1:
    writer = csv.writer(f1, delimiter=',', lineterminator='\n')
    header = next(reader)
    writer.writerow(header)
    for i,row in enumerate(reader):
        if i > 0:
            i += 1
            if i % 1000 == 0:
                writer.writerow(row)
        if i > 1000000000:
            break
```
#### output:
* * With every 1000 rows, we receive a subset of the original dataset. The subset includes 15286 rows total, including the header, while the original data set has 15285050 rows.

---
### 11. Repeat step 9 with the reduced dataset and compare the two charts.
##### Finding the length of the subset_data:
```python
with open('subset_data.csv', 'r') as file:
    csv = csv.reader(file)
    subset_data = list(csv)

print(len(subset_data))
```
#### output:
```
15286
```
###### Repeating step 9 with the subset data.
```python
hour_pass={}
for i in range(24):
    hour_pass[i]=[]

for row in range(1,len(subset_data)):
    time=datetime.strptime(subset_data[row][5],'%Y-%m-%d %H:%M:%S').hour
    hour_pass[time].append(int(subset_data[row][7]))
    
averages=[]
for i in range(len(hour_pass)):
    mean=sum(hour_pass[i])/len(hour_pass[i])
    averages.append(mean)
```
```python
sns.barplot(x=list(hour_pass.keys()),y=averages)
plt.grid(axis='y')
plt.xlabel("Hour of the Day")
plt.ylabel("Average Passenger Count")
plt.title("Average Number of Passengers by Hour (subset data)")
plt.show()
```
#### output:
![image](https://github.com/CHANDRAKANTHGONUGUNTLA/Big_Data_Inspection-NYC_Taxi_Trips/assets/97879005/9126853e-7b35-4b7d-b839-1b852c869fc5)

#### Description:
* The average number of passengers at [1, 3, 4] hours is larger when compared to the total data. due to a lack of sufficient data.
* We can observe that this chart is not as consistent as the one before it because the bar does not precisely increase or shrink consistently at every hourly interval.
* The value of "average" is influenced by the volume of data.
* The passenger count is higher at [1, 3, 4] subset data, while the overall data is balanced.
* The maximum is in the 4th hour, but after that, there is a sharp fall in the average in both figures.
