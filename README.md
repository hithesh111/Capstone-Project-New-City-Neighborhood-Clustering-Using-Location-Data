<h1>Clustering - New City Neighborhood Project</h1>

Project  as part of IBM Applied Data Science Specialization

<a href = 'https://docs.google.com/document/d/1IlCTmYZgrYKHxcLRApbsOsRRtqjjlEUqvlnT71n8_Uk/export?format=pdf'>Project Report</a> | <a href = 'https://www.linkedin.com/pulse/new-city-neighborhood-project-hithesh-kk/'>Article</a> | <a href = 'https://github.com/hithesh111/Coursera_Capstone/blob/master/Code.ipynb'>Code</a> | <a href = 'https://github.com/hithesh111/Coursera_Capstone/blob/master/Data.md'>Data</a>

<b><h2>Introduction</h2></b>

<b><h3>Background</h3></b>
Most of us have to move around to different cities for various reasons like new college, job, training, vacations etc. But we would wish to reside in a neighborhood where we get access to facilities that are important for us.

People very often find themselves in a neighborhood where their priorities are not catered easily. Traveling to far places very often for important things is not comfortable indeed. Therefore it is quite useful to know places where you get the services you want easily.

For example,a person who likes pizza and coffee would very much like to find places where they are common. But normally, it would take a lot of effort to find such places on their own. That's the motivation behind this project.

<b><h2>Business problem</h2></b>
The goal of the project is to predict and suggest the best possible neighborhoods for the user based on their priorities. The unsupervised learning method KMeans Clustering is used to learn from FourSquare location data to cluster neighborhoods based on the extent of common facilities available. I have chosen the location to be Bangalore for this project.

This is a useful feature to incorporate for online travel and hotel agencies so that they can guide their users better and provide personalized suggestions in choosing where to stay in the new city.

<h2><b>Data Description</b></h2>
<h3><b>Wikipedia page</b></h3>
Scrapped the Wikipedia Page about Zones and Neighborhoods in Bangalore. It has zone-wise tables with rows being neighborhoods in the zone and the description of the neighborhood. The description, however is not relevant to our project and the column is dropped.

<h3><b>Latitudes and Longitudes (Google)</h3></b>
Since there were only ~60 neighborhoods, googling for coordinates of these was feasible. I have entered latitudes and longitudes of these neighborhoods manually and uploaded the data.

<h3><b>FourSquare Location Data</h3></b>
And most importantly,FourSquare data was used to fetch location-based results using their Developer Portal. The API was used to get information about various venues and their details mainly.

<h2><b>Methodology</h2></b>
This is how our data looks like once we're done scraping and adding latitude and longitude columns.

![image](https://user-images.githubusercontent.com/44721008/108467889-e90f4c00-72ab-11eb-8229-9470ba072557.png)

Marking all these neighborhoods in the map.

![image](https://user-images.githubusercontent.com/44721008/108468046-270c7000-72ac-11eb-80cf-5ecdd0bced0f.png)

Then I use the FourSquare API and json_normalize function from pandas.io.json to obtain the following data from FourSquare.

![image](https://user-images.githubusercontent.com/44721008/108468109-40152100-72ac-11eb-896b-99af152ba8fe.png)

After a bit of cleaning and combining dataframes, I obtained the following dataframe of venues, neighborhoods they belong to and a few details.

![image](https://user-images.githubusercontent.com/44721008/108468175-53c08780-72ac-11eb-8f4b-3f4b7b77ac65.png)

This is then made into a onehot dataframe based on entry in 'Venue Category' column.

![image](https://user-images.githubusercontent.com/44721008/108468210-620ea380-72ac-11eb-957d-9b7051230d4a.png)

The above dataframe is grouped by Neighborhood and sum along rows for the neighborhoods are the used as onehot vectors for each neighborhood.

![image](https://user-images.githubusercontent.com/44721008/108468233-6c30a200-72ac-11eb-9dac-d1f4335d1ded.png)

The top 6 most common venues of each neighborhood can be obtained by sorting each row separately.

![image](https://user-images.githubusercontent.com/44721008/108468251-75ba0a00-72ac-11eb-8442-b41cc0f93bed.png)

Now KMeans Clustering is done on the Neighborhood venue counts dataframe. More details are provided in the results section.

The user is given a list of choices to choose their top picks for most common requirements in the neighborhood where they want to reside.

These choices are converted to a onehot vector of same size as the number of columns in the onehot encoding dataframe using data summaries of onehot neighborhood dataframe.

Here’s how the onehot vector for the user’s requirement is created. The non zero values are mean-std, mean-std/2, mean+std/2, mean+std according to the priorities set by the user where mean and std are the mean and standard deviation of the series of maximum number among all venues for a particular neighborhood.

![image](https://user-images.githubusercontent.com/44721008/108468288-84a0bc80-72ac-11eb-91cb-c381a3f624c7.png)

Now KMeans Clustering is used to predict a cluster for this vector. The predicted cluster is printed and all the neighborhoods in the cluster and their most common venues are shown.

Also, to easily navigate and find the most suited neighborhood within the cluster, the user is also shown the top neighborhoods offering his 1st priority service, top neighborhoods offering his 2nd priority service, top neighborhoods offering his 3rd priority service and top neighborhoods offering his 4th priority service to note down places of special interest(if any).

<b><h2>Results</h2></b>

The yellowbrick library was used to plot the Elbow graph to find the optimal number of clusters for the data. It was found to be 4 as shown below.

![image](https://user-images.githubusercontent.com/44721008/108468357-a306b800-72ac-11eb-9cc8-6e04fd0659a6.png)

Visualizing clustering using onehot neighborhood data is not possible as it is of very high dimensions. Principal Component Analysis helps us to bring it down to 2 dimensions without much loss of information as you can see here in the scatterplot below.

The colors indicate the 4 clusters and the x indicates the cluster centroids.

![image](https://user-images.githubusercontent.com/44721008/108468405-b4e85b00-72ac-11eb-8bf1-7dd5aa686237.png)
The neighborhoods, when marked on the map in different colors according to their clusters are as follows.

Note: The clusters are not of the same color in the scatterplot above and the map below.

![image](https://user-images.githubusercontent.com/44721008/108468685-1e686980-72ad-11eb-9585-e4dd2a8aff0d.png)

Given below is the onehot vector made for the user’s requirements (Pizza Place , Breakfast Spot , Kerala Restaurant , ATM in the same order, in this specific example)

![image](https://user-images.githubusercontent.com/44721008/108468767-3d66fb80-72ad-11eb-8d79-4f42469296a9.png)

The results below are additional information given to the user to find places of special interest within the cluster assigned.

![image](https://user-images.githubusercontent.com/44721008/108468830-54a5e900-72ad-11eb-9fb4-eade503eb869.png)

![image](https://user-images.githubusercontent.com/44721008/108468879-638c9b80-72ad-11eb-92d8-b279ed60613d.png)

![image](https://user-images.githubusercontent.com/44721008/108468909-6edfc700-72ad-11eb-8b53-d034b04bdf84.png)

![image](https://user-images.githubusercontent.com/44721008/108468949-7a32f280-72ad-11eb-8cff-ccf7cfc84444.png)

This is the image of the dataframe of all neighborhoods with cluster that the user is assigned (Cluster 2). By using the above information too, the user will be able to make a good choice about his new neighborhood. (Actually has more rows than shown in this image.)

![image](https://user-images.githubusercontent.com/44721008/108468992-89b23b80-72ad-11eb-9045-ad53d8ec092f.png)

<h2><b>Discussion</h2></b>
I observed while testing the code that if the first and second preferences of the user are not very common in general, the cluster assigned is almost always the most generic one (the one with the most neighborhoods). Therefore I have given limited choices to choose the first preference (10) and second preference (20). 

The get_nearby_venues which creates a dataframe of all venues in a neighborhood sometimes shows a strange error. However, it usually works if I re-run the code.
