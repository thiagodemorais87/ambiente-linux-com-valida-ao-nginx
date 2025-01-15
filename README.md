Projeto: Monitoramento do Serviço Nginx no WSL
Descrição
Este projeto tem como objetivo configurar um ambiente Linux no Windows utilizando o WSL (Windows Subsystem for Linux), subir um servidor Nginx e criar um script de monitoramento que valide o status do serviço. O script gera relatórios em dois arquivos de saída: um para quando o serviço estiver online e outro quando o serviço estiver offline. A execução do script é automatizada a cada 5 minutos.

Tecnologias e Ferramentas Utilizadas
Windows Subsystem for Linux (WSL)
Ubuntu 20.04 ou superior
Servidor Web Nginx
Bash Script
Crontab (para agendamento de execução automática)
Pré-requisitos
Antes de começar, certifique-se de que possui os seguintes requisitos:

1. Windows 10/11 com WSL ativado
2. Instalação do Ubuntu no WSL
Instalar o WSL: Siga as instruções do link oficial para instalar o WSL em seu Windows: Instalar o WSL no Windows

Instalar o Ubuntu no WSL: Após ativar o WSL, instale o Ubuntu 20.04 ou superior diretamente da Microsoft Store ou pelo comando:

bash
Copiar código
wsl --install -d Ubuntu-20.04
Verificar a instalação do Ubuntu: Após a instalação, execute o Ubuntu no terminal do Windows:

bash
Copiar código
wsl
Instalação do Nginx
Atualizar o sistema: Após acessar o Ubuntu no WSL, atualize o sistema para garantir que todos os pacotes estão na versão mais recente.

bash
Copiar código
sudo apt update && sudo apt upgrade -y
Instalar o Nginx: Instale o servidor Nginx utilizando o seguinte comando:

bash
Copiar código
sudo apt install nginx -y
Iniciar o serviço do Nginx: Após a instalação, inicie o servidor Nginx com o comando:

bash
Copiar código
sudo systemctl start nginx
Verificar se o Nginx está funcionando: Acesse o endereço http://localhost no seu navegador. Você deverá ver a página padrão do Nginx, o que confirma que o servidor está rodando.

Criando o Script de Monitoramento
Criar o script de monitoramento:

Crie um arquivo bash para o monitoramento do serviço:

bash
Copiar código
nano monitor_nginx.sh
Conteúdo do script:

O script monitor_nginx.sh deve validar se o serviço Nginx está online e gerar dois arquivos de saída com informações sobre o status.

bash
Copiar código
#!/bin/bash

SERVICE="nginx"
DATE=$(date '+%Y-%m-%d %H:%M:%S')
LOG_DIR="/home/usuario/logs"

# Verifica o status do serviço
if systemctl is-active --quiet $SERVICE; then
    STATUS="ONLINE"
    MESSAGE="O serviço está ONLINE."
    FILE="$LOG_DIR/online.log"
else
    STATUS="OFFLINE"
    MESSAGE="O serviço está OFFLINE."
    FILE="$LOG_DIR/offline.log"
fi

# Cria o diretório de logs, se não existir
mkdir -p $LOG_DIR

# Salva as informações no arquivo de log
echo "$DATE - $SERVICE - Status: $STATUS - $MESSAGE" >> $FILE
Explicação:

O script verifica se o serviço Nginx está ativo.
Caso esteja ativo, ele gera um log no arquivo online.log; caso contrário, no arquivo offline.log.
A mensagem contém a data, o nome do serviço, o status e uma mensagem personalizada.
Tornar o script executável: Após criar o script, torne-o executável com o comando:

bash
Copiar código
chmod +x monitor_nginx.sh
Automatizando a Execução
Para rodar o script a cada 5 minutos, use o cron:

Editar o crontab: Abra o crontab para edição:

bash
Copiar código
crontab -e
Adicionar a linha de execução do script: No crontab, adicione a seguinte linha para rodar o script a cada 5 minutos:

bash
Copiar código
*/5 * * * * /home/usuario/monitor_nginx.sh
Essa linha executa o script monitor_nginx.sh a cada 5 minutos.

Verificar o agendamento: Para verificar se o cron está configurado corretamente, use o comando:

bash
Copiar código
crontab -l
Estrutura de Diretórios
A estrutura de diretórios do projeto será a seguinte:

lua
Copiar código
/home/usuario/
├── monitor_nginx.sh           # Script de monitoramento
├── logs/                      # Diretório onde os arquivos de log serão armazenados
│   ├── online.log             # Log do serviço ONLINE
│   └── offline.log            # Log do serviço OFFLINE
Versionamento
Este projeto deve ser versionado utilizando o Git. Para inicializar o repositório Git:

Inicialize o repositório:

bash
Copiar código
git init
Adicionar os arquivos:

bash
Copiar código
git add .
Fazer o commit:

bash
Copiar código
git commit -m "Configuração inicial do monitoramento do Nginx"
Adicionar o repositório remoto (caso queira utilizar o GitHub, por exemplo):

bash
Copiar código
git remote add origin https://github.com/seu_usuario/seu_repositorio.git
Subir as alterações para o repositório remoto:

bash
Copiar código
git push -u origin master
Testando a Solução
Para testar se a solução está funcionando corretamente:

Inicie o servidor Nginx: Caso o Nginx não esteja rodando, inicie-o:

bash
Copiar código
sudo systemctl start nginx
Verifique o funcionamento do script: Execute o script manualmente para verificar se ele gera o arquivo de log corretamente:

bash
Copiar código
./monitor_nginx.sh
Verifique os logs: O script deve gerar arquivos em /home/usuario/logs/ (online.log ou offline.log) com o status do serviço.

Verifique o agendamento: Após 5 minutos, o script será executado automaticamente e gerará novos logs.

Documentação
Instalação do Linux (Ubuntu) no WSL: Siga as instruções fornecidas pelos links de referência para instalar o WSL e o Ubuntu no seu Windows.
Subir e Monitorar o Nginx: Após a instalação do Ubuntu, o servidor Nginx pode ser iniciado e monitorado conforme descrito no processo acima.
