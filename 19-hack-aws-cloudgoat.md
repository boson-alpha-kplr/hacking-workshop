### UPDATE APT-GET

`sudo apt-get -y update`

### INSTALL TERRAFORM CLI

`wget https://releases.hashicorp.com/terraform/1.0.8/terraform_1.0.8_linux_amd64.zip 
unzip terraform_1.0.8_linux_amd64.zip  
sudo mv terraform /usr/local/bin`

`terraform -version`

### INSTALL AWS CLI

`sudo apt-get -y remove --auto-remove awscli`

`curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install`

aws --version

### INSTALL CLOUDGOAT

git clone https://github.com/RhinoSecurityLabs/cloudgoat.git
sudo apt install -y python3-pip

cd cloudgoat
pip install -r ./core/python/requirements.tx
chmod u+x cloudgoat.py

./cloudgoat.py config profile

No configuration file was found at /home/kali/cloudgoat/config.yml
Would you like to create this file with a default profile name now? [y/n]: y
Enter the name of your default AWS profile: cloudgoat
A default profile name of "cloudgoat" has been saved.

./cloudgoat.py config whitelist --auto

No whitelist.txt file was found at /home/kali/cloudgoat/whitelist.txt

CloudGoat can automatically make a network request, using https://ifconfig.co to find your IP address, and then overwrite the contents of the whitelist file with the result.
Would you like to continue? [y/n]: y

whitelist.txt created with IP address 3.80.166.190/32

aws configure --profile cloudgoat
AWS Access Key ID [None]: AKIAXS5AU5FGUCOYPAK5
AWS Secret Access Key [None]: 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
Default region name [None]: us-east-1
Default output format [None]:

export AWS_DEFAULT_PROFILE=cloudgoat

### AWS : PRIVESC BY ROLLBACK


cd ~/cloudgoat
./cloudgoat.py create iam_privesc_by_rollback

[cloudgoat] Output file written to:

    /home/kali/cloudgoat/iam_privesc_by_rollback_cgid860byu66de/start.txt

    ┌──(kali㉿kali)-[~/cloudgoat]
    └─$ cat /home/kali/cloudgoat/iam_privesc_by_rollback_cgid860byu66de/start.txt                                    

    cloudgoat_output_aws_account_id = 521638570317
    cloudgoat_output_policy_arn = arn:aws:iam::521638570317:policy/cg-raynor-policy-iam_privesc_by_rollback_cgid860byu66de
    cloudgoat_output_raynor_access_key_id = AKIAXS5AU5FGW46UIYSY
    cloudgoat_output_raynor_secret_key = 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
    cloudgoat_output_username = raynor-iam_privesc_by_rollback_cgid860byu66de

                                                                                                                                       
    ┌──(kali㉿kali)-[~/cloudgoat]
    └─$ aws configure --profile ray                                                  
    AWS Access Key ID [None]: AKIAXS5AU5FGW46UIYSY             
    AWS Secret Access Key [None]: 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
    Default region name [None]: 
    Default output format [None]: 
                                                                                                                                       
    ┌──(kali㉿kali)-[~/cloudgoat]
    └─$ export AWS_PROFILE=ray              
                                                                                                                                       
    ┌──(kali㉿kali)-[~/cloudgoat]
    └─$ aws ec2 describe-instances                                                   

    An error occurred (UnauthorizedOperation) when calling the DescribeInstances operation: You are not authorized to perform this operation.
               
