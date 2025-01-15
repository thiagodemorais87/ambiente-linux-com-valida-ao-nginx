Monitoramento de Serviço Nginx no Linux
Este projeto tem como objetivo monitorar o status do serviço Nginx em um ambiente Linux. O script verifica periodicamente se o serviço está online ou offline e registra os resultados em arquivos de log.

Funcionalidades
Verificação de Status: Valida o status do serviço Nginx (se está online ou offline).
Geração de Logs: Cria logs com data, hora, status e mensagens personalizadas.
Execução Automática: O script é executado automaticamente a cada 5 minutos por meio de um cron job, garantindo monitoramento contínuo.
Logs de Monitoramento: Registra eventos de status do serviço em diferentes arquivos de log.

Navegue até o diretório do projeto:

bash
Copiar código
cd monitoramento-nginx
Garanta que o script tenha permissões de execução:

bash
Copiar código
chmod +x monitor_nginx.sh
Configure o cron job para execução automática:

Abra o crontab:
bash
Copiar código
crontab -e
Adicione a seguinte linha para executar o script a cada 5 minutos:
bash
Copiar código
*/5 * * * * /caminho/para/o/script/monitor_nginx.sh
Estrutura de Diretórios
bash
Copiar código
monitoramento-nginx/
├── monitor_nginx.sh          # Script de monitoramento
└── nginx-monitor-logs/       # Diretório contendo os arquivos de log
    ├── online.log            # Log para quando o Nginx está online
    ├── offline.log           # Log para quando o Nginx está offline
    └── cron.log              # Log das execuções do cron job
Comandos Necessários
Para rodar o script manualmente:

bash
Copiar código
./monitor_nginx.sh
Verificar status do serviço manualmente: O script executa o comando systemctl status nginx para checar se o Nginx está ativo. Caso contrário, registra a informação nos logs.

Testando o Ambiente
Para testar se o monitoramento está funcionando corretamente:

Certifique-se de que o serviço Nginx está rodando:

bash
Copiar código
systemctl start nginx
Verifique a criação dos logs após a execução do cron job (a cada 5 minutos). Os logs estarão localizados em nginx-monitor-logs/.

Para simular o Nginx offline:

Pare o serviço Nginx:
bash
Copiar código
systemctl stop nginx
Aguarde 5 minutos e verifique o arquivo offline.log para a entrada de registro do status offline.
