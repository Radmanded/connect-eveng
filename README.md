# Connecting-Eve-NG

1. In EVE-NG, select the Cloud and select ManagementCloud0 interface.


2. Configure router

>Note: Get the management range from your browser. It should match your Eve-Ng environment

```
Router#conf t
Router (config)#hostname CiscoRouter
CiscoRouter(config)#username <name> priv 15 password password1
CiscoRouter(config)#ip domain-name cisco.com
CiscoRouter(config)#ip ssh ver 2
Please create RSA keys to enable SSH (and of atleast 768 bits for SSH v2).
CiscoRouter (config)#crypto key gen rsa mod 1024
The name for the keys will be: CiscoRouter.cisco.com
% The key modulus size is 1024 bits
% Generating 1024 bit RSA keys, keys will be non-exportable..
[OK] (elapsed time was 0 seconds)
CiscoRouter(config)#line
*Dec 22 20:00:31.797: %SSH-5-ENABLED: SSH 2.0 has been enabledvty
CiscoRouter(config)#line vty 0 4
CiscoRouter(config-line)#transport input ssh
CiscoRouter(config-line)#login local
CiscoRouter(config-line)#exi
CiscoRouter(config)#vrf definition MGMT
CiscoRouter(config-vrf)#address-family ipv4
CiscoRouter(config-vrf-af)#int g0/0
CiscoRouter (config-if)#vrf forwarding MGMT
CiscoRouter(config-if#p address <eve-ng ip address> 255.255.255.0
CiscoRouter(config-if)#no shut
```

2. SSH into device from workstation

- Ensure you are on same network
- Attempt to SSH into device

>Troubleshoot: If you get this error
`Unable to negotiate with 192.168.31.101 port 22: no matching key exchange method found. Their offer: diffie-hellman-group-exchange.
sha1, diffie-hellman-group14-sha1`

- On the workstation navigate to ssh_config file
`$ sudo nano /etc/ssh/ssh_config`
  1. uncomment these lines
    - `# StrictHostKeyChecking ask` > ` StrictHostKeyChecking no`
    - `# Ciphers aes128-ctr ,aes192-ctr ,aes256-ctr ,aes128-cbc,3des-cbc` ( remove #)
  2. Add to the bottom of the file
    - `KexAlgorithms +diffie-hellman-group14-shal`

