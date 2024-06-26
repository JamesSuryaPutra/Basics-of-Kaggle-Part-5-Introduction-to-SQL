# Working with big datasets
BigQuery datasets can be huge. We allow you to do a lot of computation for free, but everyone has some limit. Each Kaggle user can scan 5TB every 30 days for free. Once you hit that limit,
you'll have to wait for it to reset.

The biggest dataset currently on Kaggle is 3TB, so you can go through your 30-day limit in a couple queries if you aren't careful. Don't worry though: we'll teach you how to avoid scanning
too much data at once, so that you don't run over your limit.

To begin, you can estimate the size of any query before running it. Here is an example using the (very large!) Hacker News dataset. To see how much data a query will scan, we create a
QueryJobConfig object and set the dry_run parameter to True.

    # Query to get the score column from every row where the type column has value "job"
    query = """
            SELECT score, title
            FROM `bigquery-public-data.hacker_news.full`
            WHERE type = "job" 
            """

    # Create a QueryJobConfig object to estimate size of query without running it
    dry_run_config = bigquery.QueryJobConfig(dry_run=True)

    # API request - dry run query to estimate costs
    dry_run_query_job = client.query(query, job_config=dry_run_config)

    print("This query will process {} bytes.".format(dry_run_query_job.total_bytes_processed))

    This query will process 553320240 bytes.


You can also specify a parameter when running the query to limit how much data you are willing to scan. Here's an example with a low limit.

    # Only run the query if it's less than 1 MB
    ONE_MB = 1000*1000
    safe_config = bigquery.QueryJobConfig(maximum_bytes_billed=ONE_MB)

    # Set up the query (will only run if it's less than 1 MB)
    safe_query_job = client.query(query, job_config=safe_config)

    # API request - try to run the query, and return a pandas DataFrame
    safe_query_job.to_dataframe()


    InternalServerError                       Traceback (most recent call last)
    /tmp/ipykernel_19/2063017411.py in <module>
          7 
          8 # API request - try to run the query, and return a pandas DataFrame
    ----> 9 safe_query_job.to_dataframe()

    /opt/conda/lib/python3.7/site-packages/google/cloud/bigquery/job.py in to_dataframe(self, bqstorage_client, dtypes, progress_bar_type, create_bqstorage_client, date_as_object)
       3403             ValueError: If the `pandas` library cannot be imported.
       3404         """
    -> 3405         return self.result().to_dataframe(
       3406             bqstorage_client=bqstorage_client,
       3407             dtypes=dtypes,

    /opt/conda/lib/python3.7/site-packages/google/cloud/bigquery/job.py in result(self, page_size, max_results, retry, timeout, start_index)
       3232         """
       3233         try:
    -> 3234             super(QueryJob, self).result(retry=retry, timeout=timeout)
       3235 
       3236             # Return an iterator instead of returning the job.
    
    /opt/conda/lib/python3.7/site-packages/google/cloud/bigquery/job.py in result(self, retry, timeout)
        819             self._begin(retry=retry, timeout=timeout)
        820         # TODO: modify PollingFuture so it can pass a retry argument to done().
    --> 821         return super(_AsyncJob, self).result(timeout=timeout)
        822 
        823     def cancelled(self):

    /opt/conda/lib/python3.7/site-packages/google/api_core/future/polling.py in result(self, timeout, retry)
        135             # pylint: disable=raising-bad-type
        136             # Pylint doesn't recognize that this is valid in this case.
    --> 137             raise self._exception
        138 
        139         return self._result
    InternalServerError: 500 Query exceeded limit for bytes billed: 1000000. 553648128 or higher required.

    (job ID: f9f41eac-073d-4fa7-ab3a-504909a72e67)

    -----Query Job SQL Follows-----             

    |    .    |    .    |    .    |    .    |    .    |
     1:
     2:        SELECT score, title
     3:        FROM `bigquery-public-data.hacker_news.full`
     4:        WHERE type = "job" 
     5:        
    |    .    |    .    |    .    |    .    |    .    |


In this case, the query was cancelled, because the limit of 1 MB was exceeded. However, we can increase the limit to run the query successfully!

    # Only run the query if it's less than 1 GB
    ONE_GB = 1000*1000*1000
    safe_config = bigquery.QueryJobConfig(maximum_bytes_billed=ONE_GB)

    # Set up the query (will only run if it's less than 1 GB)
    safe_query_job = client.query(query, job_config=safe_config)

    # API request - try to run the query, and return a pandas DataFrame
    job_post_scores = safe_query_job.to_dataframe()

    # Print average score for job posts
    job_post_scores.score.mean()

    1.7267060367454068

