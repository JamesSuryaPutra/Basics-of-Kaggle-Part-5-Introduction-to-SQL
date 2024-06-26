# Submitting the query to the dataset
We're ready to use this query to get information from the OpenAQ dataset. As in the previous tutorial, the first step is to create a Client object.

    # Create a "Client" object
    client = bigquery.Client()

    Using Kaggle's public dataset BigQuery integration.


We begin by setting up the query with the query() method. We run the method with the default parameters, but this method also allows us to specify more complicated settings that you can
read about in the documentation. We'll revisit this later.

    # Set up the query
    query_job = client.query(query)


Next, we run the query and convert the results to a pandas DataFrame.

    # API request - run the query, and return a pandas DataFrame
    us_cities = query_job.to_dataframe()

    /opt/conda/lib/python3.7/site-packages/google/cloud/bigquery/client.py:440: UserWarning: Cannot create BigQuery Storage client, the dependency google-cloud-bigquery-storage is not
    installed.
    "Cannot create BigQuery Storage client, the dependency "


Now we've got a pandas DataFrame called us_cities, which we can use like any other DataFrame.

    # What five cities have the most measurements?
    us_cities.city.value_counts().head()

    Phoenix-Mesa-Scottsdale                     39414
    Los Angeles-Long Beach-Santa Ana            27479
    Riverside-San Bernardino-Ontario            26887
    New York-Northern New Jersey-Long Island    25417
    San Francisco-Oakland-Fremont               22710
    Name: city, dtype: int64

