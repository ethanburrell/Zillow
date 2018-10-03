# Zillow Data to Database Conversion in Terminal
### To create the code for one step below use:

Trans and Asmt Data to Database.ipnb

Run this and use the output of the varible of main_sql_header as the
header (columns) to the table that we are changing from a CSV.

The following instructions will work once you ssh into quesnay.berkeley.edu
```
# this converts the CA Main Assessor data to a database format
# cd to your scratch folder first

# convert the main assessment info from CA into a CSV
>>> sqlite3 CA_Asmt_9-27-18_v2.db
  sqlite> .mode csv
  sqlite> .separator "|"
  sqlite> CREATE TABLE Main ...... From the main_sql_header from ipnb above (copy and paste)
  sqlite> .import /scratch/public/ztrax/Zillow_Assessor/06/ZAsmt/Main.txt main
  # Take a nice long break
  # control d to exit

# This takes all of the Berkeley Properties out of the CA Main Assessor database a
>>> sqlite3 berkeley_ouput.db
  sqlite> attach 'CA_Asmt_9-27-18_v2.db' as ca #use the name of the DB from the last step
  sqlite> CREATE TABLE main AS SELECT * from ca.main WHERE ca.main.PropertyCity = "BERKELEY";

#Perform an inner join on 2 datasets (EG main Assessment, and building assessment)
  sqlite> attach 'CA_Asmt_9-27-18_v2.db' as main_tbl;
  sqlite> attach 'CA_Building_9-29-18.db' as building_tbl;
  sqlite> CREATE TABLE joined AS SELECT * from main_tbl.main a inner join building_tbl.Building b on a.RowID = b.RowID2;

  #output to a CSV
  sqlite> .headers on
  sqlite> .mode csv
  sqlite3> .output berkeley_ouput.csv
  sqlite3> SELECT * FROM main;
```
