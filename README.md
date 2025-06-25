# MQTT Protocol (Message Queueing Telementry Transport Protocol)

## Introduction

- MQTT is a publish/subscribe protocol.
	- This means one client publishes (sends) data, while another client subscribes to (receives) that data.

- Key components of MQTT protocol:
	- Publisher  : A client that sends data is called a publisher (or sender).
	- Subscriber : A client that receives data is called a subscriber (or receiver).
	- Broker     : This is the central and controlling entity of the MQTT protocol.
					 - The publisher sends data to the broker, and the broker forwards that data to the subscriber.
					 - The publisher sends data to the broker, and the broker forwards that data to the subscriber.

- MQTT Topics:
	- Topics are used for publishing and subscribing to data.
	- Topics are not explicitly created on the broker.
	- A topic is automatically created on the broker when a client publishes to it for the first time.
	- Publishers (senders) and subscribers (receivers) communicate using topics.
	- A publisher sends data to a topic, and subscribers receive data from that topic.

- Communication:
	- A client publishes data to the broker on a specific topic.
	- The broker sends that data to all clients subscribed to the corresponding topic.
	- A client must subscribe to a particular topic in order to receive data from that topic.	
		
Broker:
    - The broker acts as the central unit in the MQTT communication model.
	- Publishers and subscribers are completely unaware of each other; they communicate indirectly through the broker.
	- All clients publish data to the broker, which then forwards that data to the appropriate subscribers.
	- The broker is responsible for filtering messages based on topics and delivering them only to subscribers of those topics.
		- As soon as new data is published to a topic, the broker immediately sends it to all subscribers of that topic.
	- The broker typically does not store messages from publishers.
		- When data is published to a topic, the broker simply relays it to all relevant subscribers in real time.

- All client can be publisher and subscriber at the same time.
	- When client send data, on topic, its a publisher at that time.
	- And when it receive data, on some topic, then it become subscriber.

Publisher and Subscriber Roles:
	- Any client can act as both a publisher and a subscriber at the same time.
		- When a client sends data on a topic, it acts as a publisher.
		- When it receives data on a topic, it acts as a subscriber.