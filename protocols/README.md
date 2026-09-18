# File Transfer Protocol (FTP)

### TFTP Commands

**`connect`** Sets the remote host, and optionally the port, for file transfers.

**`get`** Transfers a file or set of files from the remote host to the local host.

**`put`** Transfers a file or set of files from the local host onto the remote host.

**`quit`** Exits tftp.

**`status`** Shows the current status of tftp, including the current transfer mode (ascii or binary), connection status, time-out value, and so on.&#x20;

**`verbose`** Turns verbose mode, which displays additional information during file transfer, on or off.

* Default configuration of vsFTPd can be found in `/etc/vsftpd.conf`
* The `ftpusers` file (found at `/etc/ftpusers`) is used to deny certain users access to the FTP service.

### Dangerous Settings

`anonymous_enable=YES`

`anon_upload_enable=YES`

`anon_mkdir_write_enable=YES`

`no_anon_password=YES`

`anon_root=/home/username/ftp`

`write_enable=YES`

### Anonymous Login

* Login with `ftp x.x.x.x` command to connect to the server and then use the name "anonymous" to login if anonymous login is enabled.

#### Download All Available Files

`wget -m --no-passive ftp://anonymous:anonymous@10.129.14.136`

#### Upload a File

`ftp> put testupload.txt`

### Footprinting The Service

Looking for FTP-related Nmap Scripts

`find / -type f -name ftp* 2>/dev/null | grep scripts`&#x20;

`sudo nmap -sV -p21 -sC -A 10.129.14.136`

`sudo nmap -sV -p21 -sC -A 10.129.14.136 --script-trace`

#### Service Interaction

`nc -nv 10.129.14.136 21`

`telnet 10.129.14.136 21`

`openssl s_client -connect 10.129.14.136:21 -starttls ftp`
