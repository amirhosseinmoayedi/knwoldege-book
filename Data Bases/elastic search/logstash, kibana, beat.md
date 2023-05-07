---
title: logstash, kibana, beat
updated: 2022-02-01 12:39:19Z
created: 2021-11-28 16:17:38Z
latitude: 51.29930000
longitude: 9.49100000
altitude: 0.0000
---

Logstash is used to ==**gather data**== from multi sources to elastic or other no sql databases

Like logs or sql data.

Log stash have 3 step pipline:

1.  Input
2.  filter
3.  output

![photo_2021-11-28_19-52-51.jpg](../../../_resources/photo_2021-11-28_19-52-51.jpg)

Logstash uses jruby ( some kind of **json**), as config.

**-f** argument to pass file to logstash

**-e** to pass config as command

in logstash conf we can use different type plugins for filtering data.

The patterns and plugins can be found on github or docs(there are so many).

- Like grok for ==regex==
- Mutate for ==converting== data type
- Date for ==reformatting== dates
- Geoip to ==extract ip locatio==n
- Useagent to ==extract user agent==

We can have multiple input filter and output on a config.

Total number of document can be get from

```
Get index/_doc/_count
```

You create visuals in kibana and then make dashboard base on the visuals

Kibana is meant for business intelligence.

**Logstash must have it's own node and we use beats to ship data** to from server, container or anything to logstash node.

We have different type of beats like file, metric(for os metrics), packet beat (network)، heart beat( health of application).

File beat has something called **pressure control** which won't fulls the logstash pipeline.

File beat has a **harvester** that would get data from different sources into a poll and send them ,each file has a harvester that knows where it left off to continue reading.

beats got something called **metadata** which sends metadata to logstash.

to config the beats to work with logstash we should set the ouput of beat to logstash and input of logstash to beat.

for setting up the x-pack for elastic-search we can set up the password and ssl and other security options.