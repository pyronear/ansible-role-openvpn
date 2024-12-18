# Ansible Role: OpenVPN

This Ansible role allows you to install and configure both an **OpenVPN server** and an **OpenVPN client**.


## Requirements

The OpenVPN server requires the following Python dependencies to be installed on the target machine:
- `requests`
- `packaging`
- `pexpect`

You can install these dependencies using pip:

```bash
pip install requests packaging pexpect
```

This role relies also on the Docker image [`kylemanna/openvpn`](https://hub.docker.com/r/kylemanna/openvpn) to set up the OpenVPN server. Ensure that Docker is installed and properly configured on the server where the role will run.

## Reboot the machine & final test

You will need to reboot the client after the installation :
```
- name: Unconditionally reboot the machine with all defaults
  ansible.builtin.reboot:
```

Openvpn is configured to redirect all the trafic through the VPN so after the reboot, you will need to be connected to the VPN to be able to connect to your machine.

If you want to be sure that everything has worked, once connected to the VPN, you can launch the tasks check_ifcongif.yml.

## Role Variables

The following variables must be defined for the role to work. For sensitive values, it is recommended to use **Ansible Vault** in production environments.

| Variable                      | Description                                  | Required | Example                  |
|-------------------------------|----------------------------------------------|----------|--------------------------|
| `openvpn_ca_password`         | Password for the Certificate Authority (CA) | Yes      | `"securepassword123"`    |
| `openvpn_client_password`     | Password for the OpenVPN client             | Yes      | `"anothersecurepassword"`|
| `openvpn_server_dns`          | DNS for the OpenVPN server                  | Yes      | `"vpn.example.com"`      |
| `openvpn_server_ansible_host` | Hostname or IP address of the OpenVPN server| Yes      | `"192.168.1.100"`        |
| `dockerhub_username` | Your DockerHub Username | No      | `"myname"`        |
| `dockerhub_password` | Your DockerHub Password| No      | `"securepassword1234"`        |

**Example Playbook:**
```yaml
- hosts: openvpn_server
  become: true
  vars:
    openvpn_ca_password: "your_ca_password"  # Use Ansible Vault in production
    openvpn_client_password: "your_client_password"  # Use Ansible Vault in production
    openvpn_server_dns: "vpn.example.com"
    openvpn_server_ansible_host: "192.168.1.100"
  roles:
    - pyronear.openvpn
```

## Local Testing with Molecule

You can test this role locally using [Molecule](https://molecule.readthedocs.io/en/latest/). Molecule automates the process of creating test environments (e.g., Docker containers), applying the role, and validating its idempotence.

### Setup

1. Install the required dependencies:
   ```bash
   pip install molecule[docker]
   ```

2. Run the tests:
   ```bash
   molecule test
   ```

### Molecule Configuration

The `molecule` directory contains all the configurations needed to test this role. Specifically:
- The OpenVPN server and client are tested in separate containers.
- Python dependencies and any additional setup are handled automatically during the Molecule execution.

## License

 See the [LICENSE](LICENSE) file for details.

## Author Information

This role was created by **[Ronan SY/Pyronear]**. Contributions and feedback are welcome!
