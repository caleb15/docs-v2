---
title: Using the HTTP input plugin with Citi Bike data
description: Collect live metrics on Citi Bike stations in New York City with the HTTP input plugin.
menu:
  telegraf_1_16:
    name: Using the HTTP plugin
    weight: 30
    parent: Guides
---

This example walks through using the Telegraf HTTP input plugin to collect metrics on Citi Bike stations in New York City. Station data is available in JSON format [here](https://gist.githubusercontent.com/caleb15/dddc22420e7ea27a8a54258687eb578f/raw/070d1362c7186b6b290d735107d0a6cd6020733e/citibike.json). If you want real live data you can check out the link [here](https://gbfs.citibikenyc.com/gbfs/en/station_status.json) but the guide may be out of date compared to the live data.

For the following example to work, configure [`influxdb` output plugin](/telegraf/v1.15/plugins/#influxdb). This plugin is what allows Telegraf to write the metrics to your InfluxDB.

## Configure the HTTP Input plugin in your Telegraf configuration file

To retrieve data from the Citi Bike URL endpoint, enable the `inputs.http` input plugin in your Telegraf configuration file.

Specify the following options:

### `urls`
One or more URLs to read metrics from. For this example,  use `https://gist.githubusercontent.com/caleb15/dddc22420e7ea27a8a54258687eb578f/raw/070d1362c7186b6b290d735107d0a6cd6020733e/citibike.json`.

### `data_format`
The format of the data in the HTTP endpoints that Telegraf will ingest. For this example, use JSON.


## Add parser information to your Telegraf configuration

Specify the following JSON-specific options.

### JSON

#### `json_query`
To parse only the relevant portion of JSON data, set the `json_query` option with a [GJSON](https://github.com/tidwall/gjson) path. The result of the query should contain a JSON object or an array of objects.
In this case, we don't want to parse the JSON query's `executionTime` at the beginning of the data, so we'll limit this to include only the data in the `stationBeanList` array.

#### `tag_keys`
List of one or more JSON keys that should be added as tags. For this example, we'll use the tag keys `id`, `stationName`, `city`, and `postalCode`.

#### `json_string_fields`
List the keys of fields that are in string format so that they can be parsed as strings. Here, the string fields are `statusValue`, `stAddress1`, `stAddress2`, `location`, and `landMark`.

#### `json_time_key`
Key from the JSON file that creates the timestamp metric. In this case, we want to use the time that station data was last reported, or the `lastCommunicationTime`. If you don't specify a key, the time that Telegraf reads the data becomes the timestamp.

#### `json_time_format`
The format used to interpret the designated `json_time_key`. This example uses [Go reference time format](https://golang.org/pkg/time/#Time.Format). For example, `Mon Jan 2 15:04:05 MST 2006`.

#### `json_timezone`
The timezone We'll set this to the Unix TZ value where our bike data takes place, `America/New_York`.


#### Example configuration

  ```toml
  [[inputs.http]]
  #URL for NYC's Citi Bike station data in JSON format
  urls = ["https://gist.githubusercontent.com/caleb15/dddc22420e7ea27a8a54258687eb578f/raw/070d1362c7186b6b290d735107d0a6cd6020733e/citibike.json"]

  #Overwrite measurement name from default `http` to `citibikenyc`
  name_override = "citibikenyc"

  #Exclude url and host items from tags
  tagexclude = ["url", "host"]

  #Data from HTTP in JSON format
  data_format = "json"

  #Parse `data.stations` array only
  json_query = "data.stations"

  #Set station metadata as tags
  tag_keys = ["station_id"]

  #Latest station information reported at `lastCommunicationTime`
  json_time_key = "last_reported"

  #Time is reported in unix seconds
  json_time_format = "unix"
  
  #Data is from New York so it is in Eastern Standard Time (EST)
  json_timezone = "America/New_York"
  ```



## Start Telegraf and verify data appears

[Start the Telegraf service](/telegraf/v1.15/introduction/getting-started/#start-telegraf-service).

To test that the data is being sent to InfluxDB, run the following (replacing `telegraf.conf` with the path to your configuration file):

```
telegraf -config ~/telegraf.conf -test
```

This command should return line protocol that looks similar to the following:


```
citibikenyc,station_id=72 is_installed=1,is_renting=1,is_returning=1,num_bikes_available=10,num_bikes_disabled=1,num_docks_available=44,num_docks_disabled=0,num_ebikes_available=2 1604455485000000000
citibikenyc,station_id=79 is_installed=1,is_renting=1,is_returning=1,num_bikes_available=25,num_bikes_disabled=2,num_docks_available=6,num_docks_disabled=0,num_ebikes_available=0 1604455168000000000
citibikenyc,station_id=82 is_installed=1,is_renting=1,is_returning=1,num_bikes_available=17,num_bikes_disabled=1,num_docks_available=9,num_docks_disabled=0,num_ebikes_available=0 1604456065000000000
citibikenyc,station_id=83 is_installed=1,is_renting=1,is_returning=1,num_bikes_available=43,num_bikes_disabled=0,num_docks_available=19,num_docks_disabled=0,num_ebikes_available=1 1604454968000000000
```

Now, you can explore and query the Citi Bike data in InfluxDB. The example below is an InfluxQL query and visualization showing the number of available bikes over the past 15 minutes at the Broadway and West 29th Street station.

![Citi Bike visualization](/img/telegraf/1-13-citibike_query.png)
