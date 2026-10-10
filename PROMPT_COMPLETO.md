# Prompt completo — Pastor360: gestão pastoral e ministerial SaaS

Copie o conteúdo a partir de “Sua missão” para sua ferramenta de desenvolvimento com IA. Este documento é uma especificação de implementação; não certifica que as funcionalidades já estejam implementadas.

## Sua missão

Atue como especialista em produto, arquitetura de software, desenvolvimento full stack e experiência do usuário. Desenvolva o **Pastor360**, aplicativo SaaS em português brasileiro para o pastor administrar diariamente sua igreja, acompanhar líderes e integrar finanças, ministérios, projetos, ministrações e comunicação. Entregue código executável, banco de dados, testes, documentação e projeto versionado no GitHub. Não entregue somente telas ilustrativas nem declare recursos concluídos sem implementá-los e verificá-los.

Comece lendo README, instruções do repositório e código existente. Preserve arquivos e funcionalidades fora do escopo. Se o repositório estiver vazio, inicialize o projeto. Se alguma integração depender de credenciais externas, forneça configuração e modo demonstrativo explícito, sem simular sucesso. Não invente credenciais, dados de igreja, pastores ou integrações.

## 1. Público, proposta e rotina diária

O pastor deve abrir o painel e responder em menos de um minuto: o que exige ação hoje, quais líderes precisam de retorno, quais relatórios estão atrasados, quanto cada setor tem disponível e quais eventos se aproximam.

Fluxo principal: cadastrar igreja → configurar setores e lideranças → cadastrar metas e orçamento → criar calendário → delegar tarefas → acompanhar execução → receber relatórios → aprovar solicitações → registrar resoluções.

Uma igreja é um tenant. Permita congregações vinculadas à mesma igreja, mas isole completamente igrejas distintas. Um usuário pode participar de mais de uma igreja e alternar o contexto autorizado. Todas as consultas, alterações, arquivos, notificações e mensagens devem respeitar esse contexto.

## 2. Experiência visual e navegação

Interface moderna, sóbria e acolhedora, com azul profundo, branco e detalhes dourados. Sidebar no computador, navegação adaptada ao celular e atalhos para a rotina diária. Mostrar nome e logotipo da igreja, igreja ativa, notificações e perfil do usuário.

Menu: Visão Pastoral; Administrativo e Financeiro; Ministérios; Planejamento Ministerial; Projetos; Organograma; Ministrações; Conferências e Campanhas; Calendário Geral; Comunicação; Relatórios; Configurações.

Suportar busca, filtros por ministério/responsável/status/período, paginação, estados vazios, carregamento, erros acionáveis, confirmação de exclusões e mensagens claras de salvamento. Nenhum botão sem ação real. Datas no padrão brasileiro, valores em BRL e fuso configurável, inicialmente America/Sao_Paulo.

Usar texto e ícone junto às cores, com contraste acessível, navegação por teclado e rótulos de formulário. Não depender apenas da cor.

## 3. Perfis e permissões

- Administrador da plataforma: gerencia tenants e assinaturas; não recebe acesso automático ao conteúdo pastoral ou financeiro das igrejas.
- Pastor/administrador da igreja: gerencia sua igreja, atribui permissões e acompanha setores.
- Secretaria: cadastros, agenda e documentos autorizados.
- Tesouraria: registros financeiros, conciliação, relatórios e orçamento conforme regras de aprovação.
- Líder de ministério: membros, tarefas, projetos, conversas e relatórios de seu próprio setor; consulta a verba autorizada do setor.
- Colaborador: somente tarefas, eventos e conversas dos quais participa.
- Auditor: leitura dos registros explicitamente autorizados.

Aplicar permissões no backend e no banco, não apenas ocultar menus. Separe permissão para solicitar despesa, aprovar, registrar pagamento e auditar. Quem solicita não pode aprovar a própria solicitação. Não expor finanças ou conversas de outro departamento ao líder sem autorização específica.

## 4. Painel Visão Pastoral

Cards: saldo consolidado, contas a vencer, solicitações financeiras pendentes, demandas urgentes, relatórios atrasados, próximos eventos e projetos em andamento. Mostrar indicador por ministério com acesso às demandas que originaram o sinal.

Seções: “Minha agenda de hoje”, “Precisa da minha decisão”, “Últimos relatórios”, “Últimas conversas”, “Pendências por ministério”, “Projetos e metas” e “Resumo financeiro”. Indicadores devem vir de registros persistidos e informar o período considerado.

## 5. Administrativo e Financeiro

Cadastrar contas financeiras, categorias, centros de custo, entradas, saídas, dízimos/ofertas por categoria, contribuições, despesas recorrentes, vencimentos, comprovantes e conciliação. Dados pessoais de contribuintes devem ter acesso restrito. Não confundir contas financeiras com centros de custo.

Distribuir o orçamento entre Departamento Infantil, Grupo de Homens, Grupo de Mulheres, Departamento Familiar, Casais, Diaconato, Discipulado e Grupo de Jovens. Permitir novos setores e hierarquia configurável; Casais pode pertencer ao Departamento Familiar.

Para cada período/setor, mostrar orçamento previsto, verba aprovada, valor comprometido, despesas pagas e saldo de verba. Separar caixa disponível de verba orçamentária. Exemplo: um setor pode ter orçamento disponível sem caixa suficiente para pagamento.

Regras:
- Valores monetários em centavos inteiros ou NUMERIC decimal; nunca usar ponto flutuante para cálculos contábeis.
- Percentuais de distribuição não ultrapassam 100%; a parcela restante fica identificada como reserva. Valores fixos não podem ultrapassar o total autorizado.
- Transferências entre centros de custo mudam a distribuição orçamentária, não geram receita externa nem aumentam o caixa.
- Transferências entre contas internas não alteram o resultado consolidado.
- Com orçamento definido como verba aprovada, saldo de verba = verba aprovada − despesas comprometidas ainda não pagas − despesas pagas. Ao pagar um compromisso, ele deixa de ser “ainda não pago”, evitando dupla contagem.
- Reembolso e estorno precisam de registros vinculados; lançamento confirmado não é apagado silenciosamente.
- Aprovação não significa pagamento. Usar fluxo solicitada → em análise → aprovada/rejeitada → paga/cancelada, com responsável e histórico.
- Revalidar limites e permissões dentro da transação para impedir aprovações concorrentes acima do saldo.
- Registros confirmados devem ter trilha de auditoria, justificativa de correção e período de competência separado da data de pagamento.

Exportar extrato, fluxo de caixa, previsto versus realizado e prestação de contas por setor em CSV/PDF, obedecendo permissões. Implementar controles idempotentes para pagamentos, importações e webhooks.

## 6. Ministérios e planejamento ministerial

Cada setor possui nome, descrição, liderança, suplente, equipe, objetivos, metas, tarefas, orçamento, agenda, projetos, documentos e relatórios. Perfil do líder exibe responsabilidades, próximas entregas, pendências e última interação autorizada.

Planejamento por ano, trimestre e mês: objetivo, fundamento bíblico opcional, indicador, meta mensurável, responsável, prazo, recursos, dependências e resultado. Permitir duplicar planejamento para outro período sem copiar conclusões como se fossem novas entregas.

Tarefas: título, descrição, setor, responsável, prazo, prioridade, estado, comentários e anexos. Informações de aconselhamento pastoral devem permanecer em módulo de acesso restrito, sem aparecer automaticamente em tarefas comuns ou relatórios gerais.

## 7. Projetos

Cadastrar projetos como conferência de famílias, formação de líderes, discipulado de casais, reforma de cozinha e ação social. Cada projeto tem objetivo, patrocinador interno, líder, setores participantes, cronograma, orçamento, marcos, riscos, tarefas e prestação de contas.

Exibir lista e Kanban: planejado, em execução, bloqueado, concluído, cancelado. Calcular progresso a partir das tarefas válidas, com regra visível. Permitir projetos sem tarefas, sem mostrar percentual inventado. Despesas vinculadas a projeto e setor devem ser contabilizadas uma única vez nos totais consolidados.

## 8. Organograma interligado

Representar pastor titular, auxiliares, secretaria, tesouraria, diaconato e ministérios em árvore navegável, com vista alternativa em lista no celular. Ao clicar em um nó, abrir liderança, equipe, objetivos, orçamento autorizado, demandas, relatórios e agenda.

Prevenir ciclos hierárquicos, referências entre igrejas e vínculos inconsistentes. A estrutura do organograma não concede permissões automaticamente: estas precisam de atribuição explícita.

## 9. Sinais de acompanhamento

Usar os indicadores pedidos:
- **Vermelho — Urgente:** demanda aberta marcada como urgente ou com prazo vencido.
- **Amarelo — Atenção:** demanda aberta sem urgência, inclusive aguardando decisão, informação ou prazo futuro.
- **Verde — Resolvido:** demanda encerrada com resolução registrada, autor e data.

Manter estado de execução separado do indicador: aberta, em andamento, aguardando, resolvida e cancelada. Cancelada deve aparecer como “Cancelada”, com sinal neutro e motivo, nunca “Resolvida”. A prioridade original permanece no histórico após conclusão, mas o indicador atual passa a verde.

Para o setor, vermelho se houver demanda urgente; amarelo se houver demanda aberta sem urgência; verde se houver histórico de demandas e todas as demandas não canceladas estiverem resolvidas. Sem demandas, usar “Sem demandas” com sinal neutro. Sempre explicar a origem do indicador e permitir drill-down. Usuário só recebe a agregação dos dados que pode consultar; o painel pastoral agrega o escopo autorizado do pastor.

Relatório atrasado gera demanda de acompanhamento vinculada e sem duplicação. Mudança de prazo, responsável ou resolução atualiza indicadores e notificações. Não usar o mesmo campo para urgência e resolução.

## 10. Planejamento de ministrações

Planejar mensagens por ano, mês, série e culto. Campos: tema, título, texto bíblico de referência, objetivo, público, pregador, data, horário, duração, esboço, introdução, pontos principais, aplicação, conclusão, apelo opcional, músicas sugeridas e observações.

Estados: rascunho, em revisão, aprovado, agendado, ministrado e cancelado. Esboços privados não são visíveis para todos os participantes do culto. Integração com calendário e aviso de conflito de pregador, horário e local. Não inventar texto bíblico ou atribuir citações sem fonte; distinguir referência, citação e paráfrase.

Planejamento anual exibe séries, temas, responsáveis e lacunas na programação. Permitir versões do esboço e duplicação controlada. Sugestões de IA, se incluídas, são opcionais, identificadas e revisadas pelo usuário antes de publicação.

## 11. Conferências e campanhas

Conferências: tema, objetivo, datas, programação, preletores, equipes, local, capacidade, recursos, orçamento, tarefas, divulgação e avaliação pós-evento. Campanhas: propósito, período, encontros, responsáveis, mensagens, metas e acompanhamento. Inscrições e arrecadação são opcionais e devem ter configuração própria.

Vincular projeto, orçamento e eventos sem replicar gastos ou agendas. Alterações no registro de origem refletem no calendário. Prevenir dupla reserva de local e equipe; permitir exceção somente com justificativa e permissão.

## 12. Calendário geral

Consolidar cultos, ministrações, reuniões, projetos, conferências, campanhas, vencimentos autorizados, entregas de relatórios e tarefas. Vistas diária, semanal, mensal e anual; filtros por setor, responsável, tipo e status.

Suportar recorrência com início/fim, tratamento de exceções e escolha entre alterar uma ocorrência ou toda a série. Persistir instantes com fuso apropriado; distinguir eventos de dia inteiro. Oferecer lembretes e exportação ICS. Integração externa com Google Calendar/WhatsApp só pode ser anunciada como funcional após implementação, configuração e teste.

## 13. Comunicação com líderes

Chat interno entre pastor e líderes, com conversas individuais e por ministério, histórico persistido, anexos privados, participantes, última mensagem, mensagens não lidas e confirmação de leitura. Autorizar tanto consultas quanto publicação/assinatura em tempo real.

Mostrar “última conversa” com data e trecho apenas para participante autorizado. Sem mensagem, mostrar “Sem conversa registrada”. Não inventar presença online ou confirmação de leitura. Permitir transformar uma mensagem em tarefa/solicitação mediante confirmação explícita e mantendo vínculo de origem.

Prestação de relatórios: formulário periódico com atividades realizadas, pessoas atendidas em dados agregados, resultados, dificuldades, necessidades, execução financeira autorizada e planejamento seguinte. Fluxo rascunho → enviado → em revisão → devolvido/aprovado. Revisões preservam versões e comentários; após aprovação, exigir nova versão para alterações.

O chat não substitui o relatório estruturado. Permitir enviar o link do relatório na conversa e acompanhar prazo de entrega. Atrasos e devoluções aparecem no painel do pastor e do líder.

## 14. SaaS e operação

Onboarding de igreja, convites com validade, autenticação, recuperação de acesso, configurações, limites por plano e gestão de assinatura. Inicialmente utilizar plano interno de avaliação; não cobrar sem configurar provedor de pagamento real.

Prever planos configuráveis sem inventar preços. Separar cobrança da plataforma dos lançamentos financeiros da igreja. Webhooks de cobrança devem validar assinatura, impedir reprocessamento e atualizar benefícios de forma consistente. Definir comportamento documentado para inadimplência e cancelamento, com exportação dos dados conforme política definida.

## 15. Arquitetura proposta

Usar TypeScript, React e um framework full stack adequado ao ambiente de implantação; PostgreSQL para persistência, autenticação confiável, storage privado e serviço de tempo real autorizado. Uma opção é Next.js com Supabase, desde que a plataforma de hospedagem e versões sejam compatíveis e verificadas em documentação oficial vigente. Não fixar versões supostamente atuais sem verificá-las.

Organizar por domínio, com componentes reutilizáveis, validação de entrada, camada de autorização, operações financeiras transacionais e migrações versionadas. Não confiar no tenant_id enviado pelo cliente. Resolver tenant e papel a partir da sessão e vínculos persistidos. Todas as tabelas de negócio devem conter tenant_id e políticas de isolamento; usar chaves compostas ou validações equivalentes para prevenir relações entre tenants.

Entidades principais: churches, congregations, profiles, church_memberships, ministry_memberships, ministries, objectives, tasks, demands, projects, project_tasks, financial_accounts, financial_categories, budgets, budget_allocations, expense_requests, financial_entries, internal_transfers, attachments, sermons, sermon_series, conferences, campaigns, events, event_occurrences, conversations, conversation_participants, messages, message_reads, report_templates, ministry_reports, report_versions, notifications, audit_logs, subscription_plans e subscriptions.

Criar dicionário de dados com tipos, constraints, cardinalidades, índices, soft delete quando aplicável e retenção. Auditoria é append-only e registra ator, tenant, operação, entidade, data e alterações relevantes sem incluir senhas, tokens ou conteúdo sensível desnecessário.

## 16. Segurança e privacidade como requisitos de produto

Não usar dados reais de membros nas demonstrações. Limitar acesso a dados de crianças, contribuições individuais, conversas e aconselhamento. Organograma e relatórios públicos não expõem contatos pessoais. Não prometer conformidade jurídica automática; documentar política de acesso, retenção, exportação e exclusão a ser validada pelo responsável da igreja.

Secrets somente no ambiente; nenhum token ou senha no GitHub. Storage privado com URLs temporárias, validação de arquivos, limites de tamanho e auditoria de downloads sensíveis. Rate limiting para autenticação e mensagens. Configurar backups e testar restauração. Administrador da plataforma não pode impersonar pastor sem mecanismo explícito, temporário e auditado.

## 17. Etapas de implementação

1. Fundação: tenant, autenticação, permissões, setores, organograma, configuração e dados fictícios.
2. Rotina pastoral: painel, tarefas, demandas com sinais, planejamento, projetos e calendário.
3. Administração: orçamento, solicitações/aprovações, lançamentos, anexos e relatórios financeiros.
4. Conteúdo ministerial: ministrações, planejamento anual, conferências e campanhas.
5. Comunicação: chat persistente, tempo real autorizado, relatórios periódicos e notificações.
6. Operação SaaS: assinatura, limites, exportação, observabilidade, backups e deploy.

Em cada etapa, entregar uma fatia funcional do banco à interface. Se não for possível concluir tudo na execução, documentar o que funciona, o que é demonstração e o que está pendente. Nunca apresentar um protótipo de frontend como SaaS pronto.

## 18. Critérios de aceite obrigatórios

- Igreja A não consegue ler, alterar, baixar arquivo ou receber mensagem da igreja B, inclusive alterando IDs em requisições.
- Líder acessa seu setor; permissões de tesouraria e pastor são verificadas no servidor.
- Uma demanda vencida aparece vermelha; aberta sem urgência aparece amarela; resolvida aparece verde; setor vazio aparece neutro.
- Orçamento de R$ 10.000,00 dividido em 20%, 15% e 10% apresenta R$ 2.000,00, R$ 1.500,00 e R$ 1.000,00, com reserva de R$ 5.500,00.
- Distribuição acima de 100% é rejeitada. Duas aprovações concorrentes não ultrapassam limites quando essa política estiver ativa.
- Pagar despesa comprometida de R$ 300,00 não reduz o saldo de verba duas vezes. Transferência interna não cria receita.
- Solicitação, aprovação e pagamento possuem responsáveis, datas e histórico; autoaprovação é bloqueada.
- Criar conferência gera vínculo no calendário; editar a origem atualiza o evento; excluir/cancelar mantém tratamento consistente e histórico.
- Mensagem permanece após atualização da página; somente participantes autorizados leem e recebem eventos em tempo real.
- Relatório devolvido gera ação para o líder; aprovação preserva a versão e registra o revisor.
- Organograma rejeita ciclos. Agenda detecta conflitos e recorrência respeita fuso e exceções.
- Interface funciona em celular e computador, com estados vazios/erro e sem dados fictícios misturados à produção.
- Backup restaurado permite recuperar registros de teste, com procedimento documentado.

## 19. Testes e entrega no GitHub

Criar testes unitários para orçamento, indicadores e permissões; testes de integração para transações e isolamento por tenant; testes end-to-end para os fluxos pastor/líder/tesouraria. Incluir testes negativos de acesso e de assinaturas em tempo real. Dados de teste sempre fictícios.

Repositório sugerido: **pastor360-saas**, preferencialmente privado na criação. Usar o proprietário e repositório autorizados pelo usuário; não modificar outros projetos apenas por estarem acessíveis. Se não houver capacidade para criar o repositório, entregar pacote pronto e solicitar apenas o destino necessário. Não afirmar que houve publicação sem retorno confirmado do GitHub.

Entregar README em português, instruções de instalação, .env.example sem segredos, migrações, seed, arquitetura, matriz de permissões, documentação das regras financeiras, testes e pipeline de CI. CI deve executar lint, tipos, testes e build; verificar secrets antes do commit. Não executar migrations destrutivas ou deploy de produção sem autorização apropriada.

No relatório final: link do commit/branch ou PR, recursos implementados, testes realmente executados, configuração necessária, limitações e próximos passos. Não declarar resultados de testes que não foram executados.
