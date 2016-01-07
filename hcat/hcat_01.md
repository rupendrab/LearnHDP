## Create a hcat table and access from Pig on the HDP sandbox

### Create and populate

```
# Create Table in HCatalog
hcat -e "create table stockinfo (symbol string, price float, change float) row format delimited fields terminated by ',' stored as textfile"

# Create a data file and transfer it to hdfs
cat stockdata.csv 
YHOO,100.0,-10.0
GOOG,600.0,30.0
AAPL,800.0,70.0

hadoop fs -put stockdata.csv /user/guest/

# Populate table using hive
hive -e "load data inpath '/user/guest/stockdata.csv' overwrite into table stockinfo"

## Open pig and access the table
pig -useHCatalog
## The below is needed, -useHCatalog is not loading this required jar file
REGISTER /usr/hdp/2.3.2.0-2950/hive-hcatalog/share/hcatalog/hive-hcatalog-pig-adapter.jar;
stock_data = LOAD 'stockinfo' USING org.apache.hive.hcatalog.pig.HCatLoader();
dump stock_data;
```
