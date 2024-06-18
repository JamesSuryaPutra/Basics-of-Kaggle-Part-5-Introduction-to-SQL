# Aliasing & Other Improvements
A couple hints to make your queries even better:

1}	The column resulting from COUNT(id) was called f0__. That's not a very descriptive name. You can change the name by adding AS NumPosts after you specify the aggregation. This is called
aliasing, and it will be covered in more detail in an upcoming lesson.

2} If you are ever unsure what to put inside the COUNT() function, you can do COUNT(1) to count the rows in each group. Most people find it especially readable, because we know it's not
focusing on other columns. It also scans less data than if supplied column names (making it faster and using less of your data access quota).

Using these tricks, the user can rewrite the query:

    # Improved version of earlier query, now with aliasing & improved readability
    query_improved = """
                     SELECT parent, COUNT(1) AS NumPosts
                     FROM `bigquery-public-data.hacker_news.full`
                     GROUP BY parent
                     HAVING COUNT(1) > 10
                     """

    safe_config = bigquery.QueryJobConfig(maximum_bytes_billed=10**10)
    query_job = client.query(query_improved, job_config=safe_config)

    # API request - run the query, and convert the results to a pandas DataFrame
    improved_df = query_job.to_dataframe()

    # Print the first five rows of the DataFrame
    improved_df.head()

    parent	NumPosts
    0	24397272.0	87
    1	28875764.0	43
    2	30487933.0	40
    3	27890790.0	96
    4	19678914.0	48


Now you have the data you want, and it has descriptive names. That's good style.
