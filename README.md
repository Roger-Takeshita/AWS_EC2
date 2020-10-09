<h1 id='contents'>Table of Contents</h1>

- [AWS EC2](#aws-ec2)
  - [Links](#links)
  - [What is EC2?](#what-is-ec2)
  - [Amazon Update - Linux 1 vs Linux 2](#amazon-update---linux-1-vs-linux-2)
  - [Amazon EC2 Basics](#amazon-ec2-basics)
    - [Create a New Instance](#create-a-new-instance)
    - [Stop an Instance](#stop-an-instance)

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
       - **ATTENTION** never share the Key Pair

       * Since we don't have a key pair, we are going to create one
         - In my case I named as `EC2-Roger-Takeshita`
         - After that, **Download Key Pair**
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
