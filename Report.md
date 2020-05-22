<h1>New City Neighborhood Project </h1>
<h2>Background</h2>
Most of us have to move around to different cities for various reasons like new college, job, training, vacations etc. But we would wish to reside in a neighborhood where we get access to facilities that are important for us.<br><br> People very often find themselves in a neighborhood where their priorities are not catered easily. Traveling to far places very often for important things is not comfortable indeed. Therefore it is quite useful to know places where you get the services you want easily.<br><br>
For example,a person who likes pizza and coffee would very much like to find places where they are common. But normally, it would take a lot of effort to find such places on their own. That's the motivation behind this project.
<h2>Introduction/Business Problem</h2>
The goal of the project is to predict and suggest the best possible neighborhoods for the user based on their priorities. The unsupervised learning method KMeans Clustering is used to learn from FourSquare location data to cluster neighborhoods based on the extent of common facilities available. I have chosen the location to be Bangalore for this project.<br><br>
This is a useful feature to incorporate for online travel and hotel agencies so that they can guide their users better and provide personalised suggestions in choosing where to stay in the new city.<br><br>
For example, the user wants few good Southern restaurants and also wants a gym/fitness centre near his residence. He enters these two and two more priorities online and gets the result as a cluster of neighborhoods and their most common venues which suit his interest.

<h2>Obtaining and Cleaning Data</h2>

<h3>Wikipedia page</h3>
Scrapped the <a href = 'https://en.wikipedia.org/wiki/List_of_neighbourhoods_in_Bangalore'> Wikipedia Page </a> about Zones and Neighborhoods in Bangalore. It has zone-wise tables with rows being neighborhoods in the zone and the description of the neighborhood. The description, however is not relevant to our project and the column is dropped.

<h3>Latitudes and Longitudes (Google)</h3>
Since there were only ~60 neighborhoods, googling for coordinates of these was feasible. I have entered latitudes and longitudes of these neighborhoods manually and uploaded the data <a href = 'https://github.com/hithesh111/Coursera_Capstone/blob/master/neighborhood_lat_long.csv'> here. </a>

<h3>FourSquare Location Data</h3>
And most importantly,<a href = 'https://foursquare.com/'>FourSquare</a> data was used to fetch location-based results using their <a href = 'https://foursquare.com/developers'>Developer Portal.</a> The API was used to get information about various venues and their details mainly.<br>

<h2>Methodology </h2>
Using the API, information about various venues and their details (name, category, latitude, longitude) within some radius around points on the map is received. The 4 details mentioned above are the ones required to create onehot encodings of frequency of venues in neighborhoods which was then used to cluster the neighborhoods.<br>
Cleaning up the FourSquare API results to get a clean version of these 4 columns required a little understanding of json files and <i>json_normalize </i> function from the pandas.io.json library.<br><br>
KMeans Clustering model from Scikit-learn is then trained on this onehot encoding dataframe (with number of clusters = 6). It is an unsupervised learnning algorithm for classification based on the closeness of vectors in Euclidean space. No of clusters was chosen to be 6 so that it is large enough to offer specific options to the user, and at the same time ensuring that no single element clusters are formed. The clusters are marked on the map in different colour.<br><br>
The user is given a list of choices to choose their top picks for most common requirements in the neighborhood where they want to reside. These choices are converted to a onehot vector of same size as the number of columns in our onehot encoding dataframe. Now KMeans Clustering is used to predict a cluster for this vector. The predicted cluster is printed and all the neighborhoods in the cluster and their most common venues are shown.<br><br>
Also, to easily navigate and find the most suited neighborhood within the cluster, the user is also shown the top neighborhoods offering his 1st priority service, top neighborhoods offering his 2nd priority service, top neighborhoods offering his 3rd  priority service and top neighborhoods offering his 4th priority service to note down places of special interest(if any).<br><br>

<h3>Results</h3>
