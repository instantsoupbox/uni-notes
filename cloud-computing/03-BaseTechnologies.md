# 03 BASE TECHNOLOGIES

## Storage
### Storage Provosioning
* Direct Attached Storage
* Storage Area Network SAN
* Network Attached Storage NAS

#### SAN
* Storage Area Network
* A network provides access to block level data storage.
* This is typically a network separated from LAN

#### NAS
* Network Attached Storage
* Storage is provided over a file server that is attached to the network

## Network
* Transport time for a message:
    * Physical delay
    * Protocol delay
    * Line waiting time
    * Transmission time

## Fault tolerance
* Fault:
    * Defect of a system that may cause an error
* Error:
    * Illegal state within the system
* Failure:
    * Occurs after an error reaches the service interface of a system and results in inconsistency with specified system behavior.
* Fault tolerance:
    * A fault tolerant system never enters a state of failure, i.e. errors never reach the service interface.
    * Implementation steps:
        * Error detection
        * Error analysis
        * Recovery
* Reliability:
    * Probability that a system is free of failures up to a certain point in time.
* Availability:
    * Percentage of planned operation time in which the system is available.
