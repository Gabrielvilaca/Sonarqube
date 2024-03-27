## SonarQube

#### Tecnologias 

![SonarQube](https://img.shields.io/badge/SonarQube-black?style=for-the-badge&logo=sonarqube&logoColor=4E9BCD) 
![Ubuntu](https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)
![Java](https://img.shields.io/badge/java-%23ED8B00.svg?style=for-the-badge&logo=openjdk&logoColor=white)
![MicrosoftSQLServer](https://img.shields.io/badge/Microsoft%20SQL%20Server-CC2927?style=for-the-badge&logo=microsoft%20sql%20server&logoColor=white)

### Como instalar o SonarQube 10 no Ubuntu + SQL Server

O SonarQube possui uma versão gratuita chamada SonarQube Community, e é essa que trataremos aqui.

- Realize a conexão com a máquina 


### Passo 1 : Instalando o Java na máquina Linux

-  verifique se você já tem o Java
> java -version 

- instale o Java
> sudo apt -y install openjdk-17-jre

É uma boa ideia aumentar a memória virtual máxima para evitar o erro *[ERRO: max áreas de memória virtual vm.max_map_count]*

### Passo 2 : Baixe e configure o SonarQube

- Baixe e configure o SonarQube
> wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-10.1.0.73491.zip

- Realize a instalação do unzip
> sudo apt -y install unzip

- Descompacte o arquivo e o mova para o diretório /opt/sonarqube/
> sudo unzip  x.x.xxxxx.zip -d /opt && sudo mv /opt/sonarqube* /opt/sonarqube

- Caso não tenha, crie um usuário para ter acesso aos arquivos, criei o usuário sonarqube em */home/sonarqube
sudo chown -R sonarqube:sonarqube /opt/sonarqube/*

### Etapa 3: Configurar o banco de dados SQL

- Após a configuração do Banco de dados, use o SQL Server Management Studio e execute as consultas

> ALTER DATABASE nome_do_banco SET READ_COMMITTED_SNAPSHOT ON WITH ROLLBACK IMMEDIATE;

> ALTER DATABASE nome_do_banco COLLATE SQL_Latin1_General_CP1_CS_AS

> SELECT CONVERT (varchar(256), SERVERPROPERTY('collation'));

- Atualize a cadeia de conexão: 
> sudo nano /opt/sonarqube/conf/sonar.properties

- Após entrar no sonar.properties insira os dados abaixo

> sonar.jdbc.url=jdbc:sqlserver://[YOUR SERVER ADDRESS];databaseName=sonar <br>
sonar.jdbc.username=[YOUR USER NAME]<br>
sonar.jdbc.password=[DATABASE PASSWORD]

#### REALIZE O TESTE:
> /opt/sonarqube/bin/linux-x86-64/sonar.sh console

*Para se conectar a conta é admin e senha admin*

### Passo 4 : Faça um serviço

> sudo nano /etc/systemd/system/sonar.service

- Copie e cole as configurações no sonar.service: 

>[Unit]<br>
Description=SonarQube service<br>
After=syslog.target network.target

> [Service]<br>
Type=forking

> ExecStart=/opt/sonarqube/bin/linux-x86-64/sonar.sh start<br>
ExecStop=/opt/sonarqube/bin/linux-x86-64/sonar.sh stop<br>
ExecStatus=/opt/sonarqube/bin/linux-x86-64/sonar.sh status<br>


>User=usuário_configurado_em_/home/<br>
Group=grupo_configurado_em_/home/<br>
Restart=always<br>

- Após isso salve o arquivo e prossiga com os comandos:

> sudo systemctl enable sonar

> sudo systemctl start sonar

> sudo systemctl status sonar
