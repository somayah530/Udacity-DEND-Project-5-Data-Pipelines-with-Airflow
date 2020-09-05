## Udacity Data Engineer Nano Degree Project-5

## Data Pipelines with Airflow

# 1.Overview


A music streaming company, Sparkify, has decided that it is time to introduce more automation and monitoring to their data warehouse ETL pipelines and come to the conclusion that the best tool to achieve this is Apache Airflow.

In this project we were tasked to create high grade data pipelines that are dynamic and built from reusable tasks, can be monitored, and allow easy backfills. They have also noted that the data quality plays a big part when analyses are executed on top the data warehouse and want to run tests against their datasets after the ETL steps have been executed to catch any discrepancies in the datasets.

The source data resides in S3 and needs to be processed in Sparkify's data warehouse in Amazon Redshift. The source datasets consist of JSON logs that tell about user activity in the application and JSON metadata about the songs the users listen to.


#  2.Project Steps
<ul>

### set-up AWS Redshift cluster

*  Run `IaC.ipynb`to creat redshift cluster.

*  Run `create_tables.py` to creates Redshift database.


### Configuring the DAG

DAG guidelines:

- The DAG does not have dependencies on past runs
- On failure, the task are retried 3 times
- Retries happen every 5 minutes
- Catchup is turned off
- Do not email on retry

### Building the operators

#### Stage Operator
The stage operator is expected to be able to load any JSON formatted files from S3 to Amazon Redshift. The operator creates and runs a SQL COPY statement based on the parameters provided.
The stage operator is also containing a templated field that allows it to load timestamped files from S3 based on the execution time and run backfills.

#### Fact and Dimension Operators
The operator is expected to take as input a SQL statement and target database on which to run the query against.

####  Data Quality Operator
The operator's main functionality is to receive one or more SQL based test cases along with the expected results and execute the tests. For each the test, the test result and expected result needs to be checked and if there is no match, the operator should raise an exception and the task should retry and fail eventually.

### Start Airflow web server

- Run `/opt/airflow/start.sh` command
- Once the Airflow web server is ready, we can access the Airflow UI

### Add Airflow Connections
- Creat aws connection (use Airflow's UI to configure AWS credentials)
- Create redshift connection

#  3.Appendix


![DAG-Graph_View](https://user-images.githubusercontent.com/24846149/92313567-e6f35280-efd5-11ea-99c2-74f5ce13680c.png)
- Working DAG with correct task dependencies(Graph-View)

![DAG-Tree_View](https://user-images.githubusercontent.com/24846149/92313583-20c45900-efd6-11ea-9427-87a62595c72b.png)
- Working DAG with correct task dependencies(Tree-View)




#### References

 - link to solution solved my issue with Port 3000 (opening airflow webserver).
 https://issues.apache.org/jira/browse/AIRFLOW-1705

 - GitHub repo for the project.
 https://github.com/jukkakansanaho/udacity-dend-project-5
