This is a basic core distribution enterprise simulation lab. 

There is 1 link to the ISP coming in from the router. The router has a static default route going out to the internet to allow traffic to be sent to the internet. The Core-FW sits at the edge of the network and behind it are 2 Core switches both using layer 2 and layer 3. The main switch has higher HSRP priority and the second core switch is setup to fail over. There is a etherchannel/trunk configured between the 2 switches to move the vlan traffic between the switches.




There are 5 vlans
10 - Corp/office traffic
50 - Printers
55 - OT
100 - Server traffic
110 - Voice



In the center there is SAN traffic which connects directly to the main core switch on the left. Have the SAN only connected to the SAN switch. The SAN switch's uplink goes to the multilayer/main core switch. IDFs are your "distribution switches and anything that feeds from a switch connected to the core switch is considered an access switch.


There is around 200-250 devices. I attempted to add more but I found that packet tracer would leak memory severly if you left a sim running with anything more than what I have now.
