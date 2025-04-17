![image](https://github.com/user-attachments/assets/1ac565af-05bd-4d68-b6d1-518e892b9b83)# herovired_MERN

H1**Step 1: Creation of Instances**H1


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
    server_name yourdomain.com;  # Or use _ for default server

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
#sudo systemctl restart nginx
```
Now we can able to access application in 80 port
![image](https://github.com/user-attachments/assets/46888674-ec60-40d5-a3c5-e5a1d7594b86)

**7. Creation of AMI**
 - Stop 







