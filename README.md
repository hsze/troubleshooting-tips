# Troubleshooting-tips
Cloud Networking is all about automation and agility. The tools and troubleshooting methodology from on-prem networking is not always applicable.  Here are some useful commands and tips for troubleshooting in Azure Networking.

# Commands:
Ping/ICMP may not work (e.g. if you're testing reachability of a Standard Public IP or workload behind a Standard Load Balancer).  Instead for Windows VMs:
1. Powershell command "test-netconnection": e.g. test-netconnection 10.0.0.4 -Port 3389
2. PSPING: download [PSPING](https://technet.microsoft.com/en-us/sysinternals/psping.aspx)
For Linux VMs:
1. tcpping: download as part of tcptraceroute package (sudo apt-get install tcptraceroute) 

# Tools:
1. Netmon and Wireshark on Windows VMs will be very helpful
2. tcpdump

Web Server:
It is often helpful just to install a web server on the VM to test HTTP/HTTPs.
1. On Windows Server, install IIS
Set-AzVMExtension `
  -ResourceGroupName myResourceGroup `
  -ExtensionName IIS `
  -VMName myVM `
  -Publisher Microsoft.Compute `
  -ExtensionType CustomScriptExtension `
  -TypeHandlerVersion 1.4 `
  -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
  -Location myLocation
(replace myResourceGroup, myVM and myLocation)
   To test, connect to:  http://myVM

2. On Linux, install NGINX (sudo apt install nginx) 
   To test, connect to:  http://nginxIP

And remember to allow ICMP which by default is not enabled on Windows machines.


