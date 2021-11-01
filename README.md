# AMI 
```
Amazon Machine Image
```
...
# Techstack
```
1. Amazon Web Services
2. Hashicorp Packer
3. JSON
```

## Assignment 4: How to Demo?
### 1. Git Demo:

1. Create pull requests between
```
1. Main branch of organization and assignment branch of fork.
2. Main branch of organization and main branch of fork.
3. Main branch of fork and assignment branch of fork.
```

There should be nothing to compare.

2.  TAs and instructors are collaborators to the GitHub repository.
```
https://github.com/orgs/csye6225org/people
```

3. Show 'README.md' file
```
https://github.com/csye6225org/webapp
https://github.com/csye6225org/infrastructure
https://github.com/csye6225org/ami
```

4. Show that repository is cloned correctly.
   Execute the following commands in terminal.
```
# cd /home/varad/Desktop/NSC

# cd /home/varad/Desktop/NSC/Github/webapp/
# git remote -v

# cd /home/varad/Desktop/NSC/Github/infrastructure/
# git remote -v

# cd /home/varad/Desktop/NSC/Github/ami/
# git remote -v
```
### 2. Demo Building AMIs

1. Build AMI

```
# cd /home/varad/Desktop/NSC/Github/ami

# ./buildAmi.sh
```

### 3. Update terraform.tfvars

```
    1. Update Ami Id
    2. Change Access Keys to prod
    3. Change ssh key to prod
```