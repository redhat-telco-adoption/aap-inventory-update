# AAP Inventory Update Playbook

This repository contains an Ansible playbook for updating host information in an Ansible Automation Platform (AAP) inventory. The playbook demonstrates how to modify a host's description and enabled status within a specified inventory.

## Prerequisites

The prerequisites differ depending on whether you're running the playbook standalone or in AAP Controller:

### For Standalone Usage:
1. Ansible installed on your local machine
2. Access to an Ansible Automation Platform instance
3. The `ansible.controller` collection or the `awx.awx` collection installed

Note on collections:
- The `ansible.controller` collection is part of Red Hat's AAP subscription and needs to be downloaded from the [Content Hub](https://console.redhat.com/ansible/automation-hub/repo/published/ansible/controller/).
- For users wanting to try out the functionality without an AAP subscription, the equivalent `awx.awx` [collection](https://docs.ansible.com/ansible/latest/collections/awx/awx/index.html) can be used. This collection is freely available and provides similar functionality.

To install the `awx.awx` collection for standalone use, run:

```
ansible-galaxy collection install awx.awx
```

### For AAP Controller Usage:
1. Access to an Ansible Automation Platform instance

Note: The `ansible.controller` collection is already provided in the default AAP execution environment, so you don't need to install it separately when running in AAP Controller.

## Usage

### Standalone Ansible

To run the playbook using standalone Ansible:

1. Clone this repository to your local machine:

2. Install the required collection:
   
   If you have an AAP subscription:
   ```
   # Download the ansible.controller collection from the Red Hat [Content Hub](https://console.redhat.com/ansible/automation-hub/repo/published/ansible/controller/)
   ```

   If you're using the AWX alternative:
   ```
   ansible-galaxy collection install awx.awx
   ```

3. Set up your AAP credentials using environment variables:

   ```
   export CONTROLLER_HOST=your-aap-hostname
   export CONTROLLER_USERNAME=your-username
   export CONTROLLER_PASSWORD=your-password
   export INVENTORY_ID=your-inventory-name
   export CONTROLLER_VERIFY_SSL=false  # Set to true if using SSL verification
   ```

4. Run the playbook:

   ```
   ansible-playbook playbook.yaml
   ```

   Note: If you're using the `awx.awx` collection, you may need to modify the playbook to use the appropriate module names, as they might differ slightly from the `ansible.controller` collection.

### Ansible Automation Platform Controller

To run this playbook in AAP Controller:

1. Log in to your AAP Controller web interface.

2. Create a "Red Hat Ansible Automation Platform" credential:
   - Go to "Credentials" and click "Add".
   - Select "Red Hat Ansible Automation Platform" as the credential type.
   - Fill in the required fields (URL, Username, Password).
   - Save the credential.

   For detailed instructions on creating this credential type, refer to the [official documentation](https://docs.ansible.com/automation-controller/4.0.0/html/userguide/credentials.html#red-hat-ansible-automation-platform).

3. Create a new Project:
   - Go to "Projects" and click "Add".
   - Name your project (e.g., "AAP Inventory Update").
   - Set the SCM Type to "Git".
   - Enter the URL of this repository.
   - Save the project.

4. Create a new Job Template:
   - Go to "Templates" and click "Add" > "Job Template".
   - Name your template (e.g., "Update AAP Inventory").
   - Select the Inventory you want to use (this should be the Controller's inventory).
   - Select the Project you created in step 3.
   - Set the Playbook to `playbook.yaml`.
   - In the "Credentials" section, add the "Red Hat Ansible Automation Platform" credential you created in step 2.
   - Save the template.

5. Run the Job Template:
   - From the Templates view, find your new template and click the rocket icon to launch it.
   - The job will execute, updating the specified host in the inventory.

Note: AAP Controller automatically provides the necessary environment variables (`CONTROLLER_HOST`, `INVENTORY_ID`, `CONTROLLER_VERIFY_SSL`, `CONTROLLER_USERNAME`, `CONTROLLER_PASSWORD`) to the Execution Environment. You don't need to manually set these when running the playbook in AAP Controller.

## Playbook Explanation

The playbook (`playbook.yaml`) performs the following tasks:

1. Runs a debug task to print a test message.
2. Updates a host in the specified AAP inventory with the following details:
   - Host name: `test.jlmayorga.xyz`
   - Description: "Test Host"
   - Enabled status: `false`
   - Ensures the host is present in the inventory

The inventory name is determined by the `INVENTORY_ID` environment variable.