<p align="center">

![image](https://github.com/user-attachments/assets/f8452367-35f2-49c5-bec2-cad2f9256303)

</p>

<h1>DNS for Dumm...DNS Done Simply 😅</h1>

<p>

  - This tutorial outlines the basic domain name to IP address resolution process which is a vital component of the Domain Name System which enables us to visit all our favourite websites and "seamlessly" (most of the time anyway) traverse the internet daily.

<h4>
 
  DISCLAIMER: This lab was performed using the Active Directory domain environment I created during [this lab](https://github.com/CyberSecuriTim/ad-configuration).
    
  - It consisted of creating a Domain Controller virtual machine with DNS server software running on it.
      - Creating a Domain Client VM, joining it to the domain governed by the Domain Controller, and statically configuring the client
        to use the Domain Controller as its DNS server.
  - Though this was the specific environment that I used to conduct this lab, realistically the essence of this exercise can be replicated 
    in any environment where you have administrative control over a DNS server and at least one client connected to that server. 
    
</h4>
</b>

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  - Now before we dive into the practical lab exercise I would just like to give you a summary of how DNS works and why it is important (to all my fellow IT nerds who already know how DNS works, feel free to skip ahead) ☺️
    - It is much easier for us as humans to remember domain names such as (google.com, facebook.com, youtube.com etc) than it is to 
     memorize an arbitrary sequence of numbers that forms the IP addresses for each of those respective websites.
       - Imagine having to type XXX.XXX.XXX.XXX every time you wanted to visit your favourite website or even access your network resources 
         on your local network...(I shiver at the thought).
     
    - However, our computers and other networked/networking devices are much more proficient at interacting with those arbitrary sequence of numbers known as IP addresses.
    - Behind the scenes, this wonderful technology (DNS) bridges the gap and allows both entities (humans and computers) to communicate with each other harmoniously.
      - It will convert the domain names (that you enter into the address bar of your web browser) into IP addresses that computers, 
        servers, routers and the other devices in the underlying internet infrastructure understand.
          - This enables your network traffic to be appropriately routed.
      - This process also occurs in the reverse (known as a Reverse DNS lookup) in which an IP address is converted to a domain name but for the sake of simplicity and focusing on more everyday use we will focus primarily on the forward DNS lookups (domain names to IP addresses).

      - DNS also uses a hierarchical structure during its resolution process:
        - At the local level:
          - It will check the local DNS host file first
          - If no DNS record exists there for the specified domain name then it checks its local DNS cache
          - Lastly, if no DNS record exists in the DNS cache then it will contact its local DNS server.
          - If the local DNS server has no entry for the domain name then the external resolution process begins.
         
        - At the remote/external level:
          - The (recursive) DNS server will begin contacting external DNS servers via the internet in a hierarchical manner:
            - Root DNS server > Top Level Domain (TLD) DNS server > Authoritative DNS server
          - This is done until the domain name lookup process is completed and the lookup is resolved...or not👀
            - If this entire DNS process is unsuccessful and the IP address for the specified domain name is not discovered then the browser or whatever searching mechanism was used will typically display an error message to the end user.    

![image](https://github.com/user-attachments/assets/f8df8821-375d-4a56-8933-88d8f55a517b)

- Now that all the boring stuff is out of the way, let us get our hands dirty and build some intuition for DNS! 
  
</p>



<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- (Windows) Command-Line Tools (nslookup, ping, ipconfig)
- Microsoft Active Directory Domain Services (AD DS)

<h2>Operating Systems Used </h2>

- Windows 10 (22H2)
- Windows Server 2022 

<h2>High-Level Steps</h2>

- Step 0: Access the Domain Controller and Domain Client VMs
- Step 1: Creating a DNS A-Record on the DNS Server (forward DNS lookups - domain names to IP addresses) 
- Step 2: Examining and Interacting with the Domain Client's Local DNS cache  
- Step 3: Examining and Interacting with the Domain Client's Local DNS Host File
- Step 4: Creating a CNAME (canonical name) DNS record (think of this like an alias for a domain name) 

<h2>Actions and Observations</h2>

<h2> STEP 0.0: Access the Domain Controller (with the DNS server installed and the Domain Client (with its DNS server assigned as the Domain Controller) </h2>


<p> 

  - Establish a Remote Desktop Connection to the Domain Controller VM via its public IP address and login as a Domain Admin.


![image](https://github.com/user-attachments/assets/7f0becde-a7fa-4498-a645-69e7709fbccb)

- Establish a Remote Desktop Connection to the Domain Client VM via its public IP address and login as a Domain Admin.

![image](https://github.com/user-attachments/assets/a0a21177-518e-4ec2-98b6-19145d58afd1)



- Verify that the Domain Client VM's DNS server is set to the IP address of the Domain Controller VM.
  - Run the "ipconfig /all" command on both VMs

![image](https://github.com/user-attachments/assets/a2de0b9c-e6f1-4956-914c-52307233f1c3) ![image](https://github.com/user-attachments/assets/cf002f1c-a4df-4fb6-812d-4a18e988a724)

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<h3> STEP 0.5: From the Domain Client VM, Attempt to Ping the "Mainframe" Host.</h3>

- Run the command "ping mainframe"
  - Notice that the ping request fails (there is no DNS record for this hostname (neither locally nor externally)
    - This hostname was entirely arbitrary; the hostname could have been anything even "fluffy zebras" for example

![image](https://github.com/user-attachments/assets/97d3db10-1cc6-4ef2-968c-3d98b244eabe)

</p>

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<h2> STEP 0.75: From the Domain Client VM, perform an Nslookup against the hostname "mainframe" </h2>

- Run the command "nslookup mainframe"
  - Notice that it contacts the domain controller VM as its DNS server and the DNS lookup cannot be resolved as the hostname cannot be found.
 
   

![image](https://github.com/user-attachments/assets/704b2383-333e-45d3-a9b9-c6a90cce6d79)









-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


<h2> STEP 1.0: From the Domain Controller VM, Create a DNS A Record on the DNS server for the Hostname "Mainframe" </h2>

<p>

  - Open the "DNS Manager" app
  - Click on the name of the Domain Controller VM running the DNS server software.
    - Navigate to Forward Lookup Zones > (name of the previously created domain)
    - Right-click the domain and select "New Host (A or AAAA)..."
    - Enter "mainframe" and assign this new host the IP address of the Domain Controller VM
      - Notice the FQDN (fully qualified domain name) is automatically configured as well and appends the new hostname before the domain's name.
     
    - "Add Host"

 ![image](https://github.com/user-attachments/assets/002256fc-4183-408f-9071-29644606c59c)

</p>

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<h2> STEP 1.33: Attempt to Ping the Host "mainframe" Again from the Domain Client VM </h2>

<p> 

- Run the command "ping mainframe"
  - It works this time! Notice that it resolves to the same IP address that we assigned in the DNS server manager.

![image](https://github.com/user-attachments/assets/09386b08-e9ed-4219-9137-9e9cbd3a4c8e)

 


</p>

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<h2> STEP 1.66: Run the Nslookup Command against the Mainframe Hostname Again. </h2>


- Run the command "nslookup mainframe"
  - The lookup is successfully resolved this time as well! 

![image](https://github.com/user-attachments/assets/d4a9d9dc-9bb8-4b9c-8650-d987738139f0)


-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


<h2> STEP 2.0: From the Domain Controller, Change the Assigned IP address to the "Mainframe" A Record. </h2>

- Access the Domain Controller and open the DNS server manager again.
- Right-click the "mainframe" A record that was created and select properties.
  - Change the assigned IP address to "8.8.8.8" (this is Google's public DNS server FYI)
  - Click Apply then OK

![image](https://github.com/user-attachments/assets/704e076a-f506-4054-a613-b82e5662c0c1)

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 <h2> STEP 2.2: Attempt to Ping the "Mainframe" Host. </h2>

- Run the command "ping mainframe"
  - Notice that it still pings the previously assigned IP address.
  
![image](https://github.com/user-attachments/assets/f19b51e4-528f-403b-ad28-87bfb85bbc6d)

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<h2> STEP 2.4: Examine the Local DNS Cache of the Domain Client VM. </h2>

- Run the command "ipconfig /displaydns"
  - Notice that the previously assigned IP address is still stored in the DNS cache

![image](https://github.com/user-attachments/assets/f97e781f-aecb-4bcf-8387-e68fca5e380f)

- This clearly shows the fact that the local DNS cache takes precedence over the DNS server's A record during the DNS resolution process.
  - This favours the speed and efficiency of the DNS lookup process but sometimes at the cost of the most accurate information as we can see here.
   
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<h2> STEP 2.6: Clear the Local DNS cache so the DNS server's A Record can be Used </h2>

- Run the command "ipconfig /flushdns"
  - This will require opening the command prompt with admin privileges
  - You can also run the command "ipconfig /displaydns" afterwards to verify that the DNS cache has been cleared.

![image](https://github.com/user-attachments/assets/e5892b66-89ee-468e-b3de-95709ff43879)


-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<h2> STEP 2.8: Ping the "mainframe" Host Again </h2>

- Notice that it resolves to the IP address assigned in the DNS A Record.

![image](https://github.com/user-attachments/assets/0a59226c-8c0c-4f74-a55a-503917c31626)

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<h2> STEP 3.0: Change the IP Address within the Mainframe Host's A Record One More Time From the DNS Manger on the Domain Controller VM.</h2>


![image](https://github.com/user-attachments/assets/6010676e-b2be-4440-9d99-d73463a59d01)

- FYI, Quad9 is a public and free DNS service.
  - Their DNS servers have an IP address of "9.9.9.9".

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<h2> STEP 3.25: Verify that the Local DNS Cache on the Domain Client Still Contains the previous IP Address  </h2>

![image](https://github.com/user-attachments/assets/3d82ffe8-18f2-475d-8113-3ef64847ca6d) ![image](https://github.com/user-attachments/assets/2385cf02-e6c2-472d-9a1d-cde9b1b49bf5)

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<h2>STEP 3.50: Access the Local DNS Host File on the Domain Client VM and Create an entry for the Mainframe Hostname. </h2>

- Open Notepad (with Admin privileges)

![image](https://github.com/user-attachments/assets/a4f25923-2361-4e4d-b603-c6d9563c5587)

  - Select "File" then select "Open"
  - Navigate to the "C:\\Windows\System32\drivers\etc\hosts" file
    - Select "All Files" from the dropdown menu in the bottom right corner of the window.
   
![image](https://github.com/user-attachments/assets/1f15b60d-5c2d-4ef5-8367-b86bd7e70c8f)



  - Add an entry to the local host file for mainframe.
    - Assign any accessible IP address in the entry (I chose the IP address of the Domain Controller)
    - Save the entry (Ctrl + S). 

![image](https://github.com/user-attachments/assets/6cc21372-056c-4124-97b3-932c6cc00d7e)



-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<h2> STEP 3.75: Ping the "mainframe" Host Again. </h2>

- Notice that it resolves to the IP address assigned in the local DNS host file, despite the DNS server's A record having a different value (9.9.9.9) and despite the local DNS cache having a different initial value as well (8.8.8.8).

![image](https://github.com/user-attachments/assets/d41c76e5-c1d7-4c30-b565-98b95b50f1ff)


  - This proves that the DNS host file takes precedence over both the DNS server's A record and the local DNS cache when resolving domain name to IP address lookups.
       - It is a useful tool for system/network administrators to statically redirect traffic in their networks as needed by using DNS's hierarchical nature.  

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<h2> STEP 4.0: Create a CNAME (Canonical Name) DNS Record in the DNS Manager on the Domain Controller VM. </h2>

- Within DNS Manager:
  - Navigate to (name of Domain Controller VM) > Forward Lookup Zone > (name of the created domain)
  - Right-click the name of the domain and select "New Alias (CNAME)..."
    - That is essentially what a CNAME record is (an alias for an already created A record)
      - For the CNAME record to work, the A record must be created (domain name mapped to an IPv4 address)

![image](https://github.com/user-attachments/assets/c691cef0-5796-4093-a9c3-574f54a5196d)

  - Name the alias "search"
    - Notice the FQDN (fully qualified domain name) is automatically configured (similarly to when we created the "mainframe" A record)
    - Enter the FQDN for the target host
      - I chose "www.gooogle.com" (it can be whatever you would like it to be, just ensure the target host is accessible)
     
![image](https://github.com/user-attachments/assets/691532cb-9f1f-48d8-9b52-dfbd82f41a12)

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
<h2> STEP 4.5: Attempt to Ping the Newly Created DNS CNAME.</h2>

- Notice that it automatically resolves to the assigned target host's (fully qualified) domain name and then resolves to that IP address.

![image](https://github.com/user-attachments/assets/57dbd35e-1d78-4bca-9284-1f09d6a0fe21)


<h2> BONUS STEP: Since the CNAME record is mapped to the FQDN of Google's web servers let's try browsing using its Fully Qualified Domain Name. </h2>

- Enter the FQDN of the CNAME record in the web browser address bar and observe what happens.


![image](https://github.com/user-attachments/assets/0981abec-5215-4b21-890d-7e4f181a4c6e)

- FYI. This message occurs because although the domain name system redirects our search to Google's web server, it does not possess the digital certificate associated with that 
  Google web server.
   - Therefore, the web browser has built-in security functionality to alert the user that the web server they are trying to access may not truly be the authentic server and the user may be the victim of an interception/on-path attack and is advising them not to proceed with their search.
   - This is beyond the scope of the DNS lab, I just wanted to include that tidbit of information for funsies...I did say I was an IT nerd after all 😜


<p> 

- But if you wanted to a go for a ride on the wild side, neglect cybersecurity best practices and still browse to this site let us see what happens.

![image](https://github.com/user-attachments/assets/1060e79a-f2ad-4c95-bbaa-dab21cc02446)

- Welp! Good old HTTP error 404. 😞
    - FYI, CNAME records operate at the DNS level only. It cannot modify the "Host" header presented in the HTTP request made by the web browser to Google's web servers.
    - The "search.timsdomain.com" hostname would be invalid and unrecognizable to Google's web servers (as they expect hostnames in the HTTP requests to be "www.google.com")
    - Therefore the Google web server will respond with the HTTP 404 (Resource not found) error.
 

</p>

<h2> Congratulations! You have made it to the end of the lab. Hopefully, this simple exercise has strengthened your understanding of the domain name system. It truly is a wonderful technology that makes our lives so much easier on the internet. 😁</h2>
