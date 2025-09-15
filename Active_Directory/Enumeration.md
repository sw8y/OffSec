# Active Directory Enumeration

## Unauthenticated 

### Host Discovery
- [ ] Network mapping: fping *switch subnet* or nmap -sn *subnet*
    Example: *fping -agq 10.0.0.0/24
        -a: list alive systems
        -g: create target list from subnet
        -q: quiet mode

- [ ] Port scanning: nmap -p *comma_separated_list_of_ports* -sV -sC -iL *file_containing_alive_hosts.txt*
    -sV: Version detection of services running on open ports
    -sC: Runs Nmap Scripting Engine (NSE) scripts in the default category
    -iL: Read the list of target hosts from the provided hosts

- Common ports and protocols associated with Active Directory usage on Domain Controllers:
| Port | Protocol | Meaning | <br>
| ----------- | ----------- | ----------- | <br>
| 88 | Kerberos | Potential for Kerberos-based enumeration <br>
| 135 | MS-RPC | Potential for RPC enumeration (null sessions) <br>
| 139 | SMB/NetBIOS | Legacy SMB access <br>
| 389 | LDAP | LDAP queries to AD <br>
| 445 | SMB | Modern SMB access, critical for enumeration <br>
| 464 | Kerberos (kpasswd) | Password-related Kerberos service <br>

## Authenticated 