# Atendechat

Atendechat é uma plataforma omnichannel para atendimento via WhatsApp que reúne backend, frontend e ferramentas de automação para facilitar a operação de suporte de múltiplas empresas.

## Funcionalidades principais
- **Gestão de tickets e contatos**: organiza tickets por filas, tags, notas e agendas, com histórico completo de mensagens e arquivos compartilhados.
- **Integração WhatsApp multiempresa**: administra sessões independentes por empresa usando Baileys, incluindo reconexão automática, leitura de QR Codes e transferência inteligente de tickets.
- **Automação operacional**: filas Bull com Redis para envio/processamento assíncrono, regras de transferência automática, campanhas, disparos em massa, respostas rápidas e prompts configuráveis.
- **Gestão administrativa**: dashboards, planos, assinaturas, faturas, anúncios, integrações de fila, rotinas de help center e recuperação de senha.
- **Experiência do agente**: aplicação web React com notificações em tempo real via Socket.IO, biblioteca de mensagens rápidas, gravação de áudio, anexos e relatórios.

## Arquitetura
### Backend (`home/deploy/mandivia/backend`)
- API REST em **Node.js + TypeScript** com Express, estruturada em controllers, services e filas, compilada com `tsc` e observabilidade via Pino e Sentry.
- Persistência com **Sequelize** (PostgreSQL por padrão) e suporte a seeds/migrations; filas com **Bull** em Redis; agendamentos com `node-cron`; integrações com serviços como OpenAI, Gerencianet PIX, e envio de e-mails.
- Arquivo `.env.example` documenta variáveis de ambiente obrigatórias como URLs, credenciais de banco, Redis e limites de uso.

### Frontend (`home/deploy/mandivia/frontend`)
- SPA em **React 16** com Material-UI, React Query, React Router e Socket.IO Client para tempo real.
- Configuração via `.env.exemple` permitindo apontar a URL do backend e definir regras automáticas de fechamento de tickets.

### Automação e deploy (`home/atendechat-instalador`)
- Scripts shell (`install_primaria`, `install_instancia`, `rebuild_front`) automatizam provisionamento de servidores, instalação de dependências de sistema, build/start do backend e frontend, além de integrações com nginx e certbot.

## Tecnologias utilizadas
- **Backend**: Node.js, TypeScript, Express, Sequelize, PostgreSQL, Redis, Bull, Socket.IO, Baileys, Sentry, OpenAI.
- **Frontend**: React, Material-UI, React Query, Axios, Styled Components, React Router, React Big Calendar.
- **Infraestrutura**: Docker não incluído; scripts shell para provisionamento, FFmpeg para manipulação de mídia, Nodemailer para e-mail e integrações PIX.

## Como executar o projeto localmente
### Pré-requisitos
- Node.js 16+ e npm.
- PostgreSQL 12+ com banco e usuário dedicados.
- Redis em execução.
- FFmpeg instalado (necessário para conversão de mídia).

### Passo a passo backend
1. Copie o arquivo de exemplo e ajuste variáveis:
   ```bash
   cd home/deploy/mandivia/backend
   cp .env.example .env
   # edite .env conforme seu ambiente
   ```
2. Instale dependências e gere o build TypeScript:
   ```bash
   npm install
   npm run build
   ```
3. Aplique migrações e (opcionalmente) seeds:
   ```bash
   npm run db:migrate
   npm run db:seed
   ```
4. Inicie o servidor em modo desenvolvimento ou produção:
   ```bash
   npm run dev   # hot reload
   # ou
   npm start     # usa build dist/
   ```

### Passo a passo frontend
1. Copie o arquivo de ambiente de exemplo e configure a URL da API:
   ```bash
   cd home/deploy/mandivia/frontend
   cp .env.exemple .env
   # ajuste REACT_APP_BACKEND_URL
   ```
2. Instale dependências e execute:
   ```bash
   npm install
   npm run dev    # desenvolvimento com react-scripts
   # ou
   npm run build  # build de produção
   ```

### Execução de testes e lint
- Backend: `npm test` para Jest e `npm run lint` para ESLint.
- Frontend: `npm test` para a suíte do React Testing Library.

## Automação completa (opcional)
Para implantações completas em servidores Linux, utilize os scripts do diretório `home/atendechat-instalador`. O script `install_primaria` provê instalação primária (dependências do sistema, clone do repositório, build e configuração de nginx/certbot). Consulte os scripts e adapte variáveis em `home/atendechat-instalador/variables` conforme o ambiente alvo.

## Estrutura do repositório
```
├── README.md
├── home
│   ├── atendechat-instalador   # Scripts de provisionamento/automação
│   └── deploy
│       └── mandivia
│           ├── backend         # API Node.js/TypeScript
│           └── frontend        # SPA React
```

## Licença
Este fork não inclui informações de licença adicionais além das presentes nos subprojetos.
