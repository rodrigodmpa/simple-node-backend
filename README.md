# simple-node-backend
Simple nodejs backend with auth

## Como fazer deploy para Digital Ocean (DO)

- Cria um droplet na DO com SSH
- Entre com SSH `ssh root@[ip]`
- Faça: `sudo vim /etc/ssh/sshd_config`
- coloque:
```
ClientAliveInterval 30
TCPKeepAlive yes
ClientAliveCountMax 99999
```
- `service sshd restart`
- Saia do ssh e entre novamente
- Faça um `apt update`
- Faça um `apt upgrade`
- Crie um usuário novo na máquina:
- `adduser deploy`
- `usermod -aG sudo deploy`
- `cd /home/deploy`
- `mkdir .ssh`
- `cd .ssh`
- `cp ~/.ssh/authorized_keys .`
- `chown deploy:deploy authorized_keys`
- Saia do ssh com exit
- Entre novamente com `ssh deploy@[ip]`

- Instalar o node via package manager
- [Link para instalar](https://github.com/nodesource/distributions/blob/master/README.md)
- Clone o repositorio para a raiz do droplet o usuario deploy
- Entre no repo
- Faça um `npm install`
- `cp .env.example .env`
- Edite o arquivo com `vim .env` e coloque as configs necessárias
- Depois, faça `npm run build`
- Faça um `sudo ufw allow 3333`
- Migration: `npx sequelize db:migrate`
- `npm run start`

- `sudo apt install nginx`
- `sudo ufw allow 80`
- `sudo vim /etc/nginx/sites-available/default`
- coloque
```
server {
        listen 80 default_server;
        listen [::]:80 default_server;

        server_name _;

        location / {
                proxy_pass http://localhost:3333;
                proxy_http_version 1.1;
                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection 'upgrade';
                proxy_set_header Host $host;
                proxy_cache_bypass $http_upgrade;
        }

}
```
- `sudo service nginx restart`
- `sudo nginx -t`


## PM2

`sudo npm install -g pm2`
`pm2 start dist/server.js`
`pm2 startup systemd`
- Em seguida, adicione a linha que o pm2 pede!


## Buddy

## Sentry

## SSL

`sudo apt install certbot python3-certbot-nginx`
`sudo vim /etc/nginx/sites-available/default`
- Coloque o dominio no server name
- `sudo nginx -t`
- `sudo systemctl reload nginx`
- 
```
sudo ufw allow 'Nginx Full'
sudo ufw delete allow 'Nginx HTTP'
```

- Coloque com seu dominio! -> `sudo certbot --nginx -d example.com -d www.example.com`
- `sudo systemctl status certbot.timer`
- `sudo certbot renew --dry-run`
