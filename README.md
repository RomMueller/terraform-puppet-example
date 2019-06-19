# Terraform Puppet Provisioner Example

## Prerequisites

 * Terraform >= 0.12.2
 * Bolt

Once Bolt is installed, a couple of additional Bolt modules are required. Add
the following lines to your Bolt `Puppetfile` (typically at
`~/.puppetlabs/bolt/Puppetfile`).

```
mod 'danieldreier-autosign'
mod 'puppetlabs-puppet_agent'
```

Then run `bolt puppetfile install`.

Create a `terraform.tfvars` file containing the following (substituting your
AWS credentials, region, etc).

```
access_key = "<AWS access key ID>"
secret_key = "<AWS secret access key>"
region = "us-east-1"
aws_key_pair = "<AWS key pair name>"
aws_ami_id = "<AWS ubuntu 16.04 x86_64 AMI in your region>"
```

## Spinning up a test Puppet master

`terraform apply -target=aws_instance.puppetmaster` will create a basic Puppet
master instance for the purposes of this test. This will take a while to run
and may show errors during the provisioning process (depending on the specs of
the instance, the Puppet master process can take a while to start accepting
connections), but the Puppet runs will retry automatically on failure.

## Spinning up test instances provisioned with Puppet agent

There are 4 preconfigured `aws_instance` resources that use the `puppet`
provisioner in `example.tf` that you can try out with `terraform apply
-target=<resource>`.

 * `aws_instance.agent` - Ubuntu instance provisioned with a Puppet Enterprise
   agent.
 * `aws_instance.os_agent` - Ubuntu instance provisioned with an open source
   Puppet agent.
 * `aws_instance.win_agent` - Windows 2012 instance provisioned with a Puppet
   Enterprise agent.
 * `aws_instance.os_win_agent` - Windows 2012 instance provisioned with an open
   source Puppet agent.
