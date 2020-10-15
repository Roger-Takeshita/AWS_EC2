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
  - [AMAZON Machine Images (AMI)](#amazon-machine-images-ami)
    - [Create an AMI](#create-an-ami)
      - [Config an Image](#config-an-image)
      - [Create an Image](#create-an-image)
    - [Start a New Instance From AMI](#start-a-new-instance-from-ami)
    - [Public AMIs](#public-amis)
    - [AMI Storage - Amazon S3](#ami-storage---amazon-s3)
  - [Choosing The Right EC2 Instance Type](#choosing-the-right-ec2-instance-type)
    - [RAM (Random Access Memory)](#ram-random-access-memory)
      - [RAM in EC2](#ram-in-ec2)
    - [CPU (Central Processing Unit)](#cpu-central-processing-unit)
      - [CPU in EC2](#cpu-in-ec2)
    - [IO (Input / Output)](#io-input--output)
      - [IO in EC2](#io-in-ec2)
    - [Network](#network)
      - [Network in EC2](#network-in-ec2)
    - [GPU](#gpu)
      - [GPU in EC2](#gpu-in-ec2)
    - [General Instance (M)](#general-instance-m)
    - [Burstable Instance (T2)](#burstable-instance-t2)
      - [CPU Credits](#cpu-credits)
    - [T2 Unlimited](#t2-unlimited)
  - [Network and Security](#network-and-security)
    - [Security Groups Overview](#security-groups-overview)
  - [IP and CIDR](#ip-and-cidr)
    - [Private vs Public IP (IPv4)](#private-vs-public-ip-ipv4)
    - [CIDR](#cidr)
      - [Understanding CIDR](#understanding-cidr)
  - [Security Group](#security-group)
    - [Create a SSH Security Group](#create-a-ssh-security-group)
    - [Create a Java Server Security Group](#create-a-java-server-security-group)
    - [Update Instance Security Group](#update-instance-security-group)
    - [Referencing Other Security Groups](#referencing-other-security-groups)
      - [Exercise](#exercise)
    - [Referencing Circular Security Groups](#referencing-circular-security-groups)
  - [Elastic IPs](#elastic-ips)
    - [Pricing](#pricing)
    - [Exercise](#exercise-1)
  - [Placement Groups](#placement-groups)
    - [Creating a Placement Group](#creating-a-placement-group)
  - [Load Balancing](#load-balancing)
    - [Why Use a Load Balancing](#why-use-a-load-balancing)
    - [Why Use an EC2 Load Balance?](#why-use-an-ec2-load-balance)
    - [Types of Load Balancer on AWS](#types-of-load-balancer-on-aws)
    - [Create Classic Load Balancer (v1)](#create-classic-load-balancer-v1)
    - [Securing Loader Balance](#securing-loader-balance)
    - [Health Checks](#health-checks)
      - [Checking If It's Health](#checking-if-its-health)
    - [Application Load Balancer (v2)](#application-load-balancer-v2)
    - [Create a Target Group](#create-a-target-group)
      - [Register Targets to Target Groups](#register-targets-to-target-groups)
    - [Create an Application Load Balancer (v2)](#create-an-application-load-balancer-v2)
    - [Load Balancer Listeners](#load-balancer-listeners)
    - [Network Load Balancer (v2)](#network-load-balancer-v2)

# AWS EC2

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

     ![](https://i.imgur.com/0tG84YA.png)

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

    - Or through **Public IPv4 address**: `18.207.122.118`

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

## AMAZON Machine Images (AMI)

[Go Back to Contents](#contents)

- As we saw, AWS comes with base images such as:
  - Ubuntu
  - Fedora
  - RedHad
  - Windows
- These images can be customized at run tim using **EC2 User Data**
- It's also possible to create images of our machine so we can use them to build instantiate a new instance (like a recovery disk). Where we have all the drivers updated and softwares installed.

- **Advantages**

  - Pre-installed packages needed
  - Faster boot time (no need for EC2 User Data at boot time)
  - Machine comes configured with monitoring / enterprise software
  - Control of maintenance and updates of AMIs over time
  - Active Directory Integration out of the box
  - Installing your app ahead of time (for faster deploys when auto-scaling)
  - Using someone elses AMI that is optimized for running an APP, DB, etc...
  - **AMI are built for a specific AWS region**

- On `EC2 Console > Images > AMIs`

  ![](https://i.imgur.com/d9IHMu9.png)

### Create an AMI

[Go Back to Contents](#contents)

- We are going o create a new AMI with the following softwares installed:

  - Ubuntu Updates
  - Java 8
  - Java Webserver - An app that we are going to download from [GitHub](https://github.com/simplesteph/ec2-masterclass-sampleapp)

- We are going to use AMI for:
  - Auto scaling
  - Load balancing
  - Troubleshooting and choosing the right EC2 instance type

#### Config an Image

[Go Back to Contents](#contents)

1. Start fresh with a new instance
2. SSH into this new instance
3. Install manually

   ```Bash
     # upgrade machine
     sudo yum update -y

     # install java 8 jdk
     sudo yum install -y java-1.8.0-openjdk-devel

     # set java jdk 8 as default
     sudo /usr/sbin/alternatives --config java
        # It'll ask to select the version os the java that you want to use
     sudo /usr/sbin/alternatives --config javac
        # It'll ask to select the version os the java that you want to use

     # verify java 8 is the default
     java -version

     # Change dir to root folder
     cd /home/ec2-user

     # Download app - Java Webserver
     wget https://github.com/simplesteph/ec2-masterclass-sampleapp/releases/download/v1.0/ec2-masterclass-sample-app.jar
        # Basically the wget will got github's page, into release page and will download the c2-masterclass-sample-app.jar file (v1.0 Release)

     # Run server
     java -Xmx700m -jar ec2-masterclass-sample-app.jar
        # java                           - this command will run java
        # -Xmx700m                       - will allocate 700MB of memory for this application
        # -jar                           - to announce where the jar is
        # ec2-masterclass-sample-app.jar - the file name
   ```

   ![](https://i.imgur.com/stCbDOB.png)

4. Start Server

   - After running the command, we can see that **Spark has started ...**
   - And our application is running on port **4567**
   - If we try to access this webpage on the browser, It won't do nothing. Because we have a security group issue. We need to add the port **4567** to our inbound ports.

     ![](https://i.imgur.com/8x1NOKn.png)

   - Now if we try again to access our website

     ![](https://i.imgur.com/5o127s8.png)

   - A few available commands
     - `http://3.234.177.143:4567/cpu` - Fibonacci
     - `http://3.234.177.143:4567/ram` - Starts to fill the RAM
     - `http://3.234.177.143:4567/ram/info` - RAM status
     - `http://3.234.177.143:4567/ram/clean` - Clean the RAM
     - `http://3.234.177.143:4567/heath` - Health
     - `http://3.234.177.143:4567/detals` - Returns the details of the request
   - `Ctrl+c` - Stop the application

5. Automate start server during boot boot time

   - Copy the following command and paste on your terminal

   ```Bash
     sudo bash -c 'cat << \EOF > /etc/systemd/system/ec2sampleapp.service
     [Unit]
     Description=EC2 Sample App
     After=network.target

     [Service]
     ExecStart=/usr/bin/java -Xmx700m -jar /home/ec2-user/ec2-masterclass-sample-app.jar
     Restart=on-failure

     [Install]
     WantedBy=multi-user.target
     EOF'
   ```

   **ATTENTION** Remove the spaces from the beginning of each command if you are copying from this page

6. Set the permissions to start our application across reboots

   - Enable on boot

     - `sudo systemctl enable ec2sampleapp.service`

   - Start now
     - `sudo systemctl start ec2sampleapp.service`

7. Reboot - after configuring the machine

   - `sudo reboot`

#### Create an Image

[Go Back to Contents](#contents)

- On `EC2 Dashboard > Instances` then `Right Click > Image > Create Image`

  ![](https://i.imgur.com/TK63Ts5.png)

- On `Create Image`

  - Image name: `EC2-Sample-Java-Server`
  - Image description: `EC2 machine that runs our Java server`
  - Click on **Create image**

    ![](https://i.imgur.com/atmDO0Z.png)

- On `EC2 Dashboard > Images > AMIs`

  - We can check the status of our image
  - **Note**: every time we create a new image, the EC2 will reboot. This can prevent corrupting the image.

    ![](https://i.imgur.com/l7mKR9g.png)

### Start a New Instance From AMI

[Go Back to Contents](#contents)

- On `EC2 Dashboard > Images > AMIs`

  - `Right Click > Launch`

    ![](https://i.imgur.com/rrVphmF.png)

  - And then the rest of the process is the same as when we created a new instance manually.

### Public AMIs

[Go Back to Contents](#contents)

- We can get other AMIs from other developer (free)
- We can also pay for other people's AMI by the hour
  - Optimised the image
  - The machine is easy to run and configure
  - We just rent the "expertise" from the AMI creator
- AMI can be found and published on the Amazon Marketplace

  - **ATTENTION** don't use an AMI that you don't trust

    ![](https://i.imgur.com/aGjx1Da.png)

### AMI Storage - Amazon S3

[Go Back to Contents](#contents)

- Your AMI take space and they live in Amazon S3
- Amazon S3 is a durable, cheap an resilient storage where most of our backups will live (but we wont' see them in the S3 console).
- By default all our AMI are **private** and **locked** for our account and region.
- We can also make ours AMIs public and share them with other AWS accounts or sell them on the AMI Marketplace.

## Choosing The Right EC2 Instance Type

[Go Back to Contents](#contents)

- Instances have 5 distinct characteristics advertised on the website:

  - The RAM (type, amount, generation)
  - The CPU (type, make, frequency, generation, number of cores)
  - The I/O (disk performance, EBS optimizations)
  - The Network (network bandwidth, network latency)
  - The Graphical Processing Unit (GPU)

- There are over 50 [types](https://aws.amazon.com/ec2/instance-types/?nc1=h_ls)
- [EC2Instances.Info](https://ec2instances.info/)

### RAM (Random Access Memory)

[Go Back to Contents](#contents)

- If our EC2 doesn't have enough memory, following errors may occur

  - OutOfMemory error
  - The RAM will extend to the disk (it's called swapping, and it's way slower)

#### RAM in EC2

[Go Back to Contents](#contents)

- Some EC2 machines come with a lot amount of RAM for cheap
  - These machines are **R generation** or **X1** (if you have a lot of money)
  - You should use them if your application requires a lot of memory
- Monitor memory usage:

  - `free -m` command, display the available memory

    ![](https://i.imgur.com/FQDDzjJ.png)

  - `top` command, display all the running process

    ![](https://i.imgur.com/Lt63pcd.png)

    - To sort the items by memory usage

      - `Shitf + F`
      - Then move around with the arrow keys (up and down)
      - `s` to set to sort by this selected item
      - `q` to go back

        ![](https://i.imgur.com/gxn09NM.png)

        ![](https://i.imgur.com/wI0YAsT.png)

### CPU (Central Processing Unit)

[Go Back to Contents](#contents)

- The central processing unit (CPU) of a computer is a piece of hardware that carries out the instructions of a computer program.
- It performs the basic arithmetical, logical, and input/output operations of a computer system.

- **When is CPU Used**

  - Anytime your server needs to perform a computation, or an instruction, the CPU will be used.
  - The CPU is always active, as your computer always processes actions
  - You can use the **top** command to find the current amount of CPU being used.
  - If the CPU is not fast enough or does not have enough cores, you'll get:

    - CPU usage of each core at 100%
    - The server will seriously slow down

  - Adding more cores does not help your application if it's single thread application.
  - You know if your application is single threaded if **top** shows 100% and you have 2 cores. If it was multi-threaded it would show 200%

#### CPU in EC2

[Go Back to Contents](#contents)

- Some EC2 machines have optimised CPU for frequency or # of cores (vCPU)
- These machines are **C generation**
- You should use them if your application requires multi-threading or very fast processor.

### IO (Input / Output)

[Go Back to Contents](#contents)

- I/O (input / output) is the concept of writing or reading from a disk
- It's measured in the following aspects:

  - Latency
  - Random I/O performance (random read / writes)
  - Sequential read and write performance

- **When is IO Used?**
  - Anytime the application will read and write to the disk, IO will be used
  - At startup, the OS will read the disk to load settings
  - You can use the **iostat** command to find the current amount of IO being used
  - IO are heavily used by database, MapReduce (BigData), websites serving big files / images coming from disk.
  - If the IO is not large enough, you will get:
    - Timeouts (some operations can't complete on time)
    - Slowdowns
    - Crashes
  - **Good IO performance will be crucial to ensure the database has always a good performance**

#### IO in EC2

[Go Back to Contents](#contents)

- Some EC2 machines come with attached disks or optimizations to read from EBS volumes
- These machines are **I generation** - SSD-backed Instance storage optimized for low latency, very high random I/O performance, high sequential read throughput and provide high IOPs at a low cost (good for ElasticSearch, NoSQL databases, analytics workloads...)
- Other machines are of **H** or of type **D** (MapReduce, HDFS, Big Data, Kafka)

### Network

[Go Back to Contents](#contents)

- Network is the concept of how fast a machine can send and receive information from other machines.
- It is measured in the following aspects:
  - Latency
  - Throughput / Bandwidth
- Networking in AWS is all done using cables (ethernet / optical fibre)

- **When is Network is Used?**
  - Anytime your application needs to interact with the web or other server, network will be used.
  - You can use the **nload** ofr **iftop** command to find the current amount of network being used.
  - If your network is not fast enough:
    - Your application may timeout
    - Latency may be increased
  - Typical applications that use a lot of network bandwidth
    - FTP servers
    - Apache Kafka
    - Framework that do shuffling such as Apache Spark
    - Distributed File System

#### Network in EC2

[Go Back to Contents](#contents)

- All EC2 machines come with certain network performances
- Some EC2 machines have much greater amount of bandwidth reserved to them
- Measure bandwidth needed for your application

### GPU

[Go Back to Contents](#contents)

- GPU is the graphical processing unit. In a normal computer that provides people a screen, it's used to compute the colour of the pixels on a screen.
- It is measured in the following aspects:
  - Number of cores (sometimes well over 1024)
  - Internal GPU memory
- **When is GPU Used?**
  - In AWS, the GPU is used in many different ways:
    - To process videos, convert a video
    - To perform computations (finance, fluid dynamics, analysis, etc...)
    - To perform machine learning (deep learning, speech recognition, etc...)

#### GPU in EC2

[Go Back to Contents](#contents)

- Most EC2 machines do not come with a GPU
- The ones tha come with GPU are:
  - P-generation (P3 latest)
  - G-generation (G3 latest)

### General Instance (M)

[Go Back to Contents](#contents)

- Some intensives come with good balance between:
  - RAM
  - CPU
  - Network
- These instances are never a bad choice to get started
- They're the **M generation**

### Burstable Instance (T2)

[Go Back to Contents](#contents)

- AWS has the concept of burstable instances (T2 machines)
- Burst means that overall, the instance has **OK CPU performance**
- When the machine need to process something unexpected (a spike in load for example), it can burst, and CPU can be very good.
- If the machine bursts, it utilizes **burst credits**
- If all the credits are gone, the CPU becomes **BAD**
- If the machine stops bursting, credits are accumulated again over time
- Burstable instances can be amazing to handle unexpected traffic and getting the insurance that it will be handled correctly
- If your instance consistently runs low on credit, you need to move to a different kind of non-burstable instance.

#### CPU Credits

[Go Back to Contents](#contents)

![](https://i.imgur.com/yzMYvef.png)

- we can monitor or CPU credit on Amazon EC2 Dashboard
- On `EC2 Dashboard > Instances`

  - Click on your instance that you want to check your cpu usage, then go to **Monitoring** tab

    ![](https://i.imgur.com/OCbcfrI.png)

### T2 Unlimited

[Go Back to Contents](#contents)

- After November 2017, It's possible to have an **unlimited burst credit balance**
- You pay extra money if you go over your credit balance, but you don't lose performance
- Overall, it is a new offering, so be careful, costs could got high if you're not monitoring the health of your instances.
- [T2 Unlimited Docs](https://aws.amazon.com/ec2/pricing/on-demand/)
- [Unlimited mode for burstable performance instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/burstable-performance-instances-unlimited-mode.html?icmpid=docs_ec2_console)

## Network and Security

### Security Groups Overview

[Go Back to Contents](#contents)

- Security groups are acting as a `firewall` on EC2 instances
- They regulate:

  - Access to ports
  - Authorized of forbidden IP (ranges) - IPV4 and IPV6
  - Control of inbound network (from other to the instance)
  - Control of outbound network (from the instance to other)

- Security group:
  - Can be attached to multiple instances
  - Locked down to a region / VPC combination
  - Does live "outside" the EC2 - if traffic is blocked the EC2 instance won't see it
  - It's good to maintain one separate security group for SSH access
  - If your application is not accessible (**timeout**), then it's most likely to be a security group issue
  - If your application gives a **connection refused error**, then it's an application error or it's not launched.
  - All the inbound traffic is **blocked** by default
  - All outbound traffic is **authorized** by default

## IP and CIDR

[Go Back to Contents](#contents)

### Private vs Public IP (IPv4)

[Go Back to Contents](#contents)

- **Private IP**
  - Can only allow certain values from:
    - `10.0.0.0 ~ 10.255.255.255 (10.0.0.0/8)` <= in big networks
    - `172.16.0.0 ~ 172.31.255.255 (172.16.0.0/12)` <= default AWS one
    - `192.168.0.0 ~ 192.168.255.255 (192.168.0.0/16)` <= home networks
- **Public IP**

  - All the rest of the IP on the internet

- **Security Groups**
  - work the same way with public and private IP

### CIDR

[Go Back to Contents](#contents)

- It would be good to be able to express a range of IP:

  - `192.168.0.0 ~ 192.168.0.63 (64 IP)`

    ![](https://i.imgur.com/M1wBDnL.png)

- For this, we use CIDR
  - CIDR is the short for Classless Inter-Domain Routing, an IP addressing scheme that replaces the older system based on classes A, B, and C. A single IP address can be used to designate many unique IP addresses with CIDR. A CIDR IP address looks like a normal IP address except that it ends with a slash followed by a number, called the IP network prefix. CIDR addresses reduce the size of routing tables and make more IP addresses available within organizations.
  - [CIDR to IPv4 Conversion](https://www.ipaddressguide.com/cidr)

#### Understanding CIDR

[Go Back to Contents](#contents)

- A CIDR has two components
  - The base IP (XX.XX.XX.XX)
  - The subnet mask (/26)
- The base IP represents the first IP contained in the range
- The subnet masks defines how many bits can change in the IP
- The subnet mask can take two forms:
  - `255.255.255.0` - Less common
  - `/24` - more common
- The subnet masks basically allows part of the underlying IP to get additional next values from the base IP.

  ```Bash
    /32 # allows for 1 IP
    /31 # allows for 2 IP
    /30 # allows for 4 IP
    /29 # allows for 8 IP
    /28 # allows for 16 IP
    /27 # allows for 32 IP
    /26 # allows for 64 IP
    /25 # allows for 128 IP
    /24 # allows for 256 IP
    /16 # allows for 65.536 IP
    /0  # allows for all IP
  ```

- Quick memo:

  ```Bash
    /32 # no IP number can change
    /24 # last IP number can change
    /16 # last IP two numbers can change
    /8  # last IP three numbers can change
    /0  # all IP numbers can change
  ```

- Example:

  ```Bash
    192.168.0.0/24 = ?
      192.168.0.0 ~ 192.168.0.255 # 256 IP

    192.168.0.0/16 = ?
      192.168.0.0 ~ 192.168.255.255 # 65.536 IP

    134.56.78.123/32 = ?
      134.56.78.123 # 1 IP

    0.0.0.0/0
      # All IP
  ```

## Security Group

[Go Back to Contents](#contents)

- On **EC2 Dashboard**

  - On the sidebar menu `EC2 Dashboard > Network and Security > Security Groups`

    ![](https://i.imgur.com/iLGvzzl.png)

### Create a SSH Security Group

[Go Back to Contents](#contents)

- We are creating this security group to be use though the whole company and want to be super secure.
- Click on **Create security group**

  ![](https://i.imgur.com/GPdkhJP.png)

- On `Create security group` page

  - Security group name: `SSH Security Group`
  - Description: `Enterprise Wide SSH Security Group`
  - VPC: `vpc-7a4a7500` (the default)
  - Inbound rules

    - Add rule
    - Type: `SSH`
    - Destination `My IP` (in this case. But for a large company would be CIDR)
      - `99.246.120.159/32` (`99.246.120.159` is my IP, `/32` only for this IP)
    - Description - optional: `Roger Takeshita IP`

  - Then click on **Create security group**

    ![](https://i.imgur.com/o705Q2i.png)
    ![](https://i.imgur.com/b9QEA5m.png)

### Create a Java Server Security Group

[Go Back to Contents](#contents)

- We now need to create a security group for our Java Server
- Click on **Create security group**

  ![](https://i.imgur.com/GPdkhJP.png)

- On `Create security group` page

  - Security group name: `Java Server Security Group`
  - Description: `Rules to allow our java server to work`
  - VPC: `vpc-7a4a7500` (the default)
  - Inbound rules

    - Add rule
    - Type: `Custom TCP`
    - Port Range: `4567`
    - Destination `Anywhere`
      - `0.0.0.0/:0` (`0.0.0.0` any IP, `/:0` infinity numbers of ips)
      - The `::/0` is IPv6. For now we don't need the IPv6.
    - Description - optional: `Java Sever Anywhere`

  - Then click on **Create security group**

    ![](https://i.imgur.com/KRclZzx.png)
    ![](https://i.imgur.com/cRzlY12.png)

- Now we have two security groups

  ![](https://i.imgur.com/maPUSGg.png)

### Update Instance Security Group

[Go Back to Contents](#contents)

- On `EC2 Dashboard > Instances > Instances`

  - Right click on the instance that you want to change the security group

    - `Right Click > Networking > Change security groups`

      ![](https://i.imgur.com/LgUAJyA.png)

- On `Change Security groups` page

  - Remove the old security group (`my-first-instance`)

    ![](https://i.imgur.com/kcnfq4g.png)

  - Then add our two new security groups that we created

    - Click on **Save**

      ![](https://i.imgur.com/oHowgq2.png)

- On `EC2 Dashboard > Instances > Instances`

  - Go to **Security** tab
  - There we can see our two security group that we've just added
  - At the bottom of the page we can see our inbound rules

    ![](https://i.imgur.com/d9RCFtC.png)

- If we search for `3.235.8.190:4567`

  - We can see that everything still works

    ![](https://i.imgur.com/y9eC2l2.png)

  - By default all the **Outbound** network is open to anywhere. This means tha our instance can access the internet

    ![](https://i.imgur.com/JNBvMxt.png)

### Referencing Other Security Groups

[Go Back to Contents](#contents)

- From a security group we can reference other security groups
- It is possible to have a security group rules using other security groups instead of IP ranges (CIDR)
  - This is for enhanced security in AWS
  - The security group can even reference itself
- Use cases
  - EC2 to EC2 direct communication within security group
  - Public Load Balance - Private EC2 instance
  - Having rules more flexible than fixed IP ranges

#### Exercise

[Go Back to Contents](#contents)

- Objectives:

  - Practice EC2 security groups rules by referencing other security groups
  - See how wen can allow certain instances to talk to our application or not
  - We'll use 2x EC2 instances in this case

- For this exercise we are going to create a new security group
- On `Create security group` page

  - Security group name: `Security Group 2`
  - Description: `Just for the hands on `
  - VPC: `vpc-7a4a7500` (the default)
  - And we don't change anything else
  - Click on **Create security group**

- On `EC2 Dashboard > Instances > Instances`

  - On our new instance we change the security group to use:
    - `Security Group 2`
    - `SSH Security Group`

- On `Terminal` SSH into our second instance

  ![](https://i.imgur.com/6UJY0he.png)

  - Curl our first instance using the **public** IPv4 (`35.153.49.22:4567`)

    - As we can see everything works fine

      ![](https://i.imgur.com/8JZsAoy.png)

- On `EC2 Security Group` we are going to remove the **inbound rule** to allow all ips from our `Java Sever Security Group`

  ![](https://i.imgur.com/51tBiKh.png)
  ![](https://i.imgur.com/1S0yL9N.png)

- On `Terminal`

  - Curl our first instance using the **public** IPv4 (`35.153.49.22:4567`) again

    - It will timeout, because we blocked all internet access to the `Java Sever Security Group`

      ![](https://i.imgur.com/h3JHCNR.png)

- On `EC2 Security Group`, Add a new **inbound rule** to allow:

  - Type: `All TCP`
  - Protocol: `TCP`
  - Port range: `0-65535`
  - Source: `Custom`
    - `Security Group 2`
  - Description `Allow instances that have security group 2 to access java server`

    ![](https://i.imgur.com/1YYvhCD.png)
    ![](https://i.imgur.com/XPlCTor.png)
    ![](https://i.imgur.com/2ssoZX0.png)

- On `Terminal`

  - Curl our first instance using the **private** IPv4 (`172.31.9.140:4567`)

    - We can access to our first instance using the security group

      ![](https://i.imgur.com/qqNGlXu.png)

  - But if we try to curl using the **public** IPv4 (`35.153.49.22:4567`)

    - It will timeout because our java sever is only allows connections from `Security Group 2`

      ![](https://i.imgur.com/SWKa8Ai.png)

### Referencing Circular Security Groups

[Go Back to Contents](#contents)

- This is useful if we need to instantiate another instance using the `Java Sever Security Group` and we need that both instances talk to each other. Since they don't have `Security Group 2`.

  - We can also restrict the port range

    ![](https://i.imgur.com/yWM19Tr.png)
    ![](https://i.imgur.com/uwVDpK9.png)

## Elastic IPs

[Go Back to Contents](#contents)

- When we stop and then start an EC2 instance, it can change its public IP
- If we need to have **fixed public IP** for our instance, we need an Elastic IP
- An Elastic IP is a public IPv4 IP we won as long as we don't delete it
- With an Elastic IP address, we can mask the failure of an instance or software by rapidly remapping the address to another instance in our account.
- We can only have 5 Elastic IP in our account (we can ask AWS to add more)
- Overall, **try to avoid using Elastic IP**:
  - They often reflect poor architectural decisions
    - Instead, we can use a random public IP and register a DNS name to it
    - Or, we can use a **Load Balancer** and don't use a public IP

### Pricing

[Go Back to Contents](#contents)

- [AWS Elastic IP Pricing Page](https://aws.amazon.com/ec2/pricing/on-demand/#Elastic_IP_Addresses)
- We don't pay as long as the Elastic IP is attached to a **running** EC2 instance
- We start paying if the EC2 instance is stopped and the Elastic IP is enabled

### Exercise

[Go Back to Contents](#contents)

- Create an Elastic IP and attach it to an instance
- Detach it and attach it to another instance
- **Objective**: The application is still accessible from the same IP but it was using two different EC2 machines

  - For this exercise we need to use 2 instances using the following security groups:

    - `Java Sever Security Group`
      - Change the **inbound** rules:
        - Remove all previous rules
        - Enable all `All traffic` from any IP (`0.0.0.0/0`)
    - `SSH Security Group`

  - On `Browser` access our instances

    ![](https://i.imgur.com/kvV4AhO.png)

    - As we can see the `Receive Request From` comes from the same source `99.246.120.159`
    - But the IP of the machine that response to our query has different private IPs
      - `ip-172-31-30-56.ec2.internal`
      - `ip-172-31-9-140.ec2.internal`

  - Creating an **Elastic IP**

    - On `EC2 Dashboard > Network and Security > Elastic IPs`

      - Click on **Allocate Elastic IP address**
        ![](https://i.imgur.com/hCK1CGI.png)

    - On `Allocate Elastic IP address` page

      - Click on **Allocate**

        ![](https://i.imgur.com/ZKDsjE9.png)
        ![](https://i.imgur.com/J0205L5.png)

      - If we click on the ip that we just created `3.210.171.214` we can see its summary

        ![](https://i.imgur.com/7k4NEZ5.png)

  - Associate an Elastic IP to an Instance

    - On `EC2 Dashboard > Network and Security > Elastic IPs`

      - Associate this new elastic IP to an instance

        - Select the elastic IP
        - Click on `Actions > Associate Elastic IP address`

          ![](https://i.imgur.com/C6A0UCw.png)

    - Associate an Elastic IP to an Instance An Elastic IP was created: `3.210.171.214`

      - Instance: `i-02adc545b62fc892e`
        - Choose an Instance - In this case we just choose the first one
      - Private IP Address: `172.31.9.140`
      - Allow this Elastic IP address to be reassociated
      - Click on **Associate**

        ![](https://i.imgur.com/qqSuXxm.png)

  - Test our Elastic IP

    - On `Browser` access our instances (elastic and normal instance)

      - As we can see, we are using different IPs but accessing the same machine

        ![](https://i.imgur.com/imwKiw1.png)

## Placement Groups

[Go Back to Contents](#contents)

- Sometimes we want to control over the EC2 Instance placement strategy
  - Can be placed together or be placed very far apart from each other
- The strategy can be defined using placement groups
- When we create a placement group, we specify one of the following strategies for the groups
  - **Cluster** - Clusters instances into a low-latency group in a single Availability Zone
    - This means that our instances will be put together into a low-latency group
    - Pros:
      - Great network (10 Gbps bandwidth between instances) - low-latency
    - Cons:
      - If the rack fails, all instances fail at the same time
    - Use case:
      - Big Data job that needs to complete fast
      - Application that needs extremely low-latency and high network throughput
  - **Spread** - Spreads instances across underlying hardware (max 7 instances per group per AZ)
    - Pros:
      - Can span across Availability Zones (AZ)
      - Reduced risk of simultaneous failure
      - EC2 instances are on different physical hardware
    - Cons:
      - Limited to 7 instances per AZ per placement group
    - Use case:
      - Application that need to maximize high availability
      - Cassandra cluster, kafka cluster, Web application that is distributed
  - **ATTENTION: Not applicable to t2 instances**

### Creating a Placement Group

[Go Back to Contents](#contents)

- On `EC2 Dashboard > Network and Security > Placement Groups`

  - Click on **Create placement group**

    ![](https://i.imgur.com/Q2Zt26H.png)

- On `Create placement group` page

  - Name: `my-cluster-placement-group-for-apache-kafka`
  - Placement strategy: `Spread` / `Cluster`
  - Click on **Create group**

    ![](https://i.imgur.com/bsbTSzs.png)
    ![](https://i.imgur.com/TFnOnWw.png)

- Creating a new instance

  - Now when create a new instance (anything different from a T2 instance)
  - We will have a new option **Placement group**

    ![](https://i.imgur.com/Qcsx7kN.png)

## Load Balancing

[Go Back to Contents](#contents)

- Load balancers are servers that forward internet traffic to multiple servers (EC2 Instances) downstream.
- [AWS Load Balancer Prices](https://aws.amazon.com/elasticloadbalancing/pricing/)
- [Application Load Balancer](https://aws.amazon.com/blogs/aws/new-aws-application-load-balancer/)
- [Network Load Balancer](https://aws.amazon.com/blogs/aws/new-network-load-balancer-effortless-scaling-to-millions-of-requests-per-second/)
- [Application Vs Network Load Balancer](https://medium.com/containers-on-aws/using-aws-application-load-balancer-and-network-load-balancer-with-ec2-container-service-d0cb0b1d5ae5)
- [Application Vs Classic Load Balancer](https://cloudacademy.com/blog/application-load-balancer-vs-classic-load-balancer/)

### Why Use a Load Balancing

[Go Back to Contents](#contents)

- Spread load across multiple downstream instances
- Expose a single point of access (DNS) to your application
- Seamlessly handle failures of downstream instances
- Do regulars heath checks to your instances
- Provide SSL termination (HTTPS) for your websites
- Enforce stickiness with cookies
- High availability across zones
- Separate public traffic from private traffic

### Why Use an EC2 Load Balance?

[Go Back to Contents](#contents)

- An ELB (EC2 Load Balance) is a **managed load balancer**
  - AWS guarantees that it will be working
  - AWS takes care of upgrades, maintenance, high availability
  - AWS provides only a few configuration knobs
- It costs less to setup our own load balancer but it will be a lot more effort on our end
- It's integrated with many AWS services

### Types of Load Balancer on AWS

[Go Back to Contents](#contents)

- AWS has **3 kinds of Load Balancer**
  - Classic Load Balancer (v1 - old generation) - 2009
  - Application Load Balancer (v2 - new generation) - 2016
  - Network Load Balancer (v2 - new generation) - 2017
  - **Overall** it's recommended to use the newer / v2 generation load balancers as they provide more features
- We can setup **internal** (private) or **external** (public) ELBs

### Create Classic Load Balancer (v1)

[Go Back to Contents](#contents)

- This will cost some money
- We are going to create a classic load balancer, because many AWS users are still using v1

  ```Bash
    #                      .--------------------.                .-----------.
    #           HTTP       |  External Classic  |      HTTP      |    EC2    |
    #   WWW <------------> |   Load Balancer    | <------------> | Instances |
    #         Port 80      |       (v1)         |    Port 4567   |           |
    #                      *--------------------*                *-----------*
  ```

- On `EC2 Dashboard > Load Balancing > Load Balancers`

  - Create a Classic Load Balance

    - Click on **Create Load Balancer**

      ![](https://i.imgur.com/gcoWVBv.png)
      ![](https://i.imgur.com/HSOHFmt.png)

    1. Step 1: Define Load Balancer

       - Load Balancer name: `classic-load-balancer`
       - Create LB Inside: `My Default VPC`
       - Create an internal load balancer: `unchecked` (because we want a public IP)
       - For now we are only going to configure `HTTP`

         - Then we have to change the **Instance Port** to **4567**
         - This means that the load balancer will listen for HTTP request to port **80** and it will forward to our instance on port **4567**

       - Click on **Next: Assign Security Groups**

         ![](https://i.imgur.com/tA54KZq.png)

    2. Step 2: Assign Security Group

       - We could create use an existing security group, but for now we are going to create a new security group
         - Assign a security group: `Create a new security group`
         - Security group name: `classic-load-balance-security-group`
         - Description: `Security group for our classic load balancer`
       - Then we need to allow `HTTP` on port `80` from any source (`0.0.0.0/0`)
       - Click on **Next: Configure Security Settings**

         ![](https://i.imgur.com/EY2TlHy.png)

    3. Step 3: Configure Security Settings

       - We are getting a warning because we are not using HTTPS at the moment
       - Click on **Next: Configure Health Check**

         ![](https://i.imgur.com/msY02X1.png)

    4. Step 4: Configure Health Check

       - Ping Protocol: `HTTP`
       - Ping Port: `4567`
       - Ping Path: `/`
       - Healthy threshold: `5`

         - Number of consecutive health check successes before declaring an EC2 instance healthy.

       - Click on **Next: Add EC2 Instances**

         ![](https://i.imgur.com/5rwPeRk.png)

    5. Step 5: Add EC2 Instances

       - Here is where the magic happens. Now we are going to select the instances that we want to add to this load balancer
       - Select the `Instance 1` and `Instance 2`
       - Click on **Next: Add Tags**

         ![](https://i.imgur.com/HKdO9Jb.png)

    6. Step 6: Add Tags

       - Since we are not using tags for now
       - Click on **Review and Create**

    7. Step 7: Review

       - Click on **Create**

         ![](https://i.imgur.com/tBFXg48.png)
         ![](https://i.imgur.com/Jy6FT3A.png)

  - Accessing our load balancer

    ![](https://i.imgur.com/Ac3inNs.png)

    - After creating the load balance, we can access our load balancer using the public DNS (`classic-load-balancer-1627616461.us-east-1.elb.amazonaws.com`)
    - On the **Instances** tab

      - We can check the status of our instances

        ![](https://i.imgur.com/zDC4JdU.png)

  - On the `Browser`

    - We can access our Java application through `classic-load-balancer-1627616461.us-east-1.elb.amazonaws.com`
    - And if we refresh the page, we can see that our private IP is changing

      ![](https://i.imgur.com/TDbJrji.png)
      ![](https://i.imgur.com/dmyj2qS.png)

    - Load balancer adds 3 more fields to our headers

      ```Bash
        X-Forwarded-For: 99.246.120.159
        X-Forwarded-Port: 80
        X-Forwarded-Proto: http
      ```

      ![](https://i.imgur.com/QLt3lyN.png)

### Securing Loader Balance

[Go Back to Contents](#contents)

- After configuring the loader balancer
- To make more secure our application we can add our loader balancer to the instance security group
- On `EC2 Dashboard > Network and Security > Security Groups`

  - Edit the `Java Server Security Group`

    - Remove all rules
    - Add:

      - Types: `Custom TCP`
      - Port range: `4567`
      - Source:
        - `Custom`
        - `classic-loader-balancer-security-group`
      - Description: `Classic load balancer`

        ![](https://i.imgur.com/xr1zuQT.png)
        ![](https://i.imgur.com/xr1zuQT.png)

- After updating the security group
  - We don't have direct access to our instances (`52.55.186.157:4567`). We only access through our load balancer dns

### Health Checks

[Go Back to Contents](#contents)

- Health Checks are crucial for Load Balancers
- They enable the load balancer to know if instances it forwards traffic to are available to replay to requests
- The health check is done on a port and a route (`/health` is common)
- If the response is not 200, then the instance is unhealthy

#### Checking If It's Health

[Go Back to Contents](#contents)

- We can check if our application is health by using the link `http://classic-load-balancer-1627616461.us-east-1.elb.amazonaws.com/health`
- To check that we need to update the our url path to point to `/health`
- On `EC2 Dashboard > Load Balancing > Load Balancers > classic-load-balancer > Health Check`

  - Click on **Edit Health Check**

    ![](https://i.imgur.com/WU0pf0T.png)

- On `Configure Health Check` Pop-up

  - Update the **Ping Path** to `/health`
  - Update the **Interval** to `10 seconds`
  - Click on **Save**

    ![](https://i.imgur.com/sCxOCCq.png)

### Application Load Balancer (v2)

[Go Back to Contents](#contents)

- Application load balancer (Layer 7) allow to do:
  - Load balancing to multiple HTTP applications across machines (target groups)
  - Load balancing to multiple applications on the same machine (ex. containers)
  - Load balancing based on route in URL
  - Load balancing base on hostname in URL
- Basically, they're awesome for micro services and container-based application (ex. Docker)
- In comparison, we would need to create one **classic load balancer** per application before. That's very expensive and inefficient.

### Create a Target Group

[Go Back to Contents](#contents)

- The first thing that we need to do is to create a target group
- On `EC2 Dashboard > Load Balancing > Target Groups`

  - Click on **Create target group**

    ![](https://i.imgur.com/vy3AzMy.png)

- On `Specify group details`

  - Basic configuration
    - Choose a target type: `Instances`
    - Target group name: `java-application-target-group`
    - Protocol: `HTTP` (how we access our instance)
    - Port: `4567`
    - VPC: `Default`
  - Health checks

    - Protocol: `HTTP`
    - Health check path: `/health`
    - Advance health check settings:
      - Port: `Traffic port` (but we could overwrite teh port)
      - Health threshold: `5`
      - Unhealthy threshold: `2`
      - Timeout: `5` seconds
      - Interval: `10` seconds
      - Success codes: `200`

  - Click on **Next**

    ![](https://i.imgur.com/lrqDbM8.png)
    ![](https://i.imgur.com/DQBMv0i.png)
    ![](https://i.imgur.com/1QQCzpO.png)

  - On `Register targets`

    - Click on **Create target group**

      ![](https://i.imgur.com/pWdz2ZE.png)
      ![](https://i.imgur.com/rxt3wbB.png)

  - Click on the new target group (`java-application-target-group`) to view the configuration

    - As we can see, we didn't assign a load balancer to it

      ![](https://i.imgur.com/05GqlbS.png)

    - And also, we didn't associate targets to it

      ![](https://i.imgur.com/H1TlWfU.png)

#### Register Targets to Target Groups

[Go Back to Contents](#contents)

- On **Targets** tab

  - Click on **Register targerts**

    ![](https://i.imgur.com/H1TlWfU.png)

- On `Register targets` page

  - Select both instances
  - Set the port to: `4567`
  - Click on **Include as pending bellow**
  - Click on **Register pending targets**

    ![](https://i.imgur.com/Rx0Rnhr.png)

  - Now we have 2 **unused targets**

    ![](https://i.imgur.com/5l5hRl8.png)

### Create an Application Load Balancer (v2)

[Go Back to Contents](#contents)

- On `EC2 Dashboard > Load Balancing > Load Balancers`

  - Create an Application Load Balancer

    - Click on **Create Load Balancer**

      ![](https://i.imgur.com/nS7EEhu.png)

  - On `Select load balancer type` page:

    - Click on **Create** `Application Load Balancer`

      ![](https://i.imgur.com/HSOHFmt.png)

    1. Step 1: Configure Load Balancer

       - name: `application-load-balancer`
       - Scheme: `internet-facing`
       - Ip address type: `ipv4`
       - Listener:
         - Load Balancer Protocol: `HTTP`
         - Load Balancer Port: `80`
       - Availability Zones
         - Select all zones
       - Click on **Next: Configure Security Settings**

         ![](https://i.imgur.com/xzg3Q1y.png)

    2. Step 2: Configure Security Settings

       - Just like before, we have a warning saying to configure HTTPS
       - Click on **Next: Configure Security Groups**

         ![](https://i.imgur.com/msY02X1.png)

    3. Step 3: Configure Security Groups

       - Assign a security group: `Create a new security group`
       - Security group name: `application-load-balancer-security-group`
       - Description: `Application security group for our load balancer`
       - Rule:
         - Type: `HTTP`
         - Port: `80`
         - Source:
           - `Custom`
           - `0.0.0.0/0`
       - Click on **Next: Configure Routing**

         ![](https://i.imgur.com/ZUxQgHg.png)

    4. Step 4: Configure Routing

       - Target group: `Existing target group`
       - Name: `java-application-target-group`
       - Click on **Next: Register Targets**

         ![](https://i.imgur.com/z0JF1SD.png)

    5. Step 5: Register Targets

       - Click on **Next: Review**

         ![](https://i.imgur.com/nC5R3MW.png)

    6. Step 6: Review

       - Click on **Create**

         ![](https://i.imgur.com/QLrvvbZ.png)
         ![](https://i.imgur.com/CHktcxK.png)

  - Update our `Java Sever Security Group`

    - Add the `application-load-balancer-security-group`

      ![](https://i.imgur.com/dixzyMQ.png)
      ![](https://i.imgur.com/uAO70Q4.png)

  - Accessing our load balancer

    - After creating the load balance, we can access our load balancer using the public DNS (`application-load-balancer-61068661.us-east-1.elb.amazonaws.com`)

      ![](https://i.imgur.com/GIvoa3w.png)

  - On the `Browser`

    - We can access our Java application through `application-load-balancer-61068661.us-east-1.elb.amazonaws.com`
    - And if we refresh the page, we can see that our private IP is changing

      ![](https://i.imgur.com/iZSFTVc.png)
      ![](https://i.imgur.com/HsyxAx7.png)

    - Load balancer adds 4 more fields to our headers

      ```Bash
        X-Amzn-Trace-Id: Root=1-5f88bfdf-05bd5a275de39f277e2f26e4
        X-Forwarded-For: 99.246.120.159
        X-Forwarded-Port: 80
        X-Forwarded-Proto: http
      ```

      ![](https://i.imgur.com/2e3S1a1.png)

  - On `EC2 Dashboard > Load Balancing > Target Groups`

    - If we check our `java-application-target-group`

      - On **Targets** tab

        - We can see that now we have 2 target with status **healthy**

          ![](https://i.imgur.com/KCuObEi.png)

### Load Balancer Listeners

[Go Back to Contents](#contents)

- On `EC2 Dashboard > Load Balancing > Load Balancers`

  - On **Listeners** tab

    ![](https://i.imgur.com/Cs0bOf6.png)

    - We can setup some rules
    - The first rule is:

      - If Requests otherwise not routed
      - Then forward to: java-application-target-group

        ![](https://i.imgur.com/n6zFgYd.png)

    - We could add any kind of rule, and forward to different applications (micro-services)

      ![](https://i.imgur.com/gz0v0Yp.png)

### Network Load Balancer (v2)

[Go Back to Contents](#contents)

- Network load balancers are similar to application load balancers
- Network load balancers (Layer 4) allow to do:
  - Operates in a lower lvl (TCP lvl)
  - Forward **TCP** traffic to your instances
  - Handle millions of requests per seconds
  - Support for static IP or elastic IP
  - Less latency ~100ms (vs 400ms for ALB)
- Network Load Balancers are mostly used for **extreme performance** and should not be the default load balancer
- Overall, the creation process is the same as Application Load Balancers
