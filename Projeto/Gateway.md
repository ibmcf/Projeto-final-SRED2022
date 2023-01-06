# Gateway

Nesta etapa iremos configurar o servidor de gateway. Ele pode através do NAT (Network Address Translation) fazer o roteamento todos as maquinas de uma rede interna  para uma rede externa.

## Passo a passo

1. Primeiramente, é necessário se conectar via ssh ao IP de gateway definido na planilha de acompanhamento. 

2. Em seguida, troque o hostname para o definido na planilha. Para isso, utilize o seguinte comando:

```
hostnamectl set-hostname <hostname>
```
No nosso caso o comando utilizado foi:
```
hostnamectl set-hostname gw.grupo7.turma924.ifalara.local
```

**IMAGEM DO HOSTNAME AQUI**

3. Habilite o firewall e permita o ssh:

```
sudo ufw enable
sudo ufw allow ssh
```
**IMAGEM DO FIREWALL AQUI**

4. Agora, habilite o encaminhamento de pacotes da interface WAN para a LAN. Isso será possível ao acessar o arquivo `sysctl.conf`

```
sudo nano /etc/ufw/sysctl.conf
```
5. Ao visualizar o arquivo, descomente a linha que contém o parâmetro net/ipv4/ip_forward=1:

**IMAGEM DO ARQUVO SYSCTL.CONF**

6. Para a configuração da interface de rede netplan utilize o comando abaixo:

```
sudo nano /etc/netplan/00-installer-config.yaml
```

7. Para configurar esse arquivo você deve:

    *  Em adresses adicione o endereço de ip do Gateway definido na tabela;
    * Nos addresses de name servers adicione os endereços de ip NS1 e NS2;
    * Em search adicione grupo7.turma924.ifalara.local (adapte de acordo com o seu grupo);
    * Deixe o dhcp4 como ```false``` nas duas interfaces;
    
    | Não se esqueça de usar ```sudo netplan apply``` no final
 
 
 8.   
  
