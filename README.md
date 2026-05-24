# ✂ Gorilaz Barber Finance

> Controle financeiro completo para barbearias — desenvolvido com HTML, CSS e JavaScript puro + Supabase.

---

## 📁 Estrutura do Projeto

```
barber-finance/
│
├── css/
│   └── main.css              # Design system centralizado (tokens, layout, componentes)
│
├── js/
│   ├── app.js                # Core: Supabase, Auth, Layout, Toast, Modal, Utils, Icons
│   └── modules/
│       ├── dashboard.js      # KPIs, gráficos, metas e últimas movimentações
│       ├── receitas.js       # CRUD de receitas
│       ├── despesas.js       # CRUD de despesas com status (pago/pendente)
│       ├── investimentos.js  # Carteira de investimentos com análise
│       ├── metas.js          # Metas financeiras com depósito
│       └── perfil.js         # Dados do usuário e segurança
│
├── assets/
│   └── icons/                # Ícones estáticos (se necessário)
│
├── dashboard.html
├── receitas.html
├── despesas.html
├── investimentos.html
├── metas.html
├── perfil.html
└── login.html
```

---

## 🚀 Como usar

### 1. Configure o Supabase

No arquivo `js/app.js`, substitua as credenciais:

```js
const SUPABASE_URL  = 'https://SEU_PROJETO.supabase.co'
const SUPABASE_ANON = 'SUA_CHAVE_ANON'
```

### 2. Crie as tabelas no Supabase

Execute no SQL Editor do seu projeto:

```sql
-- Receitas
create table receitas (
  id uuid primary key default gen_random_uuid(),
  user_id uuid references auth.users not null,
  descricao text not null,
  valor numeric not null,
  data date not null,
  categoria text,
  observacao text,
  created_at timestamptz default now()
);
alter table receitas enable row level security;
create policy "own" on receitas using (auth.uid() = user_id);

-- Despesas
create table despesas (
  id uuid primary key default gen_random_uuid(),
  user_id uuid references auth.users not null,
  descricao text not null,
  valor numeric not null,
  data date not null,
  categoria text,
  status text default 'pago',
  observacao text,
  created_at timestamptz default now()
);
alter table despesas enable row level security;
create policy "own" on despesas using (auth.uid() = user_id);

-- Investimentos
create table investimentos (
  id uuid primary key default gen_random_uuid(),
  user_id uuid references auth.users not null,
  nome_investimento text not null,
  tipo_investimento text,
  instituição text,
  valor_investido numeric,
  valor_atual numeric,
  data_aporte date,
  rentabilidade numeric,
  objetivo text,
  created_at timestamptz default now()
);
alter table investimentos enable row level security;
create policy "own" on investimentos using (auth.uid() = user_id);

-- Metas
create table metas (
  id uuid primary key default gen_random_uuid(),
  user_id uuid references auth.users not null,
  nome text not null,
  valor_alvo numeric not null,
  valor_atual numeric default 0,
  prazo date,
  created_at timestamptz default now()
);
alter table metas enable row level security;
create policy "own" on metas using (auth.uid() = user_id);

-- Perfis
create table profiles (
  id uuid primary key references auth.users,
  full_name text,
  barbershop_name text,
  phone text,
  updated_at timestamptz,
  created_at timestamptz default now()
);
alter table profiles enable row level security;
create policy "own" on profiles using (auth.uid() = id);
```

### 3. Sirva os arquivos

Qualquer servidor estático funciona:

```bash
# Python
python3 -m http.server 3000

# Node (npx)
npx serve .

# VS Code
# Instale a extensão "Live Server" e clique em "Go Live"
```

Acesse: [http://localhost:3000/login.html](http://localhost:3000/login.html)

---

## ✨ Funcionalidades

| Módulo          | Funcionalidades                                              |
|-----------------|--------------------------------------------------------------|
| **Dashboard**   | KPIs mensais, gráfico diário, metas e últimas movimentações  |
| **Receitas**    | Registro, edição, exclusão e filtros por categoria/mês       |
| **Despesas**    | CRUD completo com status pago/pendente e categorias          |
| **Investimentos**| Carteira com ganho/perda, rentabilidade e análise por tipo  |
| **Metas**       | Progresso visual, depósito incremental e prazo               |
| **Perfil**      | Dados pessoais, nome da barbearia e alteração de senha       |

---

## 🛠 Stack

- **Frontend:** HTML5 + CSS3 + JavaScript (Vanilla, sem framework)
- **Backend / Auth / DB:** [Supabase](https://supabase.com) (PostgreSQL + Auth)
- **Design:** Dark theme com sistema de tokens CSS

---

## 📄 Licença

MIT
