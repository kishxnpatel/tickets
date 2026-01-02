# TKT 002 VPN connected, but no access to DNS and routing

## Symptoms
User connects to VPN successfully but cannot access internal resources.
Common reports include file shares not opening, internal websites not loading, or RDP failing.
Internet may work, but internal resources fail, or everything fails after the VPN connects.

## Environment
Device and OS Windows 10 or Windows 11
Connection type: VPN client
Typical internal dependencies: DNS suffix search, internal DNS servers, routes to private subnets

## Triage
Step 1: Confirm what is not working. Check one internal hostname, one internal IP, and one public website. <br/>
Step 2: Confirm the VPN tunnel is actually established. Check the VPN status in the client and confirm the assigned VPN adapter IP. <br>
Step 3: Determine if the failure is name resolution or routing. Test internal hostname versus internal IP.

## Diagnostics commands and logs
```powershell
# 1) Confirm IP configuration and DNS servers
ipconfig /all

# 2) Quick connectivity tests
ping <internal-ip>
ping <internal-hostname>

# 3) DNS resolution tests
nslookup <internal-hostname>
nslookup <internal-hostname> <internal-dns-server-ip>

# 4) Check the route table for internal subnets
route print

# 5) Test TCP reachability to a known internal service
Test-NetConnection <internal-hostname-or-ip> -Port 445
Test-NetConnection <internal-hostname-or-ip> -Port 3389

# 6) Show active DNS client settings
Get-DnsClientServerAddress
Get-DnsClientGlobalSetting

# 7) Optional adapter reset if needed
ipconfig /flushdns
ipconfig /registerdns
