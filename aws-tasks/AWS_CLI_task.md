

```markdown
# AWS CLI Commands for VPC, Subnets, Internet Gateway, and EC2 Instances

## 1. Create a VPC
```bash
aws ec2 create-vpc --cidr-block 10.0.0.0/16 --tag-specifications ResourceType=vpc,Tags=[{Key=Name,Value=AJA_VPC}]
```
- **Explanation**:
  - `--cidr-block 10.0.0.0/16`: Defines the IP range for the VPC (65,536 IP addresses).
  - `ResourceType=vpc,Tags=[{Key=Name,Value=AJA_VPC}]`: Tags the VPC with the name `AJA_VPC`.

## 2. Tag an Existing VPC
```bash
aws ec2 create-tags --resources vpc-05a6edbf13172057a --tags Key=Name,Value=MyVPC
```

## 3. Create Subnets
```bash
aws ec2 create-subnet --vpc-id vpc-05a6edbf13172057a --cidr-block 10.0.0.0/24
aws ec2 create-subnet --vpc-id vpc-05a6edbf13172057a --cidr-block 10.0.2.0/24
aws ec2 create-subnet --vpc-id vpc-05a6edbf13172057a --cidr-block 10.0.1.0/24
aws ec2 create-subnet --vpc-id vpc-05a6edbf13172057a --cidr-block 10.0.16.0/24
```

## 4. Tag Subnets
```bash
aws ec2 create-tags --resources subnet-08becd6a983015845 --tags Key=Name,Value=public
aws ec2 create-tags --resources subnet-021c4d0dff230fb3b --tags Key=Name,Value=private
```

## 5. Describe Subnets
```bash
aws ec2 describe-subnets --filters "Name=vpc-id,Values=vpc-05a6edbf13172057a" --query "Subnets[*].{ID:SubnetId,CIDR:CidrBlock}"
```

## 6. Create an Internet Gateway and Attach to VPC
```bash
aws ec2 create-internet-gateway --tag-specifications ResourceType=internet-gateway,Tags=[{Key=Name,Value=my-igw}]
aws ec2 attach-internet-gateway --vpc-id vpc-05a6edbf13172057a --internet-gateway-id igw-05297a3a9804d2117
```

## 7. Detach Internet Gateway from VPC
```bash
aws ec2 detach-internet-gateway --vpc-id vpc-05a6edbf13172057a --internet-gateway-id igw-0f796cd4dfd6bebb3
```

## 8. Create Route Tables and Associate with Subnets
```bash
aws ec2 create-route-table --vpc-id vpc-05a6edbf13172057a
aws ec2 create-tags --resources rtb-02c924dab30eee963 --tags Key=Name,Value=public_route
aws ec2 create-tags --resources rtb-068a9711a7e29b5ec --tags Key=Name,Value=private_route

aws ec2 associate-route-table --route-table-id rtb-02c924dab30eee963 --subnet-id subnet-08becd6a983015845
aws ec2 associate-route-table --route-table-id rtb-068a9711a7e29b5ec --subnet-id subnet-021c4d0dff230fb3b
```

## 9. Create Routes in Route Table
```bash
aws ec2 create-route --route-table-id rtb-02c924dab30eee963 --destination-cidr-block 0.0.0.0/0 --gateway-id igw-0f796cd4dfd6bebb3
```

## 10. Create Security Groups and Set Rules
```bash
# Security group for SSH access
aws ec2 create-security-group --group-name MySSHGroup --description "Security group for SSH access" --vpc-id vpc-05a6edbf13172057a
aws ec2 authorize-security-group-ingress --group-id sg-08020d1ad8d4bbcbf --protocol tcp --port 22 --cidr 0.0.0.0/0

# Security group for HTTP access
aws ec2 create-security-group --group-name MyHTTPGroup --description "Security group for HTTP access" --vpc-id vpc-05a6edbf13172057a
aws ec2 authorize-security-group-ingress --group-id sg-08020d1ad8d4bbcbf --protocol tcp --port 80 --cidr 0.0.0.0/0
```

## 11. Launch EC2 Instances
```bash
aws ec2 run-instances --image-id ami-07c340d55b1de7924 --instance-type t2.micro --key-name devops --subnet-id subnet-08becd6a983015845 --security-group-ids sg-08020d1ad8d4bbcbf --associate-public-ip-address
aws ec2 run-instances --image-id ami-07c340d55b1de7924 --instance-type t2.micro --key-name devops --subnet-id subnet-021c4d0dff230fb3b --security-group-ids sg-08020d1ad8d4bbcbf --associate-public-ip-address
```

## Command History
```bash
history
doskey /history
```
```