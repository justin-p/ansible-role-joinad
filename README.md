# ansible-join-ad

![Travis](https://img.shields.io/travis/justin-p/ansible-role-joinad?label=Travis&logo=travis&style=flat-square)

**This role is not considered done**

This role will join a Windows host to a Active Directory domain.

Based of the work done by [@jborean93](https://github.com/jborean93) in [jborean93/ansible-windows](https://github.com/jborean93/ansible-windows)

Works on

- Windows Server 2019
- Windows Server 2016
- Windows Server 2012R2
- Windows Server 2012

Not validated (yet) on

- Windows Server 2008R2
- Windows Server 2008 x64
- Windows Server 2008 x32

## Requirements

- `python3-winrm` (`pywinrm`) is needed for WinRM.

## Role Variables

### `defaults/main.yml`

| Variable              | Default value                     | Explanation                                                                                                                    |
| :-------------------- | :-------------------------------- | :----------------------------------------------------------------------------------------------------------------------------- |
| joinad_dns_nics       | "*"                               | The name of the ethernet adapter to setup DNS on. Defaults to wildcard. 9/10 times you should leave this to the default value. |
| joinad_dns_servers    | "192.168.56.10"                   | The DNS server to use on joinad_dns_nics. This should be changed to the IP's of your domain controllers.                       |
| joinad_domain         | ad.example.test                   | The Domain of the new Active Directory Forest. This should be changed your Domain                                              |
| joinad_admin_username | administrator@{{ joinad_domain }} | The username of the account to add the computer to the domain. Change this depending on your needs.                            |
| joinad_admin_password | P@ssw0rd!                         | The password of the account to add the computer to the domain. Change this depending on your needs.                            |

## Dependencies

- WinRM on the windows host should configured for Ansible.
- justin_p.wincom
  - justin_p.posh5

## Example Playbook

    - hosts: domain_members
      roles:
         - { role: justin_p.joinad }

## Local Development

This role includes a Vagrantfile that will spin up a local Windows Server 2019 VM in Virtualbox.  
After creating the VM it will automatically run our role.

### Development requirements

`pip3 install pywinrm`

#### Usage

- Run `vagrant up` to create a VM and run our role.
- Run `vagrant provision` to reapply our role.
- Run `vagrant destroy -f && vagrant up` to recreate the VM and run our role.
- Run `vagrant destroy` to remove the VM.

## License

MIT

## Authors

- Justin Perdok ([@justin-p](https://github.com/justin-p/)), Orange Cyberdefense
