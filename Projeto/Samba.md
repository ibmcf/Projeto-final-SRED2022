# SAMBA

Dando prosseguimento ao projeto, agora configuraremos um servidor compartilhamento de arquivos usando o serviço Samba no linux



1. Mude o nome da VM de acordo com as [definições de rede](https://github.com/ibmcf/Projeto-final-SRED2022/blob/main/Projeto/Rede.md). Passo a passo detalhado em [Configurações de ambiente](https://github.com/ibmcf/Projeto-final-SRED2022/blob/main/Projeto/Configuração-ambiente.md)

## Instalação do SAMBA
1.  Primeiro., verifique se há a necessidade de atualização de pacotes do linux com o comando ```sudo apt update```

![image](https://github.com/ibmcf/Projeto-final-SRED2022/blob/main/Imagens/SAMBA/update.png)

2. Para instalar o SAMBA, digite:

```bash
sudo apt install samba
```

![Image](https://github.com/ibmcf/Projeto-final-SRED2022/blob/main/Imagens/SAMBA/Instala%C3%A7%C3%A3o-samba.png)

3. Verifique se o servidor está funcionando:

```bash
whereis samba
```

![whereis samba](https://github.com/ibmcf/Projeto-final-SRED2022/blob/main/Imagens/SAMBA/whereis-samba.png)

```bash
 sudo systemctl status smbd
```
![status smbd](https://github.com/ibmcf/Projeto-final-SRED2022/blob/main/Imagens/SAMBA/status-smbd.png)

```
netstat -an | grep LISTEN
```

![status smb](https://github.com/ibmcf/Projeto-final-SRED2022/blob/main/Imagens/SAMBA/netstat.png)

4. Faça o backup do arquivo de configuração do samba e crie um arquivo novo somente com os comandos necessários.

```
 sudo cp /etc/samba/smb.conf{,.backup}
```

```
$ ls -la
$ sudo bash -c 'grep -v -E "^#|^;" /etc/samba/smb.conf.backup | grep . > /etc/samba/smb.conf'
```

````
$ sudo nano /etc/samba/smb.conf
````

````
[global]
   workgroup = WORKGROUP
   server string = %h server (Samba, Ubuntu)
   log file = /var/log/samba/log.%m
   max log size = 1000
   logging = file
   panic action = /usr/share/samba/panic-action %d
   server role = standalone server
   obey pam restrictions = yes
   unix password sync = yes
   passwd program = /usr/bin/passwd %u
   passwd chat = *Enter\snew\s*\spassword:* %n\n *Retype\snew\s*\spassword:* %n\n *password\supdated\ssuccessfully* .
   pam password change = yes
   map to guest = bad user
   usershare allow guests = yes
[printers]
   comment = All Printers
   browseable = no
   path = /var/spool/samba
   printable = yes
   guest ok = no
   read only = yes
   create mask = 0700
[print$]
   comment = Printer Drivers
   path = /var/lib/samba/printers
   browseable = yes
   read only = yes
   guest ok = no
````
![smb.conf](https://github.com/ibmcf/Projeto-final-SRED2022/blob/main/Imagens/smb-conf.png)

5. Reinicie o serviço smbd

````
 sudo systemctl restart smbd
````

6. Volte ao arquivo ```smb.conf``` e modifique a pasta /samba/public para acesso a somente usuários do grupo sambashare. Para isso, comente as linhas do arquivo de acordo com a imagem abaixo:

![smb.conf](https://github.com/ibmcf/Projeto-final-SRED2022/blob/main/Imagens/SAMBA/smb-conf.png)

## Compartilhamento Samba


1. Crie um usuário do S.O para que possa utilizar o compartilhamento samba. Para isso, digite:

```` 
sudo adduser <username>
````
![adduser](https://github.com/ibmcf/Projeto-final-SRED2022/blob/main/Imagens/SAMBA/adduser.png)


2. Para vincular o usuário do S.O. ao Serviço Samba, utilize:

````
 sudo smbpasswd -a <username>
````
````
 sudo usermod -aG sambashare <username>
````

3. Agora, iremos criar um diretório para compartilhar o SAMBA em rede.

````
mkdir /home/<username>/sambashare/
````

````
sudo mkdir -p /samba/public
````

````
sudo chown -R nobody:nogroup /samba/public
````

````
sudo chmod -R 0775 /samba/public
````

````
sudo chgrp sambashare /samba/public
````

![Diretório](https://github.com/ibmcf/Projeto-final-SRED2022/blob/main/Imagens/SAMBA/diretorio-samba.png)

No cliente:

1. Na barra do explorador de arquivos digite o ip do servidor samba.

2. Ao clicar na pasta public, irá abrir uma janela pedindo usuário e senha. Informe de acordo com o usário que foi criado anteriormente.

3. Se as informações forem informadas corretamente, você terá acesso à pasta que foi criada.

4. Ao utilizar o comando ``` ls -la /samba/public``` você conseguirá visualizar os arquivos criados nela.

![](https://github.com/ibmcf/Projeto-final-SRED2022/blob/main/Imagens/SAMBA/pasta.png)
