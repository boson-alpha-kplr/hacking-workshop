# AWS CloudGoat Attack Setup

### UPDATE APT-GET

`sudo apt-get -y update`

### INSTALL TERRAFORM CLI

`wget https://releases.hashicorp.com/terraform/1.0.8/terraform_1.0.8_linux_amd64.zip` 
`unzip terraform_1.0.8_linux_amd64.zip ` 
`sudo mv terraform /usr/local/bin`

`terraform -version`

### INSTALL AWS CLI

`sudo apt-get -y remove --auto-remove awscli`

`curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"`
`unzip awscliv2.zip`
`sudo ./aws/install`

`aws --version`

### INSTALL CLOUDGOAT

`git clone https://github.com/RhinoSecurityLabs/cloudgoat.git`
`sudo apt install -y python3-pip`

`cd cloudgoat`
`pip install -r ./core/python/requirements.txt`
`chmod u+x cloudgoat.py`

`./cloudgoat.py config profile`

```
No configuration file was found at /home/kali/cloudgoat/config.yml
Would you like to create this file with a default profile name now? [y/n]: y
Enter the name of your default AWS profile: cloudgoat
A default profile name of "cloudgoat" has been saved.
``` 

`./cloudgoat.py config whitelist --auto`

```
No whitelist.txt file was found at /home/kali/cloudgoat/whitelist.txt

CloudGoat can automatically make a network request, using https://ifconfig.co to find your IP address, and then overwrite the contents of the whitelist file with the result.
Would you like to continue? [y/n]: y

whitelist.txt created with IP address 3.80.166.190/32
```

```
aws configure --profile cloudgoat
AWS Access Key ID [None]: AKIAXS5AU5FGUCOYPAK5
AWS Secret Access Key [None]: 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
Default region name [None]: us-east-1
Default output format [None]:
```

`export AWS_DEFAULT_PROFILE=cloudgoat`


