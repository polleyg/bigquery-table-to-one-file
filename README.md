# bigquery-table-to-one-file
Using Google Cloud Dataflow, this trivial Java application reads a table in BigQuery, and turns it into one file in GCS (GZIP compressed format). Why? Because currently BigQuery only support unsharded exports of under 1 GB.

https://cloud.google.com/bigquery/docs/exporting-data
https://cloud.google.com/dataflow/

It uses the default credentials set to the environment variable `GOOGLE_APPLICATION_CREDENTIALS`. See all about that here: https://developers.google.com/identity/protocols/application-default-credentials

In the code, change the table name and bucket details etc. to suit your needs. You will also just need to create the GCS bucket(s) yourself. I wasn't bothered making them cli parameters.

To run:

`--project=<your_project_id>
--runner=DataflowRunner
--jobName=bigquery-table-to-one-file
--maxNumWorkers=50
--zone=australia-southeast1-a
--stagingLocation=gs://<your_bucket>/jars
--tempLocation=gs://<your_bucket>/tmp`

It should look like this when it's running. I tested it with the public WIKI table (1 billion rows & ~100GB) and it took about 6 hours using 50 `n1-standard-1` workers:

![alt text](https://user-images.githubusercontent.com/5554342/30951859-7ee93e9a-a468-11e7-9875-e363b18966eb.png)
![alt text](https://user-images.githubusercontent.com/5554342/30951877-95c47de6-a468-11e7-8976-3cf4d923f4c2.png)
