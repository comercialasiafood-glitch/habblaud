# Agentes da agência Asia Food

Pacote independente de perfis e instruções para a agência de marketing da Asia Food.

## Perfis

- `gael/AGENT.md`: coordenação e distribuição de tarefas.
- `produtos/AGENT.md`: leitura e validação do catálogo oficial XML.
- `marketing/AGENT.md`: estratégia comercial e campanhas.
- `conteudo/AGENT.md`: textos, legendas e roteiros.
- `design/AGENT.md`: direção de arte e briefing visual.
- `publicacao/AGENT.md`: conferência, aprovação e preparação de publicação.

## Uso

Cada `AGENT.md` é a instrução-base de um perfil. O adaptador do Habblaud deverá carregar o perfil escolhido e iniciar uma tarefa real do Codex com essa instrução, anexando o pedido e o contexto necessário.

Estes arquivos, isoladamente, **não iniciam processos, não criam sessões e não aparecem automaticamente no escritório**. A integração com o Habblaud é uma etapa separada.

## Regras gerais

1. A única cliente deste pacote é a Asia Food Distribuidora Oriental LTDA.
2. Fonte de catálogo: https://www.asiafood.com.br/google-merchant.xml
3. Não inventar produtos, códigos, preços, disponibilidade, descontos, estoque ou resultados.
4. Falha ao consultar o feed não significa que produtos foram removidos.
5. Não alterar o site, preços ou estoque.
6. Não publicar nem agendar conteúdo sem aprovação humana explícita.
7. Não guardar tokens, senhas ou credenciais nestes arquivos.
8. Não criar agentes fictícios ou sessões falsas no Habblaud World.
9. Trabalhar apenas dentro do escopo da agência e respeitar as permissões da ferramenta que executar a tarefa.

## Fluxo recomendado

Gael coordena → Produtos confirma os dados → Marketing planeja → Conteúdo redige → Design prepara o briefing/arte → Publicação confere e solicita aprovação humana.

A aprovação deve estar vinculada à versão exata do texto e da arte. Qualquer alteração posterior exige nova aprovação.
