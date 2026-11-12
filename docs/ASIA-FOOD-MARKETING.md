# Asia Food Marketing — arquitetura inicial

## Objetivo

Transformar este fork do Habblaud em uma central privada de operações de marketing da Asia Food, mantendo o escritório virtual, o monitoramento real de sessões Codex/Claude, a linha do tempo e os recursos originais.

Cliente único: Asia Food Distribuidora Oriental LTDA.
Site: https://www.asiafood.com.br
Feed oficial de produtos: https://www.asiafood.com.br/google-merchant.xml

## Princípios obrigatórios

1. Implementar os recursos da agência como módulo isolado; não reescrever o Habblaud nem remover funcionalidades existentes.
2. Preservar a branch `main`; todo desenvolvimento desta adaptação ocorre em `asiafood-marketing`.
3. Não inventar produtos, SKUs, preços, estoque, promoções, agentes ou atividades.
4. O XML Google Merchant é a fonte de verdade para catálogo e preço. Registrar a data/hora da última sincronização e erros de importação.
5. Nenhum conteúdo pode ser publicado automaticamente. Toda publicação exige aprovação humana explícita e registrada.
6. Não guardar credenciais de Instagram, tokens ou segredos no Git, no frontend, em logs ou em arquivos de exemplo com valores reais.
7. Não simular personagens ou sessões no Habblaud World. O escritório continua mostrando somente atividade real capturada pelo Habblaud.
8. Preservar a licença MIT e os avisos de copyright do projeto original.

## Arquitetura proposta

- **Interface:** aproveitar o frontend TypeScript/Vite existente e adicionar uma área/tela própria “Asia Food Marketing”. Não substituir a interface do escritório por HTML puro nem iniciar uma reescrita.
- **Servidor:** Node.js, integrado ao servidor HTTP já existente por rotas e serviços isolados em um módulo `marketing`.
- **Persistência:** SQLite local para catálogo sincronizado, campanhas, rascunhos, versões, aprovações, histórico de publicação e auditoria.
- **Agentes:** perfis/instruções reutilizáveis para sessões reais do Codex. Gael é o coordenador lógico; os demais papéis são especialistas acionados por tarefas reais, não personagens fictícios inseridos no mundo.
- **IA local:** preparar uma interface de provedor para futura integração com Ollama, sem tornar Ollama requisito da primeira entrega.
- **Criação visual:** separar geração/edição da imagem e composição final. Textos, preços, logo e informações comerciais devem ser aplicados em camada determinística para evitar erros. O logo oficial deve ser usado sem deformação.
- **Instagram:** integração futura com Instagram Graph API para conta profissional autorizada, por adaptador isolado. A publicação só pode ser disparada pelo fluxo de aprovação.

## Papéis de trabalho

- **Gael — Coordenação:** recebe o briefing, divide tarefas, acompanha entregas e consolida a proposta. Não aprova nem publica em nome do responsável humano.
- **Produtos — Catálogo:** consulta dados sincronizados do Merchant XML e aponta produtos com dados completos.
- **Marketing — Campanhas:** propõe calendário, público, objetivo e oferta, sem inventar condições comerciais.
- **Conteúdo — Redação:** cria legendas, chamadas, hashtags e variações, sempre vinculadas aos produtos/campanhas de origem.
- **Design — Criação:** prepara briefing visual e peças; preserva identidade e logo da Asia Food.
- **Publicação — Operação:** valida formato e prepara a publicação. Não pode publicar sem aprovação válida.

Esses papéis são perfis de tarefa. Só devem aparecer como agentes ativos no escritório quando houver sessões reais correspondentes.

## Fluxo de conteúdo e aprovação

1. Sincronizar o XML e validar os itens importados.
2. Selecionar produto(s) com identificador, nome, imagem, preço e URL confirmados no feed.
3. Criar campanha e rascunho com referências aos dados de origem.
4. Gerar texto e peça visual; armazenar versões e registrar o modelo/provedor utilizado.
5. Executar validações automáticas (produto existente, preço atual, campos obrigatórios, dimensões e política de publicação).
6. Criar solicitação de aprovação com prévia completa, canal, legenda, mídia e horário pretendido.
7. Exigir ação humana explícita: aprovar ou rejeitar. Qualquer alteração material após aprovação invalida a aprovação anterior.
8. Somente após aprovação válida, permitir o envio ao Instagram.
9. Registrar resposta da plataforma, identificador da publicação, horário e eventuais erros.

A aprovação deve ser vinculada ao hash da versão exata do texto e da mídia aprovados. Alterações posteriores exigem nova aprovação.

## Modelo de dados inicial

- `products`: itens importados do Merchant XML, com ID externo, título, descrição, preço, moeda, disponibilidade, imagem, URL e data de sincronização.
- `sync_runs`: execução, resultado, contagens e erros de cada leitura do feed.
- `campaigns`: objetivo, público, período, status e responsável.
- `content_drafts`: campanha, produtos referenciados, canal, legenda, mídia e status.
- `content_versions`: versões imutáveis do texto/mídia e seus hashes.
- `approvals`: decisão humana, usuário/responsável, data, hash aprovado e comentário.
- `publication_jobs`: canal, agendamento, estado e tentativas.
- `publication_logs`: respostas da API e identificadores externos, sem tokens ou segredos.

## Entregas por etapas

### Etapa 1 — Base segura
- Criar módulo isolado e configuração.
- Definir SQLite, migrações e testes.
- Implementar leitura/sincronização do Merchant XML sem escrita no site.
- Criar tela de catálogo com data da última sincronização e origem dos dados.

### Etapa 2 — Produção de conteúdo
- Criar campanhas e rascunhos.
- Vincular cada peça aos produtos e preços de origem.
- Implementar histórico e versões.

### Etapa 3 — Aprovação humana
- Criar fila de revisão com prévia de texto e mídia.
- Aprovar/rejeitar com trilha de auditoria.
- Invalidar aprovação quando o conteúdo mudar.
- Manter publicação desabilitada até a conclusão dos testes.

### Etapa 4 — Instagram
- Configurar conta profissional e permissões da Meta.
- Implementar adaptador da Graph API e armazenamento seguro de tokens.
- Habilitar publicação exclusivamente após aprovação válida.
- Testar primeiro em ambiente controlado.

### Etapa 5 — Agentes e automação
- Criar perfis de tarefa para Gael e especialistas.
- Integrar Codex como executor inicial.
- Avaliar Ollama local como provedor complementar.
- Manter cada ação externa sujeita a permissões e aprovação.

## Fora do escopo inicial

- Publicação automática sem aprovação.
- Alterar preços, estoque ou cadastro no site.
- Reescrever o Habblaud inteiro.
- Substituir o Codex antes de validar o fluxo atual.
- Enviar dados fictícios ao Habblaud World.
