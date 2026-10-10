# Arquitetura de referência

## Limites de domínio

| Domínio | Responsabilidade | Vínculos principais |
| --- | --- | --- |
| Identidade | Igrejas, usuários e papéis | Participação por igreja e por ministério |
| Organização | Ministérios e organograma | Liderança, equipes, unidades e hierarquia |
| Execução | Metas, tarefas, demandas e projetos | Responsáveis, prazos e origem da demanda |
| Finanças | Caixa, orçamento, aprovação e pagamento | Setor, projeto, conta, período e comprovante |
| Conteúdo | Ministrações, séries, conferências e campanhas | Pregador, projeto e eventos |
| Agenda | Ocorrências e recorrências | Registro de origem e participantes |
| Comunicação | Conversas, mensagens e relatórios | Participantes e versões de relatório |
| Plataforma | Planos, assinaturas e auditoria | Tenant, limites e histórico |

## Relações estruturais

Uma igreja possui muitas unidades, ministérios e participações. Um usuário possui participações em uma ou mais igrejas. Um ministério pode ter um ministério pai da mesma igreja. Cada projeto pode envolver vários ministérios. Cada despesa pode ter um projeto e um setor sem duplicar lançamento. Uma conversa possui participantes autorizados e mensagens. Um relatório possui versões, setor, autor e revisor.

## Matriz mínima de acesso

| Recurso | Pastor | Tesouraria | Líder | Colaborador |
| --- | --- | --- | --- | --- |
| Painel | Escopo da igreja | Financeiro autorizado | Próprio setor | Próprias entregas |
| Orçamento | Definir e aprovar | Operar conforme permissão | Consultar verba do setor | Sem acesso padrão |
| Solicitação de despesa | Solicitar/aprovar, sem autoaprovação | Operar conforme atribuição | Solicitar no setor | Conforme delegação |
| Demandas | Igreja autorizada | Demandas atribuídas | Próprio setor | Atribuídas |
| Chat | Conversas das quais participa | Conversas das quais participa | Conversas das quais participa | Conversas das quais participa |
| Relatórios | Revisar escopo autorizado | Financeiros autorizados | Enviar do setor | Conforme delegação |

Não incluir acesso implícito a conversas privadas pela condição de pastor ou administrador.

## Persistência e consistência

Toda entidade de negócio carrega tenant_id. Vínculos devem impor consistência entre tenant e entidades relacionadas. O backend determina o contexto pela sessão, não por parâmetro livre. Política no banco complementa a autorização da aplicação. Aprovação/pagamento bloqueiam ou serializam registros relevantes e recalculam disponibilidade na transação. Use idempotency_key única por tenant e operação quando houver reenvio possível.

Eventos do calendário mantêm referência única à origem. Notificações são produzidas após sucesso da transação, com entrega idempotente. Histórico de relatórios e auditoria não sofre alteração retroativa. Anexos são privados e reutilizam o mesmo escopo de autorização da entidade de origem.

## Ordem de execução

Implementar primeiro identidade e isolamento. Depois setores, demandas e agenda; em seguida finanças e relatórios; por fim comunicação em tempo real e assinatura. Cada etapa deve possuir seed fictício, testes de acesso e documentação de configuração.

## Estado deste pacote

Implementado: especificação e protótipo de interface com persistência local.

Pendente: todos os serviços de backend e requisitos operacionais do SaaS descritos no prompt. A arquitetura não constitui evidência de que o produto foi implantado.
