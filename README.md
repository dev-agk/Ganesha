# Ganesha
trying luck with jenkins web hook
-> changed branch name in jenkins
-> triggered manual build the first time to test for webhook on subsequent builds
-> replacing bat with sh. -> as I am deploying to an ubuntu server using a shell, if deploying toa  windows server , can use bat
-> checking jenkins directory
-> adding timeout


Install dotnet sdk in jenkins server
Install dotnet runtime in deployment server

Here both are same.

-> for service creation and visudo refer https://faun.pub/net-core-projects-ci-cd-with-jenkins-ubuntu-and-nginx-642aa9d272c9


// ->  Jenkins

1. create a pipeline job with github web hook configured
2. add MsBuild plugin 
3. configure jenkins with ```Pipeline from SCM``` andp ick from Jenkins file
4. add git repo and creds to jenkins.

// nginx -> reverse proxy config

```
server {
    listen        80;
    server_name   example.com *.example.com;

    # This location block fixed my issue.
    location ~* /(css|js|lib) {
        root /var/lib/jenkins/workspace/Ganesha/Ganesha/Ganesha/wwwroot/; 
        # this is to be configured right for static files to be served properly
    }

    location / {
        proxy_pass         http://127.0.0.1:5000;
        proxy_http_version 1.1;
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection keep-alive;
        proxy_set_header   Host $host;
        proxy_cache_bypass $http_upgrade;
        proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Proto $scheme;
    }
}
```

// How to configure and enable a service
1. create service file and paste the below
sudo vi /etc/systemd/system/ganesha.service

```
// create a service for .net app to run in the background
[Unit]
Description=Example .NET Web API App running on Ubuntu
[Service]
WorkingDirectory=/var/lib/jenkins/workspace/Ganesha/Ganesha/
ExecStart=/usr/bin/dotnet  /var/lib/jenkins/workspace/Ganesha/Ganesha/Ganesha/bin/Release/netcoreapp3.1/Ganesha.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
KillSignal=SIGINT
SyslogIdentifier=dotnet-ganesha
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false
[Install]
WantedBy=multi-user.target
```

2. register and eable service

```
sudo systemctl enable ganesha.service
sudo systemctl start ganesha.service
sudo systemctl start ganesha.service

```

if jenkins asks for tty prompt, enable jenkins user with visudo group ->

1. On Ubuntu-based systems, run sudo visudo
2. It will open /etc/sudoers file.
3. If your Jenkins user is already in that file, then modify to look like this: jenkins ALL=(ALL) NOPASSWD: ALL
4. save the file by doing Ctrl+O (don't save in tmp file. save in /etc/sudoers, confirm overwrite)
5. Exit by doing Ctrl+X
6. Relaunch your ganesha service
7. You shouldn't see that error message again :)