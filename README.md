# NJIT Smart Water Monitoring System

## Overview
----
This is a project by the { insert Team here } of the {insert course num.} class. To deliver a means of consistently instrumenting water data for a span of at least 3 months. In order to accomplish this task, the following components were identified:

* _Sensor Gateway_: Acts as a gathering point for a set of sensors. Data is received, batched, and serialized here. This sends the serialized data to another device acting as an Internet Gateway.
* _Internet Gateway_: Acts as the host of any server-side web workers, and the broker for sending / receiving data  to/from third party services. This receives serialized data from the sensor gateway, deserializes it into a programmable object, and fans out deconstructed copies of the object to outside services.

* _Data Storage Blob_: Pretty much any server / service that can hold large binary objects. Data here will be accessed by our apps, services, and interested third parties

## System Outline
----
Now that we have classified the main types of actors in our system; let's match them up with corresponding devices:

### Hardware
----
* _Sensor Gateway_: Arduino UNO + Sensors( list here )
* _Internet Gateway_: Raspberry Pi 3
* _Data Storage Blob_: [Azure IoT Hub](https://docs.microsoft.com/en-us/rest/api/iothub/)

### Software
----
* _Sensor Gateway_: 
  * Python 3.x
  * requests library
  * Arduino library
* _Internet Gateway_:
  * MonaServer
  * Lua 5.2.x
  * Luarocks
  
## System at Face Value
----
Now that we ironed out a light bill of materials, let's get into describing how our system's interactions are organized:

### Reality -> Sensor Gateway
* Each Arduino Uno is connected to a mesh of the following sensors:
  * Sensor A
  * SensorB
  * Sensor X
* A long-running python script listens for raw sensor streams, packages them as a multiplexed, serialized data object, and sends it to the Raspberry Pi Monaserver for further processing

### Sensor Gateway -> Internet Gateway
* The Python script in the Sensor Gateway caches and sends serialized data stream objects to the Internet Gateway over a Layer 7 protocol( HTTP )
* The Internet Gateway receives the data stream over HTTP, de-serializing the data for post-processing
* The Internet Gateway then selects a Cloud Service to send a serialized, updated copy of the Data Stream to, in the form of a machine-readable file

### Internet Gateway -> Cloud Services( e.g. Data Blob Storage )
* After selecting a relevant list of Cloud Services, the Internet Gateway prepares to send out messages, encapsulating copies of the Data Stream object
* Upon properly validating and preparing the Data Stream object copies, the Internet Gateway sends the serialized copies to endpoints of each listed Cloud Service
* The Cloud Service logs these activities for administrative observation.

## Getting Started with Development
----
  
