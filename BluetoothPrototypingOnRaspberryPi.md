# Bluetooth LE Prototyping on Raspberry PI

## History 

Bluetooth Low Energy (LE) (Bluetooth 4.0) is a light-weight subset of classic Bluetooth. It was started by Nokia and although it's a part of the Bluetooth classic spec now, it has a different lineage.

## Platform Support

Most major platforms support it. Older Windows versions only support Bluetooth 2.1 (no LE support). iOS5+ has support.

## Why is it so vital?

Can run on many devices, much of which require low power. It also bypasses Apples app store requirements for hardware certification if you use full Bluetooth.

## Topology

### Generic Access Profile

Controls connections and advertising in Bluetooth. GAP is what makes your device visible to the outside world and determines how two devices can (or can't) interact with each other.

GAP defines various roles for devices, but the two key concepts to keep in mind are **central** devies and **peripheral** devices (roles).

### Central and Peripherals

Central is a hub. Peripherals connect to the hub.

Central can connect to many peripherals, but a peripheral can only connect to one central.

Central devices are usually the mobile phone or tablet that you connect to with far more power and memory.

Peripherals are small, low power, resource constrained devices that can connect to a much more powerful central device.

### Bluetooth LE Broadcast Topology

* Beacon Mode
   * Observer = central
   * Broadcaster = peropheral

*One way advertisement information transfer from broadcaster to observer*

### GATT Transactions

* Profile: one approved by the bluetooth SIG or custom to your hardware. E.G. Heart Rate profile.

* Service: break data up into logic entities and contain specific chunks of data called characteristics.

* Characteristics: The lowest level concept in GATT transactions which encapsulates a single data point. These expose a data point that can have read and write access.

## Demo

`pybleno` Python library interfaces with sensors.

Service developed needs to have an identifier which applications can use to connect to it. 

You define service characteristics:
   * Read
   * Write
   * Notify

### .NETStandard Library

#### nexus.protocols.ble