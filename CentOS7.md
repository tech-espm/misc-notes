Comandos importantes para CentOS 7
==================================

Verificar a versão
```bash
hostnamectl
```

Listar programas atribuídos à portas abertas
```bash
netstat -tulnp
```

---

Inicialização geral do ambiente em um CentOS 7
==============================================

Instalar o wget
```bash
sudo yum install wget
```

Instalar o Apache
```bash
sudo yum install httpd mod_ssl
```

Iniciar o Apache
```bash
sudo /usr/sbin/apachectl start
```

Fazer o Apache iniciar automaticamente
```bash
sudo systemctl enable httpd.service
```

Verificar o status do serviço
```bash
sudo systemctl status httpd
```

Instalar o Node.js 10 + npm

https://github.com/nodesource/distributions
```bash
yum install gcc-c++ make
curl -sL https://rpm.nodesource.com/setup_10.x | bash -
sudo yum install -y nodejs
```

Instalar o TypeScript
```bash
sudo npm install -g typescript
```

Instalar o .Net Core Runtime

https://dotnet.microsoft.com/download/linux-package-manager/centos/runtime-current
```bash
sudo rpm -Uvh https://packages.microsoft.com/config/rhel/7/packages-microsoft-prod.rpm
```
Apesar de pedir para atualizar utilizando `sudo yum update`, devido ao bug que ocorreu no ambiente da última que `sudo yum update` foi executado no CentOS 7, resolvi não executar `sudo yum update`, e ainda assim o comando abaixo foi OK.
```bash
sudo yum install aspnetcore-runtime-2.2
```
Se algum dia precisar utilizar alguma classe do assembly `System.Drawing.Common`, como `Bitmap` ou `Image`, pode ocorrer a exceção `"The type initializer for 'Gdip' threw an exception."`. Para corrigir isso, é preciso instalar a biblioteca `libgdiplus`:
```bash
sudo yum install libgdiplus
```

Listar portas e serviços abertos
```bash
sudo firewall-cmd --list-all
```

Liberar a porta 80 e 443
```bash
sudo firewall-cmd --add-service=http --permanent
sudo firewall-cmd --add-service=https --permanent
sudo firewall-cmd --reload
```

Nesse ponto, para testar se está OK, a partir de um browser externo, basta acessar `http://ip-externo` (deve aparecer a página de teste do Apache).

Caminho do arquivo de configuração do Apache: `/etc/httpd/conf/httpd.conf`

Diretório padrão do Apache para o conteúdo dos sites: `/var/www/html`

Instalar o PHP 7 + Módulos PHP comuns

https://www.linuxtechi.com/install-php-7-centos-7-rhel-7-server/
```bash
sudo yum install epel-release yum-utils -y
sudo yum install http://rpms.remirepo.net/enterprise/remi-release-7.rpm
sudo yum-config-manager --enable remi-php72
sudo yum install php php-common php-opcache php-mcrypt php-cli php-gd php-curl php-mysql -y
php -v (Apenas para verificar a versão)
sudo systemctl restart httpd.service (Precisa reiniciar o Apache depois de instalar o PHP)
```

Para verificar se o PHP está OK, pode criar o arquivo
```bash
sudo nano /var/www/html/info.php
```
com o conteúdo
```php
<?php 
phpinfo(); 
?>
```
e acessar `http://ip-externo/info.php` para confirmar se tudo está OK.

---

Certificado SSL do Let's Encrypt
================================

Instalar os repositórios necessários
```bash
sudo yum install epel-release mod_ssl
```

Instalar o certbot do Let's Encrypt
```bash
sudo yum install python-certbot-apache
```

Configurar o certbot
```bash
sudo certbot --apache -d nome-do-subdominio.nome-do-dominio.com
```

A configuração vai pedir um e-mail de contato e vai perguntar se deseja alterar o arquivo de configuração do servidor para redirecionar o tráfico de HTTP para HTTPS por padrão (o que é comum responder sim).

Configurar o certbot renew para executar duas vezes por dia
```bash
crontab -e
```
O Vim será aberto para editar o crontab, então, basta acrescentar a linha
```bash
* */12 * * * /usr/bin/certbot renew >/dev/null 2>&1
```

> Para entrar no modo de edição basta teclar `Esc` e depois pressionar uma letra qualquer, para então poder colar a linha acima.
>
> Ao final, basta sair do modo de edição teclando `Esc`, digitando `:x` e teclando `Enter` novamente para salvar o arquivo e sair do Vim.

---

MySQL
=====

Baixar o rpm correto na pasta ~ (Para confirmar o link abaixo, basta entrar em https://dev.mysql.com/downloads/repo/yum/, fazer o download manual e conferir o link do arquivo RPM)
```bash
sudo wget https://repo.mysql.com/mysql80-community-release-el7-3.noarch.rpm
```

Se quiser conferir o download, basta executar o comando abaixo e comparar o hash com o hash mostrado no site https://dev.mysql.com/downloads/repo/yum/
```bash
sudo md5sum mysql80-community-release-el7-3.noarch.rpm
```

Atualizar o repositório
```bash
sudo rpm -ivh mysql80-community-release-el7-3.noarch.rpm
```

Instalar o MySQL
```bash
sudo yum install mysql-server
```

Iniciar o MySQL
```bash
sudo systemctl start mysqld
```

Fazer o MySQL iniciar automaticamente
```bash
sudo systemctl enable mysqld
```

Mostrar na tela a senha padrão gerada para o usuário root
```bash
sudo grep 'temporary password' /var/log/mysqld.log
```

Executar o script para terminar a instalação com segurança (quando pedir a senha atual do root, coloca a senha listada acima, depois escolhe uma senha nova para o root)
```bash
sudo mysql_secure_installation
```

Efetuar login on MySQL com o usuário root
```bash
mysql -u root -p
```

Criar um banco novo
```SQL
CREATE DATABASE nome-do-banco;
```

Criar um usuário novo
```SQL
CREATE USER 'nome-do-usuario'@'localhost' IDENTIFIED BY 'senha-do-usuario';
```

Conceder permissões ao usuário novo somente a esse banco de dados
```SQL
GRANT ALL PRIVILEGES ON nome-do-banco.* TO 'nome-do-usuario'@'localhost';
```

Se quiser limitar apenas para as permissões comuns
```SQL
GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, ALTER, CREATE TEMPORARY TABLES, LOCK TABLES ON nome-do-banco.* TO 'nome-do-usuario'@'localhost';
```

> Toda vez que uma tabela/view/etc for criada pelo root, ou por outro usuário que não o usuário `nome-do-usuario`, será preciso executar o comando `GRANT` acima, ou ocorrerão erros na hora do usuário tentar logar/acessar!

Caso ocorra algum problema de conexão remota utilizando Node.js, .Net ou outra linguagem/framework
```SQL
ALTER USER 'nome-do-usuario'@'localhost' IDENTIFIED WITH mysql_native_password BY 'senha-do-usuario';
```

Para restaurar um arquivo de dump no formato `.sql`, basta criar o banco e o usuário utilizando os comandos acima, copiar o arquivo de dump, por exemplo `dump.sql`, para um diretório do servidor, ir até o diretório e executar o comando abaixo
```bash
mysql -u nome-do-usuario -p nome-do-banco < dump.sql
```

> Dependendo dos comandos utilizados dentro do arquivo de dump, antes de executar o dump será necessário executar `GRANT ALL PRIVILEGES` em vez da versão apenas com permissões comuns.

---

Configurando o Apache para servir diversos hosts/hosts virtuais
===============================================================

Criar as pastas sites-available e sites-enabled
```bash
sudo mkdir /etc/httpd/sites-available /etc/httpd/sites-enabled
```

Alterar o arquivo de configuração do Apache para procurar por hosts virtuais
```bash
sudo nano /etc/httpd/conf/httpd.conf
```
Depois, acrescentar essa linha ao final
```Apache
IncludeOptional sites-enabled/*.conf
```

Com isso feito, basta criar o arquivo de configuração (poderia ser `/etc/httpd/sites-available/site-teste.conf` caso fosse uma "subpasta virtual" do domínio raiz)
```bash
sudo nano /etc/httpd/sites-available/teste.com.br.conf
```

```Apache
<VirtualHost ...>
...
</VirtualHost>
```

O conteúdo do arquivo varia conforme o objetivo desejado/linguagem utilizada.

Por fim, basta criar um link simbólico de `sites-enabled/xxx.conf` apontando para o arquivo real `sites-available/xxx.conf` (faz isso para poder excluir o arquivo/link da pasta sites-enabled, que é onde o Apache vai procurar de verdade, mas não perder o arquivo de configuração real, que pode ser reutilizado depois)
```bash
sudo ln -s /etc/httpd/sites-available/teste.com.br.conf /etc/httpd/sites-enabled/teste.com.br.conf
```

Depois disso, deve-se reiniciar o Apache
```bash
sudo systemctl restart httpd.service
```

> Normalmente o SELinux vai bloquear o Apache de realizar conexões remotas, o que faz com que o Apache nunca funcione como proxy reverso no CentOS, nem que ele consiga realizar requisições para outros servidores!!! Para corrigir isso:
> ```bash 
> sudo /usr/sbin/setsebool -P httpd_can_network_connect 1
> ```

Se o Apache for acessar os arquivos por si próprio, deve-se criar as pastas do Apache para o subdomínio teste.com.br (poderia ser `/var/www/site-teste/xxx` caso fosse uma "subpasta virtual" do domínio raiz). As pastas convencionais são:
```bash
sudo mkdir -p /var/www/teste.com.br/html
sudo mkdir -p /var/www/teste.com.br/log
sudo mkdir -p /var/www/teste.com.br/code
```

Executar as linhas abaixo apenas nos diretórios onde o usuário apache terá permissão de escrita:
```bash
sudo chown -R apache /var/www/teste.com.br/log
```

Para garantir que as permissões estejam corretas
```bahs
sudo chmod -R 755 /var/www
```

---

Permissões
==========

```
Read (r)    4
Write (w)   2 
Execute (x) 1
```

755 = owner (primeiro dígito: 7) group (segundo dígito: 5) others (terceiro dígito: 5)

Isso fará com que o usuário apache (que é o usuário padrão do Apache) consiga apenas ler e executar os arquivos das pastas. Para conferir as permissões de um arquivo, basta executar `ls -l` para ver todos os arquivos/diretórios do diretório atual, ou `ls -l nome-do-arquivo`, como `ls -l app.js`, para ver apenas as permissões daquele arquivo.

A saída vai ser algo como
```
-rw-r--r--. 1 root root 1479 Ago 12 21:48 app.js
```

`-rw-r--r--` é dividido em 4 partes:

O primeiro caractere é o tipo (- arquivo comum, d diretório ou l link simbólico). Depois são três grupos com três caracteres com as permissões do owner, grupo e others.
1. `-`: arquivo comum
2. `rw-`: owner rw
3. `r--`: group r
4. `r--`: others r

O primeiro root é o owner e o segundo root é o grupo ao qual aquele arquivo pertence

---

Deploy de aplicações Node
=========================

Antes de fazer deploy de qualquer aplicação, é preciso instalar o `PM2`, que é um gerenciador de processos de aplicações Node.js. Ele reinicia a aplicação Node caso ela pare de funcionar.

```bash
sudo npm install -g pm2@latest
```

Fazer com que o PM2 inicie automaticamente
```bash
sudo pm2 startup systemd
```

Para listar aplicações Node registradas/executando
```bash
pm2 list
```

Para iniciar uma aplicação e já adicioná-la à lista de aplicações gerenciadas (deve executar esse comando dentro do diretório onde está o arquivo app.js)
```bash
pm2 start app.js --name nome-do-app
```

Para reiniciar uma aplicação
```bash
pm2 restart nome-do-app --update-env
```

---

Deploy de aplicações .Net Core
==============================

https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/linux-apache.md

https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/linux-apache?view=aspnetcore-2.1

https://docs.microsoft.com/en-us/aspnet/core/fundamentals/servers/kestrel?view=aspnetcore-2.1

https://github.com/aspnet/MetaPackages/blob/master/src/Microsoft.AspNetCore/WebHost.cs#L161

Antes de fazer deploy de qualquer aplicação, é preciso ter instalado o runtime do .Net Core, como descrito anteriormente.

Dentro de Program.cs, configurar o Kestrel (mesmo não utilizando .UseKestrel(), ele é utilizado por padrão, a não ser que outro servidor seja especificado)
```C#
return WebHost.CreateDefaultBuilder(args)
    .ConfigureAppConfiguration((hostingContext, config) => {
        var env = hostingContext.HostingEnvironment;

        config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
              .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, reloadOnChange: true);
    })
    .UseKestrel((builderContext, options) => {
        options.Configure(builderContext.Configuration.GetSection("Kestrel"));
    })
    .UseStartup<Startup>();
```

Configurar as opções do Kestrel, inclusive a porta, dentro de appsettings.json:
```json
{
    "Logging": {
        "LogLevel": {
            "Default": "Warning"
        }
    },
    "AllowedHosts": "*",
    ...
    "Kestrel": {
        "EndPoints": {
            "Http": {
                ...
                "Url": "http://localhost:5003"
            }
        }
    }
}
```

Essa porta não afeta o comportamento durante o debug pelo IIS Express.

Em seguida, alterar o arquivo Startup.cs:

Caso a aplicação não esteja sendo executada a partir da raiz, deve-se especificar qual é a raiz, para fazer funcionar os links, controllers e o mapeamento de "~/..." do Razor:
```C#
app.UsePathBase("/teste");
```

Com isso, convém deixar o arquivo de configuração do Apache assim:
```Apache
ProxyPass /teste http://127.0.0.1:5005/teste retry=0
ProxyPassReverse /teste http://127.0.0.1:5003/teste
```

Configurar o app para usar encaminhamento de cabeçalhos (que será necessário quando utilizar o app .Net Core via proxy do Apache no Linux)
```C#
app.UseForwardedHeaders(new ForwardedHeadersOptions {
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});
```

Fazer o deploy via Publish, utilizando
```
Deployment Mode: Framework Dependent
Target Runtime: linux-x64
```

Copiar os arquivos publicados para uma pasta dentro do local padrão do Apache: `/var/www/nome-do-app`

Configurar um serviço no Linux para iniciar/reiniciar o app automaticamente
```bash
sudo nano /etc/systemd/system/kestrel-nome-do-app.service
```

Conteúdo:

```ini
[Unit]
Description=nome-do-app app

[Service]
WorkingDirectory=/var/www/nome-do-app
ExecStart=/usr/bin/dotnet /var/www/nome-do-app/nome-do-app.dll
Restart=always
# Reiniciar depois de 10 segundos em caso de problemas
RestartSec=10
KillSignal=SIGINT
SyslogIdentifier=dotnet-nome-do-app
# Configurar o user correto
User=apache
Environment=ASPNETCORE_ENVIRONMENT=Production 

[Install]
WantedBy=multi-user.target
```

Configurar o serviço para iniciar automaticamente
```bash
sudo systemctl enable kestrel-nome-do-app.service
```

Iniciar o serviço
```bash
sudo systemctl start kestrel-nome-do-app.service
```

Verificar o status do serviço/app funcionando (utilizar a porta correta)
```bash
sudo systemctl status kestrel-nome-do-app.service
curl localhost:5003
```

Por fim, configurar um proxy no Apache, apontando para `localhost:5003`
```bash
sudo nano /etc/httpd/sites-available/nome-do-app.conf
```

```Apache
<VirtualHost ...>
...
</VirtualHost>
```

```bash
sudo ln -s /etc/httpd/sites-available/nome-do-app.conf /etc/httpd/sites-enabled/nome-do-app.conf
```

Depois disso, deve-se reiniciar o Apache
```bash
sudo systemctl restart httpd.service
```

---

Exemplos de arquivos de configuração
====================================

Pasta virtual redirecionando para o Node
```Apache
<VirtualHost *:80>
    ProxyPreserveHost On

    # Cuidado para não colocar o destino como http://127.0.0.1:3000/
    # porque o Apache vai concatenar tudo depois da origem ao final
    # do destino. Por exemplo, se vier /teste/ o Apache vai chamar
    # http://127.0.0.1:3000/. Se o destino já fosse http://127.0.0.1:3000/
    # então o Apache chamaria http://127.0.0.1:3000// o que pode dar
    # problemas com Node e com .Net Core!!!
    # Mais um exemplo: se a URL pedida for /teste/a/b/c/, como a origem
    # está configurada como /teste, /a/b/c/ será concatenado ao final do
    # destino, que se for http://127.0.0.1:3000, fará com que o Apache
    # chame http://127.0.0.1:3000/a/b/c/
    ProxyPass /teste http://127.0.0.1:3000
    ProxyPassReverse /teste http://127.0.0.1:3000
</VirtualHost>
```

Sub-domínio redirecionando para o Node
```Apache
<VirtualHost teste.com.br:80>
    ProxyPreserveHost On

    ProxyPass / http://127.0.0.1:3000 retry=0
    ProxyPassReverse / http://127.0.0.1:3000

    ServerName teste.com.br
</VirtualHost>
```

Pasta virtual redirecionando para o Node, mas servindo arquivos estáticos direto pelo Apache, sem o Node
```Apache
<VirtualHost *:80>
    ProxyPreserveHost On

    Alias "/teste/public" "/var/www/teste.com.br/html"
    # A ordem dos ProxyPass é importante!!!
    # ProxyPass /teste/public !    fará com que tudo que começar por
    # /teste/public não seja encaminhado para o proxy
    # https://stackoverflow.com/a/35928307/3569421
    ProxyPass /teste/public !
    ProxyPass /teste http://127.0.0.1:3000 retry=0
    ProxyPassReverse /teste http://127.0.0.1:3000
</VirtualHost>
```

Domínio principal (único domínio)
```Apache
<VirtualHost *:80>
    ServerName teste.com.br
    ServerAlias www.teste.com.br
    DocumentRoot /var/www/teste.com.br/public_html
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Caso tenha instalado o SSL
```Apache
<VirtualHost *:80>
    ... Conteúdo original do arquivo ...
RewriteEngine on
RewriteCond %{SERVER_NAME} =nome-do-subdominio.nome-do-dominio.com
RewriteRule ^ https://%{SERVER_NAME}%{REQUEST_URI} [END,NE,R=permanent]
</VirtualHost>
<IfModule mod_ssl.c>
<VirtualHost *:443>
    ... Conteúdo original do arquivo, igual ao mostrado acima ...
ServerName nome-do-subdominio.nome-do-dominio.com
SSLCertificateFile /etc/letsencrypt/live/nome-do-subdominio.nome-do-dominio.com/cert.pem
SSLCertificateKeyFile /etc/letsencrypt/live/nome-do-subdominio.nome-do-dominio.com/privkey.pem
Include /etc/letsencrypt/options-ssl-apache.conf
SSLCertificateChainFile /etc/letsencrypt/live/nome-do-subdominio.nome-do-dominio.com/chain.pem
</VirtualHost>
</IfModule>
```
