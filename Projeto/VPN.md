# VPN

Esta etapa do projeto tem o objetivo de configurar o cliente do Open VPN no linux para acessar o Laboratório Virtual de Redes (LabRedes)

* No terminal do PC com Linux do Laboratório Lab92, faça login com o usuário ``redes``
```bash
su redes
cd ~
```

Antes de iniciar a instalação do OpenVPN3, verifique se os seguintes pré-requisitos estão OK. Caso não, complete-os de acordo com as instruções abaixo.

## Pré-Requisitos para baixar o OpenVPN3

```bash
sudo apt install apt-transport-https -y
sudo apt install curl -y
sudo apt install gpg -y
```

```bash
curl -fsSL https://swupdate.openvpn.net/repos/openvpn-repo-pkg-key.pub | gpg --dearmor > ~/openvpn-repo-pkg-keyring.gpg
sudo mv openvpn-repo-pkg-keyring.gpg /etc/apt/trusted.gpg.d/openvpn-repo-pkg-keyring.gpg

curl -fsSL https://swupdate.openvpn.net/community/openvpn3/repos/openvpn3-focal.list > ~/openvpn3.list
sudo mv openvpn3.list /etc/apt/sources.list.d/openvpn3.list
sudo apt update
```

## Instalar OpenVPN3

1. Importe o certificado para o OpenVPN. Para isso, digite no terminal:

```bash
curl -fsSL https://www.dropbox.com/s/hb8ee3kiwkhutbl/vpn924.labredes.arapiraca.ifal.edu.br.ovpn?dl=0 > ~/vpn924.labredes.arapiraca.ifal.edu.br.ovpn
```

2. Com os pré-requisitos de instalação feitos, digite os seguintes comandos para instalar o OpenVPN3:

```bash
sudo apt install openvpn3
sudo apt install kmod-ovpn-dco
```

## Perfil de configuração

* Para verificar se o perfil de configuração está funcionando, digite o comando abaixo:

```bash
openvpn3 configs-list
```

* Caso queira remover algum perfil de configuração já instalado, digite:

```bash
openvpn3 config-remove --path CONFIG_PATH
```
 O CONFIG_PATH é o caminho da configuração listado no configs-list
 
 ## Conexão VPN
 
 * Para iniciar uma conexão, digite o comando abaixo:

```bash
openvpn3 session-start --config CONFIG_NAME
```

* Para listar as conexões abertas, digite:

```bash
openvpn3 sessions-list
```

* Para finalizar uma conexão:

```bash
openvpn3 session-manage --config CONFIG_NAME --disconnect
```

ou

```bash
openvpn3 session-manage --path CONFIG_PATH --disconnect
```


 





