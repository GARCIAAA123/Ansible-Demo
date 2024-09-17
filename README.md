# Ansible and Vagrant Demo

## Project Overview

This project demonstrates the use of [Ansible](https://www.ansible.com/) and [Vagrant](https://www.vagrantup.com/) to automate the provisioning and configuration of virtual machines (VMs). The setup consists of a Vagrant-controlled environment with two virtual machines (VMs): an Ansible controller VM and two host VMs (a web server and a database server).

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

## Apache Installation on the Web Server

The Ansible playbook located in `playbook.yml` is responsible for installing and starting the Apache web server on the web server VM. Here's a brief overview of what the playbook does:

1. **Install Apache**

    The playbook uses the `yum` module to install the `httpd` package, which is the Apache HTTP server:

    ```yaml
    - name: Install Apache
      yum:
        name: httpd
        state: present
    ```

2. **Start and Enable Apache Service**

    After installing Apache, the playbook ensures that the Apache service is started and enabled to start on boot:

    ```yaml
    - name: Start and enable Apache service
      service:
        name: httpd
        state: started
        enabled: yes
    ```

3. **Verify Installation**

    To verify that Apache is running, you can access the web server VM using a web browser and navigate to the server's IP address. You should see the default Apache welcome page.

## Troubleshooting

### Fixing `baseurl` Issues

If you encounter a `Could not retrieve mirrorlist` error, follow these steps to resolve it:

1. **SSH into the Problematic VM**

    ```bash
    vagrant ssh [vm-name]
    ```

    Replace `[vm-name]` with the name of the VM where you are experiencing the issue (e.g., `ansible-controller`, `webserver`, or `dbserver`).

2. **Edit the Repository Configuration File**

    For CentOS-based systems, this file is typically located at `/etc/yum.repos.d/CentOS-Base.repo`. Open it with a text editor:

    ```bash
    sudo vi /etc/yum.repos.d/CentOS-Base.repo
    ```

3. **Update the `baseurl`**

    Look for lines starting with `baseurl=` under the repository sections (e.g., `[base]`, `[updates]`). Update these lines to use a valid base URL. You can find updated URLs from CentOS mirror sites or repositories.

    Example:

    ```ini
    baseurl=http://mirror.centos.org/centos/7/os/x86_64/
    ```

4. **Save and Exit**

    Save the changes and exit the editor (`:wq` in `vi`).

5. **Clean the YUM Cache**

    Run the following command to clean up and refresh the repository metadata:

    ```bash
    sudo yum clean all
    sudo yum update
    ```

6. **Retry the Provisioning**

    Retry the `vagrant up` or `ansible-playbook` command to see if the issue is resolved.

## Files

- **ansible-controller/Vagrantfile**: Configures the Ansible controller VM. Installs Ansible and sets up provisioning.
- **ansible-host/Vagrantfile**: Configures the web and database VMs with specified network settings and IPs.
- **playbook.yml**: Ansible playbook for installing and starting the Apache web server on the web server VM.

## Contributing

Feel free to open an issue or submit a pull request if you have improvements or suggestions for this project.

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Acknowledgments

- [Ansible Documentation](https://docs.ansible.com/)
- [Vagrant Documentation](https://developer.hashicorp.com/vagrant/docs)
- [VirtualBox Documentation](https://forum.virtualbox.org/wiki/Documentation)
- [Apache Documentation](https://httpd.apache.org/docs/)
