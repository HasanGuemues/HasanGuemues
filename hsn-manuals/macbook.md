## install homebrew paketmanager on macos
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

## install git on macos
```
brew install git
```

## git clone
```
git clone https://github.com/HasanGuemues/HasanGuemues.git
```

https://formulae.brew.sh/formula/pyenv-pip-migrate#default

## install ansible
```
brew install ansible
```

```
sudo touch /usr/local/etc/ansible/hosts
sudo vi /usr/local/etc/ansible/hosts
```


busra-nurs-mbp.fritz.box
# configure ansible on localhost
https://www.howtoforge.com/how-to-install-and-test-ansible-on-linux/
# enable the ssh connection to localhost on mac

1) In Mac, go to System Preference -> Security  -> Festplattenvollzugriff -> Häckchen bei Terminal 
2) Enable ssh connection
a) Häckchen bei sshd setzen
b) mit Terminal
```
systemsetup -getremotelogin
sudo systemsetup -getremotelogin
ssh localhost
sudo systemsetup -setremotelogin on
```
Create folder and the hosts file
```
cd /etc
sudo mkdir ansible
cd ansible
sudo touch hosts
```

Open the file with root privileges by executing the below command:
```
sudo vi /etc/ansible/hosts

Let's add below mentioned configuration in hosts file.
```
[servers]
host1 ansible_ssh_host=localhost ansible_ssh_user=hasangumus ansible_ssh_pass=f#mpg6RP
```

Let’s try to connect to managed nodes from the Ansible controller now.
```
ansible -m ping servers
```

You might encounter errors for different reasons. 
Ansible will, by default, try to connect to the managed node using your current username if you didn't provide one. If that user doesn’t exist on the node server, you will receive the below error.
If ssh port 22 is not open for connection on managed nodes. (As mentioned before, Ansible connects on ssh port)
If the IP in the hosts file is not correct.
If any of the above conditions fail, you will encounter the below error. 

host1 | UNREACHABLE! => {
    "changed": false,
    "msg": "Failed to connect to the host via ssh: hasangumus@localhost: Permission denied (publickey,password,keyboard-interactive).",
    "unreachable": true
}

```
sudo mkdir /etc/ansible/group_vars
sudo touch /etc/ansible/group_vars/node_servers.yml
sudo vim /etc/ansible/group_vars/node_servers.yml
```

Test again
```
ansible -m ping servers
```
host1 | FAILED! => {
    "msg": "to use the 'ssh' connection type with passwords, you must install the sshpass program"
}

```
brew install hudochenkov/sshpass/sshpass
```

add the host informations to node_servers.yaml
```
servers:
  hosts:
    host1:
      ansible_ssh_host: localhost
      ansible_ssh_user: hasangumus
      ansible_ssh_pass: f#mpg6RP
``` 

test the connection
```
ansible servers -m ping -i /etc/ansible/group_vars/node_servers.yml 
```

host1 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": false,
    "ping": "pong"
}

Ansible benötigt – wie jedes Konfiguration-Management-System – Informationen über den Host, den es verwalten soll:

Betriebsystem
CPU
RAM
Netzwerk
installierte Pakete
...
Um diese einzusammeln bzw. anzuzeigen gibt es das Modul setup:
```
ansible servers -m setup -i /etc/ansible/group_vars/node_servers.yml 
```

# Gute ansible tutorial
https://www.informatik-aktuell.de/entwicklung/programmiersprachen/einfuehrung-in-ansible.html