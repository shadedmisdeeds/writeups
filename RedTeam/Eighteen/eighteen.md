[Eighteen](https://app.hackthebox.com/machines/Eighteen) is a seasonal windows machine hosted on Hackthebox. Before we begin, we've been given a set of credentials:
`As is common in real life Windows penetration tests, you will start the Eighteen box with credentials for the following account: kevin / iNa2we6haRj2gaw!`\
To start, we'll do the usual nmap scan.

```
Nmap scan report for eighteen.htb (10.10.11.95)
Host is up (0.28s latency).
Not shown: 997 filtered tcp ports (no-response)
PORT     STATE SERVICE  VERSION
80/tcp   open  http     Microsoft IIS httpd 10.0
| http-methods: 
|_  Supported Methods: HEAD OPTIONS GET
|_http-server-header: Microsoft-IIS/10.0
|_http-title: Welcome - eighteen.htb
1433/tcp open  ms-sql-s Microsoft SQL Server 2022 16.00.1000.00; RTM
| ms-sql-ntlm-info: 
|   10.10.11.95:1433: 
|     Target_Name: EIGHTEEN
|     NetBIOS_Domain_Name: EIGHTEEN
|     NetBIOS_Computer_Name: DC01
|     DNS_Domain_Name: eighteen.htb
|     DNS_Computer_Name: DC01.eighteen.htb
|     DNS_Tree_Name: eighteen.htb
|_    Product_Version: 10.0.26100
| ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback
| Issuer: commonName=SSL_Self_Signed_Fallback
| Public Key type: rsa
| Public Key bits: 3072
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-11-18T19:46:12
| Not valid after:  2055-11-18T19:46:12
| MD5:   3f03:dce5:52a3:d674:19b1:6834:473d:5284
|_SHA-1: 451d:dcd6:73b7:08fb:9648:6768:04fd:5a54:875f:e81e
| ms-sql-info: 
|   10.10.11.95:1433: 
|     Version: 
|       name: Microsoft SQL Server 2022 RTM
|       number: 16.00.1000.00
|       Product: Microsoft SQL Server 2022
|       Service pack level: RTM
|       Post-SP patches applied: false
|_    TCP port: 1433
|_ssl-date: 2025-11-18T21:45:59+00:00; +7h00m00s from scanner time.
5985/tcp open  http     Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running (JUST GUESSING): Microsoft Windows 2022|2012|2016 (88%)
```

We can see that we have a web server, an MSSQL server, and winRM is potentially up. We'll check out the web server first.

![](./images/img1.png)

The webserver seems to be running some finance tracking app. I'll make sure to traverse this site and build up my site map on burpsuite. I made sure to check out our credentials. They didn't work.

Another thing to note is that WebDAV is disabled.



