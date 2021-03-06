# Ansible Inventory Updater

This script adds and deletes hosts from the ansible hosts file. It will backup the hosts file and reverse the changes in case something goes wrong.
On first run it will create a virtual Python enviroment in the folder next to this README file and install it's dependencies.

```console
$ inventory_updater -h
usage: inventory_updater [-h] [--hosts_file PATH] [--group GROUP] {print_hosts,print_hosts_file,set_host,delete_host} ...

This script will add or delete entries in your ansible inventory

optional arguments:
  -h, --help            show this help message and exit
  --hosts_file PATH     Path to your ansible hosts file
  --group GROUP         Ansible Inventory group

subcommands:
  {print_hosts,print_hosts_file,set_host,delete_host}
                        For additional help type: inventory_updater COMMAND --help
    print_hosts         Print all host and IP's of the group
    print_hosts_file    Print the ansible hosts file
    set_host            Add new host to the ansible hosts file
    delete_host         Delete a host from the ansible hosts file
```

```console
$ inventory_updater print_hosts -h
usage: inventory_updater print_hosts [-h]

optional arguments:
  -h, --help  show this help message and exit
```

```console
$inventory_updater print_hosts_file -h
usage: inventory_updater print_hosts_file [-h] [--json]

optional arguments:
  -h, --help  show this help message and exit
  --json      Print in json format
```

```console
$ inventory_updater set_host -h
usage: inventory_updater set_host [-h] [--path PATH] [--keep N] [--no_backup] NAME IP

positional arguments:
  NAME         Name of the container
  IP           IP of the container

optional arguments:
  -h, --help   show this help message and exit
  --path PATH  Ansible python interpreter path on host
  --keep N     Backups to keep of the ansible hosts file
  --no_backup  DON'T (!!!) Backup ansible hosts file
```

```console
$ inventory_updater delete_host -h
usage: inventory_updater delete_host [-h] [--keep N] [--no_backup] NAME

positional arguments:
  NAME         Name of the container

optional arguments:
  -h, --help   show this help message and exit
  --keep N     Backups to keep of the ansible hosts file
  --no_backup  DON'T (!!!) Backup ansible hosts file
```

## Install

```console
$ git clone https://github.com/n-stone/inventory_updater
$ inventory_updater
```

You can also add symlink to `~/.local/bin` make sure it is on your `PATH`

```console
$ ln -s "$(pwd)/inventory_updater" "$HOME/.local/bin/inventory_updater"
```

## Example

```console
$ inventory_updater print_hosts_file
all:
  children:
    lxc:
      hosts:
        test 1:
          ansible_host: 192.168.1.100
          ansible_python_interpreter: /usr/bin/python3
    ungrouped: {}

$ inventory_updater --group lxc set_host test2 192.168.1.101
$ inventory_updater print_hosts_file 
all:
  children:
    lxc:
      hosts:
        test 1:
          ansible_host: 192.168.1.100
          ansible_python_interpreter: /usr/bin/python3
        test 2:
          ansible_host: 192.168.1.101
          ansible_python_interpreter: /usr/bin/python3
    ungrouped: {}

$ inventory_updater --group lxc delete_host test 
$ inventory_updater print_hosts_file  
all:
  children:
    lxc:
      hosts:
        test 1:
          ansible_host: 192.168.1.100
          ansible_python_interpreter: /usr/bin/python3
    ungrouped: {}
```
