# 📁 Estrutura do Projeto FunnelFy AI

## Visão Geral da Arquitetura

```
funnelfy-ai/                    # Raiz do monorepo
│
├── apps/                       # Aplicações
│   ├── web/                    # Frontend Next.js
│   └── api/                    # Backend NestJS
│
├── packages/                   # Código compartilhado
│   ├── prompts/                # Prompts da OpenAI
│   ├── schemas/                # Schemas de validação
│   └── ui/                     # Componentes UI (futuro)
│
├── infra/                      # Infraestrutura (preparado)
│
├── .env                        # Variáveis de ambiente
├── .env.example                # Template de variáveis
├── package.json                # Config do monorepo
├── README.md                   # Documentação completa
├── QUICKSTART.md               # Guia rápido
└── PROJECT_STRUCTURE.md        # Este arquivo
```

## Detalhamento dos Diretórios

### 📱 Frontend (apps/web/)

```
apps/web/
├── app/                        # App Router do Next.js
│   ├── layout.tsx              # Layout raiz com AuthProvider
│   ├── page.tsx                # Landing page
│   │
│   ├── login/
│   │   └── page.tsx            # Página de login
│   │
│   ├── register/
│   │   └── page.tsx            # Página de registro
│   │
│   ├── dashboard/
│   │   ├── page.tsx            # Dashboard principal
│   │   └── funnels/
│   │       └── [id]/
│   │           └── page.tsx    # Detalhes de um funil
│   │
│   ├── create/
│   │   └── page.tsx            # Criar funil com IA
│   │
│   └── f/
│       └── [slug]/
│           └── page.tsx        # Funil público
│
├── lib/
│   ├── auth-context.tsx        # Context de autenticação
│   └── utils.ts                # Utilidades gerais
│
├── package.json                # Dependências do frontend
├── tsconfig.json               # Config TypeScript
├── tailwind.config.ts          # Config Tailwind
└── next.config.js              # Config Next.js
```

### 🔧 Backend (apps/api/)

```
apps/api/
├── src/
│   ├── main.ts                 # Bootstrap do NestJS
│   ├── app.module.ts           # Módulo raiz
│   │
│   ├── config/
│   │   ├── supabase.config.ts  # Cliente Supabase
│   │   └── openai.config.ts    # Cliente OpenAI
│   │
│   └── modules/
│       ├── auth/               # Módulo de autenticação
│       │   ├── auth.module.ts
│       │   ├── auth.controller.ts
│       │   └── auth.service.ts
│       │
│       ├── funnels/            # Módulo de funis
│       │   ├── funnels.module.ts
│       │   ├── funnels.controller.ts
│       │   └── funnels.service.ts
│       │
│       └── billing/            # Preparado para Stripe
│           └── .gitkeep
│
├── package.json                # Dependências do backend
├── tsconfig.json               # Config TypeScript
└── nest-cli.json               # Config NestJS
```

### 📦 Packages Compartilhados

```
packages/
├── prompts/                    # Prompts da OpenAI
│   ├── src/
│   │   ├── funnel-generator.ts # Sistema de prompts
│   │   └── index.ts            # Exports
│   ├── package.json
│   └── tsconfig.json
│
├── schemas/                    # Validação com Zod
│   ├── src/
│   │   ├── auth.schema.ts      # Schemas de auth
│   │   ├── funnel.schema.ts    # Schemas de funis
│   │   └── index.ts            # Exports
│   ├── package.json
│   └── tsconfig.json
│
└── ui/                         # Componentes compartilhados
    ├── src/
    │   └── index.ts
    ├── package.json
    └── tsconfig.json
```

## Arquivos Importantes na Raiz

| Arquivo | Descrição |
|---------|-----------|
| `.env` | Variáveis de ambiente (não commitado) |
| `.env.example` | Template de variáveis |
| `package.json` | Gerenciamento do monorepo (npm workspaces) |
| `README.md` | Documentação completa do projeto |
| `QUICKSTART.md` | Guia rápido para iniciar |
| `PROJECT_STRUCTURE.md` | Este arquivo |
| `.eslintrc.json` | Configuração ESLint |
| `.gitignore` | Arquivos ignorados pelo Git |

## Fluxo de Dados

```
┌─────────────────────────────────────────────────────────────┐
│                         USUÁRIO                              │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                    FRONTEND (Next.js)                        │
│                    localhost:3000                            │
│                                                              │
│  Páginas:                                                    │
│  ├── Landing, Login, Register                               │
│  ├── Dashboard, Create                                       │
│  └── Public Funnel (/f/[slug])                              │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
                        fetch API calls
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                    BACKEND (NestJS)                          │
│                    localhost:3001/api                        │
│                                                              │
│  Módulos:                                                    │
│  ├── Auth Module (register, login)                          │
│  └── Funnels Module (generate, list, publish)               │
└─────────────────────────────────────────────────────────────┘
                    │                    │
                    │                    │
                    ▼                    ▼
        ┌───────────────────┐  ┌──────────────────┐
        │    SUPABASE       │  │    OPENAI        │
        │   (PostgreSQL)    │  │    (GPT-4)       │
        │                   │  │                  │
        │  Tables:          │  │  Geração de      │
        │  ├── users        │  │  funis com IA    │
        │  └── funnels      │  │                  │
        └───────────────────┘  └──────────────────┘
```

## Tecnologias por Camada

### Frontend
- **Framework**: Next.js 14 (App Router)
- **UI**: React 18 + TypeScript
- **Styling**: Tailwind CSS
- **Icons**: Lucide React
- **State**: React Context (Auth)

### Backend
- **Framework**: NestJS
- **Language**: TypeScript
- **Validation**: class-validator + Zod
- **HTTP**: Express (interno do NestJS)

### Database
- **Provider**: Supabase (PostgreSQL)
- **Security**: Row Level Security (RLS)
- **ORM**: Cliente JavaScript do Supabase

### IA
- **Provider**: OpenAI
- **Model**: GPT-4
- **Purpose**: Geração de funis estruturados

### Monorepo
- **Tool**: npm workspaces
- **Package Manager**: npm
- **Build**: Paralelo com concurrently

## Scripts npm Disponíveis

### Raiz do Projeto

| Script | Comando | Descrição |
|--------|---------|-----------|
| `dev` | `npm run dev` | Roda backend e frontend juntos |
| `dev:api` | `npm run dev:api` | Roda apenas o backend |
| `dev:web` | `npm run dev:web` | Roda apenas o frontend |
| `build` | `npm run build` | Build de produção (api + web) |
| `build:api` | `npm run build:api` | Build apenas do backend |
| `build:web` | `npm run build:web` | Build apenas do frontend |

### Backend (apps/api)

```bash
cd apps/api
npm run dev       # Desenvolvimento com hot-reload
npm run build     # Build para produção
npm run start     # Inicia versão de produção
```

### Frontend (apps/web)

```bash
cd apps/web
npm run dev       # Desenvolvimento com hot-reload
npm run build     # Build para produção
npm run start     # Inicia versão de produção
```

## Variáveis de Ambiente

### Frontend (.env)
- `NEXT_PUBLIC_SUPABASE_URL` - URL do Supabase
- `NEXT_PUBLIC_SUPABASE_ANON_KEY` - Chave pública Supabase
- `NEXT_PUBLIC_API_URL` - URL da API
- `NEXT_PUBLIC_WEB_URL` - URL do frontend

### Backend (.env)
- `OPENAI_API_KEY` - Chave da OpenAI
- `PORT` - Porta do servidor (3001)
- `WEB_URL` - URL do frontend (CORS)
- `NEXT_PUBLIC_SUPABASE_URL` - URL do Supabase
- `NEXT_PUBLIC_SUPABASE_ANON_KEY` - Chave Supabase

## Convenções de Código

### TypeScript
- Strict mode habilitado
- Interfaces para tipos públicos
- Types para tipos internos
- Evitar `any`, preferir `unknown`

### Nomenclatura
- **Componentes**: PascalCase (`LoginPage.tsx`)
- **Funções**: camelCase (`fetchFunnels()`)
- **Constantes**: UPPER_CASE (`API_URL`)
- **Arquivos**: kebab-case (`auth-context.tsx`)

### Organização
- Um componente por arquivo
- Funções auxiliares em arquivos separados
- Exports nomeados quando possível
- Imports organizados (externos > internos)

## Database Schema

### Tabela `users`
```sql
id              uuid PRIMARY KEY
email           text UNIQUE NOT NULL
password_hash   text NOT NULL
full_name       text
created_at      timestamptz DEFAULT now()
updated_at      timestamptz DEFAULT now()
```

### Tabela `funnels`
```sql
id              uuid PRIMARY KEY
user_id         uuid REFERENCES users(id)
name            text NOT NULL
slug            text UNIQUE NOT NULL
prompt          text NOT NULL
funnel_data     jsonb NOT NULL
is_published    boolean DEFAULT false
created_at      timestamptz DEFAULT now()
updated_at      timestamptz DEFAULT now()
```

## Tipos de Steps de Funil

| Tipo | Descrição | Campos |
|------|-----------|--------|
| `quiz_question` | Pergunta de quiz | question, options[] |
| `result` | Resultado do quiz | headline, description |
| `sales_page` | Página de vendas | headline, benefits[], cta, price |
| `opt_in` | Captura de email | headline, description, cta |
| `video` | Vídeo + descrição | headline, videoUrl, description |

## Segurança Implementada

✅ **Row Level Security (RLS)**
- Todas as tabelas com RLS habilitado
- Políticas restritivas por padrão

✅ **Autenticação**
- Tokens Bearer para autenticação
- Senhas hasheadas (SHA-256)
- Validação em todos os endpoints protegidos

✅ **Validação de Dados**
- Zod para validação de schemas
- class-validator no NestJS
- Sanitização de inputs

✅ **CORS**
- Configurado apenas para origem permitida
- Credentials habilitado corretamente

## Próximas Implementações Planejadas

1. **Billing Module** (Stripe)
   - Localização: `apps/api/src/modules/billing/`
   - Status: Estrutura preparada

2. **Analytics**
   - Rastreamento de visualizações
   - Taxa de conversão por step
   - Dashboard de métricas

3. **Editor Visual**
   - Editar funis manualmente
   - Drag & drop de steps
   - Preview em tempo real

4. **Mais Tipos de Steps**
   - Countdown timer
   - Depoimentos
   - FAQ
   - Comparação de preços

5. **Testes**
   - Unit tests (Jest)
   - Integration tests
   - E2E tests (Playwright)

---

**Última atualização**: Dezembro 2023
