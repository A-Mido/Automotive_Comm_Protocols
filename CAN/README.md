### Table of Contents

1. [The CAN concept in a few words](#Concept)
2. [CAN Benefits](#Benefits)
3. [CAN communication protocol specifications](#Specifications)
4. [CAN Node](#Node)
4. [CAN Frame](#Frame)
5. [Error Confinement Mechanisms](#Errors)
6. [Bit stuffing](#stuffing)
7. [CAN summary](#Summary)
8. [Acknowledgements](#Acknowledgements)



## The CAN concept in a few words <a name="Concept"></a>

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

## CAN communication protocol specifications <a name="Specifications"></a>

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
if instead of the 3 bits a frame of 6 zeros comes this is known as overload frame sended after the EOF tells the listeners that the node recieved the request, but it's busy. a big difference between active error frame and overload frame is that overload frame sended after the EOF unlike the active error frame.


## Error Confinement Mechanisms <a name="Errors"></a>

Every CAN controller along a bus will try to detect the errors outlined above within each message. If an error is found, the discovering node will transmit an Error Flag, thus destroying the bus traffic. each node maintains two error counters: the Transmit Error Counter TEC and the Receive Error Counter REC. There are several rules governing how these counters are incremented and/or decremented depends on the CAN software configurations. In essence, a transmitter detecting a fault increments its Transmit Error Counter faster than the listening nodes will increment their Receive Error Counter. This is because there is a good chance that it is the transmitter who is at fault!

Every node starts out in Error Active mode. When any one of the two Error Counters raises above 127, the node will enter a state known as Error Passive and when the Transmit Error Counter raises above 255, the node will enter the Bus Off state.

* Active error frame: is a 6 consecutive zeros sent if any node detcect any sort of error in the frame.
* Passive error frame: is a 6 consecutive ones sent if any node detcect any sort of error in the frame.
* A node which is Bus Off will not transmit anything on the bus at all.

The rules for increasing and decreasing the error counters are somewhat complex, but the principle is simple: transmit errors give 8 error points, and receive errors give 1 error point. Correctly transmitted and/or received messages causes the counter(s) to decrease.

Let’s assume that node A on a bus has a bad day. Whenever A tries to transmit a message, it fails (for whatever reason). Each time this happens, it increases its Transmit Error Counter by 8 and transmits an Active Error Flag. Then it will attempt to retransmit the message.. and the same thing happens.
When the Transmit Error Counter raises above 127 (i.e. after 16 attempts), node A goes Error Passive. 

The difference is that it will now transmit Passive Error Flags on the bus. A Passive Error Flag comprises 6 recessive bits, and will not destroy other bus traffic – so the other nodes will not hear A complaining about bus errors. However, A continues to increase its Transmit Error Counter. When it raises above 255, node A finally gives in and goes Bus Off.

What does the other nodes think about node A? – For every active error flag that A transmitted, the other nodes will increase their Receive Error Counters by 1. By the time that A goes Bus Off, the other nodes will have a count in their Receive Error Counters that is well below the limit for Error Passive, i.e. 127. This count will decrease by one for every correctly received message. However, node A will stay bus off untill it's fixed or reseted.


## Bit stuffing <a name="Stuffing"></a>

When five consecutive bits of the same level have been transmitted by a node, it will add a sixth bit of the opposite level to the outgoing bit stream. The receivers will remove this extra bit. This is done to avoid excessive DC components on the bus, but it also gives the receivers an extra opportunity to detect errors: if more than five consecutive bits of the same level occurs on the bus, a Stuff Error is signaled.


## CAN summary <a name="Summary"></a>

* CAN Specifications
1. Plug and Play
2. Differential and Twisted
3. Asynchronous, Serial and MMNS
4. Wired and Half duplex

* CAN Frames
1. Data frame
2. Remote frame
3. Active Error frame
4. Passive Error frame
5. Overload frame


* CAN PROS
1. No Bus starvation
2. Error Confinement

* CAN CONS
1. Max data frame size is 8 Byte --> lower throughput
2. According to the standard max nodes is only 64 node



## Acknowledgements <a name="Acknowledgements"></a>

1. this is an intresting [video](https://www.youtube.com/watch?v=FqLDpHsxvf8).

2. This [site](https://www.ni.com/en-lb/innovations/white-papers/06/controller-area-network--can--overview.html) is great.

3. This is also a good [site](https://www.csselectronics.com/screen/page/simple-intro-to-can-bus).

4. CAN Bus Error Handling [site](https://www.kvaser.com/about-can/the-can-protocol/can-error-handling/).
