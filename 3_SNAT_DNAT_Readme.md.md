What is Network Address Translation is Network Address Translation (NAT) is a process that changes a one ip address into a another ip address.as network traffic passes through a router or firewall.



NAT is mainly used to:



Allow private devices to access the Internet using a public IP.

Allow outside users to access an internal server using a public IP.





SNAT (Source Network Address Translation) is a process of changing the source IP address of a packet as it passes through a router or firewall.

* SNAT changes the source IP address of outgoing traffic.
* We configure SNAT (Source NAT) when we need to change the source IP address of outgoing traffic.
* SNAT is commonly configured when outgoing traffic needs its source IP changed, especially when the next Layer-3 device does not have a route back to the original private network.





DNAT (Destination Network Address Translation) is a process of changing the destination IP address of incoming traffic as it passes through a router or firewall.

* In simple words: DNAT changes the destination IP address of incoming traffic, usually to allow outside users to access an internal server
* We configure DNAT (Destination Network Address Translation) when we want outside users to access an internal server that has a private IP address.

