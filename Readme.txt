Create network
you need to create a network and define an ip address range and ports don't forget to have DHCP enabled
    networking>>network 
    name ; chose region ;select"Create complete network,containing subnet"
    give the necessary ip address and gateway and DHCP allocation pools range and done 
Create Keypair

you need to create ssh keypair which we use to connect the hosts 
    compute>> keypair
    name, region, keypair type (ssh)

Configuring Servers 
create 5 hosts with names "Bastion","Haproxy",Dev A,Dev B,Dev C with network 
    select region; serverprofile Generic ;boot source image ubuntu ; don't change any thing ;Network select the one just created. 
there is a option for floating ip for bastion and Haproxy you require floating IP and for remaining its not required
    select security group public for bastion and Haproxy and private for Dev A B C
    select the keypair you have created earlier
leave login credentials 
after creating servers you need to create rules open security group tab in a server and click on create rule 
     select a protocol; give range ;select network 
        you need tcp and icmp 
  
Haproxy public ip address:xxxx
command we use to connect to haproxy from local host make sure you are in directory where your privatekey is located
	 ssh -i ".\privatekey--xxxx.txt" ubuntu@Xxxxxxxxx
the public ip address we use for connecting into bastion from our local host 
bastion ubuntu@xxxxx

To login into bastion we use the follwing command from the directory where our ssh key is located in our local host (host from which we are using to configure )
PS F:\User\> ssh -i ".\privatekey--xxxx.txt" ubuntu@

from bastion we connect to remaining severs Dev A,B,C
you need key.pem which we use to connect dev A B C

we use the following to connect with intended hosts
    ssh -i key.pem ubuntu@<private ip address>

Intial Configuration of Bastion
gain root access using sudo su and install a text editor like vim 
    apt install vim
then create a file named "key.pem" for storing your private key just paste the privte key in the file and save it 
    vim key.pem
change ownership of it user by using sudo chmod 600 key.pem
intial configuration for Dev servers
login into Bastion
change directory into .ssh  to where you have key.pemwhich you have created earlier (i have mine in .ssh)
    cd .ssh
from ths directory use the following command to login into dev a
     ssh -i key.pem ubuntu@<ip address>
change directory to .ssh
        cd .ssh
you need to add your public key to allow access to this host you do so with the following
    echo -n "<insert your public key here>" >>  authorized_keys
repeat the intial configuration for all Dev servers 
you can check the connectivity between hosts by
    ansible all -i hosts -m ping
you can cehck the syntax of your file 
    ansible-playbook --syntax-check nodes.yaml
you can run your play book 
 ansible-playbook -i hosts site.yaml