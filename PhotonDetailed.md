# **Photon standalone**

## **In the Local Machine:**
- **Create VM by following the InstallVMGuide.md file (in the gi/devops from Codeberg):**
  - https://codeberg.org/gi/devops/src/branch/main/gitea/Documentations/InstallVMGuide.md
  - 64GB ram
  - 8 cores
  - 150G storage

- **Ask VM's IP address after Installation of the VM:**
  - Log in in the terminal:
    - `<username>` and `password`
    - `ip address`
  - Save the IP address

- **Open the terminal and use the ssh-copy-id command for not typing the password after ssh:**
  - `ssh-copy-id -i ~/.ssh/id_rsa.pub <username>@IP`
  - Type username's password

## **In the VM:**
- **Add sudo permission to user of vm:**
  - Give the sudo permission to the user in the terminal as root:
  - https://phpraxis.wordpress.com/2016/09/27/enable-sudo-without-password-in-ubuntudebian/
    - `su -l`
    - `adduser <user> sudo`
    - `apt-get install sudo`
    - `sudo visudo /etc/sudoers`
        - `<username> ALL=(ALL) NOPASSWD:ALL` to **the end of file**
        - `%sudo ALL=(ALL) NOPASSWD:ALL`
    - `logout` or `exit`
    - `sudo apt-get install curl`

## **In the Local Machine:**
- **Open the new terminal:**
  - `cd ~/Desktop`
  - `mkdir git`
  - `cd git`
  - `git clone https://github.com/fahrgemeinschaft/digitransit-ansible.git`
  - `ċd digitransit-ansible`

- **Create virtual environment and install certain packages:**
  - `python3 -m virtualenv venv`
  - `. venv/bin/activate`
  - `pip3 install -r requirements.txt`
  - `ansible --help`

- **Add user to sudo group:**
  - `su` # enter root password here
  - `/sbin/usermod -aG sudo <username>`

- **Normal user should be able to run sudo commands without PW prompt**
  - `su` # enter root password here
  - `sudo visudo` /etc/sudoers
    - `username ALL=(ALL) NOPASSWD:ALL` to **the end of file**
  - `logout` or `exit`

- **Install geerlingguy.docker:**
  - `ansible-galaxy install geerlingguy.docker`

- **Add user to docker group:**
  - `sudo groupadd docker`
  - `su` # enter root password here
  - `/sbin/usermod -aG docker <username>`

## If you want to use it on a local vm or direct IP connection:
- **Change ip address and hostname on the certain files:**
  - `nano inventory-local.yml`
  - `nano ansible-playbook.yml` OR `nano photon-playbook.yml`

- **Run the playbook:**
  - `make photon-local`
## If you want to use it on a remote machine or with hostname:
 - Add ssh configuration on your local machine, if it requires multiple ssh, use proxyjump too:
    - `nano .ssh/config` on your local computer
      ```bash 
      Host services1
          HostName 162.55.99.187
          User ride2go
          Port 38765
          IdentityFile ~/.ssh/gi-student

      Host photon-services1
          ProxyJump services1
          HostName 192.168.122.91
          User ride2go
          Port 22
          IdentityFile ~/.ssh/gi-student
      ``` 

- **Change hostname on the certain files:**
  - `nano inventory-remote.yml`
  - `nano photon-playbook.yml`

- **Run the playbook:**
  - `make photon-remote`

- **Add VM user to docker group after playbook successfully ran for the first time:**
  - `su` # enter root password here
  - `/sbin/usermod -aG docker <username>`

- **Open the new terminal again:**
  - `ssh <username>@IP`
  - `docker ps`
  - **Take the first 3 character of the nominatim**
  - `docker logs $3_character -f`
  - **Wait until the LOG shows the line:** (It may takes 40 minutes)
    - **database system is ready to accept connections**
  - `sudo service photon start`
  - `docker ps`
  - **Take the first 3 character of the photon**
  - `docker logs $3_character -f`
  - **Wait until the LOG shows the line:** (It may takes 5 minutes)
    - **de.komoot.photon.App - ES cluster is now ready.**
  - After them you can call the API with:
  - `curl "http://localhost:2322/api/?q=stuttgart&lang=de"`