# 🧩 Setup Domain Controller in Azure (Part 1)

This lab guides you through setting up a **Domain Controller (DC)** and a **Client VM** in Microsoft Azure. You’ll create a resource group, virtual network, and virtual machines, then configure IP and DNS settings for domain communication.

# Setup Domain Controller in Azure


## Create a Resource Group

 Home-> Resource Group, click on the create button and it will take you to a screen like this:
 <img width="1920" height="1080" alt="Screenshot (5)" src="https://github.com/user-attachments/assets/92fd263c-378e-417c-ae97-23b15fc78e8c" />

 

Put any credentials you want. Then press Review + Create



Create a Virtual Network and Subnet

Home-> Virtual Networks -> Create. Screen should look like this:
<img width="1920" height="1080" alt="Screenshot (9)" src="https://github.com/user-attachments/assets/9f3f6628-68b9-4836-9cd7-1e1263ea57dc" />




Make your you have this Vnet for a resource group you just created. Put a name and press Review + Create. We don’t need worry anything else. We will have the default settings for this network including the subnet

# Create the Domain Controller VM (Windows Server 2022) named “DC-1”

Go to Home Portal -> Virtual Machines -> Create -> Virtual machines. Screen should look like this.Make sure the VM version you choose is Windows Server 2022
<img width="1920" height="1080" alt="Screenshot (12)" src="https://github.com/user-attachments/assets/70c4830c-0cf8-4310-880f-671cce23f71e" />






Username: labuser or anything username you want

Password: Cyberlab123! Or anything you want

Click Next: Disk -> Next: Networking. Then choose the Vnet you just created. Then Press Review + Create -> Create

After VM is created, set Domain Controller’s NIC Private IP address to be static

- (Note)-The reason why we are changing the private IP to be static is because later one we will have client1 to point to dc-1 private IP. We do this so dc-1’s private IP does not change each time the server is restarted or stopped. The server client1 will use dc-1’s private IP for DNS. 

- Go to Home-> Virtual machines and click on dc-1. Go to Network -> Network Settings. There should be a network  interface / IP configuration icon. This is the network identification card. This is what your screen should look like.

- Click on the  network identification card. It should take you this screen:
- <img width="1920" height="1080" alt="Screenshot (16)" src="https://github.com/user-attachments/assets/63dc9559-1e70-4365-b82c-b501f494fd38" />




- Click on Settings -> IP Configurations and take a note on the Private IP Address. Its Dynamic for the moment, but we are going to change it to static.
- <img width="1920" height="1080" alt="Screenshot (18)" src="https://github.com/user-attachments/assets/d8fbf32a-cd08-4be6-be65-e19fb4483382" />


- Click on ipconfig1. Change the setting to Static. 
- <img width="1920" height="1080" alt="Screenshot (19)" src="https://github.com/user-attachments/assets/07ff07cc-6a8f-46f8-bac4-9f0cdbc0f197" />





Log into the VM and disable the Windows Firewall (for testing connectivity)

Once you log in your screen should load up this.



If not, you probably didn’t pick the right server. 

Now we going to turn off the firewall settings

Right click the start menu -> run -> type wf.msc -> Ok. This screen should load 



Click on Windows Defender Firewall Properties, then the first profile you will see is Domain Firewall. Go to Firewall State Dropdown and switch it to off. Then do the same with each of the profiles, Public and Private. Then click Apply, then click OK.







Setup Client-1 in Azure

—

Create the Client VM (Windows 10) named “Client-1”

Username: labuser

Password: Cyberlab123!

Attach it to the same region and Virtual Network as DC-1

After VM is created, set Client-1’s DNS settings to DC-1’s Private IP address

Go to Home-> Virtual Machines -> dc-1 and then grab dc-1’s private IP. Yours might be different



Go to client1 VM portal, then Networking-> Networking Settings, then click the Network Interface / IP configuration card



Go to Settings -> DNS servers. Check customs and then add dc-1’s private IP address. This makes client1’s DNS servers point to dc-1’s DNS server. 



From the Azure Portal, restart Client-1

Login to Client-1. Open Powershell, and attempt to ping DC-1’s private IP address and ensure the ping has succeeded.



From Client-1, open PowerShell and run ipconfig /all

The output for the DNS settings should show DC-1’s private IP Address



Finish the lab, but do not delete the VMs in Azure. We will use them for upcoming labs.

If you are done for the day and want to save money, simply “Stop”/turn off the VMs within the Azure Portal
