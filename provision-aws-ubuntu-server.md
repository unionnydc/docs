# AWS Ubuntu 14.04

### Launch Instance
- go to [ec2 dashboard](https://console.aws.amazon.com/ec2)
- choose nearest region in far top right navbar
- click `Launch Instance`

### Step 1: Choose an Amazon Machine Image (AMI)
- scroll down to `Ubuntu Server 14.04`, click `Select`

### Step 2: Choose an Instance Type
- choose instance type: typically `t2.small` for staging, `t2.medium` for production
- click `Next: Configure Instance Details`

### Step 3: Configure Instance Details
- check `Protect against accidental termination`
- click `Next: Add Storage`

### Step 4: Add Storage
- defaults are usually fine
- click ``Next: Tag Instance``

### Step 5: Tag Instance
- defaults are usually fine
- click `Next: Configure Security Group`

### Step 6: Configure Security Group
- if this is first ec2 instance, add `Security group name` and `Description`
- if not first ec2 instance, `Select an existing security group`, unless config is unique
- consider changing `Port Range` for `SSH` to a custom port
- click `Review and Launch`

### Step 7: Review Instance Launch
- review settings
- click `Launch`

### Select an existing key pair or create a new key pair
- if this is first ec2 instance, `Create a new key pair`, give it a descriptive name
- otherwise `Choose an existing key pair`
- click `Launch Instance`
