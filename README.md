# FunnelFy AI

SaaS profissional para criar funis de vendas automaticamente usando Inteligência Artificial.

## Arquitetura

Este projeto segue uma arquitetura de **monorepo** com separação clara entre front-end e back-end:

```
funnelfy-ai/
├── apps/
│   ├── web/                 # Front-end (Next.js App Router)
│   └── api/                 # Back-end (NestJS)
│
├── packages/
│   ├── prompts/             # Prompts da IA (compartilhado)
│   ├── schemas/             # Schemas de validação (compartilhado)
│   └── ui/                  # Componentes UI (preparado para futuro)
│
└── infra/                   # Infraestrutura (futuro)
```

## Stack Tecnológica

### Front-end (apps/web)
- **Next.js 14** com App Router
- **React 18**
- **TypeScript**
- **Tailwind CSS**
- **Lucide Icons**

### Back-end (apps/api)
- **NestJS** - Framework Node.js enterprise
- **TypeScript**
- **OpenAI API** - Geração de funis com IA
- **Supabase** - Database PostgreSQL

### Banco de Dados
- **Supabase** (PostgreSQL)
- Row Level Security (RLS) habilitado
- Autenticação integrada

## Pré-requisitos

- Node.js >= 18.0.0
- npm >= 9.0.0
- Conta Supabase (gratuita)
- OpenAI API Key

## Instalação

### 1. Clone o repositório

```bash
git clone <repo-url>
cd funnelfy-ai
```

### 2. Configure as variáveis de ambiente

Copie o arquivo `.env.example` para `.env` na raiz do projeto:

```bash
cp .env.example .env
```

Edite o arquivo `.env` com suas credenciais:

```env
# Supabase (obtenha em https://app.supabase.com)
NEXT_PUBLIC_SUPABASE_URL=https://seu-projeto.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=sua-anon-key

# OpenAI (obtenha em https://platform.openai.com/api-keys)
OPENAI_API_KEY=sk-...

# URLs locais (não alterar para desenvolvimento)
NEXT_PUBLIC_API_URL=http://localhost:3001/api
NEXT_PUBLIC_WEB_URL=http://localhost:3000
PORT=3001
WEB_URL=http://localhost:3000
```

### 3. Instale as dependências

```bash
npm install
```

Este comando instalará as dependências de todos os workspaces (root, web, api, e packages).

### 4. Configure o banco de dados

O schema do Supabase já foi aplicado durante o setup. Verifique no painel do Supabase se as tabelas `users` e `funnels` foram criadas.

## Executando o Projeto

### Desenvolvimento

Para rodar front-end e back-end simultaneamente:

```bash
npm run dev
```

Este comando iniciará:
- API (NestJS) em `http://localhost:3001`
- Web (Next.js) em `http://localhost:3000`

Ou rode separadamente:

```bash
# Apenas API
npm run dev:api

# Apenas Web
npm run dev:web
```

### Build

Para fazer build de produção:

```bash
npm run build
```

Este comando fará build do back-end e front-end em sequência.

### Produção

```bash
npm run start:api  # Inicia o back-end em produção
npm run start:web  # Inicia o front-end em produção
```

## Estrutura de Módulos

### Back-end (NestJS)

#### Módulo Auth (`apps/api/src/modules/auth/`)
- **Responsabilidade**: Autenticação e gerenciamento de usuários
- **Endpoints**:
  - `POST /api/auth/register` - Criar nova conta
  - `POST /api/auth/login` - Fazer login
- **Tecnologia**: Supabase + hash SHA-256

#### Módulo Funnels (`apps/api/src/modules/funnels/`)
- **Responsabilidade**: Criação e gerenciamento de funis com IA
- **Endpoints**:
  - `POST /api/funnels/generate` - Gerar novo funil com IA
  - `GET /api/funnels` - Listar funis do usuário
  - `GET /api/funnels/:id` - Detalhes de um funil
  - `GET /api/public/funnel/:slug` - Funil público (sem auth)
  - `PATCH /api/funnels/:id/publish` - Publicar funil
- **Tecnologia**: OpenAI GPT-4

### Front-end (Next.js)

#### Páginas Públicas
- `/` - Landing page
- `/login` - Página de login
- `/register` - Página de cadastro
- `/f/[slug]` - Funil público (acessível sem login)

#### Páginas Privadas (requer autenticação)
- `/dashboard` - Dashboard com lista de funis
- `/dashboard/funnels/[id]` - Detalhes e gerenciamento de funil
- `/create` - Criar novo funil com IA

## Fluxo de Uso

1. **Usuário cria conta** (`/register`)
2. **Faz login** (`/login`)
3. **Vai para o dashboard** (`/dashboard`)
4. **Clica em "Criar Novo Funil"** (`/create`)
5. **Descreve o produto em texto** (prompt)
6. **IA gera o funil completo** (OpenAI processa e retorna JSON)
7. **Funil aparece no dashboard**
8. **Usuário visualiza e publica** (`/dashboard/funnels/[id]`)
9. **Funil fica disponível em URL pública** (`/f/[slug]`)

## Packages Compartilhados

### @funnelfy/prompts
Contém os prompts do sistema para a OpenAI, garantindo consistência na geração de funis.

### @funnelfy/schemas
Schemas de validação com Zod para garantir integridade dos dados entre front e back.

### @funnelfy/ui
Preparado para componentes compartilhados (futuro).

## Segurança

- ✅ Row Level Security (RLS) habilitado no Supabase
- ✅ Tokens de autenticação em Bearer token
- ✅ Validação de dados com Zod
- ✅ Hash de senhas com SHA-256
- ✅ CORS configurado corretamente
- ✅ Políticas RLS restritivas (users só veem seus dados)

## Próximos Passos

- [ ] Implementar módulo de billing com Stripe
- [ ] Adicionar analytics de conversão
- [ ] Implementar editor visual de funis
- [ ] Adicionar mais tipos de steps (vídeo, countdown, etc)
- [ ] Testes automatizados (Jest/Vitest)
- [ ] CI/CD pipeline

## Tecnologias Utilizadas

- **TypeScript** - Tipagem estática
- **Next.js** - Framework React
- **NestJS** - Framework Node.js
- **Supabase** - Database PostgreSQL + Auth
- **OpenAI** - IA generativa
- **Tailwind CSS** - Estilização
- **Zod** - Validação de schemas
- **npm workspaces** - Gerenciamento de monorepo

## Licença

MIT

## Suporte

Para dúvidas ou problemas, abra uma issue no repositório.
