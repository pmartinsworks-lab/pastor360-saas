# Pastor360

Pacote de definição de produto e protótipo para um SaaS de gestão pastoral.

## Conteúdo

- `PROMPT_COMPLETO.md`: prompt pronto para orientar uma ferramenta de desenvolvimento.
- `index.html`: protótipo interativo sem dependências, com dados fictícios.
- `docs/ARQUITETURA.md`: domínios, vínculos e decisões para implementação.

## Abrir a demonstração

Abra `index.html` no navegador. Para servir localmente, na pasta do projeto execute `python3 -m http.server 8080` e abra `http://localhost:8080`.

A demonstração permite navegar pelos módulos, cadastrar demandas, resolver pendências, visualizar distribuição orçamentária, registrar mensagens locais e consultar planejamento/calendário. Os dados são armazenados apenas no navegador. O botão “Restaurar demonstração” apaga exclusivamente os dados locais deste protótipo.

**Esta versão não é um SaaS de produção.** Não possui autenticação, servidor, isolamento entre igrejas, pagamentos, chat entre usuários, uploads, exportação de relatórios ou banco remoto. Não insira dados reais. O financeiro apresenta orçamento demonstrativo e não executa movimentações.

## Implementação

Use o prompt completo como briefing técnico. A arquitetura define requisitos, não recursos já implementados. Valide versões e serviços antes de instalar. Conclua os testes de isolamento, autorização e transações antes de utilizar com dados reais.

## GitHub

Nome proposto: `pastor360-saas`. Criar em repositório privado autorizado. Este pacote não inclui credenciais. A publicação precisa de um destino acessível; não utilize um repositório de outro projeto sem indicação do proprietário.
