# MQTT Protocol

## Introduction

- MQTT is a lightweight messaging protocol ideal for IoT (Internet of Things).
- MQTT is a publish/subscribe protocol.
	- This means one client publishes (sends) data, while another client subscribes to (receives) that data.

- <ins>Key components of MQTT protocol:</ins>
	- Publisher  : A client that sends data is called a publisher (or sender).
	- Subscriber : A client that receives data is called a subscriber (or receiver).
	- Broker     : This is the central and controlling entity of the MQTT protocol.

- <ins>MQTT Topics:</ins>
	- Topics are used for publishing and subscribing to data.
	- Topics are not explicitly created on the broker.
	- A topic is automatically created on the broker when a client publishes to it for the first time.
	- Publishers (senders) and subscribers (receivers) communicate using topics.
	- A publisher sends data to a topic, and subscribers receive data from that topic.

- <ins>Communication:</ins>
	- A client publishes data to the broker on a specific topic.
	- The broker sends that data to all clients subscribed to the corresponding topic.
	- A client must subscribe to a particular topic in order to receive data from that topic.	
		
- <ins>Broker:</ins>
	- The broker acts as the central unit in the MQTT communication model.
	- Publishers and subscribers are completely unaware of each other; they communicate indirectly through the broker.
	- All clients publish data to the broker, which then forwards that data to the appropriate subscribers.
	- The broker is responsible for filtering messages based on topics and delivering them only to subscribers of those topics.
		- As soon as new data is published to a topic, the broker immediately sends it to all subscribers of that topic.
	- The broker typically does not store messages from publishers.
		- When data is published to a topic, the broker simply relays it to all relevant subscribers in real time.

- <ins>Publisher and Subscriber Roles:</ins>
	- Any client can act as both a publisher and a subscriber at the same time.
		- When a client sends data on a topic, it acts as a publisher.
		- When it receives data on a topic, it acts as a subscriber.

## MQTT Client-Broker Connections

- MQTT is a connection-oriented protocol that uses TCP/IP for communication.
- It provides error correction and guaranteed message delivery (in-order) over TCP/IP.
- Every message requires an acknowledgment (ACK) for reliable delivery.
	- If the sender doesn't receive an ACK, the message is resent automatically.

__MQTT Message Flow:__

	MQTT Client                        MQTT Broker
	    |                                  |
        |             Connect              |
	    |          ------------->          |
  	    |             CONNACK              |
  	    |          <-------------          |
  	    |                                  |
  	    |             Subscribe            |
  	    |          ------------->          |
  	    |             SUBACK               |
  	    |          <-------------          |
  	    |                                  |
  	    |             Publish              |
  	    |          ------------->          |
  	    |             PUBACK               |
  	    |          <-------------          |
	
- <ins>KeepAlive Time:</ins>
	- Broker maintain keepalive interval to ensure connectivity of client.
	- Its time intrval (default __60 sec__) after which broker sends ping request __(PINGREQ)__ to client and receive ping response __(PINGRESP)__.
		- If broker received ping response then it means client is still connected with broker.
	- Broker waits for 1.5x the keepalive interval (e.g., __90 seconds__ for default __60s__ keepalive).
		- If no __PINGRESP__ is received within this grace period, the broker considers the connection dead.
		- Broker initiates connection cleanup upon no response.
		- The broker initiates connection cleanup when no keepalive __(PINGRESP)__ is received within the timeout period.

- <ins>Connection Cleanup:</ins>
	- When broker does not receive __PINGRESP__ for keepalive interval then broker initiate connection cleanup.
	- Broker forcibly closes the TCP connection.
	- All active subscriptions from that client are removed.
	- Session state is handled based on the client's Clean Session flag:
		- Clean Session = 1: All session data is deleted
		- Clean Session = 0: Session is retained (per QoS rules) until client reconnects
	- Last Will Execution (if configured):
		- Broker publishes the client's predefined *"Last Will and Testament"*. This alerts subscribers that the client disconnected abnormally.
	- Resource Release:
		- Network resources are freed.
		- Broker memory allocated for the connection is reclaimed.
		
- <ins>Last Will Message:</ins>
	- The purpose of the last will message is to notify a subscriber that the publisher is offline because of a network failure.
	- The last will message is set per topic by publisher, so each topic has its own last will message.
	- The last will message is stored on the broker. If the broker detects a connection break it sends the last will message to all subscribers of that topic.
	- If client (publisher) disconnects normally (using the disconnect command), the last will message is not send.
	- *"__Summary:__ The broker sends a last will message to all topic subscribers when it detects either an unexpected disconnection or a network failure with the publisher."*
		
## MQTT Topic

- MQTT topics are a form of addressig used for communication between clients.

- <ins>Topic name structure:</ins>
	- There is no formal or fixed structure for mqtt topics.
	- Topic names are case sensitive.
		- e.g., *home/light ≠ home/LIGHT*
	- A topics name must consist of at least one character.
	- Topic name consist of level seperated by forward slash (/), which act as delimeters.
		- e.g., *house/room1/main-light*
		- e.g., *house/room1/alarm*
	- System topics start with __$SYS__ and user can't use system topics names as topics.
		- e.g., *$SYS/broker/version*
		

