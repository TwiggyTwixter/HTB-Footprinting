## Summary

<p>
    FTP - File Transfer Protocol
        <ul>
            <li>Runs within Application Layer of the TCP/IP Stack.</li>
            <li>TCP ports <mark>20</mark> and <mark>21</mark></li>
            <li>Two modes: Active and Passive</li>
            <li>Active: Control Channel established on port 21 and communication is moved to port 20. Firewalls protecting the client can block this</li>
            <li>Passive: Server announces a port through which the client can establish the data channel</li>
            <li>FTP is a <mark>clear-text</mark> protocol</li>
        </ul>    
</p>
<p>
    TFTP - Trivial File Transfer Protocol
        <ul>
            <li>Simpler than FTP</li>
            <li>Does not provide user authentication</li>
            <li>Uses UDP</li>
        </ul>
</p>

<p>
    Useful Commands
    <ul>
        <li><mark>connect</mark> - Sets the remote host, and optionally the port, for file transfers.</li>
        <li><mark>get</mark> - Transfers a file or set of files from the remote host to the local host.</li>
        <li><mark>put</mark> - Transfers a file or set of files from the local host to the remote host</li>
        <li><mark>quit</mark> - Quits</li>
        <li><mark>status</mark> - Shows the current status of ftp, including transfer mode (ascii or binary), connection status, time-out value, and so on.</li>
        <li><mark>verbose</mark> - Turns verbose mode, which displays additional information during file transfer, on or off.</li>
    </ul>
</p>

## Default Config
<p>Based off vsFTPd. Default config can be found in /etc/vsftpd.conf</p>

<p>
    <ul>
        <li><mark>listen=NO</mark> - Run from inetd or as a standalone daemon?</li>
        <li><mark>listen_ipv6=YES</mark> - Listen on IPv6?</li>
        <li><mark>anonymous_enable=NO</mark> - Enable Anonymous access?</li>
        <li><mark>local_enable=YES</mark> - Allow local users to login?</li>
        <li><mark>dirmessage_enable=YES</mark> - Display active directory messages when users go into certain directories?</li>
        <li><mark>use_localtime=YES</mark> - Use local time?</li>
        <li><mark>xferlog_enable=YES</mark> - Activate logging of uploads/downloads?</li>
        <li><mark>connect_from_port_20=YES</mark> - Connect from port 20?</li>
        <li><mark>secure_chroot_dir=/var/run/vstftpd/empty</mark> - Name of an empty directory</li>
        <li><mark>pam_service_name=vsftpd</mark> - Name of the PAM service vsftpd will use</li>
        <li><mark>rsa_cert_file=/etc/ss;/certs/ssl-cert-snakeoil.pem</mark> - The last three options specify the location of the RSA certificate to use for SSL encrypted connections</li>
        <li><mark>rsa_private_key_file=/etc/ssl/private/ssl-cert-snakeoil.key</mark</li>
        <li><mark>ssl_enable=NO</mark</li>
    </ul>
</p>
<p>Blocked user files: /etc/ftpusers</p>

## Dangerous Settings

<p>
    <ul>
        <li><mark>anonymous_enable=YES</mark> - Allowing anonymous login</li>
        <li><mark>anon_upload_enable=YES</mark> - Allowing anonymous to upload files</li>
        <li><mark>anon_mkdir_write_enable=YES</mark> - Allow anonymous to create new directories</li>
        <li><mark>no_anon_password=YES</mark> - Ask anonymous for password</li>
        <li><mark>anon_root=/home/username/ftp</mark> - Directory for anonymous</li>
        <li><mark>write_enable=YES</mark> - Allow the usage of FTP commands: STORE, DELE, RNFR, RNTO, MKD, RMD, APPE, and SITE</li>
    </ul>
</p>

<p>Helpful settings during enumeration:
    <ul>
        <li><mark>dirmessage_enable=YES</mark> - Show a message when they first enter a new directory</li>
        <li><mark>chown_upload=YES</mark> - Change ownership of anonymously uploaded files</li>
        <li><mark>chown_username=username</mark> - User who is given ownership of anonymously uploaded files</li>
        <li><mark>local_enable=YES</mark> - Enable local users to login</li>
        <li><mark>chroot_local_user=YES</mark> - Place local users into their home directory</li>
        <li><mark>chroot_list_enable=YES</mark - Use a list of local users that will be placed in their home directory</li>
        <li><mark>hide_ids=YES</mark> - All user and group informaton in directory listings will be displayed as "ftp"</li>
        <li><mark>ls_recurse_enable=YES</mark> - Allows the use of recurse listings</li>
    </ul>
</p>


## Footprinting

```
sudo nmap -sV -p21 -sC -A <target IP>
```

<p>Options:
    <ul>
        <li>-sV - Probe open ports to determine service/version info</li>
        <li>-p21 - Scan port 21</li>
        <li>-sC - Use script. Same as --script=default</li>
        <li>-A - Enable OS detection, version detection, script scanning, and traceroute (aggressive scan)</li>
    </ul>
</p>
<p>--script-trace option can be used to see what commands nmap sends, what ports are used, and the responses received from the scanned server.</p>
<p>Can try to use netcat or telnet to interact with the server.</p>

## Pop Quiz

```
Target Host: 10.129.31.227
```

1. Which version of the FTP server is running on the target system?

```
sudo nmap 10.129.31.227 -sV -sC -p21 -A
PORT   STATE SERVICE VERSION
21/tcp open  ftp
| fingerprint-strings:
|   GenericLines:
|     220 InFreight FTP v1.1
|     Invalid command: try being more creative
|_    Invalid command: try being more creative
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port21-TCP:V=7.95%I=7%D=11/4%Time=6AEB4312%P=x86_64-pc-linux-gnu%r(Gene
SF:ricLines,74,"220\x20InFreight\x20FTP\x20v1\.1\r\n500\x20Invalid\x20comm
SF:and:\x20try\x20being\x20more\x20creative\r\n500\x20Invalid\x20command:\
SF:x20try\x20being\x20more\x20creative\r\n");
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running: Linux 4.X|5.X
OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5
OS details: Linux 4.15 - 5.19, Linux 5.0 - 5.14
Network Distance: 2 hops

TRACEROUTE (using port 443/tcp)
HOP RTT       ADDRESS
1   141.71 ms 10.10.14.1
2   141.89 ms 10.129.31.227
```
<p>FTP Server version will be output using the -A option</p>

2. Enumerate the FTP server and find the flag.txt file.

<p>anonymous logging allowed:</p>

```
ftp 10.129.31.227
Connected to 10.129.31.227.
220 InFreight FTP v1.1
Name (10.129.31.227:twiggy): anonymous
331 Anonymous login ok, send your complete email address as your password

```

<p>Use normal ftp/directory commands to find and download the flag</p>
