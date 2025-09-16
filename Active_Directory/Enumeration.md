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

### LDAP Enumeration
- [ ] ldapsearch -x -H ldap://10.211.11.10 -s base
    -x: Simple authentication, in our case, anonymous authentication
    -H: Specifies the LDAP server
    -s: Limits the query only to the base object and does not search subtrees or children
- [ ] More refined to Domain: ldapsearch -x -H ldap://10.211.11.10 -b "dc=FIRST_COMPONENT,dc=SECOND_COMPONENT" "(objectClass=person)"

### RPC Enumeration (Null Sessions)
- [ ] Unathenticated: rpcclient -U "" 10.211.11.10 -N
    -U: Used to specify the username, in our case, we are using an empty string for anonymous login
    -N: Tells RPC not to prompt us for a password
- [ ] Get user info using RPC: enumdomusers 
- [ ] Get group info using RPC: enumdomgroups
- [ ] Get password info using RPC: getdompwinfo
    More info: https://cheatsheet.haax.fr/network/services-enumeration/135_rpc/ 

 
## Authenticated 

### AS-REP Roasting
* ==IMPORTATNT:== This technique requires that Kerberos pre-authentication be disabled. The specific flag to look for is (UF_DONT_REQUIRE_PREAUTH). 
- [ ] Indentifying vulnerable accounts: 