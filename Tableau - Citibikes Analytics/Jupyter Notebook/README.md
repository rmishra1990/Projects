
### Data Merging: Merging multiple datasets - from Jun 2018 to May 2019


```python
# Import Dependencies
import glob
import pandas as pd
```


```python
# Import all the CSV files and append them to a list
citibike_df_list = []
citibike_df = {}

filepath = "../Resources/*.csv"
for file in glob.glob(filepath):
    # Read each CSV file using pandas
    citibike_df = pd.read_csv(file)
    # Append it to a list
    citibike_df_list.append(citibike_df)
```


```python
# Combine all CSV files into one dataframe
pd.set_option('display.max_rows', 10000)
citibike_data = pd.concat(citibike_df_list)
```


```python
# Display the final dataframe
citibike_data.head()
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>tripduration</th>
      <th>starttime</th>
      <th>stoptime</th>
      <th>start station id</th>
      <th>start station name</th>
      <th>start station latitude</th>
      <th>start station longitude</th>
      <th>end station id</th>
      <th>end station name</th>
      <th>end station latitude</th>
      <th>end station longitude</th>
      <th>bikeid</th>
      <th>usertype</th>
      <th>birth year</th>
      <th>gender</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>142</td>
      <td>2019-02-01 15:35:02.0820</td>
      <td>2019-02-01 15:37:24.1360</td>
      <td>3183</td>
      <td>Exchange Place</td>
      <td>40.716247</td>
      <td>-74.033459</td>
      <td>3639</td>
      <td>Harborside</td>
      <td>40.719252</td>
      <td>-74.034234</td>
      <td>29677</td>
      <td>Subscriber</td>
      <td>1963</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>223</td>
      <td>2019-02-01 17:00:46.8900</td>
      <td>2019-02-01 17:04:30.5500</td>
      <td>3183</td>
      <td>Exchange Place</td>
      <td>40.716247</td>
      <td>-74.033459</td>
      <td>3681</td>
      <td>Grand St</td>
      <td>40.715178</td>
      <td>-74.037683</td>
      <td>26234</td>
      <td>Subscriber</td>
      <td>1992</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>106</td>
      <td>2019-02-01 17:08:01.3260</td>
      <td>2019-02-01 17:09:47.4400</td>
      <td>3183</td>
      <td>Exchange Place</td>
      <td>40.716247</td>
      <td>-74.033459</td>
      <td>3184</td>
      <td>Paulus Hook</td>
      <td>40.714145</td>
      <td>-74.033552</td>
      <td>29588</td>
      <td>Subscriber</td>
      <td>1960</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>370</td>
      <td>2019-02-01 17:09:31.2100</td>
      <td>2019-02-01 17:15:41.6550</td>
      <td>3183</td>
      <td>Exchange Place</td>
      <td>40.716247</td>
      <td>-74.033459</td>
      <td>3211</td>
      <td>Newark Ave</td>
      <td>40.721525</td>
      <td>-74.046305</td>
      <td>29250</td>
      <td>Subscriber</td>
      <td>1976</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>315</td>
      <td>2019-02-01 17:19:53.2490</td>
      <td>2019-02-01 17:25:09.1400</td>
      <td>3183</td>
      <td>Exchange Place</td>
      <td>40.716247</td>
      <td>-74.033459</td>
      <td>3273</td>
      <td>Manila &amp; 1st</td>
      <td>40.721651</td>
      <td>-74.042884</td>
      <td>29586</td>
      <td>Subscriber</td>
      <td>1980</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Split the starttime and stoptime columns in two
starttime = citibike_data["starttime"].str.split(" ", n=1, expand=True)
citibike_data["startdate"] = starttime[0]
citibike_data["starttime"] = starttime[1]

stoptime = citibike_data["stoptime"].str.split(" ", n=1, expand=True)
citibike_data["stopdate"] = stoptime[0]
citibike_data["stoptime"] = stoptime[1]
```


```python
# Reorder the columns
column_order = ["tripduration", "startdate", "stopdate", "starttime", "stoptime", "start station id", "start station name", "start station latitude", "start station longitude", "end station id", "end station name", "end station latitude", "end station longitude", "bikeid", "usertype", "birth year", "gender"]
citibike_data = citibike_data.reindex(columns=column_order)
```


```python
# Display the final re-ordered dataframe
citibike_data.head()
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>tripduration</th>
      <th>startdate</th>
      <th>stopdate</th>
      <th>starttime</th>
      <th>stoptime</th>
      <th>start station id</th>
      <th>start station name</th>
      <th>start station latitude</th>
      <th>start station longitude</th>
      <th>end station id</th>
      <th>end station name</th>
      <th>end station latitude</th>
      <th>end station longitude</th>
      <th>bikeid</th>
      <th>usertype</th>
      <th>birth year</th>
      <th>gender</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>142</td>
      <td>2019-02-01</td>
      <td>2019-02-01</td>
      <td>15:35:02.0820</td>
      <td>15:37:24.1360</td>
      <td>3183</td>
      <td>Exchange Place</td>
      <td>40.716247</td>
      <td>-74.033459</td>
      <td>3639</td>
      <td>Harborside</td>
      <td>40.719252</td>
      <td>-74.034234</td>
      <td>29677</td>
      <td>Subscriber</td>
      <td>1963</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>223</td>
      <td>2019-02-01</td>
      <td>2019-02-01</td>
      <td>17:00:46.8900</td>
      <td>17:04:30.5500</td>
      <td>3183</td>
      <td>Exchange Place</td>
      <td>40.716247</td>
      <td>-74.033459</td>
      <td>3681</td>
      <td>Grand St</td>
      <td>40.715178</td>
      <td>-74.037683</td>
      <td>26234</td>
      <td>Subscriber</td>
      <td>1992</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>106</td>
      <td>2019-02-01</td>
      <td>2019-02-01</td>
      <td>17:08:01.3260</td>
      <td>17:09:47.4400</td>
      <td>3183</td>
      <td>Exchange Place</td>
      <td>40.716247</td>
      <td>-74.033459</td>
      <td>3184</td>
      <td>Paulus Hook</td>
      <td>40.714145</td>
      <td>-74.033552</td>
      <td>29588</td>
      <td>Subscriber</td>
      <td>1960</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>370</td>
      <td>2019-02-01</td>
      <td>2019-02-01</td>
      <td>17:09:31.2100</td>
      <td>17:15:41.6550</td>
      <td>3183</td>
      <td>Exchange Place</td>
      <td>40.716247</td>
      <td>-74.033459</td>
      <td>3211</td>
      <td>Newark Ave</td>
      <td>40.721525</td>
      <td>-74.046305</td>
      <td>29250</td>
      <td>Subscriber</td>
      <td>1976</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>315</td>
      <td>2019-02-01</td>
      <td>2019-02-01</td>
      <td>17:19:53.2490</td>
      <td>17:25:09.1400</td>
      <td>3183</td>
      <td>Exchange Place</td>
      <td>40.716247</td>
      <td>-74.033459</td>
      <td>3273</td>
      <td>Manila &amp; 1st</td>
      <td>40.721651</td>
      <td>-74.042884</td>
      <td>29586</td>
      <td>Subscriber</td>
      <td>1980</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Export the dataframe into CSV
citibike_data.to_csv("../Resources/Citibike/citibike_data.csv", sep=',')
```
