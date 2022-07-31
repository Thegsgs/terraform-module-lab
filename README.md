# Uploading modules to the terraform registry

1. Login into github and create a new repository and name it according to the terraform naming convention, i.e: terraform-<PROVIDER>-<NAME> (example: terraform-aws-ec2-instance).

2. (Optional) Add a description to the the repository for terrafrom to use as the module desctription you can also add a README.md file for this purpose.

3. Make sure your terraform file structure adheres to the terraform standard module structure. Make sure your root folder contains at least the main.tf file and your modules are under the modules/ directory.

4. 