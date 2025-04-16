![image](https://github.com/user-attachments/assets/1ac565af-05bd-4d68-b6d1-518e892b9b83)# herovired_MERN

**Step 1: Creation of Instances**


Follow the steps below to create EC2 instances for your application:

1. In the AWS Management Console, search for `EC2` in the search bar and click on **EC2**.
2. Click on **Launch Instance**.
3. Configure the instance with the following settings:
   - **Number of instances**: `2`
   - **AMI**: `Ubuntu 24.04 LTS`
   - **Instance Type**: `t3.micro`
4. **Key Pair**:
   - Create a new key pair or select an existing one.
5. **Network Settings**:
   - Select the appropriate **VPC**.
   - Enable **Auto-assign Public IP**.
   - Create or select a **Security Group** with the following inbound rules:
     - Port `22` (SSH)
     - Port `80` (HTTP)
     - Port `443` (HTTPS)
     - Port `3000` (Custom App)
     - Port `3001` (Custom App)
6. **Storage**:
   - Set the storage size to `8 GB`.

Once all settings are configured, click **Launch Instance** to start the EC2 instances.


![image](https://github.com/user-attachments/assets/e649618a-3ba9-48b8-85f4-3acf1b41e174)


**Step 2: Configuring Backend service**
1. Clone this repository(https://github.com/UnpredictablePrashant/TravelMemory.git) to the Backend server
```
git clone https://github.com/UnpredictablePrashant/TravelMemory.git
```
