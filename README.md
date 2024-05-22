# Tun2socks Guide - Using a Socks5 Proxy with Wintun Device on Windows

This guide provides detailed instructions for configuring a Socks5 proxy with a wintun device on Windows systems. Follow the steps below to set up your Socks5 proxy connection.

## Prerequisites

- Make sure you have `tun2socks` installed and configured correctly on your system.
- You will also need details of your Socks5 proxy server (`host` and `port`).
- Administrative privileges on your system to execute network configuration commands.

## Configuration Steps

1. **Set Up the Socks5 Proxy:**
   Configure the tun2socks to use the Socks5 proxy by replacing `host` and `port` with your proxy server's host address and port number.

   ```bash
   tun2socks -device wintun -proxy socks5://host:port
   ```

2. **Find the Default Gateway:**
   Identify the IP address of your default gateway using this command:

   ```bash
   ipconfig | findstr /r /c:"Default Gateway.*:"
   ```

3. **Configure Wintun Device IP Address and Subnet Mask:**
   Set the IP address and subnet mask of the wintun device. Replace `10.10.10.1` and `255.255.255.0` with your chosen IP and subnet mask.

   ```bash
   netsh interface ip set address name=wintun source=static addr=10.10.10.1 mask=255.255.255.0 gateway=none
   ```

4. **Add DNS to Wintun Device:**
   Add a DNS server to your wintun device configuration:

   ```bash
   netsh interface ip add dns name=wintun addr=1.1.1.1
   ```

5. **Disable IPv6 Forwarding:**
   For security, disable IPv6 forwarding on the wintun device:

   ```bash
   netsh interface ipv6 set interface wintun forwarding=disabled
   ```

6. **Set Default Route for Wintun Device:**
   Configure the default route for all traffic to go through the wintun device:

   ```bash
   route add 0.0.0.0 mask 0.0.0.0 10.10.10.1 metric 5
   ```

7. **Add Route to Specific Server IP:**
   If needed, add a route to ensure traffic for a specific server IP goes through the correct gateway. Replace `[server ip]` and `[Default Gateway]` with the actual IPs.

   ```bash
   route add [server ip] mask 255.255.255.255 [Default Gateway] metric 5
   ```

## Notes

- Replace placeholders (e.g., `[server ip]`, `host`, `port`, `10.10.10.1`) with actual values before running the commands.
- The steps involve making significant changes to your network configuration. Ensure you understand each step and its consequences.
- These instructions are for the Windows command-line interface and assume you are running the commands with administrative privileges.

## Troubleshooting

If you encounter any issues, ensure that:

- The `host` and `port` details are correct for your Socks5 proxy server.
- There are no typos in the commands you have entered.
- You have the necessary privileges to alter your network settings.
- Your firewall or antivirus software is not blocking `tun2socks` connections.

For detailed `tun2socks` documentation, refer to the official tun2socks repository.
