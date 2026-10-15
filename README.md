# Phoenix Insurance — D&O B2B Premium (15/10/2026)

## O que foi entregue

**Landing page premium (index.html):** aplicação corporativa de tela cheia com símbolo ORIGINAL fornecido pelo usuário (`assets/phoenix-symbol.png`), visual navy/azul Phoenix, design desktop/mobile, preenchimento em três etapas, validação de CNPJ e WhatsApp, data de vencimento condicional ao seguro D&O e consentimento LGPD. Não há qualquer referência ao sorteio ou passeio de helicóptero.

**PDF Phoenix:** ao concluir com sucesso o envio, o interessado pode tocar em **Gerar meu briefing em PDF**. O PDF é gerado diretamente no navegador, com logo Phoenix, layout A4, quatro seções, data de renovação e próximo passo comercial. Código 100% local em `js/pdf-report.js`; dispensa CDN, pacote npm, chave secreta ou serviço externo de PDF. *É um briefing com as informações declaradas, não uma análise automatizada de riscos ou proposta de seguro.*

**Módulo executivo `/admin.html`:** portal complementar de oportunidades D&O. Exige login por e-mail e senha do **Supabase Auth** e verifica perfil `public.phoenix_profiles.active`. Filtra a campanha `B2B | Patrimônio de Quem Decide` em `public.leads`. Pesquisa por nome/empresa/CNPJ, apresenta contagem de leads, leads com D&O, renovações em até 120 dias e pedidos de reunião, mostra detalhes e gera **PDF por lead em um clique**. Nunca armazena senhas nem chaves secretas no HTML. Utiliza chave PUBLICÁVEL do Supabase, própria para navegadores, com acesso efetivo condicionado às políticas de banco.

## Publicação na Vercel

1. Crie um novo projeto Vercel (ou faça deploy manual sem sobrescrever o ADM original).
2. Envie o **conteúdo** deste ZIP mantendo `index.html`, `admin.html`, `assets/`, `js/`, `vercel.json` e `robots.txt` na raiz do projeto. Se usar GitHub, extraia os arquivos no repositório e importe o projeto na Vercel. Não é necessário rodar build; Framework Preset: **Other**.
3. Após a publicação, abra a URL principal e teste os três passos, validação e PDF. O login interno ficará em `https://SEU-PROJETO.vercel.app/admin.html`.
4. Antes de divulgar, faça um envio autorizado real de teste e confirme que ele aparece em `public.leads` e no módulo `/admin.html`.
5. Após confirmar a URL de produção, gere o QR Code com a **URL definitiva**; não use a URL da prévia.

## Integração Supabase e ADM

- Supabase: projeto `cxyxrhmpjehrqhwihxul`.
- Recepção: a Edge Function já existente `phoenix-b2b-do` em `https://cxyxrhmpjehrqhwihxul.supabase.co/functions/v1/phoenix-b2b-do`.
- Destino: **tabela existente** `public.leads`, sem adicionar coluna/tabela. Campanha `B2B | Patrimônio de Quem Decide`; origem `Landing Page Phoenix D&O`.
- O campo `interesse` recebe todas as perguntas/respostas como ficha textual, incluindo apólice D&O vigente, vencimento e reunião. O script `js/pdf-report.js` sabe ler esse conteúdo e montar o PDF no módulo interno.
- O `/admin.html` é um **módulo complementar funcionalmente integrado aos leads**. Não é uma modificação do botão já presente no ADM original. Para que o próprio botão **Gerar PDF** do ADM original use este modelo, será necessário modificar o código do ADM (por exemplo, incorporar `js/pdf-report.js` e chamá-lo com os dados do lead), o que **não foi feito**, pois não há permissão de edição daquele aplicativo.

## Atenção: segurança e privacidade (importante antes de publicar)

Auditoria em 15/10/2026 identificou na tabela existente `public.leads` políticas de leitura pública (`anon`) amplas, que podem permitir que pessoas sem login consultem dados por fora deste módulo. **A tela de login do admin não elimina essa exposição no banco de dados**. Revisar e corrigir as políticas RLS é essencial antes de captar dados pessoais reais nesta campanha. Remover políticas antigas pode afetar aplicações antigas; testar dependências do ADM e landing pages existentes antes de corrigir. Nenhuma política/tabela foi alterada por este pacote.

O endpoint público de envio tem honeypot/validação básica, porém a proteção adicional contra automação abusiva/rate limiting ainda não foi auditada. O formulário oferece texto básico de consentimento; vincular à política de privacidade oficial da Phoenix quando disponível.

## Estado de implementação e testes

- HTML/JavaScript testado localmente em Chromium com desktop e mobile: passos, campos e surgimento condicional do vencimento.
- Geração PDF: testada localmente; PDF A4 com logo incorporado, campos e texto selecionável.
- Projeto atualizado **não publicado na Vercel** nesta entrega, portanto não há URL/QR Code de produção.
- Nenhum envio de teste com dados pessoais foi inserido no Supabase nesta atualização.
