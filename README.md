# chicagocrime-elk
This repo contains configs to help you get Chicago crime data into Elasticsearch using Logstash.

* logstash.conf: The Logstash config file which helps parse and clean up the crime data.
* index_template.json: An Elasticsearch index template for the crime data set. The Logstash config references this template in its elasticsearch output.
* community_areas.yml: Mapping between community area codes and names.
* wards.yml: Mapping between ward numbers and (current) aldermen for those wards.

You will need to download the raw Chicago crime data files in CSV format from the City of Chicago Data Portal: [https://data.cityofchicago.org/Public-Safety/Crimes-2001-to-present/ijzp-q8t2](https://data.cityofchicago.org/Public-Safety/Crimes-2001-to-present/ijzp-q8t2).

# Install products
## Download

For this exercise, you'll want to download three products:

* Elasticsearch: [https://www.elastic.co/downloads/elasticsearch](https://www.elastic.co/downloads/elasticsearch)
* Logstash: [https://www.elastic.co/downloads/logstash](https://www.elastic.co/downloads/logstash)
* Kibana: [https://www.elastic.co/downloads/kibana](https://www.elastic.co/downloads/kibana)

Get the appropriate package for your operating system for each of the three projects.

## Installation

If you aren't using the package installer (e.g. RPM or DEB), for the sake of simplicity, you can uncompress them into the same folder. For example, on my Mac, I created an ~/elk directory that looks like this:
```
~/elk
  elasticsearch-1.7.1/
  logstash-1.5.3/
  kibana-4.1.1-darwin-x64/
```

You should also install Marvel, which is free for development use. Marvel is an Elasticsearch plugin primarily for monitoring an Elasticsearch cluster, but also includes a great web-based query sandbox called Sense for writing Elasticsearch queries. Marvel can be installed by running:
```
bin/plugin -i elasticsearch/marvel/latest
```
from your Elasticsearch directory. This requires you to be connected to the internet.

See the documentation here for more installation options: [https://www.elastic.co/guide/en/marvel/current/_installation.html](https://www.elastic.co/guide/en/marvel/current/_installation.html)

## Starting Elasticsearch

Before starting Elasticsearch, you should change the name of your "cluster" to something that's likely unique. You can do this in the config file found at config/elasticsearch.yml under your Elasticsearch directory (or at /etc/elasticsearch/elasticsearch.yml if you used the DEB or RPM package installer). Set cluster.name to a unique value; for example:
```
cluster.name: peters_awesome_cluster
```
Now you can run Elasticsearch! Go to your Elasticsearch install dir and run:
```
bin/elasticsearch
```
With the DEB/RPM installers, you can start the service:
```
service elasticsearch start
```
You now have a running Elasticsearch instance!

## Indexing data into Elasticsearch using Logstash

If you haven't downloaded the CSV formatted Chicago crime data yet, do so now using the link at the top of this README.

You'll need to chop off the first line of the file since the Logstash CSV filter doesn't do anything special with the header row:
```
sed -i -e "1d" $FILE
```
where $FILE is the name of the Chicago crime CSV file.

Indexing this data set is as simple as running this command:
```
~/elk/logstash-1.5.3/bin/logstash -f logstash.conf < Crimes_-_2001_to_present.csv
```
Depending on the speed of your machine, the data should finish indexing in somewhere between 5-25 minutes.

## Running Kibana

Run Kibana with this command:
```
~/elk/kibana-4.1.1-darwin-x64/bin/kibana
```
Detailed instructions on getting started with Kibana can be found here:

  https://www.elastic.co/guide/en/kibana/current/index.html

# More information about Elastic

You can learn more about the Elastic products here:

[https://www.elastic.co/products](https://www.elastic.co/products)

For technical documentation for Elasticsearch, Logstash, Kibana and others, go here:

[https://www.elastic.co/guide/index.html](https://www.elastic.co/guide/index.html)

Interact with the community of users on our forums:

[https://discuss.elastic.co/](https://discuss.elastic.co/)
