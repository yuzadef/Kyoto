# MQTT

## Summary
- [Theory](#theory)
- [Basic usages](#basic-usages)
  - [Subscribe to a topic](#subscribe-to-a-topic)
  - [Listens to all published models except "$SYS"](#listens-to-all-published-models-except-sys)
  - [Listens to "$SYS" published models only](#listens-to-sys-published-models-only)


## Theory
MQTT is a standards-based messaging protocol, or set of rules, used for machine-to-machine communication. Smart sensors, wearables, and other Internet of Things (IoT) devices typically have to transmit and receive data over a resource-constrained network with limited bandwidth.

## Basic usages
### Subscribe to a topic
`mosquitto_sub -t 'topic-name/#' -h 10.0.0.1 -p 1883 -V mqttv31 --tls-version tlsv1.2`

### Listens to all published models except "$SYS"
`mosquitto_sub -h 10.0.0.1 -p 1883 -t "#"`

### Listens to "$SYS" published models only
`mosquitto_sub -h 10.0.0.1 -p 1883 -t "$SYS"`
