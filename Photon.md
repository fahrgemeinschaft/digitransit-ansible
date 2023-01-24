# Photon standalone

There is a role in the `roles/` directory called `photon`. We will use that role form `photon-playbook.yml`.

## Install ansible locally

```bash
python3 -m virtualenv venv
. venv/bin/activate
pip3 install -r requirements.txt
ansible --help
```

### Install geerlingguy.docker plugin

```bash
ansible-galaxy install geerlingguy.docker
```

## Automate SSH access to the target VM by adding a new Host in `.ssh/config`

`ssh-copy-id $photon-vm-host`

## Add VM user to sudo group

```bash
su # enter root password here
/sbin/usermod -aG sudo $username
```

### Normal user should be able to run sudo commands without PW prompt

This is needed for ansible to be able to install docker and other stuff.
 
https://phpraxis.wordpress.com/2016/09/27/enable-sudo-without-password-in-ubuntudebian/

After executing `sudo visudo`, add the following line at the end of the file.

```
ride2go     ALL=(ALL) NOPASSWD:ALL
```

## Run the playbook

Before running anyplaybooks, make sure the corresponding inventory file (inventory-local.yml or inventory-remote.yml) cites the correct host, e.g. 
```
$ cat inventory-remote.yml 
all:
  hosts:
    photon-vm:
      ansible_host: services1-photon # set ~/.ssh/config with ProxyJump
```

If you have a local VM, you can run the following `make` command:

```bash
make photon-local
```

or for a remote VM use:

```bash
make photon-remote
```

After the ansible playbook has run successfully for the first time add the VM user to docker group

```bash
sudo usermod -aG docker $username
```

After nominatim data has been read and you see this line when typing `docker logs nominatim`:
`database system is ready to accept connections`, you must also start the photon service with:

```bash
sudo service photon start
```

After photon has read the nominatim data and you see this line:
`de.komoot.photon.App - ES cluster is now ready.`


You can call the API with:

```bash
curl "http://localhost:2322/api/?q=munchen&lang=de"
```

## Photon role

Some tasks are commented out for now (might change later):
- nginx
- pelias adapter

## Add all needed languages
All needed languages is now set in the `roles/photon/templates/photon.service` file as an environment variable called PHOTN_LANGUAGES.


## Reset installation

There is a not so perfect solution for this, see: `./roles/photon/templates/remove-nominatim-and-photon`.