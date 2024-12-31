# Ethernet_Channel-L2-
Here's an updated README file that includes details about the **logical link (EtherChannel)** and its benefits.

---

# ** Network Configuration Details**

This document provides a complete guide to the network configuration, including Spanning Tree Protocol (STP) setup, EtherChannel configuration, logical link concepts, and testing procedures.

---

## **1. Configuring the Root Bridge (M1)**

The **Spanning Tree Protocol (STP)** is configured to prevent loops and ensure optimal path selection. **M1** is set as the root bridge for VLAN 1.

### **Steps to Configure the Root Bridge:**

On **M1**, execute the following command:
```bash
M1(config)# spanning-tree vlan 1 root primary
```

- This sets **M1** as the root bridge for VLAN 1 by assigning it the lowest bridge priority.
- To verify root bridge status, use:
  ```bash
  show spanning-tree vlan 1
  ```

---

## **2. Configuring EtherChannel with Trunking**

EtherChannel allows bundling multiple physical links into a **logical link**, improving redundancy and increasing bandwidth between switches. This is applied between **M1** and **M2**.

### **Steps for M1 Configuration:**

1. **Select the Range of Interfaces to be Bundled:**
   ```bash
   M1(config)#interface range FastEthernet 0/1-4
   ```

2. **Create an EtherChannel Group:**
   ```bash
   M1(config-if-range)#channel-group 1 mode desirable
   ```
   - The `desirable` mode enables **LACP** (Link Aggregation Control Protocol), which actively negotiates EtherChannel formation.

3. **Configure Interfaces as Trunk Ports:**
   ```bash
   M1(config-if-range)#switchport trunk encapsulation dot1q
   M1(config-if-range)#switchport mode trunk
   ```

4. **Access the Port-Channel Interface and Set Trunking:**
   ```bash
   M1(config)#int port-channel 1
   M1(config-if)#switchport trunk encapsulation dot1q
   M1(config-if)#switchport mode trunk
   ```

### **Steps for M2 Configuration:**

Repeat the same configuration on **M2** to ensure consistency with **M1**:

```bash
M2(config)#interface range FastEthernet 0/1-4
M2(config-if-range)#channel-group 1 mode desirable
M2(config-if-range)#switchport trunk encapsulation dot1q
M2(config-if-range)#switchport mode trunk
M2(config)#int port-channel 1
M2(config-if)#switchport trunk encapsulation dot1q
M2(config-if)#switchport mode trunk
```

---

## **3. Logical Link (Port-Channel)**

### **What is a Logical Link?**
- A **logical link** refers to the aggregated connection created by combining multiple physical links into a single virtual interface called **Port-Channel**.
- In this setup, the **Port-Channel 1** acts as a single interface for the switches **M1** and **M2**, even though it consists of four physical links (`FastEthernet 0/1-4`).

### **Benefits of a Logical Link:**
1. **Increased Bandwidth:**
   - The bandwidth of all physical links is combined (e.g., 4 × 100 Mbps = 400 Mbps).
2. **Redundancy:**
   - If one physical link fails, the logical link remains operational using the remaining links.
3. **Simplified STP Operation:**
   - STP treats the entire EtherChannel as a single link, reducing complexity and preventing loops.
4. **Improved Load Balancing:**
   - Traffic is distributed across the physical links within the EtherChannel.

---

## **4. Verifying the Configuration**

### **a. Verify Root Bridge Configuration:**
```bash
show spanning-tree vlan 1
```
- Confirms that **M1** is the root bridge for VLAN 1.

### **b. Verify EtherChannel Status:**
```bash
show etherchannel summary
```
- Displays the operational status of the EtherChannel, including the interfaces in the group.

### **c. Verify Logical Link (Port-Channel) Trunking:**
```bash
show interfaces port-channel 1
```
- Verifies the operational status and configuration of the Port-Channel interface.

### **d. Verify Trunking Configuration:**
```bash
show interfaces trunk
```
- Ensures trunking is properly configured on the logical link.

---

## **5. Network Testing**

### **a. Connectivity Testing:**
- Use the `ping` command to ensure connectivity between devices. Examples:
  ```bash
  ping 192.168.0.2
  ping 192.168.0.3
  ```
  Test from **Laptop0** to devices like **PC0** or **PC3**.

### **b. Traffic Simulation:**
- Use simulation tools in **Cisco Packet Tracer** to visualize traffic flow through the logical link.

---

## **6. Saving Configuration**

Always save the running configuration to avoid losing changes after a reboot:
```bash
copy running-config startup-config
```

---

## **7. Summary of Configuration**

### **Root Bridge:**
- **M1** is configured as the root bridge for VLAN 1.

### **EtherChannel (Logical Link):**
- An EtherChannel is configured between **M1** and **M2** using `FastEthernet 0/1-4`.
- The EtherChannel operates in trunk mode with `dot1q` encapsulation.

### **Verification Commands:**
- `show spanning-tree vlan 1`
- `show etherchannel summary`
- `show interfaces port-channel 1`
- `show interfaces trunk`

### **Testing:**
- Ensure network connectivity using `ping` and traffic simulation.

---

This configuration ensures a robust and redundant network, leveraging the benefits of logical links (EtherChannel) and STP. For further modifications or troubleshooting, refer to the commands provided.

--- 

Let me know if you need any additional details!
