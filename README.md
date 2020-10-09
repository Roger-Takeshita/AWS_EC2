<h1 id='contents'>Table of Contents</h1>

- [AWS EC2](#aws-ec2)
  - [Links](#links)
  - [What is EC2?](#what-is-ec2)
  - [Amazon Update - Linux 1 vs Linux 2](#amazon-update---linux-1-vs-linux-2)
  - [Amazon EC2 Basics](#amazon-ec2-basics)
    - [Create a New Instance](#create-a-new-instance)
    - [Stop an Instance](#stop-an-instance)
    - [SSH Into EC2 Instance](#ssh-into-ec2-instance)
      - [Connecting Via Terminal](#connecting-via-terminal)
        - [SSH CONFIG FILE](#ssh-config-file)
    - [Security Groups](#security-groups)
    - [EC2 User Data](#ec2-user-data)
      - [Installing Apache HTTP Manually](#installing-apache-http-manually)
      - [Installing Apache HTTP Using Script](#installing-apache-http-using-script)
    - [EC2 Logs](#ec2-logs)

# AWS EC2

[Go Back to Summary](#contents)

## Links

[Go Back to Summary](#contents)

- [AWS US-EAST-1 - Free Tier Info](https://console.aws.amazon.com/billing/home?region=us-east-1#/freetier)

  ![](https://i.imgur.com/1Yi2nAH.png)

## What is EC2?

[Go Back to Contents](#contents)

- It mainly consists in the capacity of:
  - Renting virtual machines (EC2)
  - Store data on virtual drives (EBS)
  - Distributing load across machines (Elastic Load Balance - ELB)
  - Scaling the services using an auto-scaling group (ASG)
- Knowing EC2 is fundamental to understand how the Cloud works.

## Amazon Update - Linux 1 vs Linux 2

[Go Back to Contents](#contents)

- **Amazon Linux 1** uses **sysvinit** for launching the services.
- **Amazon Linux 2** uses **systemd** for launching the services.

* Overall, the general idea is the exact same

## Amazon EC2 Basics

[Go Back to Contents](#contents)

- On AWS console go to `Services > Compute > EC2`

  ![](https://i.imgur.com/xqNEe8s.png)

- On Resources we can see that we don't have anything yet for this region. It says:

  ```Bash
    0 Instances
    0 Volumes
    0 Key Pairs
  ```

  ![](https://i.imgur.com/Neq4PSH.png)

### Create a New Instance

[Go Back to Contents](#contents)

- On `EC2 Dashboard > instances > instance`

  1. Click on **Launch instances**

     ![](https://i.imgur.com/NP5sA8x.png)

  2. Choose an Amazon Machine Image (AMI)

     - In our case we are going to choose a free tier of **Amazon Linux 2 AMI (HVM), SSD Volume Type**

     ![](https://i.imgur.com/d3IeO9V.png)

  3. Choose an Instance Type

     - Instance types is basically how big our instance is going to be
     - For now we are going to use the **Free tier eligible**
     - Then click on **Next: Configure Instance Details**

     ![](https://i.imgur.com/L11fgJn.png)

  4. Configure Instance Details

     - For now make sure to set:

       ```Bash
         Number of Instances: 1
         Network: (default)
         Subnet: subnet ... | Default in ... a
         Shutdown behavior: Stop
       ```

     - Then click on **Next: Add Storage**

     ![](https://i.imgur.com/UACfgZb.png)

  5. Add Storage

     - Here we can the see the `volume type`, the `path` and the `size of the disc`.
     - click on **Next: Add Tags**

     ![](https://i.imgur.com/wLo3Vxc.png)

  6. Add Tags

     - Tags aren't necessary right now
     - Click on **Next: Configure Security Group**

  7. Configure Security Group

     - Here we are going to create a new security group
     - We are going to call it, **my-first-instance**
     - For now, leave it the **Port Range (22)** and **Source (0.0.0.0)** as it is. We are going to change later
     - Then click on **Review and Launch**

     ![](https://i.imgur.com/JQOAIM5.png)

  8. Review Instance Launch

     - Here we can review all the setting of our machine
     - Then click on **Launch**
     - ![](https://i.imgur.com/0tG84YA.png)

     - It will prompt a popup asking to **select an existing key pair or create a new key pair**

       - A key pair consists of a public key that AWS stores, and a private key file that you store. Together, they allow you to connect to your instance securely. For Windows AMIs, the private key file is required to obtain the password used to log into your instance. For Linux AMIs, the private key file allows you to securely SSH into your instance.
       - **Note:** The selected key pair will be added to the set of keys authorized for this instance. Learn more about [removing existing key pairs from a public AMI](https://docs.aws.amazon.com/console/ec2/launchinstance/key-pair/remove).
       - **ATTENTION** never share the Key Pair, it contains the private ssh key

       * Since we don't have a key pair, we are going to create one
         - In my case I named as `EC2-Roger-Takeshita`
         - After that, **Download Key Pair** (`EC2-Roger-Takeshita.pem` File)
           - The `.pem` file, contains our private key that
           ```Bash
             -----BEGIN RSA PRIVATE KEY-----
             vfasdfasdfasdfasdfkasdfjalsjdflajsldjflajsldfjlajsdlfjasjdfjavi5
             MIIEfhahdKHALSKDHFLASDFhajsdlfjalkjSLKDJA;SLDFJASDFAAFSDFASDBfVM
             2OrM2+3F+tlIjZdNAFW/lVASDFAFASFASDFAASDF+KU2c8qcvlgSQgGX1+cxdMJr
             vfasdfasdfasdfasdfkasdfjalsjdflajsldjflajsldfjlajsdlfjasjdfjavi5
             IASDFcRPdaJ64BRlwVElt9fm0RPAjHumU1Zw6Rv/VnFASDFASDFASDFASDFASDFA
             ADFAlSgc3L/fasdfasdfasdfasdfasdf/bASDTqEUbB4lHsn S/6asdfasasH9M=
             -----END RSA PRIVATE KEY-----
           ```
         - Then click on **Launch Instances**

       ![](https://i.imgur.com/kOGzsGI.png)

  9. Launch Status

     - Click on **View Instance**

     ![](https://i.imgur.com/KvmSHeB.png)

  10. Instances

      - Here we can see that we have an instance running

      ![](https://i.imgur.com/lf2P92y.png)

### Stop an Instance

[Go Back to Contents](#contents)

- If you are not using an instance and want to save some money. We can stop an instance by

  - `Right Click > Instance State > Stop`

    ![](https://i.imgur.com/mGbUECF.png)

### SSH Into EC2 Instance

[Go Back to Contents](#contents)

- SSH is one of the most important function. It allows you to control a remote machine, all using the command line.
- SSH will allow us to connect to our EC2 Instance on **Port 22** that we configured on **Step 7**

#### Connecting Via Terminal

[Go Back to Contents](#contents)

- To connect to our EC2 instance using SSH via terminal, we have to get:

  - The Port: `Port 22`
  - Public DNS: `ec2-18-207-122-118.compute-1.amazonaws.com`

  ![](https://i.imgur.com/D6ERLOo.png)

  1. Open up the terminal
  2. Type `ssh -i ./path-to-your-private-key ec2-user@your-dns`

     - `ssh ./EC2-Roger-Takeshita.pem ec2-user@ec2-18-207-122-118.compute-1.amazonaws.com`

     - The first time we try to access our ssh, It will display a warning saying **UNPROTECTED PRIVATE KEY FILE!**

       - `Permission 0644 for ./EC2-Roger-Takeshita.pem are too open`

       ![](https://i.imgur.com/cFhDEYk.png)

     - We need to change the permission to **400** - [Permission 400](https://chmodcommand.com/chmod-400/) means that is only allowed for **owner to read**
       - The command to change the permission is `chmod 400`
         - `chmod 400 EC2-Roger-Takeshita.pem`
         - We only do this one time per `.pem` file
     - After that, we just run the same command again
       - `ssh ./EC2-Roger-Takeshita.pem ec2-user@ec2-18-207-122-118.compute-1.amazonaws.com`
         ![](https://i.imgur.com/Q4x8WqW.png)

  3. Type `exit` to close the connection to your EC2

##### SSH CONFIG FILE

[Go Back to Contents](#contents)

- A quick and easier way to connect to EC2, is to configure the ssh config file
- On your terminal type `code ~/.ssh/config`, then add:

  ```Bash
    Host my-first-instance
    HostName ec2-18-207-122-118.compute-1.amazonaws.com
    User ec2-user
    IdentityFile ~/path/EC2-Roger-Takeshita.pem
  ```

- To connect, just run `ssh my-first-instance`

### Security Groups

[Go Back to Contents](#contents)

- Security Groups are the fundamental of network security in AWS
- They control how traffic is allowed or denied into or out of our EC2 Machines

1. On `AWS Console > Instances`

- Click on `Security Tab`

  ![](https://i.imgur.com/31AsvIi.png)

2. On `Security Tab`

   - We can check our **Inbound** and **Outbound**
   - Click on `sg-07d3e6e243ba033d2 (my-first-instance)`
     ![](https://i.imgur.com/jJIFtNh.png)

3. On `sg-07d3e6e243ba033d2 - my-first-instance`

   - Click on **Edit inbound rules**
     ![](https://i.imgur.com/GeqAKK5.png)

   - If we delete our **Inbound** rule, this will block all connections
   - In our case we are going to change the Source, from `custom` to `MyIp`. This will automatically get our Ip Address. And only connections from this Ip Address is allowed to connect to your EC2 instance.
   - **NOTE** Home internet changes the Ip weekly, so If your try to connect to your EC2 instance, and the connection times out, probably you are using a different IP.
     ![](https://i.imgur.com/n3QGSGF.png)

### EC2 User Data

[Go Back to Contents](#contents)

- It is possible to boostrap our instances using an **EC2 User Data script**
- **bootstraping** means launching commands when a machine starts
- The script is **only run once** at the instance **first start**
- EC2 user data is used to automate boot tasks such as:
  - Installing updates
  - Installing softwares
  - Downloading common files from the internet

#### Installing Apache HTTP Manually

[Go Back to Contents](#contents)

- On `Instances > Security Groups > ... - my first instance > Edit inbounds rules`

  - Adde a new inbound rule to allow http access

    ![](https://i.imgur.com/2zJcdw3.png)

- After enabling http, we need to install **http** on our EC2

  - On terminal, SSH into our EC2
  - Install the following commands

    ```Bash
      sudo su
      yum update
      yum install -y httpd.x86_64
      systemctl start httpd.service
      systemctl enable httpd.service
    ```

  - On the browser we now can access our EC2 page

    ![](https://i.imgur.com/3YOUsBR.png)

#### Installing Apache HTTP Using Script

[Go Back to Contents](#contents)

- To install Apache while creating a new instance. On `Configure Instance Details`

  - On `Advanced Details`

    - Set the `User Data` **As text**
    - Paste the following command:
      - The `#!/bin/bash` is necessary so AWS can understand that this is a Bash file

    ```Bash
      #!/bin/bash
      yum update
      yum install -y httpd.x86_64
      systemctl start httpd.service
      systemctl enable httpd.service
    ```

    ![](https://i.imgur.com/ycpUKYO.png)

### EC2 Logs

[Go Back to Contents](#contents)

- To check the instances log, you just need to `Right Click > Instance Settings > Get System Log`

  ![](https://i.imgur.com/D8ruGAE.png)

  ![](https://i.imgur.com/1blK8j8.png)
