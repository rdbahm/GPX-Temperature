GPX Temperature Data Synthesizer
================================

Imports a GPX file, parses out the time and location of each point, and retrieves weather data for that spot at that time, then re-exports to GPX.
Methodology:
------------
1. Import GPX file.
    i. Check that this is a valid GPX and contains location and time data.
    ii. Check if the file already has temperature data.
    iii. Check if file is within history window of weather provider.
3. Cluster location data by time (?) and location.
    -This could be done in a few ways. It may be smart to allow the user to select the clustering algorithm based on the number of API calls they have available.
    -Options include every 15 minutes, (number of XML nodes/API calls available), or a more intelligent algorithm with knowledge of how historical data is returned.
    -Note: Weather Underground returns history as one big XML file, so it should be possible to reduce requests while maintaining resolution by clustering intelligently.
4. Retrieve location info.
    -Take input from clusters and return ZIP or city name for each location.
    -Use Google Maps API?
5. Get weather data
    -For each location, request historical weather.
    -Parse returned data and associate weather with nodes.
6. Smooth weather data
    -To make data more representative of actual weather.
    -Use linear interpolation to go between points.
7. Write GPX output
    -Be sure to include all columns that were in the original file but add <temp> tag.
	
Required methods:
--------
1. GPX-IO
    i. Reads GPX file, checks validity.
    ii. Writes to data structure.
    iii. Provides resources to check if the imported file has certain columns.
    iv. Provides start and end times for data.
2. Location-Cluster
    i. Takes a set of location points and clusters them by location to the provided number of clusters.
    ii. Returns the data with an extra column for "cluster."
    iii. May need a secondary function to get the middle of each cluster.
3. Get-Historical-Weather
    i. Calls a weather backend for data and can provide interpolated data for any given time.
4. Query-WeatherUnderground
    i. Performs the API call to WeatherUnderground and provides the output in a format Get-Historical-Weather can handle.
5. Query-GoogleMaps
    i. Performs an API call to Google Maps API for Location-Cluster.
6. ???
    i. Goes through each node in the dataset and adds weather data.
