# mqtt-executor

A simple MQTT client written in go that subscribes to a configurable list of MQTT topics on the specified broker and 
executes a given shell script/command whenever a message arrives.

# Configuration
Create a configuration file named "config.json"
```json
{
  "sys/file": {
    "create": ["/usr/bin/touch", "/tmp/example"],
    "remove": ["/bin/rm", "-f", "/tmp/example"],
    "info": ["/bin/ls", "-l", "/tmp/example"]
  }
}
```

# Usage

Start the tool with the path to the config file and the URL of the MQTT broker
```bash
mqtt-executor -broker tcp://127.0.0.1:1883 -config /path/to/config.json
```

# Trigger command execution

```bash
mosquitto_pub -d -t sys/file -m "create"
mosquitto_pub -d -t sys/file -m "info"
mosquitto_pub -d -t sys/file -m "remove"
```

# Get the command results

Each command result will be written in **&lt;incomingTopicName&gt;/RESULT**. For example:

```bash
mosquitto_sub -d -t sys/file/RESULT
```