node-red-meeseeks-perf
======================

### About

a node-RED MQTT client that executes the following flows-

1. On startup, performs a Geo-IP lookup on it's own public IP address to determine location.
2. Connects to an MQTT Broker and performs the following-

  -every second, publishes a message that includes it's geo information and timestamp in milliseconds to a topic (DATATOPICPATH).
  -subscribes to DATATOPIC PATH, and compares latency of current time minus publish time as messages are received.
  -Publishes this on-going latnecy result, along with publish-subscribe GEO infromation, to a another topic (RESULTSTOPICPATH).
  -subscribes to RESULTSTOPICPATH, and graphs the latency to http://localhost:1880/ui/
  
  ### Setup
  
  The following variable values need to be provided-
  
  CLIENTID- the MQTT Client ID for the connection to the broker.
  JWT - the json web token (JWT) for broker authentication.
  BROKER- the hostname of the MQTT broker.
  DATATOPICPATH - the MQTT topic path used for the generated messages described above.
  RESULTSTOPICPATH - the MQTT topic path used for latency results messages described above.
  
  These values can be defined in a few ways-
  
  -Through the GUI of node-RED, which is available at http://localhost:1880/ when the container is launched.
  -by replacing the values within settings.js manually. The container will need to be restarted after changing the settings.js file.


### Usage

Once it's been determined how the variables will be set, the container can be launched via the following command-

```
  docker run -it -p 1880:1880 -v node_red_data:/data brianapley/mqtt-perf-client
  ```
  
  Again, the variables can be set via the node-RED GUI, or by manually updating the settings.js file (which will be in the /data directory of the volume that docker creates on container launch).
  
  The client will begin to send one message per second to the DATATOPICPATH, and subscribe to all messages from the same path. If only one client is running, it will determine latency on its own messages. If more than one client is running, each client will see it's own messages along with the messages of all the other clients. This allows for a full-mesh, realtime view of latency to and from different endpoint locations.
  
  Each client also subscribes to the results messages, and graphs them via the node-RED dashboard, which can be found on http://localhost:1880/. Clicking each entry in the graph legend can enable/disable graphing for that source-destination pair.
  
  

  
