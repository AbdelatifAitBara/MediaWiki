### Clone this project to your pc:

git clone https://github.com/AbdelatifAitBara/MediaWiki


IPs:

- Master   : 192.168.10.10
- Database : 192.168.10.11
- MediaWiki: 192.168.10.12


Before starting connect to the "master machine" and copy the ssh key to the other nodes:

- ssh-copy-id vagrant@192.168.10.11
- ssh-copy-id vagrant@192.168.10.12


### Step 1 : 

Add the bellow details to your inventory file: sudo nano /etc/ansible/hosts

[Database]

db ansible_host=192.168.10.11

[MediaWiki]

media_wiki ansible_host=192.168.10.12

Check that your master can ping the other machines by running : ansible all -m ping

### Step 2 : 

On the "master" run : ansible-playbook install_media_wiki.yml


### Step 3 : 

On "Database machine" run the following commands in order:

		sudo mysql -u root

		CREATE USER 'media_user'@'192.168.10.12' IDENTIFIED BY 'password';


		GRANT ALL PRIVILEGES ON mediawiki.* TO 'media_user'@'192.168.10.12';

		FLUSH PRIVILEGES;
		
		exit
		

### Step 4 : 

Install Mediawiki via : http://192.168.10.12/mediawiki


- Database host 		: 192.168.10.11
- Database name 		: mediawiki
- Database username	: media_user
- Database password	: password








