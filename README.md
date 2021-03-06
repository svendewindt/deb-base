# Read me

[![Build Status](https://travis-ci.org/svendewindt/ansible-role-deb-base.svg?branch=master)](https://travis-ci.org/svendewindt/ansible-role-deb-base)

## Deb-Base

This is a role to setup a Debian like system with a basic configuration.

- Managing users and groups
- Managing packages
- Managing time zone
- Set message of the day
- Set custom bashrc and vimrc

## Requirements

No requirements.

## Role Variables

None of the variables is mandatory.

| variable 			| default 				| explanation							|
| ---				| ---					| ---						 		|
| apt_update_cache		| 3600					| apt cache expiration, default 3600 seconds			|
| install_core_packages		| ['sudo', 'ntp', 'locate', 'git', 'jq']| installs default packages					|
| remove_core_packages		| [ ]					| removes default packages (none)				|
| install_packages		| [ ]					| packages to install						|
| remove_packages		| [ ]					| packages to remove						|
| add_groups			| [ ]					| groups to add							|
| remove_groups			| [ ]					| groups to remove						|
| add_users			| [ ]					| users to add							|
| remove_users			| [ ]					| users to remove						|
| ssh_keys			| [ ]					| ssh_keys to add. note: the user must already exist		|
| timezone			| Europe/Brussels			| sets the timezoneurope/Brussels				|
| custom_motd			| true					| sets a custom message of the day				|
| custom_bashrc			| false					| sets a custom bashrd, default					|
| custom_vimrc			| false					| sets a custom vimrc						|

### Managing users

With this role it is easy to create users and groups. 

An example of users

```yaml
add_users:
      - username: 'johndoe'
        comment: 'John Doe'
        groups:
          - 'IT'
          - 'Admins'
        password: '$6$mlO/SXHhYGM0KKIG1PwN0...'
```

The password should be set to a SHA-512 hash, starting with `&6&`. An easy way to do this is with [https://www.mkpasswd.net/](https://www.mkpasswd.net/), crypt-sha-512.

To set the SSH key for a user, make sure the user exists and set the key like this

```yaml
 ssh_keys:
      - user: johndoe
        key: 'ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCoZlYxAdDmfcjnwiKyyTceK2ldPsV2KzG3EEDy9o8a7f7GiKfNpM/U3ZN4eFHK8DUoHlG+GGmKjvJ207VPsUQK0obi/7snaPu19m1wcoqnluaY2jcsTSiIHBFn+aVDWKNhc+UzbjZ+zFcHKqF0NIr1HaEpz4RV0N19UeyiIeqX7RpamkQX1MBTAHbQcBFB6eHJte9iWOpmMBmNManvU0rSZYWmdQzvK8+SFfHFB/93K1Cl4MLwG6gRfqGCmwgGmUiSgzG48uBa8N+cQCJie6ikbkKPV109kGVsnufx1kF/ka5/cgaABaxsKBXVxnpojUsFI1E6jS8lM5VZW32K23rB johndoe@PC-jd'

```

### Managing packages

Some core packages will be installed by default. I need them on every machine. This role will make them available. To add additional packages use `install_packages`.

### Managing time zone

The ntp deamon will be installed and set to run automatically. The timezone will be set to Europe/Brussels by default. To find a list of possible timezones, run `ls /usr/share/zoneinfo`.

### Set message of the day

A custom message of the day will be shown at login showing the name of the machine and some interesting statistics.

### Set custom bashrc and vimrc

This role will set some colorfull aliases in bashrc and set a colorfull vimrc to increase readability.

## Example

```yaml
---
- hosts: 127.0.0.1
  connection: local
  roles:
    - svendewindt.deb_base
  vars:
    install_packages: ['apache2']
    remove_packages: []
    add_users:
      - username: 'johndoe'
        comment: 'John Doe'
        groups:
          - 'IT'
          - 'Admins'
        password: '$6$mlO/SXHhYGMSKKIF$13slgnS8BV62QAuIVD19EAV1rINCLQ3OQbil6hkPOv9D19J8sAU1xv.msIfHSpA8P3tr.Eln2I6GuRUQ1ePwN0'
    ssh_keys:
      - user: svdw
        key: 'ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCoZlYxAdDmfcjnwiKyyTceK2ldPsV2KzG3EEDy9o8a7f7GiKfNpM/U3ZN4eFHK8DUoHlG+GGmKjvJ207VPsUQK0obi/7snaPu19m1wcoqnluaY2jcsTSiIHBFn+aVDWKNhc+UzbjZ+zFcHKqF0NIr1HaEpz4RV0N19UeyiIeqX7RpamkQX1MBTAHbQcBFB6eHJte9iWOpmMBmNManvU0rSZYWmdQzvK8+SFfHFB/93K1Cl4MLwG6gRfqGCmwgGmUiSgzG48uBa8N+cQCJie6ikbkKPV109kGVsnufx1kF/ka5/cgaABaxsKBXVxnpojUsFI1E6jS8lM5VZW32K23rB johndoe@PC-JD'
```



License: MIT

Author Information

Sven de Windt
