# frp-deployment

## Here we will learn how to setup FRP using your pc as a client server(server A) and AWS EC2 instance as proxy server(server B).
---
## First we will setup proxy server
### Step 1.
- launch a EC2 instance in aws.
  - allow security port 7000, 8080, 22, 80.
  ![image](https://github.com/user-attachments/assets/fd02de38-c7ca-4a0f-951a-057c6dae5c15)

### Step 2.
- Open AWS web CLI of your EC2 instance.
- Download the latest program for your operating system and architecture from the [Release](https://github.com/fatedier/frp/releases) page using "wget" command.
  - For example :
  ```
  wget https://github.com/fatedier/frp/releases/download/v0.61.2/frp_0.61.2_linux_amd64.tar.gz
  ```
- Extract the file using 'tar'.
  - For example :
  ```
  tar -xvzf frp_0.61.2_linux_amd64.tar.gz
  ```
- get inside the directory using 'cd'.
  - For example :
  ```
  cd frp_0.61.2_linux_amd64
  ```
      
### Step 3 : start proxy server
- open your frps.toml
  ```
  nano frps.toml
  ```
- configure your frps.toml
  ```
  bindPort = 7000
  auth.token = "my-secret-key"
  ```
- Start your proxy server
  ```
  ./frps -c frps.toml
  ```

### After all this you will get this 
![Screenshot from 2025-03-15 18-01-30](https://github.com/user-attachments/assets/19b57fa0-7750-41ed-baa1-8c65934f8143)

---

## now we will setup client server 

### Step 1.
- Open CLI of your pc.
- Download the latest program for your operating system and architecture from the [Release](https://github.com/fatedier/frp/releases) page using "wget" command.
  - For example :
  ```
  wget https://github.com/fatedier/frp/releases/download/v0.61.2/frp_0.61.2_linux_amd64.tar.gz
  ```
- Extract the file using 'tar'.
  - For example :
  ```
  tar -xvzf frp_0.61.2_linux_amd64.tar.gz
  ```
- get inside the directory using 'cd'.
  - For example :
  ```
  cd frp_0.61.2_linux_amd64
  ```
- modify frpc.toml.
  - ``` nano frpc.toml ``` 
  - ```
    serverAddr = "x.x.x.x"
    serverPort = 7000
    auth.token = "my-secret-key"
    loginFailExit = false

    [[proxies]]
    name = "websitetest"
    type = "tcp"
    localPort = 8080
    remotePort = 8080
    ```
   - replace serverAddr with your AWS EC2 instance's public ip.
     ![Screenshot from 2025-03-17 10-05-40](https://github.com/user-attachments/assets/37f1617f-82b5-4f90-81eb-328539eba5d3)

- run frpc
  - ```
    ./frpc -c ./frpc.toml
    ```
    ![Screenshot from 2025-03-17 10-07-15](https://github.com/user-attachments/assets/369e4eb6-130b-421d-9b62-81325390e3e8)


### To access you client from any machine  ping x.x.x.x:8080

- where x.x.x.x is the public ip of your proxy server. here we pinged 13.60.238..64:8080 and got output
  
