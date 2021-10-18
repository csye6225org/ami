# AMI 
```
Amazon Machine Language
```
## File name

```
ami.json
```
## Command to validate ami script

```
packer validate ami.json
```
## Command to Generate AMI
Please add correct values before running the command

```
packer build \
-var 'aws_region=' \
-var 'aws_access_key=' \
-var 'aws_secret_key=' \
-var 'subnet_id=' \
-var 'source_ami=' \
ami.json
```
