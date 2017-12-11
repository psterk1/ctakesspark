# Attempt to integrate ctakes with Apache Spark
## Introduction


## Prerequisites
 * Download and install [Apache cTAKES](http://ctakes.apache.org) v4.0.0 as shown below. It is important to install v4.0.0 as this is expected later on.
```
$ cd /usr/local
$ wget "http://archive.apache.org/dist/ctakes/ctakes-4.0.0/apache-ctakes-4.0.0-bin.tar.gz"
$ tar -zxvf apache-ctakes-4.0.0-bin.tar.gz
```

```
$ cd /usr/local
$ git clone https://github.com/yugagarin/ctakesspark.git
$ cd ctakesspark
$ apt install maven
$ mvn clean install 
 ```

## Installation
Update CtakesFunction.java with [UMLS](http://www.nlm.nih.gov/research/umls/) username and password.
The case be obtained by [registering with and signing the UMLS Metathesaurus License](https://uts.nlm.nih.gov//license.html). Once you have it, proceeed as below:
```
$ vim src/main/java/org/dia/red/ctakes/spark/CtakesFunction.java
```
Then populate the following two properties with your username and password respectively:
```
	private void setup() throws UIMAException {
		System.setProperty("ctakes.umlsuser", "");
		System.setProperty("ctakes.umlspw", "");
```
Then build the project.
```
$ mvn clean install
```

## Download and install resources


## Executing on an Existing Spark Cluster
When you installed the project as above, you may have also noticied the build system generates an spark-ctakes-0.1-shaded.jar artifact.

This artifact is a self-contained job JAR (uber jar) that contains all the dependencies required to run our application on an existing cluster.

The [Spark Documentation](https://spark.apache.org/docs/1.1.0/submitting-applications.html) provides excellent context on how to submit your jobs. A bare bones example is provided below:
 * Typical syntax using spark-submit
```
./bin/spark-submit \
  --class <main-class>
  --master <master-url> \
  --deploy-mode <deploy-mode> \
  --conf <key>=<value> \
  ... # other options
  <application-jar> \
  [application-arguments]
```
Now an example for our application
```
$ ./usr/bin/spark-submit \
--class org.poc.ctakes.spark.CtakesSparkMain \
--master yarn --deploy-mode cluster \
--conf spark.executor.extraClassPath=/tmp/ctakesdependencies/  \
--conf spark.driver.extraClassPath=/tmp/ctakesdependencies/ \
--conf spark.driver.memory=5g --executor-memory 10g \
spark-ctakes-0.1-shaded.jar

```

# License
Ctakesspark is licensed under the [Apache License v2.0](http://www.apache.org/licenses/LICENSE-2.0).
A copy of that license is shipped with this source code.
