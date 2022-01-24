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
  
  ### Usage
  
  The following values need to be provided-
  
  CLIENTID- the MQTT Client ID for the connection to the broker.
  JWT - the json web token (JWT) for broker authentication.
  HOSTNAME- the hostname of the MQTT broker.
  DATATOPICPATH - the MQTT topic path used for the generated messages described above.
  RESULTSTOPICPATH - the MQTT topic path used for latency results messages described above.
  
  These values can be defined in a few ways-
  
  -Through the GUI of node-RED, which is avialable at http://localhost:1880/ when the container is launched.
  -by replacing the values within settings.js manually.
  -by defining the variables at docker run-time, i.e. 
  ```
  docker run -it -p 1880:1880 -env CLIENTID={clientid} -env JWT={JWT} -env HOSTNAME={hostname} -env DATATOPICPATH={datatopicpath} -env RESULTSTOPICPATH={resultstopicpath} -v node_red_data:/data brianapley/mqtt-perf-client
```

  
