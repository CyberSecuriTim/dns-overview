<p align="center">

![image](https://github.com/user-attachments/assets/f8452367-35f2-49c5-bec2-cad2f9256303)

</p>

<h1>DNS for Dumm...DNS Done Simply 😅</h1> <br />

<p>

  - This tutorial outlines the basic domain name to IP address resolution process which is a vital component of the Domain Name System which allows us to visit all our favourite websites 
    and "seamlessly" (most of the time anyways) traverse the internet daily.

<h4>
 
  DISCLAIMER: This lab was performed using the Active Directory domain environment I created during [this lab](https://github.com/CyberSecuriTim/ad-configuration).
    
  - It consisted of creating a Domain Controller virtual machine with DNS server software running on it.
      - Creating a Client VM, joining it to the domain governed by the Domain Controller and statically configuring the client's DNS server to be the DNS server installed on the Domain 
         controller.
  - Though this was the specific environment that I used to conduct this lab, realistically the essence of this exercise can be replicated in any environment where you have 
    adminstrative control over a DNS server and at least one client connected to that server. 
    
</h4>
</b>

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  - Now before we dive into the practical lab exercise I would just like to give you a brief summary of how DNS works and why it is important (to all my fellow IT nerds who already know 
    how DNS works feel free to skip ahead) ☺️
    - It is much easier for us as humans to remember domain names such as (google.com, facebook.com, youtube.com etc) than it is to memorize an arbitrary sequence of numbers that forms 
      the IP addresses for each of those websites.
       - Imagine having to type XXX.XXX.XXX.XXX everytime you wanted to visit your favourite website or even access your network resources on your local network...(I shiver at the 
         thought).
     
    - However, our computers and other networked/networking devices are much more proficient at interacting with those arbitrary sequence of numbers known as IP addresses.
    - Behind the scenes, this wonderful technology (DNS) bridges the gap and allows both entities (humans and computers) to communicate with each other harmoniously.
      - It will convert the domain names that you enter into the address bar of your favourite web browser into IP addresses that computers and servers (and the other devices in 
        underlying internet infrastructure understand) so your network traffic can be appropriately routed.
      - This process also occurs in the reverse (known as a reverse DNS lookup) in which an IP address is converted to a domain name but for the sake of simplicity and focusing on more 
        everyday use we will focus primarily on the forward DNS lookups (domain names to IP addresses).

      - DNS also uses a hierarchical structure during its resolution process:
        - At the local level:
          - It will check the local DNS host file first
          - If no DNS record exists there for the specified domain name then it checks its local DNS cache
          - Lastly, if no DNS record exists in the DNS cache then it will contact its local DNS server.
          - If the local DNS server has no entry for the domain name then the external resolution process begins.
         
        - At the remote/external level:
          - The (recursive) DNS server will begin contacting external DNS servers via the internet in a hierarchical manner:
          - Root DNS server > Top Level Domain (TLD) DNS server > Authoritative DNS server
          - Until a the domain name lookup process is completed and the lookup is resolved...or not👀 

     - Now that all the boring stuff is out of the way, let us get our hands dirty and build some intuition for DNS! 
  
</p>



<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Windows Command-Line Tools (primarily nslookup)
- Microsoft Active Directory Domain Services (AD DS)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Windows Server 2022 

<h2>High-Level Steps</h2>

- Step 0: Access the Domain Controller and Domain Client VMs
- Step 1: Creating a DNS A-Record on the DNS Server (forward DNS lookups - domain names to IP addresses) 
- Step 2: Examining and Interacting with the Domain Client's Local DNS cache  
- Step 3: Examining and Intercating with the Domain Client's Local DNS Host File
- Step 4: Creating a CNAME (canonical name) DNS record (think of this like an alias for a domain name) 

<h2>Actions and Observations</h2>

<h2> STEP 0: Access the Domain Controller (with the DNS server installed and the Domain Client (with its DNS server assigned as the Domain Controller) </h2>


<p> 

  - Establish a Remote Desktop Connection to the Domain Controller VM via its public IP address and login as a Domain Admin.


![image](https://github.com/user-attachments/assets/7f0becde-a7fa-4498-a645-69e7709fbccb)

</p>

























