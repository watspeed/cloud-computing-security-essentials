# Lab 5: Networking Lab

To debug network and security problems, network administrators need to examine the configuration and status of the network on cluster nodes. There are a set of tools that enable inspecting and changing the configuration of the network devices and software stack. This lab will introduce students to a set of these tools. In particular, students will learn commands related to:

- Inspecting the configuration of the network devices
- Checking the status of the network stack

All the commands will be run using the command line interface (CLI). The CLI is a powerful tool for running applications, inspecting the system status, and configuring a machine.

First, open a terminal application. Click on the terminal application on the desktop.

![](../images/lab1_fig1.png)

The terminal opens showing the shell prompt. The shell allows running commands and applications.

![](../images/lab1_fig2.png)

## 1. Check Local Machine Network with `ifconfig`

To check the configuration of the local node, one commonly used command is `ifconfig`. `ifconfig` is a legacy command and is slowly being replaced by the `ip` command. Nevertheless, it is still widely supported. Users can access important information such as their network interface's IP address (Internet Protocol address), subnet mask, default gateway, and DNS server configurations by running the `ifconfig` command. It is a useful tool for quickly diagnosing network connectivity difficulties, validating network settings, and acquiring critical network information.

• Run `ifconfig` with no arguments to print essential information about all active network interfaces:

```bash
ifconfig
```

![](../images/lab8-1.png)

The output shows crucial data about the network interfaces, like:

- The names of the active network interfaces (e.g., eth0 and lo, the loopback interface).
- The hardware MAC address.
- The IP address (inet), netmask, and broadcast address.
- The MTU (Maximum Transmission Unit) value.

In the output above the interface eth0 has the IPv4 address of 10.0.0.4 and IPv6 address of fe80::222::48ff:fe21:1445.

• Display more details with `ifconfig`

The verbose option (`-v`) prints a more detailed output. Depending on the system, the outcome is either the same as `ifconfig` without arguments or slightly more in-depth.

```bash
ifconfig -v
```

The output of the command shows additional stats about each interface, including the number of packets transmitted (TX) or received (RX) and stats about all transmission errors.

### Controlling Interfaces

`ifconfig` can also be used to disable or enable an interface.

To disable the `lo` interface, you can use the following command:

```
sudo ifconfig lo down
```

Use `ifconfig` to verify that the interface is down.

To bring an interface up again, you can use the following command:

```
sudo ifconfig lo up
```
Verify it with `ifconfig`.

## 2. Check Local Machine Network with `ip`
The `ip` command is the replacement for `ifconfig` and can do the same functionality and more. To list all the devices on a machine, use the following command:

```
ip link show
```

The output shows each interface.

To view the IP addresses as well, use the following command:

```
ip addr
```

![](../images/lab8-5.png)

The output shows the IPv4 and/or IPv6 addresses.

To check the states for the interface, use the "-s" option:

```
ip -s -s link show
```

![](../images/lab8-6.png)

This shows the states on the transmission and reception side, including the number of packets processed and errors experienced.

## 3. Checking the Status of the Connections on a Machine

### `netstat`

The `netstat` command, as the name suggests, displays an overview of all the network connections in the device. The table shows details about the connection protocol, address, and the current state of the network.

The `netstat` command can be used without parameters to display active network connections:

```
netstat
```

![](../images/lab8-2.png)

The first list in the output displays details of active internet connections across six columns:
- Proto: Protocol of the connection (e.g., TCP, UDP).
- Recv-Q: Number of bytes received and waiting in the queue to be processed.
- Send-Q: Number of bytes waiting in the queue to be sent.
- Local address: Local address and port of the connection. An asterisk (*) in the host indicates that the server is listening on all available interfaces, and a port may not yet be established.
- Foreign address: Remote address and port of the connection. An asterisk (*) appears if a connection is not yet established.
- State: State of the local socket. Values include ESTABLISHED, LISTENING, CLOSED, or blank.

The second list shows all active Unix Domain open sockets with the following details:
- Proto: Protocol used by the socket (always unix).
- RefCnt: Reference count of the number of processes attached to this socket.
- Flags: Flags associated with the socket, usually ACC or blank.
- Type: The socket type.
- State: State of the socket, most often CONNECTED, LISTENING, or blank.
- I-Node: File system inode (index node) associated with this socket.
- Path: File system path to the socket.
  
You can expand the netstat command with options to filter or customize output:
```
netstat [option]
```

OPTION	  DESCRIPTION
- `-a`	  Display all active TCP and UDP connections and listening ports.
- `-t`	  Display only TCP connections.
- `-u`	  Display only UDP connections.
- `-l`	  Show only sockets that are actively listening for incoming connections.
- `-p`	  Display the Process ID (PID) and program name associated with each socket.
- `-n`	  Display addresses and port numbers in numeric format without resolving names.
- `-r`	  Display the kernel routing table.
- `-s`	  Provide summary statistics for each protocol.
- `-c`	  Continuously update the output every second.
- `-e`	  Display extended information, including timers and user IDs.

### `ss`

Another command that can display connection information is the `ss` command.

Use the `ss` command without arguments to list all connections on a machine:

```
ss
```

![](../images/lab8-3.png)

As with `netstat`, you can expand the `ss` command with options to filter or customize output.

## 4. Checking Network Operations

### `ping`

`ping` is the primary command used to troubleshoot connectivity, reachability, and name resolution. It allows admins to test if the network stack works properly, if the network is up, and if a certain destination is reachable. `ping` sends Internet Control Message Protocol (ICMP) echo request messages to a given destination. The destination will echo back the message to the sender. The tool measures the time and displays the round-trip times.

For instance, to check the connectivity to facebook.com, use the following command:

```
ping www.facebook.com
```

![](../images/lab8-4-u.png)

The output shows the Facebook server contacted for the ping (star-mini.c10r.facebook.com) and its IP address (31.13.66.35). It also shows multiple trials for ping and their round-trip times. Once the command is interrupted with `CTRL + C`, it shows stats about all trials and the average round-trip time.

### `traceroute`

Another command helpful for checking and debugging connectivity is `traceroute`. `traceroute` is a command-line utility that you can use to trace the path that an Internet Protocol (IP) packet takes to its destination. This tool helps debug connectivity issues and detect any anomalies in the path. It can also be used to detect if a malicious entity has hacked the network to change the path to go through untrusted networks.

`traceroute` determines the route to a destination by sending ICMP echo packets to the destination. In these packets, `traceroute` uses varying IP Time-To-Live (TTL) values. Because each router along the path is required to decrease the packet's TTL by at least 1 before forwarding the packet, the TTL is effectively a hop counter. When the TTL on a packet reaches zero (0), the router sends an ICMP "Time Exceeded" message back to the source computer.

`traceroute` sends the first echo packet with a TTL of 1 and increments the TTL by 1 on each subsequent transmission, until the destination responds or until the maximum TTL is reached. The ICMP "Time Exceeded" messages that intermediate routers send back show the route. Note, however, that some routers silently drop packets that have expired TTLs, and these packets are invisible to `traceroute`.

`traceroute` prints out an ordered list of the intermediate routers that return ICMP "Time Exceeded" messages. Using the `-d` option with the `traceroute` command instructs `traceroute` not to perform a DNS lookup on each IP address, so that `traceroute` reports the IP address of the near-side interface of the routers.

Compared to `ping`, the primary difference between `ping` and `traceroute` is that while `ping` simply tells you if a server is reachable and the time it takes to transmit and receive data, `traceroute` details the precise route info, router by router, as well as the time it took for each hop.

Some cloud providers do not support `tracerout` in their platform since it can be used by malicious users to infer network details. This lab does not support `traceroute`, but it is a valuable tool that you can use in your personal work.

## 5. Inspecting the Network Communication

### `tcpdump`

`tcpdump` is a command-line tool used to capture and examine network traffic on your system. It's commonly used for troubleshooting network problems and as a security tool. `tcpdump` is very flexible, with many options and filters to suit different needs. Since it runs in the command line, it's perfect for use on remote servers or devices without a graphical interface.

To use `tcpdump`, we need to determine which network interface is active using `ifconfig` or `ip addr` as we mentioned earlier. We will use `eth0` interface to monitor network traffic. Since `tcpdump` works with IP addresses, resolve `www.facebook.com` to its IP address using the `ping` command.

```
ping www.facebook.com
```

The output is something like this:
```
PING star-mini.c10r.facebook.com (31.13.66.35) 56(84) bytes of data.
```
In this case, the IP address of `www.facebook.com` is `31.13.66.35`.

You will run `tcpdump` to capture traffic to and from the resolved IP address (31.13.66.35) with the active network interface `eth0`. Use the following command, replacing the IP address with the one that came up in your output if it isn't 31.13.66.35:

```
sudo tcpdump -i eth0 host 31.13.66.35
```

While `tcpdump` is running, we need to generate network traffic to `www.facebook.com` by using a tool like `curl`. Open a second terminal windown and run this command:

```
curl www.facebook.com
```

This will send HTTP requests, and `tcpdump` will capture the packets. The output of `tcpdump` will display the captured packets as shown in the following figure:

![](../images/lab8-7.png)

In the output above, we can see the packets involved in establishing a TCP connection and sending an HTTP GET request. 

- TCP Handshake:
    - The first packet is the initial request to establish a TCP connection, indicated by the SYN flag ([S]). It is sent from the node with the hostname "labvm-1568140.internal.cloudapp.net.41268" to the Facebook server "edge-star-mini-shv-01-iad3.facebook.com.http".
    - In the second packet, the Facebook server responds with a SYN-ACK ([S.]), acknowledging the request and sending its own SYN.
    - In the third packet, the client sends an ACK ([.]), completing the 3-way TCP handshake.


- HTTP GET Request:
    - The fourth packet contains the HTTP GET request from the client, requesting the root resource (/) over HTTP 1.1. This marks the start of the actual data exchange over the established TCP connection.


- HTTP Response:
    - The server responds with "HTTP/1.1 301 Moved Permanently," indicating a redirection (possibly to an HTTPS version of the site or another domain). This demonstrates the transition from the TCP layer to the HTTP protocol.


- Connection closing:
    - Subsequent packets include the graceful termination of the connection using FIN flags ([F.]), where both sides agree to close the connection.

`tcpdump` allows for inspecting the connection and the TCP state for packets sent or recieved on the interface. 

### `Tshark/Wireshark`
`Wireshark` is another tool used to capture and inspect packets in real-time. It allows users to monitor network traffic, troubleshoot issues, and analyze communication between devices. `Wireshark` provides detailed insights into network protocols such as DNS, HTTP, TCP, and UDP. It comes with two tools. `tshark` is a command line tool that allows for scripting and automating inspection, and `wireshark` is the same tool with a graphical user interface that allows user driven inspection and exploration of the network traffic. `Wireshark` comes with a graphical interface to help with filtering, capturing, and analyzing packets, helping users identify connectivity problems, security vulnerabilities, or performance bottlenecks in networks.

- Launch `Wireshark` from the virtual machine desktop.
- You will see a list of available network interfaces, choose the interface connected to the internet `eth0`.
- We will inspect the traffic on the `eth0` interface. Double-click the interface to start capturing packets

  ![](../images/lab8-8.png)
  
- Now we need to generate some traffic on the interface. We will visit the `www.facebook.com` site. Use the following command to get the facebook page.

  ```
  curl www.facebook.com
  ```

- Check the Wireshark interface for the captured packets. You will see a long list of packets captured at the interface.

    ![](../images/lab8-wireshark-111.png)

- To filter traffic while capturing or viewing, use the filter bar at the top.
- There are some common filters you can use like:
  * DNS Traffic: `dns`

    You can use the filter `dns` in the filter bar to isolate DNS packets. In the output you will see a DNS query that requests the IP address for `www.facebook.com` domain and DNS response.
    When a system tries to resolve a domain name, it sends a DNS query to request the corresponding IP address. Modern systems often request both IPv4 (A record) and IPv6 (AAAA record) addresses, resulting in two queries. However, for simplicity, we will focus on the IPv4 response.

    Click on one of the packets at the top of the screen. This will show the packets details in the lower screen.
    The DNS query (Packet No. 431) asks for the IPv4 address of `www.facebook.com`.
    The DNS response (Packet No. 433) on the screen, shows the DNS response with the IPv4 address.
    
    ![](../images/lab8-wireshark-dns-111.png)

    The response indicates that the facebook IP address is `157.240.229.35`

  * HTTP Traffic: `http`

    ![](../images/lab8-wk-http-111.png)

    In this output you see all the HTTP related packets. The output shows multiple HTTP request and response packets. This is because modern websites may store different objects such as images and videos on different servers and are retrieved through a separate HTTP request.

    - Find the first HTTP request. In our example it is the first HTTP GET request at the top (No. 438).
  The lower screen shows the packet details. It shows all the header of the packets including ethernet, IP, TCP, and HTTP. The HTTP request shows a GET request for www.facebook.com

    - Following that request there is a HTTP response.
      Find the packet in your output. In our output it is packet No. 443
  
      ![](../images/lab8-wk-http-222.png)
  
      The response states response code of 200. Which indicates a successful processing of the HTTP request.
      At the end of the response we can see the bytes of the returned page.
    
    - Also, you can filter the packets using the host name: `http.host == "www.facebook.com"` 

      ![](../images/lab8-wk-http-333.png)

## 6. Inspect Network Vulnerability with `nmap`

### `nmap`
The `nmap` (Network Mapper) command is a powerful tool used for network exploration and security auditing. It allows you to discover hosts, services, and open ports on a network.

You need to install `nmap` using the following instructions:

- Update System Package Repository:
  ```
  sudo apt update
  ```
  
- Install Nmap:
  ```
  sudo apt install nmap -y
  ```

- Verify Installation:
  ```
  nmap --version
  ```

The following example shows how `nmap` works. The command `nmap www.facebook.com` was used to scan a public web server to discover accessible services.
    
  ![](../images/lab8-444-u.png)

The `nmap` output provides an overview of the publicly accessible services on the target domain `www.facebook.com`. The scan starts by identifying that the host is online and reachable, confirming connectivity. Then, it shows the domain resolves to the IP address "157.240.22.35", which is the primary target of the scan. Also, there is a reverse DNS (rDNS) lookup of the IP address which reveals the hostname "edge-star-mini-shv-01-sjc3.facebook.com", which is a server used by Facebook. 
The `nmap` results show that two ports are open:
- Port 80 (HTTP): which is used for standard, unsecured web traffic.
- Port 443 (HTTPS): which is used for secure, encrypted web traffic over HTTPS. 

## Terminate the Shell Session

Before validating the lab, you must exit the shell session. Run the following command to exit:
```
exit
```

## You have successfully completed this lab.

To confirm your completion for this lab, go to the **Lab Validation** tab in the navigation bar at the top of this guide and click the **Validate** button for each task. To receive credit for the lab, submit a screenshot of your **Lab Validation** tab to the course dropbox.
