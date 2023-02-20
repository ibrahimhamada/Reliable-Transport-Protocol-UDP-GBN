# Reliable-Transport-Protocol-UDP-GBN

My Project of the Computer Networks Course Offered in Spring 2022 @ Zewail City.

In Go-Back-N the sender is allowed to send multiple messages within a specified window without waiting for acknowledgement. The sender has two important parameters; the base which is the sequence number of the oldest unacknowledged packet and NextSeqNum which is the smallest unused sequence number (the next packet to be sent). Packets with ids in the interval [base, NextSeqNum - 1] are sent immediately. When the sender receives an acknowledgement from the receiver it slides its windows depending on the packet id of the acknowledgement packet (cumulative acknowledgement).


In this project, I implemented a transport protocol that provides some reliability services
on top of the unreliable UDP. This is done by augmenting UDP with the GBN protocol.



The sender executes three types of events:
=========================

1) When it is invoked to send data. It checks to see whether the window is full. If the window is not full. The packet is created and ready to be sent and variables are modified accordingly .
2) When it receives an ACK. An acknowledgement packet with sequence n indicates that all packets up to n have been received successfully prompting the sender to slide its window and update its base to n+1
3) Timeout. If a timeout occurs, the sender is required to resend all the packets that have been already sent and they might have lost or their acknowledgements weren’t received. There is one single timer which is for the oldest transmitted unacknowledged packet. The timer restarts upon each ACK reception.


Here is the Sender Packet Structure
=========================

![image](https://user-images.githubusercontent.com/58476343/220149355-a58cfc44-b87e-443f-b156-3993f8681fb8.png)

Here is the Acknowledgment Structure
=========================

![image](https://user-images.githubusercontent.com/58476343/220149428-cfe8a5f0-1e33-4c43-a77f-c698760e57d8.png)


The receiver executes these events:
=========================

1) The receiver starts by establishing the connection with the sender. 
2) The channel loss rate is simulated using a random number generator between 0 and 1 and the loss rate is set to 10%. 
3) If the random number is less than 0.1, the packet will be lost. 
4) The received packet is divided in order to obtain packet_id, file_id, data, trailer.
5) The receiver checks if the received sequence number is the expected number to be received or not.
6) If the received packet isn’t the required one, it will be discarded and the receiver will send an acknowledgment to the sender with the ID of the last correctly received packet.
7) When the expected packet is received, the extracted data will be appended to the data array and an acknowledgement packet will be sent to the sender.
8) If the trailer of the received packet is “FFFF”, which indicates the end of the file, the receiver stops waiting for packets and the received data will be written to a file.



