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

- Common ports and protocols associated with Active Directory usage on Domain Controllers: <br>

    | Port | Protocol | Meaning | 
    | ----------- | ----------- | ----------- | 
    | 88 | Kerberos | Potential for Kerberos-based enumeration | 
    | 135 | MS-RPC | Potential for RPC enumeration (null sessions) | 
    | 139 | SMB/NetBIOS Session Service | Legacy SMB access |
    | 389 | LDAP | LDAP queries to AD |
    | 445 | SMB | Modern SMB access, critical for enumeration | 
    | 464 | Kerberos (kpasswd) | Password-related Kerberos service | 
    | 636 | LDAPS | Secure LDAP; encrypted, but may expose AD structure if misconfigured |

### SMB Shares
- [ ] List shares using smbclient: smbclient -L //TARGET_IP -N
- [ ] List shares using smbmap: ./smbmap.py -H IP_ADDRESS
- [ ] Enumerate using enum4linux: enum4linux -a TARGET_IP
- [ ] Enumerate using nmap: nmap --script=smb-enum-shares TARGET_IP
- [ ] Access shares: smbclient //IP_ADDRESS/FOLDER -N

## Authenticated 