# Relatório de Bugs Identificados

## Resumo
- Lista de problemas encontrados durante a análise do repositório Atendechat.
- Cada item inclui impacto observado e recomendação de correção.

## Problemas

### 1. Filtro por data ignora demais critérios em listagem de tickets
- **Local**: `backend/src/services/TicketServices/ListTicketsService.ts` e `ListTicketsServiceKanban.ts`.
- **Descrição**: quando `date` ou `updatedAt` são informados, o serviço redefine `whereCondition` apenas com o intervalo de datas, descartando filtros anteriores (status, filas, empresa etc.). Resultado: respostas trazem tickets de outras filas/empresas e ignoram paginação contextual.
- **Correção sugerida**: mesclar o filtro de data ao `whereCondition` existente (ex.: `whereCondition = { ...whereCondition, createdAt: { ... } }`) em vez de sobrescrevê-lo.

### 2. Busca textual não encontra mensagens
- **Local**: mesmos serviços da listagem de tickets.
- **Descrição**: o alias usado na cláusula `$message.body$` não corresponde ao alias `messages` definido no include Sequelize, quebrando o filtro por texto em mensagens.
- **Correção sugerida**: ajustar para `$messages.body$` (ou renomear o include) garantindo consistência.

### 3. Endpoint `/sessions/me` acessa token antes da validação
- **Local**: `backend/src/controllers/SessionController.ts`.
- **Descrição**: a função chama `FindUserFromToken(token)` antes de verificar se o cookie `jrt` existe; quando ausente, o serviço recebe `undefined` e falha com erro inesperado em vez de `ERR_SESSION_EXPIRED`.
- **Correção sugerida**: validar `token` antes de chamar `FindUserFromToken` e retornar o erro esperado quando vazio.

