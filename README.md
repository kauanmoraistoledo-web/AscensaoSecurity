ASCSECURITY
AscensaoSecurity
Plugin de segurança para servidores Paper/Spigot com foco em proteção de administradores e controles de ações críticas. O objetivo principal é garantir que apenas o proprietário principal do servidor tenha controle total sobre as configurações e ações sensíveis do sistema.

Visão geral
O AscensaoSecurity foi desenvolvido para:

limitar ações administrativas sensíveis;
impedir uso indevido de comandos de segurança;
registrar tentativas e ações relevantes em logs locais;
preservar backups automáticos da configuração principal;
oferecer integrações opcionais com ferramentas externas sem tornar o plugin dependente delas.
Funcionalidades principais
controle de acesso restrito ao plugin e comandos de administração;
bloqueio configurável de ações como teleporte, inventário, itens, comandos e interações com containers;
sistema de logs com armazenamento em arquivo local;
backup automático e restauração de configuração;
monitoramento de ações críticas e tentativas de uso indevido;
integração opcional com:
PlaceholderAPI
Vault
WorldGuard
webhook genérico
API pública para outros plugins
Requisitos
Java 21
Paper 1.21.1+
Gradle 9.x
Instalação
Compile o plugin:
./gradlew build

Copie o arquivo gerado em build/libs para a pasta plugins do servidor.
Reinicie o servidor.
Configure o server-owner-uuid no arquivo config.yml.
Configuração
O arquivo principal de configuração fica em:

server-owner-uuid: ""
protected-admins: []
admins:
  default:
    enabled: true
    allowed-actions: []
    blocked-actions: []
    allowed-commands: []
    blocked-commands: []
    blocked-command-args: {}
    blocked-worlds: []
    allowed-worlds: []

log-retention-days: 30
security-center:
  enabled: true
  critical-actions:
    - ban
    - kick
    - op
    - deop
    - clear-inventory
    - teleport-other
    - give-item
    - set-time

integrations:
  placeholderapi:
    enabled: false
  vault:
    enabled: false
  worldguard:
    enabled: false
  webhook:
    enabled: false
    url: ""
  admin-api:
    enabled: true
    event-bus: true

Importante
server-owner-uuid deve conter o UUID do único dono autorizado do servidor.
O plugin foi projetado para permitir apenas esse identificador como administrador principal.
Integrações externas são opcionais e não afetam o funcionamento do plugin quando indisponíveis.
Modelo de segurança
A lógica de segurança considera que:

existe apenas um administrador principal válido;
UUID é o identificador confiável;
qualquer tentativa de uso indevido de comandos de segurança é registrada;
ações sensíveis são detectadas e bloqueadas quando não autorizadas;
backups são salvos automaticamente para preservar a integridade da configuração.
Logs e auditoria
O plugin registra ações em logs.yml dentro da pasta do plugin. Os registros incluem:

UUID e nome do administrador;
ação executada;
comando ou operação envolvida;
resultado;
local e timestamp.
Integrações opcionais
PlaceholderAPI
Ativa a detecção de presença do plugin sem exigir dependência obrigatória.

Vault
Usado apenas quando disponível, sem quebrar o funcionamento em ambientes sem esse plugin.

WorldGuard
Verifica a presença do sistema e pode ser usado por integrações futuras sem bloquear a execução do plugin.

Webhook
A configuração fica em integrations.webhook.url.

Se configurado, o plugin envia alertas de ações externas para o endpoint definido.

API pública
Outros plugins podem disparar eventos administrativos via API do AdminSecurityManager, por exemplo:

AdminSecurityManager manager = ...;
manager.dispatchExternalAdminAction(
    "MyPlugin",
    "kick",
    "Alice",
    "Bob",
    "exploit suspected"
);

Segurança operacional
O plugin usa softdepend para evitar falhas quando módulos externos não forem instalados.
O processo de backup impede que a configuração principal fique sem proteção.
Tentativas de acesso não autorizadas são bloqueadas e armazenadas em log.
O sistema evita dependências rígidas que comprometam a disponibilidade do servidor.
Desenvolvimento
Para compilar e validar:

./gradlew test

Licença
Este projeto foi desenvolvido para uso interno e operacional em ambiente de servidor. Ajustes e extensões podem ser feitos conforme a necessidade da infraestrutura.
