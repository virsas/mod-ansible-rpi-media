# rpi-media

## Role installation

### create requirements file

``` bash
cd ANSIBLE_FOLDER
mkdir requirements
touch requirements/pi_media.yml
```

### file content

``` yaml
---

# ansible-galaxy install -p vss_galaxy_roles --force -r requirements/pi_media.yml
- src: "https://github.com/virsas/mod-ansible-rpi-media"
  scm: git
  version: v1.0.0
  name: rpi-media
  path: vss_galaxy_roles
```

## Install role

### Installation

``` bash
ansible-galaxy install -p galaxy_roles --force -r requirements/pi_media.yml
```

### Best practices

If you are using git for your playbooks and sites configuration, add vss_galaxy_roles to your .gitignore file. You are free to download the role directly to your roles directory, but it will be then pushed to your repo too.

## Role access

### Direct access

``` bash
$ cd vss_galaxy_roles
$ ls
rpi-media
$ cd rpi-media
$ ls -1
medias
handlers
LICENSE
meta
README.md
tasks
templates
$
```

### Access from ansible (ansible.cfg)

``` bash
[defaults]
gathering = smart
roles_path=./roles:../roles:../../roles:./vss_galaxy_roles
log_path=./ansible_run.log
retry_files_enabled=False
[ssh_connection]
scp_if_ssh=True
host_key_checking=False
```

## Files

### Playbook (./playbooks/pi_media.yml)

``` yaml
---

- hosts: pi_media
  remote_user: pi
  become: yes
  roles:
    - rpi-media
```

### Inventory (./sites/NAME/inventory)

``` txt
[pi_media]
pi_media_01
```

### Host vars (./sites/NAME/host_vars/pi_media_01.yml)

``` yaml
---

ansible_ssh_host: 1.1.1.1
```

### Group vars (./sites/NAME/group_vars/pi_media.yml)

Please see the variables below to get the complete list of possible modifications. The YAML file below is just a basic example of a minimal configuration.

``` yaml
APACHE_PLEX_DOMAIN: plex.example.com
APACHE_ARIA_DOMAIN: aria.example.com
```

## Usage

``` bash
ansible-playbook -i sites/NAME/inventory playbooks/pi_media.yml --diff
```

## Variables

``` yml
---

MDENABLED: false
MDID: 0
MDNAME: raspberrypi
MDUUID: 123:456:789

APACHE_PLEX_DOMAIN: plex.example.org
APACHE_ARIA_DOMAIN: aria.example.org

MOUNT_POINT: /shared

ARIA_DOWNLOAD_DESTINATION: /shared/download
ARIA_MAX_CON_PER_SERV: 1
ARIA_MAX_CONCUR_DOWNLOADS: 5
ARIA_SPLIT: 5
ARIA_MIN_SPLIT_SIZE: 20MB

NFS_IP_BLOCK: 192.168.0.0/24
NFS_DIRECTORY: /shared
```
