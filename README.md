# a scalable and secure static web application

---

- There are two instances, one is chef-server, the other is workstation.
- When spining up the recipe, it updates apt cache, enables apache and generates index.html.
- Using openssl tool to generate self seigned certificate for the server.
- Creating a chef kitchen environment to test and automate the recipe, once everything runs good, destroy the kitchen.
- Before creating test kitchen, modify your .kitchen.yml configration as the example below.
- Using shell script kitchen-create.sh to create test kitchen and executing `kitchen verify` to see the test result

---

```.kitchen.yml example
driver:
  name: ec2
  aws_ssh_key_id: <Your Aws ssh key pair name>
  region: <Your availablety region>
  availability_zone: <Your availiablity zone>
  subnet_id: <Your VPC subnet id>
  instance_type: t2.micro //<- You can keep this unchanged
  image_id: <Your Image Id>
  security_group_ids: <Your SG Id>
  retryable_tries: 120

provisioner:
  name: chef_zero

verifier:
  name: inspec

transport:
  ssh_key: <where your local key pair loaction>

platforms:
  - name: ubuntu-16.04

suites:
  - name: default
    run_list:
      - recipe[webserver-ss::default]
    verifier:
      inspec_tests:
        - test/smoke/default
    attributes:
```