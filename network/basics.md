**Networking Basics: A Comprehensive Guide**

## **Concepts and Explanations**

| **Concept**          | **Explanation** |
|----------------------|---------------------------------------------------------------|
| **Switching**        | Connects devices in a LAN using MAC addresses. |
| **Routing**         | Connects different networks using IP addresses. |
| **Router’s Two IPs** | LAN IP (e.g., 192.168.1.1) for local network, WAN IP (e.g., 103.21.200.15) for internet. |
| **How Communication Works** | Devices send data to gateway → router → internet → destination. |
| **Gateway**         | The IP address used to leave a network (e.g., 192.168.1.1). |
| **Destination IP**  | The final address where a packet is sent (e.g., Google’s server). |
| **IP Address**      | A unique identifier for devices. Can be private (e.g., 192.168.1.x) or public (e.g., 103.x.x.x). |
| **Routing Table**   | A list of rules telling the router where to send packets. |
| **Default Route**   | The route used when no specific route exists (0.0.0.0/0). |

## **1. Introduction to Networking**
A network is a collection of interconnected devices that communicate with each other using network protocols. Devices in a network use IP addresses to identify themselves and communicate.

## **2. Network Interfaces and Addresses**
- **Network Interface:** A physical or virtual interface that connects a device to a network (e.g., Ethernet, Wi-Fi).
- **IP Address:** A unique identifier assigned to a device in a network.
- **Network Address:** The address that represents the entire network (e.g., `192.168.1.0/24`).
- **Subnet Mask:** Defines the range of addresses within a network. Example: `255.255.255.0` means a `/24` network, supporting 256 addresses (0-255).

## **3. Switching and Routing**
### **Switching**
- Switches operate at Layer 2 (Data Link Layer) of the OSI model.
- They forward packets based on **MAC addresses**.
- Devices connected to a switch belong to the same local network.

### **Routing**
- Routers operate at Layer 3 (Network Layer).
- They forward packets between different networks based on **IP addresses**.
- A router has multiple interfaces, each assigned an IP from a different network.

## **4. Gateways and Internet Connectivity**
- **Gateway:** A device (often a router) that serves as the entry/exit point for a network.
- **Default Gateway:** The IP address of a router that forwards packets to external networks (e.g., the internet).
- **Default Route:** Instead of defining routes for all possible networks, a router uses a default route (`0.0.0.0/0`) to forward packets to the internet.

## **5. How Middle Devices Enable Communication (IP Forwarding)**
When two devices belong to different networks and communicate through an intermediate router, the following steps occur:
1. The router is assigned **two IP addresses**, one for each connected network.
2. IP forwarding is enabled on the router.
3. Routes are added to inform devices how to reach other networks.

### **Example:**
| **Device** | **IP Address** | **Network** |
|------------|----------------|--------------|
| A | 192.168.1.10 | 192.168.1.0/24 |
| B (Router, Interface 1) | 192.168.1.1 | 192.168.1.0/24 |
| B (Router, Interface 2) | 192.168.2.1 | 192.168.2.0/24 |
| C | 192.168.2.10 | 192.168.2.0/24 |

### **Steps to Enable Communication:**
1. Assign IP addresses to the router interfaces.
2. Enable IP forwarding.
3. Add routes so A knows to reach C via B and vice versa.

## **6. Important Linux Networking Commands**
### **Viewing and Managing Network Interfaces**
```bash
ip link show                 # Show available network interfaces
ip link set eth0 up          # Enable interface eth0
ip link set eth0 down        # Disable interface eth0
```

### **Configuring IP Addresses**
```bash
ip addr show                 # Show assigned IP addresses
ip addr add 192.168.1.1/24 dev eth0  # Assign an IP to interface eth0
ip addr del 192.168.1.1/24 dev eth0  # Remove an IP from interface eth0
```

### **Routing Configuration**
```bash
ip route show                # Show routing table
ip route add 192.168.2.0/24 via 192.168.1.1  # Add a route
ip route del 192.168.2.0/24  # Remove a route
```

### **Enabling IP Forwarding**
```bash
echo 1 > /proc/sys/net/ipv4/ip_forward  # Enable IP forwarding
sysctl -w net.ipv4.ip_forward=1        # Alternative way to enable forwarding
```

## **7. Summary**
- **Switches** operate within a local network using MAC addresses.
- **Routers** connect different networks using IP addresses.
- **Default gateways** forward traffic outside the local network.
- **IP forwarding** must be enabled for devices to communicate across networks.
- **Routing tables** define paths for packets to reach their destinations.

By using the listed commands, network administrators can configure, manage, and troubleshoot network connectivity effectively.

