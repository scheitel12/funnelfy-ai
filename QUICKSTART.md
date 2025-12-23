# 🚀 FunnelFy AI - Quick Start

## Configuração Rápida (5 minutos)

### 1. Configure a OpenAI API Key

Edite o arquivo `.env` na raiz do projeto e adicione sua chave da OpenAI:

```bash
OPENAI_API_KEY=sk-sua-chave-aqui
```

**Como obter sua chave:**
- Acesse https://platform.openai.com/api-keys
- Crie uma nova chave
- Cole no arquivo `.env`

### 2. Rode o projeto

```bash
npm run dev
```

Isso iniciará:
- **Backend (NestJS)**: http://localhost:3001/api
- **Frontend (Next.js)**: http://localhost:3000

### 3. Teste o fluxo completo

1. Abra http://localhost:3000
2. Clique em "Começar Grátis"
3. Crie uma conta
4. Clique em "Criar Novo Funil"
5. Digite um prompt, por exemplo:

```
Crie um funil de diagnóstico para psicólogas que trabalham com ansiedade.
O público-alvo são mulheres de 25-45 anos que sentem ansiedade no dia a dia.
O funil deve começar com um quiz de 5 perguntas sobre sintomas de ansiedade,
depois mostrar um resultado personalizado e por fim apresentar uma oferta
de consulta online por R$ 197.
```

6. A IA gerará o funil completo
7. Visualize o funil gerado
8. Clique em "Publicar Funil"
9. Acesse a URL pública gerada

## Arquitetura do Projeto

```
funnelfy-ai/
├── apps/
│   ├── web/          # Next.js (Frontend) - Porta 3000
│   └── api/          # NestJS (Backend) - Porta 3001
└── packages/
    ├── prompts/      # Prompts da OpenAI
    └── schemas/      # Validação com Zod
```

## Endpoints da API

### Autenticação
- `POST /api/auth/register` - Criar conta
- `POST /api/auth/login` - Login

### Funis
- `POST /api/funnels/generate` - Gerar funil com IA (requer auth)
- `GET /api/funnels` - Listar meus funis (requer auth)
- `GET /api/funnels/:id` - Ver detalhes (requer auth)
- `PATCH /api/funnels/:id/publish` - Publicar funil (requer auth)
- `GET /api/public/funnel/:slug` - Ver funil público (sem auth)

## Rotas do Frontend

### Públicas
- `/` - Landing page
- `/login` - Login
- `/register` - Cadastro
- `/f/[slug]` - Funil público

### Privadas (requer login)
- `/dashboard` - Lista de funis
- `/create` - Criar novo funil
- `/dashboard/funnels/[id]` - Detalhes do funil

## Banco de Dados

O projeto usa **Supabase** (PostgreSQL) com:

### Tabela `users`
- `id` - UUID
- `email` - Email único
- `password_hash` - Senha criptografada
- `full_name` - Nome completo
- `created_at` - Data de criação

### Tabela `funnels`
- `id` - UUID
- `user_id` - Referência ao usuário
- `name` - Nome do funil
- `slug` - Slug único para URL pública
- `prompt` - Prompt original enviado
- `funnel_data` - JSON com estrutura do funil
- `is_published` - Se está publicado ou não
- `created_at` - Data de criação

## Segurança

- ✅ Row Level Security (RLS) habilitado
- ✅ Políticas restritivas (users só veem seus dados)
- ✅ Funis públicos só acessíveis quando `is_published = true`
- ✅ Tokens de autenticação em Bearer token
- ✅ Validação de dados com Zod
- ✅ Senhas hasheadas com SHA-256

## Como a IA Funciona

1. Usuário envia um prompt descrevendo o produto
2. Backend recebe e envia para OpenAI (GPT-4)
3. OpenAI retorna um JSON estruturado com o funil
4. Backend valida o JSON com Zod
5. Backend salva no Supabase
6. Frontend renderiza o funil

### Estrutura do JSON do Funil

```json
{
  "funnel": {
    "name": "Nome do Funil",
    "type": "quiz",
    "target_audience": "Descrição do público"
  },
  "steps": [
    {
      "type": "quiz_question",
      "question": "Pergunta?",
      "options": ["Opção 1", "Opção 2"]
    },
    {
      "type": "result",
      "headline": "Resultado",
      "description": "Explicação"
    },
    {
      "type": "sales_page",
      "headline": "Oferta",
      "cta": "Comprar Agora",
      "price": "R$ 97,00"
    }
  ]
}
```

## Comandos Úteis

```bash
# Rodar tudo
npm run dev

# Rodar só o backend
npm run dev:api

# Rodar só o frontend
npm run dev:web

# Build de produção
npm run build

# Listar workspaces
npm ls --workspaces

# Instalar dependência no backend
npm install <package> --workspace=apps/api

# Instalar dependência no frontend
npm install <package> --workspace=apps/web
```

## Troubleshooting

### Erro: "OpenAI API key not configured"
- Adicione `OPENAI_API_KEY` no arquivo `.env`

### Erro: "Supabase environment variables not configured"
- Verifique se `NEXT_PUBLIC_SUPABASE_URL` e `NEXT_PUBLIC_SUPABASE_ANON_KEY` estão no `.env`

### Porta 3001 já está em uso
- Mude a `PORT` no `.env` para outra porta (ex: 3002)
- Atualize também `NEXT_PUBLIC_API_URL` para refletir a nova porta

### Build falha
- Rode `npm install` novamente
- Verifique se tem Node.js >= 18

## Próximos Passos

Após o MVP funcionar, você pode:

1. **Implementar Stripe** - Módulo billing já preparado em `apps/api/src/modules/billing/`
2. **Adicionar mais tipos de steps** - Video, countdown, depoimentos
3. **Editor visual** - Permitir editar funis manualmente
4. **Analytics** - Rastrear visualizações e conversões
5. **Testes** - Adicionar Jest/Vitest
6. **CI/CD** - Deploy automatizado

## Stack Completa

- **Frontend**: Next.js 14, React 18, TypeScript, Tailwind CSS
- **Backend**: NestJS, TypeScript
- **Database**: Supabase (PostgreSQL)
- **IA**: OpenAI GPT-4
- **Validação**: Zod
- **Monorepo**: npm workspaces

---

**Dúvidas?** Consulte o `README.md` completo na raiz do projeto.
