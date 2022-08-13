# Virtual Machines

Hypervisor Types
1. Type 1 --> Where hypervisors are created directly on m/c. No Operating System.
ex : VMware ESXi, Microsoft Hyper-v
2. Type 2 --> Run on existing OS of Host. ex: VirtualBox, VMware Workstation


## Connectivity for VM


IP for MAC m/c : 192.168.29.186
IP for machine centos : 192.168.57.3 (my-centos-vm2) , 192.168.57.4 (my-centos)
username/ password : osboxes / osboxes.org

Use Ctrl+Command to get the mouse out of the VM

1. Using NAT for networking doesn't expose the VM to external n/w directly and hence
we can't do SSH to the VM.
2. To access it, we would have to do port-forwarding
3. We add rule for port forwading to forward SSH connections from HOST(mac) on port
2222 to Guest(VM) on 22
4. SSH by default runs on 22 but we don't want to forward everything from mac to VM,
we used 2222 as new port for this VM only .
5. `ssh root@127.0.0.1 -p 2222` use this to login to VM. This starts SSH on port 2222
and logs in to local system


## Connectivity Basics

1. Host-Only Network:
  * Use case: How does multiple VMs Communicate with each other
  * For Multiple VMs within HOST communicate with each other, we would
    need to create virtual n/w inside the host. We will attach all virtual interfaces of VMs
    to the n/w and then every VM will be able to connect to one another.
  * Our Host will also be connected to it by one of virtual interfaces
  * This n/w is limited to HOST only and no one from outside world can discover this
  * Go to File -> Host N/w Manager --> Create. This adds new vboxnet Host only n/w
  * Go to each vm , settings -> networks -> use host-only adapter and choose the n/w
  * Internet can't be reached directly. We need to enable IP Forwarding so that outside
    world can communicate.
  * or we can configure second adapter with NAT for each VM which will help us do this.

2. NAT (Network address Translation) N/w:
  * Use case: how does VMs communicate with each other and outside world
  * NOTE : NAT is different from NAT n/w
  * It is similar to host only n/w only difference is we would now be able to communicate with other M/c outside of host
  * Whenever there is a request out of host, NAT replaces the source address with address of NAT engine on host so that outside world will see it coming from our HOST m/c
  * Response from outside world is intercepted and destination IP is changed accordingly
  * To create NAT n/w go to preference -> networks -> +
  * Assign it to VM using settings -> adapter
  * NAT allows to communicate with outside world but can't talk with each other
  * NAT N/w allows all things that is given by NAT + communicate b/w each other
  * Internet can be reached as long as HOST can reach to INTERNET

3. Bridge n/w
  * Use case: how does outside world communicate with internal VMs
  * Once VMs connect to bridge n/w they get IPs same as external world (like same as our HOST m/c n/w) and outside world can communicate with them independently
  * Internet can be reached as long as HOST can reach to INTERNET

4. Port Forwarding
  * Maps port on Host (80) to port on guest(80). Both ports can be different or same


## Multiple VMs

1. Clone the VM
  * Power off VM and then click on Settings --> Clone
  * Name the VM accordingly and continue
  * Full Clone
    - Complete new VMs
    - occupies disk usage same as earlier
  * linked clone
    - use the same disk of template VM
    - can be trouble if we want to delete VM or move VM to other space
  * Configure HOST-only n/w and then add new adapter 2 for each VM using created n/w
  * Power on VMs and inspect Ip address `ip addr show`
  * New interface will be created for Host-only n/w and IP address will be assigned.
    ```
    Host : 192.168.57.1
    VM1 : 192.168.57.4
    VM2: 192.168.57.3
    ```
  * Now we can reach VMs directly using IP address
    ```
    ssh root@192.168.57.3 (password : osboxes.org)
    ```
  * this ensures we have inter-communication + access to outside n/w

### How to backup or snapshot

1. Go to VM and click on VM , go to snapshots
2. Save the Snapshot


# Vagrant

1. Helps to automation for bringing up VMs
2. Vagrant Box -->Package containing image of OS and related scripts
3.
