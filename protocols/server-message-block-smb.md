# Server Message Block (SMB)

### Samba Default Configuration

`cat /etc/samba/smb.conf | grep -v "#|;"`

### Connecting with SMBClient & SMBMap

`smbclient -N -L //10.129.14.128`

`smbclient //10.129.14.128/notes`

`smbmap -H 10.129.14.128`

#### Downloading Files

`get filename.txt`

#### Samba Server Status

`smbstatus`

#### Footprinting SMB with Nmap

`sudo nmap 10.129.14.128 -sV -sC -p139,445`
