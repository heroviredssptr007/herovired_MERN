# herovired_MERN

## **Step 1: Creation of Instances**


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


## **Step 2: Configuring Backend service**

**1. Installation of latest nodejs package**
```
   curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
   sudo apt install -y nodejs
```

**2.Clone this repository(https://github.com/UnpredictablePrashant/TravelMemory.git) to the Backend server**
```
git clone https://github.com/UnpredictablePrashant/TravelMemory.git
```
![image](https://github.com/user-attachments/assets/c475e229-3ca6-4777-94e2-dc8ebe9e9878)

**3.Get into the ./TravelMemory/backend/ and create .env file, add below values into the .env file**
```
MONGO_URI='mongodb+srv://ssptr007:Srikrishna123@cluster0.38nvplb.mongodb.net/travelmemory'
PORT=3001
```
![image](https://github.com/user-attachments/assets/f8bfb644-bd9c-4f50-8740-2b3def21c0f2)

**4.Run below commands**
```
#npm install
#node index.js
```
![image](https://github.com/user-attachments/assets/6c57de43-cddc-4801-9f1b-2d213568d667)

![image](https://github.com/user-attachments/assets/29c9ce69-5563-48d6-be55-1fec30a1c71a)


**5.Make as startup application**
```
[Unit]
Description=Backend application
After=network.target

[Service]
WorkingDirectory=/home/ubuntu/TravelMemory/backend/
ExecStart=/usr/bin/node index.js
Restart=always
Environment=NODE_ENV=production
User=ubuntu
# Optional: capture logs
StandardOutput=journal
StandardError=journal

[Install]
WantedBy=multi-user.target
```
**6. With the help of nginx make reverse proxy to map 3001 port to 80 port**
```
$sudo apt install -y nginx
```
```
#cat > /etc/nginx/sites-enabled/default
```

Add below line to file

```
server {
    listen 80;
    server_name sandykrishna4u.info;  # Or use _ for default server

    location / {
        proxy_pass http://localhost:3001;  # or http://127.0.0.1:3001
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```
```
#nginx -t --> Verify the configuration
```
```
#sudo systemctl daemon-reexec
#sudo systemctl daemon-reload
#sudo systemctl enable travelmemory_frontend.service
sudo systemctl restart travelmemory_frontend.service
#sudo systemctl status nginx
```
![image](https://github.com/user-attachments/assets/319afdcc-6e7d-4b4a-9dfa-e663915f23cc)

Now we can able to access application in 80 port
![image](https://github.com/user-attachments/assets/46888674-ec60-40d5-a3c5-e5a1d7594b86)

**7. Creation of AMI**
 - Stop the instance
 - Select the Instance Click on Actions -> Select **Image and templates** -> Select Create Image.
 - Enter Image name and click on Create Image.
   
![image](https://github.com/user-attachments/assets/346993d4-0728-401c-8841-dfd14d312baf)

**8. Create another ec2 instance with that AMI**

1. In the AWS Management Console, search for `EC2` in the search bar and click on **EC2**.
2. Click on **Launch Instance**.
3. Configure the instance with the following settings:
   - **Number of instances**: `1`
   - **AMI**: `Backend-Image which will be there My AMI`
   - **Instance Type**: `t3.micro`
4. **Key Pair**:
   - select an existing one which we have created before.
5. **Network Settings**:
   - Select the appropriate **VPC**.
   - Enable **Auto-assign Public IP**.
   -select a **Security Group** which we created before
     
6. **Storage**:
   - Set the storage size to `8 GB`.

**9. Creation of Backend load balancer**
   - **Creation of Target Groups**
     - Click on **Create Target Group**
       - In Basic Configuration-> Choose a target type as **Instances**
       - Provide **Target group name**-> No need to change any settings, Click on next
     - In **Register targets**
       - select the instance
       - click on **Include as Pending below** 
       - Click **Create Target Group**
   
   ![image](https://github.com/user-attachments/assets/dbbfa534-67fc-4d53-91a0-99ef6ad73161)

   - **Creation of Load Balancer**
     - Click on Create Load balancer
     - Select Application Load balancer
     - Provide the Load balancer name
     - In scheme section select internet facing
     - In Network mapping section select vpc and Select appropriate Availability zone.
     - In Security Groups, select appropriate security group.
     - In Listener and routing section, Select the target in default action and click on **Create load balancer**.

![image](https://github.com/user-attachments/assets/a7d09141-ba08-44fa-a0f5-99047528fbb5)

**10. Test your connection backend load balancer with help of postman**

![image](https://github.com/user-attachments/assets/846a7975-4d57-4d4d-b9f2-42e2cf18d7f2)
![image](https://github.com/user-attachments/assets/ab009ce9-ff8b-4b9e-bd65-fde0f44f8226)

## **Step 3: Configuring Frontend service**

**1. Installation of latest nodejs package**
```
   curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
   sudo apt install -y nodejs
```

**2.Clone this repository(https://github.com/UnpredictablePrashant/TravelMemory.git) to the Backend server**
```
git clone https://github.com/UnpredictablePrashant/TravelMemory.git
```
![image](https://github.com/user-attachments/assets/c475e229-3ca6-4777-94e2-dc8ebe9e9878)

**3.Get into the ./TravelMemory/frontend/ and create .env file, add below values into the .env file**
```
REACT_APP_BACKEND_URL=http://backend-ALB-1559357213.eu-north-1.elb.amazonaws.com
```
![image](https://github.com/user-attachments/assets/f8bfb644-bd9c-4f50-8740-2b3def21c0f2)

**4.Run below commands**
```
#npm install
#npm start
```
![image](https://github.com/user-attachments/assets/98c57f83-950c-439d-bdf6-7bc97ffc0cf0)

**5.Make as startup application**
```
[Unit]
Description=Backend application
After=network.target

[Service]
WorkingDirectory=/home/ubuntu/TravelMemory/frontend/
ExecStart=/usr/bin/npm start
Restart=always
Environment=NODE_ENV=production
User=ubuntu
# Optional: capture logs
StandardOutput=journal
StandardError=journal

[Install]
WantedBy=multi-user.target
```
**6. With the help of nginx make reverse proxy to map 3001 port to 80 port**
```
$sudo apt install -y nginx
```
```
#cat > /etc/nginx/sites-enabled/default
```

Add below line to file

```
server {
    listen 80;
    server_name sandykrishna4u.info;  # Or use _ for default server

    location / {
        proxy_pass http://localhost:3000;  # or http://127.0.0.1:3000
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```
```
#nginx -t --> Verify the configuration
```
```
#sudo systemctl daemon-reexec
#sudo systemctl daemon-reload
#sudo systemctl enable travelmemory_frontend.service
sudo systemctl restart travelmemory_frontend.service
#sudo systemctl status nginx
```
![image](https://github.com/user-attachments/assets/06840d55-7c0d-4f1b-93ee-ad8cfca22ad4)

Now we can able to access application in 80 port
![image](https://github.com/user-attachments/assets/e40e9f32-36a9-4f59-87b4-0e3f30426ed6)

**7. Creation of AMI**
 - Stop the instance
 - Select the Instance Click on Actions -> Select **Image and templates** -> Select Create Image.
 - Enter Image name and click on Create Image.
   
![image](https://github.com/user-attachments/assets/edacee0e-d889-4071-ae34-3a3b4dacd114)



**8. Create another ec2 instance with that AMI**

1. In the AWS Management Console, search for `EC2` in the search bar and click on **EC2**.
2. Click on **Launch Instance**.
3. Configure the instance with the following settings:
   - **Number of instances**: `1`
   - **AMI**: `Backend-Image which will be there My AMI`
   - **Instance Type**: `t3.micro`
4. **Key Pair**:
   - select an existing one which we have created before.
5. **Network Settings**:
   - Select the appropriate **VPC**.
   - Enable **Auto-assign Public IP**.
   -select a **Security Group** which we created before
     
6. **Storage**:
   - Set the storage size to `8 GB`.
     
![image](https://github.com/user-attachments/assets/be531599-3054-4b3e-84f8-74524fa42ecd)




**9. Creation of Frontend load balancer**
   - **Creation of Target Groups**
     - Click on **Create Target Group**
       - In Basic Configuration-> Choose a target type as **Instances**
       - Provide **Target group name**-> No need to change any settings, Click on next
     - In **Register targets**
       - select the instance
       - click on **Include as Pending below** 
       - Click **Create Target Group**
   
   ![image](https://github.com/user-attachments/assets/7bccf1a5-c173-4933-af33-1d6a2d47e62a)


   - **Creation of Load Balancer**
     - Click on Create Load balancer
     - Select Application Load balancer
     - Provide the Load balancer name
     - In scheme section select internet facing
     - In Network mapping section select vpc and Select appropriate Availability zone.
     - In Security Groups, select appropriate security group.
     - In Listener and routing section, Select the target in default action and click on **Create load balancer**.



![image](https://github.com/user-attachments/assets/fb3fb882-a8f8-43b4-8630-9c9bafa6e120)



















