# SparkplugB Certificate Handler for Non-SparkplugB Aware MQTT Brokers (Node-RED)

[SparkplugB Certificate Handler in Node-RED library](https://flows.nodered.org/flow/3914b66ef2b3cff0435ae431831847cc)

This project provides a lightweight Node-RED flow that enhances a standard MQTT broker with basic SparkplugB certificate awareness.

![image](https://github.com/user-attachments/assets/b5b3ea7c-ea8a-4134-8a07-d5c4f299d89f)

**Problem:**  
SparkplugB specification requires brokers to retain the latest NBIRTH/DBIRTH (birth) messages and make them available on special `$sparkplug/certificates/...` topics with `retain=true`.  
However, standard MQTT brokers (that are not SparkplugB-aware) do not provide this behavior out of the box.

**Solution:**  
This Node-RED flow subscribes to SparkplugB topics (`spBv1.0/...`) for birth and death messages and republishes them under the correct certificate topics with retention enabled.  
This allows SCADA systems and clients to reliably retrieve the latest known device and node states when connecting.

## Features

- 🛠 Intercepts NBIRTH, DBIRTH, NDEATH, DDEATH messages from SparkplugB devices.
- 🔁 Republishes retained messages on `$sparkplug/certificates/...` topics according to specification.
- ⚡ Sets `retain = true` and `QoS = 1` for certificate messages.
- 🛡 Dummy-proof error handling for malformed topics or unexpected messages.
- 🧩 100% Node-RED, no custom plugins required on the broker side.

## Requirements

- [Node-RED](https://nodered.org/)
- A standard MQTT broker (e.g., Mosquitto) without native SparkplugB awareness.
- [node-red-contrib-mqtt-sparkplug-plus](https://flows.nodered.org/node/node-red-contrib-mqtt-sparkplug-plus) Node-RED package for easy SparkplugB message parsing and generation.
- Working SparkplugB devices or test publishers.

## Installation

1. Install Node-RED if you haven't already.
2. Install the `node-red-contrib-mqtt-sparkplug-plus` package into your Node-RED:
   npm install node-red-contrib-mqtt-sparkplug-plus
   (Or install via the Node-RED palette manager.)
3. Import the provided Node-RED flow into your Node-RED editor.
4. Configure your MQTT Broker connection in Node-RED.
5. Deploy the flow.
6. Start receiving and retaining SparkplugB certificates automatically.

## Usage

- Devices publish NBIRTH/DBIRTH messages to `spBv1.0/<GroupID>/<Type>/<NodeID>/<DeviceID>`.
- The flow republishes them with retention to `$sparkplug/certificates/<GroupID>/<Type>/<NodeID>/<DeviceID>`.
- SCADA clients can subscribe to `$sparkplug/certificates/#` to retrieve the latest device and node states at any time.

## License

This project is licensed under the MIT License.  
See the [LICENSE](LICENSE) file for details.

## Author

Created by **Juri Krivoruchko**.  
Contributions, feedback, and ideas are always welcome!

