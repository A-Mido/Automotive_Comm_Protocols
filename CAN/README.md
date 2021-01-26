### Table of Contents

1. [The CAN concept in a few words](#concept)
2. [CAN Benefits](#Benefits)
3. [CAN communication protocol specifications](#specifications)
4. [CAN Node](#Node)
4. [CAN Frame](#Frame)
5. [Acknowledgements](#Acknowledgements)



## The CAN concept in a few words <a name="concept"></a>
CAN (controller area network) protocol. It was decided from the outset that CAN should be capable of covering all communication applications found in motor vehicles, in other words, it should carry and multiplex many types
of messages, from the fastest to the slowest. Because of its origin in the car industry, CAN was designed to operate in environments subject to a high level of pollution, primarily due to electromagnetic disturbance. In addition
to the transmission reliability provided by an efficient error detection mechanism, CAN has multimaster functionality to increase the possibility of providing fast recovery from errors after their detection.

## CAN Benefits <a name="Benefits"></a>
* Low-Cost: 
CAN provides an inexpensive, durable network that helps multiple CAN devices communicate with one another.

* Broadcast Communication: 
Each of the devices on the network has a CAN controller chip and is therefore intelligent. All devices on the network see all transmitted messages. Each device can decide if a message is relevant or if it should be filtered. 

* Priority and Arbitration: 
Every message has a priority, so if two nodes try to send messages simultaneously, the one with the higher priority gets transmitted and the one with the lower priority gets postponed. This arbitration is non-destructive and results in non-interrupted transmission of the highest priority message.

* Error detection: 
The CAN specification includes a Cyclic Redundancy Code (CRC) to perform error checking on each frame's contents.

## CAN communication protocol specifications <a name="specifications"></a>
* Serial 
* Asynchronous 
* Master/Slave "Multi Master No Slaves"
* Half duplex communication
* Throughput is variable

## CAN Node <a name="Node"></a>
* Host Controller: 
The CAN Controller is an interface between the Application 'peripheral or master in general' and the CAN bus. The function of the CAN Controller is to convert the data provided by the application into a CAN message frame fit to be transmitted across the bus.

* CAN Controller: 
The CAN controller is a database that holds informations about messages id's and whether this is a listener or an owner to Node. 

* CAN Transceiver: 
The role of the transceiver is simply to drive and detect data to and from the bus. It converts the single- ended logic used by the controller to the differential signal transmitted over the bus.

## CAN Frame <a name="Frame"></a>
* Start Of Frame [SOF 1-bit]: 
Indicates the beginning of a message with a dominant (logic 0) bit.

* Message ID [ID 11-bit]: 
Identifies the message and indicates the message's priority.

* Remote Transmission Requesr [RTR 1-bit]: 
serves to differentiate a remote frame from a data frame. A dominant (logic 0) RTR bit indicates a data frame. A recessive (logic 1) RTR bit indicates a remote frame.

* Data Length Code [DLC 4-bit]: 
Indicates the number of bytes the data field contains.

* Data [Data 0 - 4 byte]: 
Contains 0 to 8 bytes of data.

* Cycle Redundancy Check [CRC 15-bit]: 
Contains 15-bit cyclic redundancy check code and a recessive delimiter bit. The CRC field is used for error detection.

* Acknowledge [ACK 1-bit]: 
Any CAN controller that correctly receives the message sends an ACK bit at the end of the message. The transmitting node checks for the presence of the ACK bit on the bus and reattempts transmission if no acknowledge is detected.

* Delimiter [DEL 1-bit]: 
The delimiter bits must come at a predefined place so that the form of the CAN frame is maintained. if the receiver does not find the delimiter bits at a proper place , it generates an Form Error Frame.

* End of Freme [EOF 6-bit]: 
Indicates the end of frame.

* InterFrame Space [IFS 3-bit]: 
During the Interframe Space (intermission) no node can start the transmission of a data or remote frame. 


## Acknowledgements <a name="Acknowledgements"></a>
1. This [site](https://www.ni.com/en-lb/innovations/white-papers/06/controller-area-network--can--overview.html) is great.

2.This is also a good [site](http://www.copperhilltechnologies.com/can-bus-guide-message-frame-format/)

3. Also this book is also great " Multiplexed Networks for Embedded Systems by Dominique Paret ".
