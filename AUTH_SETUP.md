# Configuração obrigatória do cadastro e login (Supabase)

O código usa Supabase Auth. O cadastro não cria uma conta local: o usuário é criado no projeto Supabase configurado nas variáveis `VITE_SUPABASE_URL` e `VITE_SUPABASE_ANON_KEY`.

## 1. Variáveis de ambiente

Na raiz do projeto, crie `.env.local` (local) ou configure as mesmas variáveis no painel do Netlify:

```env
VITE_SUPABASE_URL=https://SEU-PROJETO.supabase.co
VITE_SUPABASE_ANON_KEY=SUA_CHAVE_PUBLICA_ANON_OU_PUBLISHABLE
```

Depois de alterar variáveis no Netlify, faça um novo deploy. No desenvolvimento local, reinicie `npm run dev`.

## 2. URLs de autenticação

No Supabase, abra **Authentication → URL Configuration**:

- **Site URL**: endereço principal publicado, por exemplo `https://seu-site.netlify.app`.
- **Redirect URLs**: adicione `https://seu-site.netlify.app/conta` e `http://localhost:5173/conta` para testes locais.
- Adicione também o domínio personalizado, se houver.

## 3. Confirmação por e-mail

Em **Authentication → Providers → Email**, confira se o provedor Email está habilitado.

Se **Confirm email** estiver ativado, a pessoa precisa abrir o link recebido antes de conseguir entrar. A tela da Vitriny informa isso e permite reenviar a confirmação. Confira Spam/Lixo eletrônico.

O envio padrão de e-mails do Supabase tem limites. Para produção, configure SMTP próprio em **Authentication → SMTP Settings**; sem isso, e-mails podem atrasar ou não chegar.

Se quiser permitir login imediatamente sem confirmação, é possível desativar **Confirm email** no Supabase, mas isso reduz a verificação de propriedade do e-mail. Para uma plataforma pública, é melhor manter a confirmação e configurar SMTP.

## 4. Banco de dados

Execute `supabase/schema.sql` uma vez no **SQL Editor** do projeto Supabase. O script cria `profiles` e o gatilho que gera o perfil após a criação do usuário no Auth, além das tabelas e políticas RLS.

Se o SQL já foi executado anteriormente, não execute o arquivo inteiro novamente sem revisar: ele contém criação de tipos e políticas que podem já existir.

## 5. Teste

1. Abra o site publicado.
2. Vá em **Entrar → Criar conta gratuita**.
3. Cadastre nome, e-mail e senha com pelo menos 8 caracteres.
4. Se a confirmação estiver ativada, abra o e-mail e clique no link.
5. Volte ao site e entre com o mesmo e-mail e senha.
6. No Supabase, confirme em **Authentication → Users** se a conta aparece.

Se a conta aparece em Authentication → Users, mas não entra, confira a confirmação de e-mail e as Redirect URLs. Se não aparece, confira as variáveis de ambiente e o projeto Supabase conectado.
