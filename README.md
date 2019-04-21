# Hackathon Globo - Grupo 3 - Captive Portal GloboPlay


[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](#)

Captive Portal GloboPlay é um projeto desenvolvido no Hackathon Globo 2019 usando as tecnologias e ferramentas.

*       Rede Wireless;
*   	Sistema Operacional CentOS 7;
*   	Captive Portal: projeto original disponivel em [public repository][https://github.com/zoilomora/captive-portal]on GitHub. 
*       [HTML] - linguagem de marcação web;
*       [PHP] - linguagem interpretada livre;
*       [CSS] - Mecanismo para adicionar estilo a um documento web;
*       [MySql] - sistema de gerenciamento de banco de dados;
 *       [Visual Code] - editor de código-fonte desenvolvido pela  [Microsoft] 
*       [Adobe XD](https://www.adobe.com/br/products/xd.html) - ferramenta de prototipação

#  Características!

  -      Captive Portal GloboPlay para Coleta e análise de dados para captar métricas (KPI) para assim fomentar produtos da Globo fora do país.



1.	Como funciona
  -         Tudo começa quando o turista ao ingressar na rede wi-fi de nossos parceiros que estar contemplado com um captive portal que fara o controle de acesso e fornecerá um acesso patrocinado a plataforma do globo play. 
-	        Assim, a aplicação permitirá à internacionalização do GloboPlay e direcionar quais conteúdos focar no exterior e como tratar esse produto para a audiência local.










### Instalação
Seguir os passos abaixos: 

  Atualizar CentOS 7:
```sh
$ check-update
$ yum update
```
 Desativar firewall por padrão:
 
```sh
$ systemctl stop firewalld
$ systemctl disable firewalld
```

Instalar pacotes e dependências
```sh
# Tools
$ yum install wget nano
```

Firewall
```sh
$  yum install iptables-services
```

    # FreeRADIUS
```sh
$  yum install freeradius freeradius-utils
```
 Web Server
```sh
$  yum install httpd openssl mod_ssl
```

Chillispot Dependências 
    
```sh
$  yum install glibc-devel.i686 glibc-i686 perl-Digest-MD5
```
Instalar Chillispot:
```sh
$  wget https://raw.githubusercontent.com/zoilomora/captive-portal/master/chillispot-1.1.0.i386.rpm
```
```sh
$   rpm -Uvh chillispot-1.1.0.i386.rpm
```

    
    Edit the file /etc/chilli.conf and modify the following lines:

 DNS
 
 ```sh
   $ dns1 8.8.8.8
   $ dns2 8.8.4.4
```
    

 FreeRADIUS
```sh
   $ radiusserver1 127.0.0.1
   $ radiusserver2 127.0.0.1
   $ radiussecret secret-password-for-radius
```
    

 DHCP
  
```sh
   $   dhcpif eth1
```
 Universal access method (UAM) - MUDAR PARA SEU IP 
 
 ```sh
   $    uamserver https://192.168.0.33/cgi-bin/hotspotlogin.cgi
   $    uamhomepage https://192.168.0.33/
   $    uamsecret secret-password-for-uam
```
   

Link dicionário de Chillispot para FreeRADIUS

    
 ```sh
   $    echo "\$INCLUDE /usr/share/doc/chillispot-1.1.0/dictionary.chillispot">>/etc/raddb/dictionary
```
    Copie o script de login e conceda as permissões:

 
 ```sh
    $ cd /var/www/cgi-bin/
    $ cp /usr/share/doc/chillispot-1.1.0/hotspotlogin.cgi ./hotspotlogin.cgi
    $ chown apache.apache ./hotspotlogin.cgi
    $ chmod 700 ./hotspotlogin.cgi
```
   Edite o arquivo /var/www/cgi-bin/hotspotlogin.cgi:

   # Descomente as linhas
    
    
```sh
   $ uamsecret = "secret-password-for-uam";
   $ userpassword = 1;
```



    Copiar pasta HTML_Aplicacao em   /var/www/html/:



 Ative as regras de firewall do Chillispot:

        Executa regras iptables e está habilitado na memória
  
```sh
   $   /usr/share/doc/chillispot-1.1.0/firewall.iptables
```
 The rules persist
   
```sh
  $  service iptables save
```
   Ativar encaminhamento de IP:


```sh
  $  echo "net.ipv4.ip_forward = 1" >> /usr/lib/sysctl.d/50-default.conf
```


Aplica as configurações ao sistema   
    
```sh
  $   /sbin/sysctl -p
```



Ajuste o segredo compartilhado do FreeRADIUS editando o arquivo /etc/raddb/clients.conf:

    client localhost {
        # Replace the default password with that of step 5 (radiussecret)
        secret = secret-password-for-radius
    }


Registre o usuário no FreeRADIUS editando o arquivo  /etc/raddb/users:

Insira uma linha para cada usuário no final do arquivo

    hacka Cleartext-Password := "letgohack"

Check access to FreeRADIUS from console:

    radtest "hacka" "letgohack" 127.0.0.1 0 testando


Ative os serviços para que eles iniciem na inicialização:

```sh
    $ systemctl enable iptables
    $ systemctl enable httpd
    $ systemctl enable radiusd
    $ systemctl enable chilli
```
   
Reinicie o servidor para aplicar e ativar os serviços

   

```sh
    $  reboot
```

License
----

MIT

