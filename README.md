# MQTT-Server-Installation
Introduction
Mosquitto is an open-source message broker that uses the Message Queuing Telemetry Transport (MQTT) Protocol. MQTT runs on top of the TCP/IP model and is the standard messaging platform for the Internet of Things (IoT).
Since the MQQT protocol is extremely lightweight, its small code footprint allows you to create applications for devices with minimal resources such as short battery life, limited network bandwidth, and unreliable internet connections.
The Mosquitto application supports the publisher/subscriber topology. In this model, clients connect to the Mosquitto server, which acts as a broker to distribute information to other clients subscribed or sending messages to a channel.
In this guide, you'll install and configure the Mosquitto application and learn how the event-driven MQQT protocol works with IoT applications.
Prerequisites
To follow along with this guide, you need:
•	An Ubuntu 20.04 server.
•	A non-root user with sudo rights.
<img src="./images/MQTT.jpg" width=100% height=40%>
# 1. Install the Mosquitto Server
You'll pull the mosquitto package from Ubuntu's software repository by executing the following steps.

1.	SSH to your server and update the package information index.

          $ sudo apt update 

2.	Install the mosquitto package.

         $ sudo apt install -y mosquitto

3.	The mosquitto package should now load on your server. Confirm the status of the mosquitto service.
         
        $ sudo systemctl status mosquitto

Ensure the package is loaded and active.

 ● mosquitto.service - Mosquitto MQTT v3.1/v3.1.1 Broker
      Loaded: loaded (/lib/systemd/system/mosquitto.service; enabled; vendor pr>
      Active: active (running) since Fri 2021-10-08 06:29:25 UTC; 12s ago
        Docs: man:mosquitto.conf(5)
              man:mosquitto(8)

 ...
4.	Once running, you can manage the mosquitto services by executing the following commands.
o	Stop the mosquitto service:

        $ sudo systemctl stop mosquitto

o	Start the mosquitto service:

        $ sudo systemctl start mosquitto

o	Restart the mosquitto service:

        $ sudo systemctl restart mosquitto

# 2. Install and Test the Mosquitto Clients
When using an MQTT client, you connect to the Mosquitto broker to send and receive messages on different topics depending on the application's use case. A client can either be a publisher, a subscriber, or both.
1.	The Mosquitto package ships with a command-line client that allows you to test the server functionalities. Install the client.

       $ sudo apt install -y mosquitto-clients

# 3. Secure the Mosquitto Server
By default, the Mosquitto server is not secured. However, you can make some configuration settings to secure it with usernames and passwords.
1.	Mosquitto reads configuration information from the following location.
 /etc/mosquitto/conf.d
2.	Create a default.conf under the directory.
 $ sudo nano /etc/mosquitto/conf.d/default.conf
3.	Paste the information below to disable anonymous connections and allow Mosquitto to read valid credentials from the /etc/mosquitto/passwd file.
4.	 allow_anonymous false
 password_file /etc/mosquitto/passwd
5.	Save and close the file.
6.	Open the /etc/mosquitto/passwd file with nano.
 $ sudo nano /etc/mosquitto/passwd
7.	Then, populate the file with the account details for the users that you want to connect to the Mosquitto server. Replace EXAMPLE_PASSWORD and EXAMPLE_PASSWORD_2 with strong values.
8.	 john_doe:EXAMPLE_PASSWORD
 mary_smith:EXAMPLE_PASSWORD_2
9.	Save and close the file.
10.	Next, use the mosquitto_passwd utility to encrypt the passwords.
 $ sudo mosquitto_passwd -U /etc/mosquitto/passwd
11.	Your passwords are now encrypted in a format that only the Mosquitto server can decrypt. Use the Linux cat command to confirm the encryption process.
 $ sudo cat /etc/mosquitto/passwd
Output.
 john_doe:$6$TSzNycsj...5Qyvgd4g==
 mary_smith:$6$DtlKf1lG.../rLHIL0Q==
12.	Restart the mosquitto service to load the new changes.
$ sudo systemctl restart mosquitto

