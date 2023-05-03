# Hotspot analysis

This project performs range join operations on rectangle datasets and point datasets. It identifies the number of points located within each rectangle and calculates the hotness of all the rectangles. Additionally, it applies spatial statistics to spatio-temporal big data using Apache Spark to identify statistically significant spatial hot spots.


### Hot zone 

The project calculates the hotness of each rectangle. The hotter rectangle includes more points.

### Hot cell analysis

The main objective of this project is to detect statistically significant spatial hotspots by applying the Getis-Ord statistic to the NYC Taxi Trip datasets using Apache Spark. In order to reduce the computational power required, some modifications have been made to the data and methodology. The input is now limited to a monthly taxi trip dataset from 2009-2012 and the cell unit size has been standardized to 0.01 * 0.01 in terms of latitude and longitude degrees. The time step size is 1 day and only the pickup location is considered. Additionally, Jaccard similarity is not utilized to check the answer.

## Coding template specification

### Input parameters

Output path (mandatory)
Task name: "hotzoneanalysis" or "hotcellanalysis"
#### Task parameters:
- Hot zone (2 parameters): nyc taxi data path, zone path
- Hot cell (1 parameter): nyc taxi data path
Example:
```
test/output hotzoneanalysis src/resources/point-hotzone.csv src/resources/zone-hotzone.csv hotcellanalysis src/resources/yellow_tripdata_2009-01_point.csv
```

Note: 

- The number/order of tasks does not matter.
- The first 7 of the final test cases on Vocareum will be hot zone analysis, and the last 8 will be hot cell analysis.

### Input data format
The main function/entrace is "cse512.Entrance" scala file.

1. The input point dataset for this project is the pickup point of New York Taxi trip datasets. The format of this dataset is the original format of NYC taxi trip, which is distinct from Phase 2. However, the coding template has already parsed the data for the user. The dataset can be found in the S3 bucket of the Data Systems Lab. [Data Systems Lab S3 Bucket](https://datasyslab.s3.amazonaws.com/index.html?prefix=nyctaxitrips/)

2. Zone data (only for hot zone analysis): at "src/resources/zone-hotzone" of the template

#### Hot zone analysis
The input point data can be any small subset of NYC taxi dataset.

#### Hot cell analysis
The input point data is a monthly NYC taxi trip dataset (2009-2012) like "yellow\_tripdata\_2009-01\_point.csv"

### Output data format

#### Hot zone analysis
The output for hot zone analysis is a list of all the rectangles along with the number of points they contain, sorted in ascending order based on their rectangle string.

```
"-73.795658,40.743334,-73.753772,40.779114",1
"-73.797297,40.738291,-73.775740,40.770411",1
"-73.832707,40.620010,-73.746541,40.665414",20
```


#### Hot cell analysis
The top 50 hottest cells' coordinates should be sorted in descending order according to their G score. It's important to note that the G score should not be included in the output.
```
-7399,4075,15
-7399,4075,29
-7399,4075,22
```
### Example answers
An example input and its corresponding output are available in the "testcase" folder of the coding template.


### How to debug your code in IDE

If you are using the Scala template, follow these steps to debug your code in an IDE:

1. Use IntelliJ Idea with Scala plug-in or any other Scala IDE.
2. Replace the logic of User Defined Functions ST_Contains and ST_Within in SpatialQuery.scala.
3. Append .master("local[*]") after .config("spark.some.config.option", "some-value") to tell the IDE that the master IP is localhost.
4. In some cases, you may need to go to "build.sbt" file and change % "provided" to % "compile" to debug your code in the IDE.
5. Run your code in the IDE.
6. Remember to revert Steps 3 and 4 and recompile your code before using spark-submit.

