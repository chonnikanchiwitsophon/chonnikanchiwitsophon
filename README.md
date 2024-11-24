**Test Version_01 : Chonnikan Chiwitsophon : 2024-11-23**

// Process of loading data (order_detail and restaurant_detail) from PostgreSQL to Hive and executing transformations by Airflow Scheduler //

**Prerequisites Tasks**
1.	Apache Airflow: Installed and configured.
2.	Apache Hive: Installed and accessible.
3.	PostgreSQL Database: Running with the required schema and data.
4.	Spark with Hive Support: Installed with configurations for Hive metastore.
5.	JDBC Driver: Ensure the PostgreSQL JDBC driver is available to Spark.

**Before testing/running the project, ensure the following:**
1.	PostgreSQL is running, and the schema and table are already created.
2.	CSV Files for Input Data are available at the specified paths.
3.	Hive is accessible, with the metastore properly set up.
4.	Airflow is running with the DAG loaded into its dags folder.

**Steps to Deploy**
1. Set Up PostgreSQL
- Create the database and tables as per the provided script : create_tbl_postgre.sql
- Load data into PostgreSQL using the Python script : load_data_postgre.py
2.	Set Up Hive
- Create two Hive external tables with the provided script : create_tbl_hive.sql
3.	Create Two New table
o	Run SQL scripts to create and load data to 
- _order_detail_new_ table : create_tbl_order_detail_new.sql
- _restaurant_detail_new_ tables : create_tbl_restaurant_detail_new.sql
4.	ETL and Export Scripts:
- Save the ETL PySpark script in file “etl_postgres_to_hive.py” and place to path C:/Users/ <user>/Downloads/scripts/
- Save the export outputs (discount.csv and cooking.csv) in PySpark script in  file “export_csv_file.py” and place to path C:/Users/<user>/Downloads/scripts/
- Test the script locally to ensure proper connectivity and transformations.
5.	Deploy Airflow DAG:
- Place the DAG script in the Airflow DAGs folder (e.g., /<path>/airflow/dags/ETL_dag.py).
- Start the Airflow scheduler and webserver.
- Trigger the DAG from the Airflow UI.
6. Verify Outputs:
- Confirm data is loaded into Hive tables (order_detail and restaurant_detail).
- Check results CSV file in path “C:/Users/<user>/Downloads/<file>.csv”

**How to Test the ETL Process**
**Step 1: Test PostgreSQL Data**
1.	Connect to the PostgreSQL database
```
psql -h localhost -U test_username -d test_database
```
2.	Verify the order_detail and restaurant_detail tables
```
SELECT * FROM order_detail LIMIT 5;
SELECT * FROM restaurant_detail LIMIT 5;
```

**Step 2: Test Hive Tables**
1.	Connect to the Hive

```Hive```

**2.	Check the external tables are created**
```
USE test_database;
SHOW TABLES;
DESCRIBE order_detail;
DESCRIBE restaurant_detail;
```

**Step 3: Test PySpark Code**
1.	Execute PySpark ETL script locally
```
spark-submit etl_postgres_to_hive.py
```
2.	Check Hive tables for data and partitions
```
SELECT COUNT(*) FROM test_database.order_detail;
SELECT COUNT(*) FROM test_database.restaurant_detail;
```

**Step 4: Test new table**
1.	Verify the data is transformed and loaded correctly

```
SELECT * FROM test_database._order_detail_new_ LIMIT 5;
SELECT * FROM test_database._restaurant_detail_new_ LIMIT 5;
```
