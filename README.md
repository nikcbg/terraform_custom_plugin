# terraform_custom_plugin

### Purpose of the repository 
- The repository will allow us to run custom plugin locally on a virtual machine. 

#### List of files in the repository

File name                            | File description 
------------------------------------ | --------------------------------------------------------------
`scripts/provision.sh` | script that installs Go and Terraform on virtual machine. 
`Vagrantfile` | configuration file that creates and installs software on VM.
`vagrant/terraform.d/plugins/linux_amd64` | directory on VM that contains compiled plugin. 
`main.tf` | Terraform configuration file for creating a null resource. 

### How to use this repository. 
- Install `terraform` by following this [instructions](https://www.terraform.io/intro/getting-started/install.html).
- Install `vagrant` by following this [instructions](https://www.vagrantup.com/downloads.html)
- Clone the repository to your local computer: `git clone git@github.com:nikcbg/terraform_custom_plugin`.
- Go to the cloned repo on your computer: `cd terraform_custom_plugin`.

### Compiling custom plugin
- Execute `vagrant up` to create and configure the VM.
- Execute `vagrant ssh` to login to the VM we just cretaed.
- Execute `go get github.com/petems/terraform-provider-extip` to clone the repository with custom plugin.
- Execute `cd ~/go/src/github.com/petems/terraform-provider-extip` to go into the directory where the repo is cloned.
- Execute `make build` to compile the plugin the plugin from the downloaded repository.
- Execute `ls -al ~/go/bin/` to list the compiled plugin. 
- After all this we need to copy the plugin to the required path by executing below commands:
 - `mkdir -p /vagrant/terraform.d/plugins/linux_amd64`
 - `cp ~/go/bin/terraform-provider-extip /vagrant/terraform.d/plugins/linux_amd64/`

### Update `main.tf` file and test if it works
- Add below code to your `main.tf` file so it can use the custom plugin:
```
data "extip" "external_ip" {}

output "external_ip" {
  value = "${data.extip.external_ip.ipaddress}"
}
```

- After that execute below commands to see if it is working:
 - `terraform init` to initialize the working directory
 - `terraform apply` to apply the desired changes.
- If the commnds are successful the output should be as follow:

```
Apply complete! Resources: 0 added, 0 changed, 0 destroyed.

Outputs:

external_ip = 11.22.333.444
```
- The above IP is not real, it is made up for security purpose.


### TO DO: 
- Check if everything works as expected. 
