
Task 1: Defining custom topologies 

1.What is the output of “nodes” and “net” 

The output for Nodes is :

mininet> nodes
available nodes are: 
c0 h1 h2 h3 h4 h5 h6 h7 h8 s1 s2 s3 s4 s5 s6 s7

The output for net is :

mininet> net
h1 h1-eth0:s3-eth1
h2 h2-eth0:s3-eth2
h3 h3-eth0:s4-eth1
h4 h4-eth0:s4-eth2
h5 h5-eth0:s6-eth1
h6 h6-eth0:s6-eth2
h7 h7-eth0:s7-eth1
h8 h8-eth0:s7-eth2
s1 lo:  s1-eth1:s2-eth3 s1-eth2:s5-eth3
s2 lo:  s2-eth1:s3-eth3 s2-eth2:s4-eth3 s2-eth3:s1-eth1
s3 lo:  s3-eth1:h1-eth0 s3-eth2:h2-eth0 s3-eth3:s2-eth1
s4 lo:  s4-eth1:h3-eth0 s4-eth2:h4-eth0 s4-eth3:s2-eth2
s5 lo:  s5-eth1:s6-eth3 s5-eth2:s7-eth3 s5-eth3:s1-eth2
s6 lo:  s6-eth1:h5-eth0 s6-eth2:h6-eth0 s6-eth3:s5-eth1
s7 lo:  s7-eth1:h7-eth0 s7-eth2:h8-eth0 s7-eth3:s5-eth2
c0

-------------------------------------------------------------------------------------------------------------------

2. What is the output of “h7 ifconfig”? 


mininet> h7 ifconfig
h7-eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.0.0.7  netmask 255.0.0.0  broadcast 10.255.255.255
        inet6 fe80::f0eb:43ff:fec8:3021  prefixlen 64  scopeid 0x20<link>
        ether f2:eb:43:c8:30:21  txqueuelen 1000  (Ethernet)
        RX packets 76  bytes 5792 (5.7 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 12  bytes 936 (936.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
-------------------------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------------------------- 

Task 2: Analyze the “of_tutorial’ controller 

1. Draw the function call graph of this controller. For example, once a packet comes to the controller, which function is the first to be called, which one is the second, and so forth? 

Answer : When packet arrives to the controller : 

def _handle_PacketIn (self, event) 

            |
            |
           \|/

def act_like_switch (self, packet, packet_in).  # if uncommented in the python file. By default it will go to { def act_like_hub (self, packet, packet_in) }


            |
            |
           \|/

def resend_packet (self, packet_in, out_port)

            |
            |
           \|/

self.connection.send(msg)

-------------------------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------------------------- 
2. Have h1 ping h2, and h1 ping h8 for 100 times (e.g., h1 ping -c100 p2).  

H1 ping h2 100 times:
      
mininet> h1 ping -c100 h2
PING 10.0.0.2 (10.0.0.2) 56(84) bytes of data.
64 bytes from 10.0.0.2: icmp_seq=1 ttl=64 time=15.2 ms
64 bytes from 10.0.0.2: icmp_seq=2 ttl=64 time=4.65 ms
64 bytes from 10.0.0.2: icmp_seq=3 ttl=64 time=5.81 ms


H1 ping h8 100 times:

mininet> h1 ping -c100 h8
PING 10.0.0.8 (10.0.0.8) 56(84) bytes of data.
64 bytes from 10.0.0.8: icmp_seq=1 ttl=64 time=23.7 ms
64 bytes from 10.0.0.8: icmp_seq=2 ttl=64 time=18.7 ms
64 bytes from 10.0.0.8: icmp_seq=3 ttl=64 time=23.0 ms
---------------------------------------------------------------------------------------------------------------------
2a.How long does it take (on average) to ping for each case? 

   •H1 ping h2 100 times:
    Average time to ping: 5.527ms

--- 10.0.0.2 ping statistics ---
100 packets transmitted, 100 received, 0% packet loss, time 99154ms
rtt min/avg/max/mdev = 1.644/5.527/15.235/1.337 ms


    • H1 ping h8 100 times.
      Average time to ping: 22.920 ms

--- 10.0.0.8 ping statistics ---
100 packets transmitted, 100 received, 0% packet loss, time 99147ms
rtt min/avg/max/mdev = 5.333/22.920/42.299/3.836 ms

----------------------------------------------------------------------------------------------------------------------
2b. What is the minimum and maximum ping you have observed? 

H1 ping h2 100 times:
minimum and maximum ping observed: 1.644 ms & 15.235 ms
H1 ping h8 100 times.
minimum and maximum ping observed: 5.333 ms & 42.299 ms

----------------------------------------------------------------------------------------------------------------------
2c. What is the difference, and why?

H1 ping h8 took longer compared to h1 ping h2. This is due to greater number of switches between h1& h8.

-------------------------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------------------------- 

3. Run “iperf h1 h2” and “iperf h1 h8” 

   3a. What is “iperf” used for? 

      It is used to test TCP bandwidth to evaluate the network performance and quality of a network line. 
----------------------------------------------------------------------------------------------------------------------
   3b. What is the throughput for each case? 

      For “iperf h1 h2” :


mininet> iperf h1 h2
*** Iperf: testing TCP bandwidth between h1 and h2 
*** Results: ['9.98 Mbits/sec', '11.4 Mbits/sec']

      For “iperf h1 h8” :

mininet> iperf h1 h8
*** Iperf: testing TCP bandwidth between h1 and h8 
*** Results: ['3.09 Mbits/sec', '3.75 Mbits/sec']

----------------------------------------------------------------------------------------------------------------------    
     3c. What is the difference, and explain the reasons for the difference? 

         The throughput for h1 -> h8 is low compared to h1->h2. This is because there are multiple switches between h1 & h8, as compared to h1& h2. 
         Hence there is more packet drop in case of h1 -> h8 as the packet flows through a greater number of switches.

-------------------------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------------------------- 

4. Which of the switches observe traffic? Please describe your way for observing such traffic on switches (e.g., adding some functions in the “of_tutorial” controller). 

   I have added below code in the of_tutorial.py file to display traffic in the connection. 
         print("Swtich:", event.connection,"source address :", packet.src,"destination address:", packet.dst)

   As an output we can see there is traffic in each switches. For "iperf h1 h2", s3 observes traffic & for "iperf h1 h8", s3, s2, s1, s5, s7 observe traffic. 
   This is in accordance to the binary tree topology created. 


-------------------------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------------------------- 

Task 3: MAC Learning Controller 

1. Describe how the above code works, such as how the "MAC to Port" map is established. You could use a ‘ping’ example to describe the establishment process (e.g., h1 ping h2). 

   As "MAC to Port" map is established.
       •	When we enable the “act_like_switch” function, there is learning by mapping of each switch connection to a host.
       •	When the source MAC isn't in the map mac_to_port, script adds add the {MAC, Port} into the map mac_to_port.
       •	When the destination MAC is in the map, script sends the packet out the port associated with the MAC.
       •	When the destination MAC isn't in the map, script sends output to all ports except the input port.

-------------------------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------------------------- 

2. (Comment out all prints before doing this experiment) Have h1 ping h2, and h1 ping h8 for 100 times (e.g., h1 ping -c100 p2). 
   2a. How long did it take (on average) to ping for each case? 
      H1 ping h2 100 times:
      Average time to ping: 4.946ms
      H1 ping h8 100 times:
      Average time to ping: 18.015 ms
----------------------------------------------------------------------------------------------------------------------
   2b. What is the minimum and maximum ping you have observed? 
      H1 ping h2 100 times:
       minimum and maximum ping observed: 1.179 ms & 7.733 ms
      H1 ping h8 100 times.
       minimum and maximum ping observed: 3.944 ms & 46.686 ms
----------------------------------------------------------------------------------------------------------------------
   2c. Any difference from Task 2 and why do you think there is a change if there is? 
       Yes, there is a difference in all the three parameters:
       • average ping time: Task3 < Task 2. 
       •	There is decrease in minimum ping in task 3 compared to Task 2. 
       •	Overall, there is a decrease in ping time compared to task2. This because of “mac_to_port “is established. 
         There is no flooding of packets everywhere. Now the specific port to transfer the packet is learnt.


	                                               Task 2                                         Task 3

H1 ping h2 100 times: Average time to ping             5.527ms                                        4.946ms

H1 ping h8 100 times: Average time to ping             22.920 ms                                      18.015 ms

H1 ping h2 100 times: minimum and maximum        1.644 ms & 15.235 ms                            1.179 ms & 7.733 ms

H1 ping h8 100 times: minimum and maximum        5.333 ms & 42.299 ms                            3.944 ms & 46.686 ms

---------------------------------------------------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------------------------------------------------

3. Run “iperf h1 h2” and “iperf h1 h8”. 

   3a. What is the throughput for each case? 
       “iperf h1 h2”
         
mininet> iperf h1 h2
*** Iperf: testing TCP bandwidth between h1 and h2 
*** Results: ['34.7 Mbits/sec', '36.5 Mbits/sec']


       “iperf h1 h8”

mininet> iperf h1 h8
*** Iperf: testing TCP bandwidth between h1 and h8 
*** Results: ['2.81 Mbits/sec', '3.34 Mbits/sec']
mininet> 
----------------------------------------------------------------------------------------------------------------------

   3b. What is the difference from Task 2 and why do you think there is a change if there is? 

     Yes, there is visible difference in the throughput for “iperf h1 h2”. It has increased in case of Task3. 
     This is because now after the “mac_to_port “is established there is no flooding of packets everywhere.            
     Now the specific port to transfer the packet is learnt.

----------------------------------------------------------------------------------------------------------------------





