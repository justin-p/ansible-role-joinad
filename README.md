# ansible-role-joinad

[![Ansible Role Name](https://img.shields.io/ansible/role/d/justin_p/joinad?style=flat-square
)](https://galaxy.ansible.com/justin_p/joinad)
[![Github Actions](https://img.shields.io/github/actions/workflow/status/justin-p/ansible-role-joinad/main.yml?label=Github%20Actions&logo=github&style=flat-square)](https://github.com/justin-p/ansible-role-joinad/actions)

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

`defaults/main.yml`

| Variable              | Description                                                                                                                    | Default value                     |
| :-------------------- | :----------------------------------------------------------------------------------------------------------------------------- | :-------------------------------- |
| joinad_domain         | The Domain of the new Active Directory Forest. This should be changed your Domain                                              | ad.example.test                   |
| joinad_admin_username | The username of the account to add the computer to the domain. Change this depending on your needs.                            | administrator@{{ joinad_domain }} |
| joinad_admin_password | The password of the account to add the computer to the domain. Change this depending on your needs.                            | P@ssw0rd!                         |
| joinad_reboot_timeout  | Maximum seconds to wait for machine to re-appear on the network and respond to a test command. | 600 |
| joinad_post_reboot_delay | Seconds to wait after the reboot command was successful before attempting to validate the system rebooted successfully. | 300 |

## Dependencies

- WinRM on the windows host should configured for Ansible.
- justin_p.posh5
- justin_p.wincom

## Example Playbook

    - hosts: domain_members
      roles:
         - role: justin_p.posh5
         - role: justin_p.wincom
         - role: justin_p.joinad

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

## Contributing

Feel free to open issues, contribute and submit your Pull Requests. You can also ping me on Twitter ([@JustinPerdok](https://twitter.com/JustinPerdok)).
