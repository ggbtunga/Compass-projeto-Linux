# Atividade Prática 1 #PB - NOV 2024 | DevSecOps

Este projeto demonstra a configuração do WSL2 e do servidor NGINX, incluindo automação e criação de scripts personalizados.


## Pré-requisitos
- Sistemas Operacionais suportados : **Windows 10/11**
- Windows Subsystem for Linux 2 (**WSL2**)
- Distribuição Linux no WSL2 : **Ubuntu 20.04.6 LTS**

## Índice

 1. [Instalação do WSL2](#1-instalação-do-wsl2)
 2. [Instalação da Distribuição Linux Ubuntu 20.04.6 LTS](#2-instalação-da-distribuição-linux-ubuntu-20046-lts)
 3. [Instalação do NGINX](#3-instalação-do-nginx)
 4. [Configurações de permissões e logs](#4-configurações-de-permissões-e-logs)
 5. [Criação do script verificador](#5-criação-do-script-verificador)
 6. [Automatizando a execução do script](#6-automatizando-a-execução-do-script)

## 1. Instalação do WSL2
#### 1.1 Abra Prompt de Comando ou PowerShell com permissão de admnistrador e insira o comando:
```bash
wsl --install
```   
#### 1.2 Verifique se foi instalado corretamente como seguinte comando: 
```bash
wsl -l -v
```  
## 2. Instalação da Distribuição Linux Ubuntu 20.04.6 LTS
#### 2.1 Abra a Microsoft Store e pesquise por Ubuntu 20.04.6 LTS
#### 2.2 Procure por Ubuntu 20.04.6 LTS
#### 2.3 Adquira e instale a distribuição.

## 3.Instalação do NGINX 
#### 3.1 Abra o terminal do Ubuntu 20.04 e configure um usuário.
#### 3.2 Atualize a instância Ubuntu dentro do wsl2:
```bash
sudo apt update
```  
    
#### 3.3 Faça a instalação do NGINX:
```bash
sudo apt install nginx
```  
    
#### 3.4 Após a instalação, verifique o status do NGINX, basta abrir um navegador e ir em localhost ou digite este comando no terminal:
```bash
sudo systemctl status nginx
```  
![status_active](https://github.com/user-attachments/assets/1d02b512-0f96-4c4d-8b3d-68f4ef72978e)
![browser_localhost](https://github.com/user-attachments/assets/51b410c3-25cd-407b-9de7-b5c88d73720f)

#### 3.5 Se estiver parado,é necessário inicializá-lo:
```bash
sudo systemctl enable nginx
sudo systemctl start nginx
```  
    
#### 3.6 Verifique a porta em que o nginx está:
```bash
sudo ss -tunelp | grep nginx
```  
![porta](https://github.com/user-attachments/assets/fed9774d-7d1d-4253-89eb-b2b0547bc58b)    

## 4. Configurações de permissões e logs
#### 4.1 Altere a permissão de escrita do diretório /var/log/nginx:
```bash
sudo chmod 755 /var/log/nginx
```  
    
#### 4.2 Crie dois arquivos de logs para saída do script:
```bash
sudo touch /var/log/nginx/status_online.log
sudo touch /var/log/nginx/status_offline.log
```  

#### 4.3 Altere o proprietário dos arquivos:
```bash
sudo chown $user:$user /var/log/nginx/status_online.log
sudo chown $user:$user /var/log/nginx/status_offline.log
```  

Substitua o "$user" pelo nome de seu usuário.
## 5. Criação do script verificador
#### 5.1 Crie um diretório para guardar scripts:
```bash
mkdir scripts
```  
    
#### 5.2 Crie o arquivo script nginx_status_check.sh:
```bash
sudo nano scripts/nginx_status_check.sh
```  
    
#### 5.3 Utilize esse script para verificar o status do NGINX:
![script](https://github.com/user-attachments/assets/e06867a5-4903-4083-8eff-e6b3f9f9cf66)
#### 5.4 Dê permissão de execução para o script:
```bash
sudo chmod +x scripts/nginx_status_check.sh
```

#### 5.5 Execute o script para verificar o status do NGINX.
Script executado com o serviço online:
![script_online](https://github.com/user-attachments/assets/d5f36558-a848-4f28-8052-acb6fd6cb3ea)

Script executado com o serviço offline:
![script_offline](https://github.com/user-attachments/assets/a73326b0-a20a-40c5-b37e-a307c7f9cbed)

## 6. Automatizando a execução do script
#### 6.1 Para automatizar a execução do script, utilize o cron:
```bash
crontab -e
```     
#### 6.2 Dentro da crontab, insira esta linha de comando e salve o arquivo para ser executado a cada 5 minutos:
```bash
*/5 * * * * scripts/nginx_status_check.sh
```  
#### 6.3 Verifique a saída dos logs:

```bash
cat /var/log/nginx/status_online.log
cat /var/log/nginx/status_offline.log
```
Saída do log de status online:

![log_online](https://github.com/user-attachments/assets/3d2b1a31-5abf-42ce-87e4-beeab9972d84)

Saída do log de status offline:

![log_offline](https://github.com/user-attachments/assets/0655adb5-76ba-456b-8338-2761b74dba2d)
