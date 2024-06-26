# list_rows() method
We can use the list_rows() method to check just the first five lines of of the full table to make sure this is right (Sometimes databases have outdated descriptions, so it's good to check).
This returns a BigQuery RowIterator object that can quickly be converted to a pandas DataFrame with the to_dataframe() method.

    # Preview the first five lines of the "full" table
    client.list_rows(table, max_results=5).to_dataframe()

    /opt/conda/lib/python3.7/site-packages/ipykernel_launcher.py:2: UserWarning: Cannot use bqstorage_client if max_results is set, reverting to fetching data with the tabledata.list endpoint.

    title	url	text	dead	by	score	time	timestamp	type	id	parent	descendants	ranking	deleted
    0	None	None	I would rather just have wired earbuds, period...	None	zeveb	NaN	1591717736	2020-06-09 15:48:56+00:00	comment	23467666	23456782	NaN	NaN	None
    1	None	None	DNS?	None	nly	NaN	1572810465	2019-11-03 19:47:45+00:00	comment	21436112	21435130	NaN	NaN	None
    2	None	None	These benchmarks seem pretty good. Filterable...	None	mrkeen	NaN	1591717727	2020-06-09 15:48:47+00:00	comment	23467665	23467426	NaN	NaN	None
    3	None	None	Oh really?<p>* Excel alone uses 86.1MB of priv...	None	oceanswave	NaN	1462987532	2016-05-11 17:25:32+00:00	comment	11677248	11676886	NaN	NaN	None
    4	None	None	These systems are useless. Of the many flaws:...	None	nyxxie	NaN	1572810473	2019-11-03 19:47:53+00:00	comment	21436113	21435025	NaN	NaN	None


The list_rows() method will also let us look at just the information in a specific column. If we want to see the first five entries in the by column, for example, we can do that!

    # Preview the first five entries in the "by" column of the "full" table
    client.list_rows(table, selected_fields=table.schema[:1], max_results=5).to_dataframe()

    /opt/conda/lib/python3.7/site-packages/ipykernel_launcher.py:2: UserWarning: Cannot use bqstorage_client if max_results is set, reverting to fetching data with the tabledata.list endpoint.

    title
    0	None
    1	None
    2	None
    3	None
    4	None

