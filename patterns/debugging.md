# Pattern: Debugging AWS Networking

This pattern codifies the lessons learned from debugging the database proxy. When a connection hangs, follow these steps in order.

### 1. Isolate the Service
- **Test Locally:** Can the service (e.g., HAProxy) connect to its backend when triggered from `localhost`?
- **Result:** If this fails, the problem is the service's configuration. If it succeeds, the problem is in the network path.

### 2. Check the Destination's Inbound Rules
- **Security Groups:** This is the most common cause of timeouts.
- **Action:** Check the Security Group of the destination resource (e.g., the RDS instance). Ensure it has an inbound rule allowing traffic on the correct port from the source's IP address.
- **Note:** Remember to check both **public and private** IPs if you are unsure which one is being used.

### 3. Check the Source's Outbound Rules
- **Security Groups:** Less common, but check if the source's Security Group has any egress rules that would block the connection.

### 4. Check the Network Path
- **VPC Route Tables:** Is there a route that correctly directs traffic from the source to the destination?
- **`tcpdump`:** Use `tcpdump` on the destination host to see if packets are arriving. If they are not, the problem is almost certainly a routing or Security Group issue.

### 5. The Proxy/NAT Checklist (If the source or destination is a proxy)
If you are routing traffic *through* an instance, it has special requirements.

1.  **Routing Loop:** The proxy **must** be in a different subnet (with a different route table) from the client. Otherwise, its own traffic will be looped back to itself.
2.  **Source/Destination Check:** This **must** be disabled on the proxy's network interface.
3.  **Proxy's Own Security Group:** The proxy's SG must allow inbound traffic on the port it's proxying.
4.  **Kernel IP Forwarding:** `net.ipv4.ip_forward` must be set to `1`.
5.  **`iptables`:** An `iptables` rule (e.g., `REDIRECT` or `DNAT`) is often needed to forward the intercepted traffic to the local proxy service.
