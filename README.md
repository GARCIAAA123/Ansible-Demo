# Ansible and Vagrant Demo

## Project Overview

This project demonstrates the use of [Ansible](https://www.ansible.com/) and [Vagrant](https://www.vagrantup.com/) to automate the provisioning and configuration of virtual machines. The setup consists of a Vagrant-controlled environment with two virtual machines (VMs): an Ansible controller VM and two host VMs (a web server and a database server).

- **Ansible Controller VM**: Manages the Ansible playbooks.
- **Host VMs**: Consisting of a web server and a database server.

## Project Structure

- **ansible-controller/**: Contains the Vagrantfile for provisioning the Ansible controller VM.
- **ansible-host/**: Contains the Vagrantfile for provisioning the web and database host VMs.
- **playbook.yml**: Ansible playbook to configure the web server.
- **README.md**: This file.

## Getting Started

### Prerequisites

- [Vagrant](https://www.vagrantup.com/downloads) - Ensure you have Vagrant installed on your local machine.
- [VirtualBox](https://www.virtualbox.org/wiki/Downloads) - VirtualBox is used as the provider for Vagrant.
- [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) - Installed on the Ansible controller VM.

### Setup

1. **Clone the Repository**

    ```bash
    git clone https://github.com/GARCIAAA123/Ansible-Demo.git
    cd Ansible-Demo
    ```

2. **Provision the Vagrant VMs**

    - Navigate to the `ansible-controller` directory and run:

      ```bash
      cd ansible-controller
      vagrant up
      ```

    - Navigate to the `ansible-host` directory and run:

      ```bash
      cd ../ansible-host
      vagrant up
      ```

    This will start the provisioning process for both the controller and the host VMs.

3. **Configure Ansible Inventory**

    - SSH into the controller VM:

      ```bash
      vagrant ssh ansible-controller
      ```

    - Edit or create the Ansible inventory file at `/vagrant/hosts` on the controller VM:

      ```ini
      [webservers]
      192.168.33.10

      [dbservers]
      192.168.33.11
      ```

4. **Run the Ansible Playbook**

    - Still on the controller VM, execute the playbook:

      ```bash
      ansible-playbook /vagrant/playbook.yml
      ```

    This playbook will install and start the Apache web server on the web server VM.

## Files

- **ansible-controller/Vagrantfile**: Configures the Ansible controller VM. Installs Ansible and sets up provisioning.
- **ansible-host/Vagrantfile**: Configures the web and database VMs with specified network settings and IPs.
- **playbook.yml**: Ansible playbook for installing and starting the Apache web server on the web server VM.

## Troubleshooting

- **Could not retrieve mirrorlist error**: Indicates a network issue or a problem with the CentOS repositories. Ensure that the VMs have network access and that the repositories are reachable.
- **Authentication issues with GitHub**: GitHub no longer supports password authentication for Git operations. Use a Personal Access Token (PAT) or SSH keys for authentication.

## Contributing

Feel free to open an issue or submit a pull request if you have improvements or suggestions for this project.

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Acknowledgments

Ansible Documentation
Vagrant Documentation
VirtualBox Documentation
