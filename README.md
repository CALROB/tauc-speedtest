# TAUC Speedtest Server

This repo aims to allow the easy creation of a TR-143 speed test server supported by the TP-Link TAUC platform. You can use all or parts of this repo to help configure and set up your own speed test server.

## Dependancies

- Docker and Docker-Compose
- Git

## Installation

1. Install Docker - [Official Installation Guide](https://docs.docker.com/engine/install)
2. Install Git - [Official Installation Guide](https://github.com/git-guides/install-git)
3. Clone the repo `git clone https://github.com/roadrunnerspeed/tauc-speedtest.git`
4. Adjust the `site-config/default.conf` to work for you

> [!TIP]
> It's recommended to change the `server_name` to represent your own domain eg. tpspeedtest.mydomain.com
>
> `client_max_body_size` has been set to 200MB; adjust this if you'd like a larger upload size.

5. Run Docker Compose `docker compose up -d`

> [!NOTE]
> This will start nginx on port 80. If you are already using this port on your server, you will need to rebind by adjusting the `docker-compose.yml` file and changing the ports to something like:
> ```
>     ports:
>      - '8080:80'
> ```

6. Assuming you have configured your domain name and DNS correctly to point to the IP of this server, you should be able to access the downloads via: `http://<domain>/downloads/20MB.file`
7. [OPTIONAL] Create additional download files. This repo comes with a default 20MB file, however you will likely want to create a larger download file for better testing. To do this, just place any additional files within the `download-files` directory. Or alternatively, create a 200MB file by running `dd if=/dev/zero of=200MB.file bs=1024 count=200KB` 
`

## Configuring TAUC

The next step is to configure TAUC to use the correct urls.

1. Login to TAUC
2. Click the bottom left cog to go to **System Settings**
3. Select the **Profile Management** tab
4. If you don't already have a speedtest server, click the **Add** button.
5. There are several boxes you will need to fill out:
  - **Profile Name**: This can be anything you want and is just the name you'd like for your speedtest
    
  - **Upload Speed Test Server**: This will be the domain you setup earlier in the format `http://<domain>/uploads/upload.file` - please note, it's **IMPORTANT** you keep the filename at the end `upload.file`, otherwise it will fail.
    
  - **Advanced > Test Options > Test File Size**: Set this to the file size you want to test with. If you didn't adjust the `default.conf > client_max_body_size` then this must be equal or less than 200MB.
   
  - **Download Speed Test Server**: `http://<domain>/downloads/20MB.file` - please note you can adjust the filename to any additional sizes you added in step 7 of the installation eg. `200MB.file`
    
  - **Internet Ping Speed Test Server**: This can be the server IP to get a latency score to your speedtest server or you can use a public service such as 1.1.1.1 or 8.8.8.8.

![Screenshot example of TAUC Config.](https://cdn.duplia.xyz/Screenshot%202024-01-03%20at%2014.30.09.png)

> [!CAUTION]
> Whilst there are limits on what methods can be used to access the server it is very much a basic setup and it is STRONGLY recommended that this isn't publically accessible and/or is limited to only trusted IP sources.
