# PRD — API2MCP

| Campo | Valor |
|---|---|
| **Produto** | API2MCP |
| **Versão do documento** | 1.0 |
| **Status** | Planejamento Inicial |
| **Data** | 2026-07-02 |
| **Stack** | Next.js + Supabase + MCP + TypeScript |

## 1. Sumário Executivo

O **API2MCP** é uma plataforma SaaS que transforma qualquer API REST em um **Servidor MCP (Model Context Protocol)** sem necessidade de programação. O usuário conecta sua API, escolhe quais endpoints deseja expor, e a plataforma gera automaticamente um servidor MCP pronto para ser utilizado por ferramentas como Claude, Cursor, VS Code, OpenAI, Windsurf e qualquer cliente compatível com MCP.

O objetivo de longo prazo é se tornar a **principal plataforma de publicação e gerenciamento de servidores MCP** do mercado.

## 2. Problema

Hoje, criar um servidor MCP exige conhecimento em diversas áreas técnicas simultâneas:

- Model Context Protocol
- JSON Schema
- Autenticação
- Servidores HTTP
- SDKs MCP
- Deploy
- Versionamento

Essa barreira de entrada técnica limita fortemente a adoção da tecnologia MCP por desenvolvedores, empresas e agências que não têm tempo ou expertise para implementar tudo do zero.

## 3. Solução Proposta

Uma plataforma onde o usuário, via interface gráfica, pode:

- Criar um projeto
- Conectar uma API
- Importar Swagger/OpenAPI
- Selecionar endpoints
- Gerar automaticamente Tools MCP
- Publicar um servidor MCP
- Monitorar uso
- Versionar ferramentas

Todo o processo é feito sem escrever código.

## 4. Público-alvo / Personas

| Persona | Necessidade |
|---|---|
| **Desenvolvedores** | Integrar seus sistemas com IA rapidamente |
| **Empresas** | Possuem APIs internas e querem disponibilizá-las para agentes de IA |
| **Agências** | Criam integrações MCP para clientes |
| **Empresas SaaS** | Desejam oferecer integração MCP para seus próprios clientes |
| **Plataformas White Label** | Querem disponibilizar IA consumindo seus próprios sistemas |

## 5. Objetivos e Metas

### 5.1 Objetivos do MVP

- Login
- Dashboard
- Projetos
- Cadastro de APIs
- Importação OpenAPI
- Conversão automática
- Editor de Tools
- Publicação MCP
- Testador
- Logs

### 5.2 Objetivos da V2

- Marketplace
- Templates
- OAuth automático
- Billing
- Multi Workspace
- Compartilhamento
- Versionamento
- Analytics
- CLI
- SDK
- Gateway Global

## 6. Escopo do MVP

O MVP entrega o fluxo completo de ponta a ponta: da criação de conta à publicação e consumo de um servidor MCP funcional. Os módulos 1 a 12 (seção 9) descrevem em detalhe cada funcionalidade dentro deste escopo.

## 7. Fora de Escopo do MVP (V2+)

As funcionalidades abaixo são reconhecidas como necessárias para a evolução comercial e de plataforma do produto, mas **não fazem parte do MVP**:

- Marketplace de Tools/servidores MCP
- Templates prontos de integração
- OAuth automático (fluxo assistido de autorização)
- Billing e cobrança recorrente
- Multi Workspace / múltiplas organizações por conta
- Compartilhamento de projetos entre usuários
- Versionamento formal de ferramentas (histórico de versões)
- Analytics avançado
- CLI e SDK para desenvolvedores
- Gateway Global multi-região
- White Label completo
- Assinaturas HMAC nas APIs conectadas (planejado, fora do MVP)

## 8. Fluxo do Usuário

```
Criar conta
  ↓
Criar Projeto
  ↓
Cadastrar API
  ↓
Escolher autenticação
  ↓
Importar Swagger
  ↓
Selecionar endpoints
  ↓
Gerar Tools
  ↓
Editar descrições
  ↓
Publicar MCP
  ↓
Receber Endpoint
  ↓
Consumir no Claude/Cursor/OpenAI
```

## 9. Requisitos Funcionais por Módulo

### Módulo 1 — Autenticação

**Objetivo:** permitir que usuários criem conta e acessem a plataforma com segurança.

Funcionalidades:
- Login
- Cadastro
- Google OAuth
- GitHub OAuth
- Recuperação de senha
- Convites

### Módulo 2 — Projetos

**Objetivo:** organizar o trabalho do usuário em unidades isoladas de configuração.

Cada usuário pode possuir diversos projetos. Campos: Nome, Descrição, Ambiente, Organização, Plano.

### Módulo 3 — APIs

**Objetivo:** registrar as APIs que serão convertidas em servidores MCP.

Cadastro manual ou importação automática. Campos: Nome, Base URL, Swagger URL, Tipo, Ambiente.

### Módulo 4 — Autenticação das APIs

**Objetivo:** suportar os principais mecanismos de autenticação usados por APIs externas/internas.

Tipos suportados:
- API Key
- Bearer Token
- OAuth2
- Basic Auth
- JWT
- Cookie
- Header Personalizado
- Query Parameter
- Assinaturas HMAC (planejado)

Credenciais são armazenadas de forma criptografada.

### Módulo 5 — Importador OpenAPI

**Objetivo:** extrair automaticamente a estrutura de uma API a partir de um documento Swagger/OpenAPI.

```
Swagger URL
  ↓
Parser
  ↓
Extração
  ↓
Endpoints / Schemas / Métodos / Parâmetros / Responses
  ↓
Conversão
```

### Módulo 6 — Endpoints

**Objetivo:** representar cada rota importada com seus metadados.

Cada endpoint possui: Método, URL, Summary, Description, Tags, Request Body, Parameters, Responses.

### Módulo 7 — Conversor MCP

**Objetivo:** transformar um endpoint REST em uma Tool MCP válida.

Gera automaticamente: Nome da Tool, Descrição, Input Schema, Output Schema, Metadata, Categoria.

Exemplo:
```
GET /users/{id}
  ↓
Tool: get_user
  ↓
Input: id
  ↓
Output: User
```

### Módulo 8 — Editor Visual

**Objetivo:** permitir ajuste manual das Tools geradas automaticamente.

Campos editáveis: Nome, Descrição, Categoria, Tags, Schema, Exemplos, Status.

### Módulo 9 — Runtime MCP

**Objetivo:** executar as chamadas reais das Tools publicadas.

Fluxo: recebe requisições do cliente MCP → localiza a Tool → executa chamada HTTP → aplica autenticação → retorna resposta padronizada.

### Módulo 10 — Publicação

**Objetivo:** disponibilizar um servidor MCP consumível externamente com um clique.

```
Botão Deploy
  ↓
Servidor publicado
  ↓
Endpoint: https://app.api2mcp.com/mcp/{server_id}
```

### Módulo 11 — Testador

**Objetivo:** permitir validar Tools diretamente pela plataforma, sem cliente MCP externo.

Executa Tools e visualiza: Request, Headers, Tempo, Response, Erros.

### Módulo 12 — Logs

**Objetivo:** dar observabilidade sobre o uso das Tools publicadas.

Registra: Usuário, Projeto, Tool, Tempo, Status, Request, Response, Tokens, Custos, IP, User Agent.

## 10. Modelo de Dados (visão preliminar)

| Tabela | Campos principais |
|---|---|
| `users` | id, email, created_at |
| `organizations` | id, owner_id, name |
| `projects` | id, organization_id, name, description, created_at |
| `apis` | id, project_id, base_url, swagger_url, auth_type, auth_config |
| `endpoints` | id, api_id, method, path, summary, description |
| `tools` | id, endpoint_id, enabled, schema, output_schema, metadata |
| `deployments` | id, project_id, version, endpoint |
| `logs` | id, tool_id, latency, request, response, status |

> Este modelo é uma visão preliminar de schema para orientar o desenho do banco de dados (Supabase/PostgreSQL); os campos de auditoria, RLS e chaves estrangeiras completas serão definidos na fase de implementação técnica.

## 11. Arquitetura

### 11.1 Visão Geral

```
                    Usuário
                       │
                 Next.js Dashboard
                       │
                 Supabase Backend
                       │
        PostgreSQL + Storage + Auth
                       │
               Runtime API2MCP
                       │
            MCP Gateway Multi-Tenant
                       │
        APIs REST externas / internas
```

### 11.2 Arquitetura Multi-Tenant

Todos os clientes compartilham o mesmo runtime. Cada projeto possui: APIs, Ferramentas, Configurações, Autenticação, Logs.

O runtime identifica o projeto através do **token do servidor MCP**. Não é criado um servidor individual por cliente — isso reduz custos operacionais e melhora a escalabilidade horizontal da plataforma.

## 12. Stack Tecnológica

**Front-end**
- Next.js 15
- React
- TypeScript
- TailwindCSS
- Shadcn UI
- TanStack Query
- React Hook Form
- Zod
- Framer Motion

**Backend**
- Supabase Auth
- PostgreSQL
- Storage
- Edge Functions
- Realtime

**Runtime**
- Node.js
- MCP SDK
- Fastify
- Zod
- OpenAPI Parser

**Deploy**
- Vercel
- Supabase
- Docker
- Cloudflare

## 13. Dashboard (estrutura de navegação)

**Página Inicial**
- Projetos
- Últimos acessos
- Logs recentes
- Erros
- Consumo

**Projeto**
- APIs
- Ferramentas
- Deploy
- Logs
- Configurações

**API**
- Swagger
- Endpoints
- Autenticação

**Ferramentas**
- Lista
- Editor
- Testador

**Deploy**
- Histórico
- Endpoint
- Token
- Status

## 14. Requisitos Não-Funcionais

### 14.1 Segurança

- RLS (Row Level Security) no Supabase
- Criptografia das credenciais de APIs conectadas
- Rate Limit
- Auditoria
- Logs
- CORS configurável
- Rotação de tokens
- Ambiente Sandbox
- Secrets isolados por projeto

### 14.2 Escalabilidade

- Runtime Stateless
- Cache Redis (planejado)
- Filas para sincronização
- Horizontal Scaling
- CDN
- Multi Região

## 15. Modelo Comercial

| Plano | Recursos |
|---|---|
| **Gratuito** | 1 Projeto, 1 API, até 20 Tools, 1.000 requisições/mês |
| **Pro** | Projetos ilimitados, APIs ilimitadas, Versionamento, Analytics, Logs completos, Equipes |
| **Enterprise** | SSO, White Label, Deploy dedicado, Auditoria, SLA, Suporte prioritário |

## 16. Diferenciais Competitivos

- Conversão automática de APIs REST em MCP
- Gateway MCP multi-tenant com alta escalabilidade
- Importação inteligente via OpenAPI/Swagger
- Editor visual de Tools e Schemas
- Publicação com um clique
- Testador integrado
- Observabilidade completa
- Versionamento de ferramentas
- Preparado para ambientes corporativos
- Extensível para GraphQL, gRPC e WebSockets em versões futuras

## 17. Métricas de Sucesso

### 17.1 Produto

- APIs cadastradas
- Tools geradas
- Servidores publicados
- Requisições MCP/dia
- Tempo médio de publicação

### 17.2 Negócio

- Usuários ativos
- Conversão Free → Pro
- MRR (Monthly Recurring Revenue)
- Churn
- CAC (Custo de Aquisição de Cliente)
- LTV (Lifetime Value)

## 18. Roadmap

| Fase | Entregas |
|---|---|
| **Fase 1 — Fundação** | Configuração do projeto, Autenticação, Banco de dados, Dashboard |
| **Fase 2 — APIs** | Cadastro, Teste, Importador Swagger |
| **Fase 3 — Conversor** | Parser OpenAPI, Gerador de Tools, Editor |
| **Fase 4 — Runtime** | Gateway MCP, Publicação, Testador |
| **Fase 5 — Observabilidade** | Logs, Analytics, Auditoria |
| **Fase 6 — Plataforma** | Marketplace, Templates, Billing, Times, Convites, White Label |

> O MVP (seção 6) corresponde às Fases 1 a 4. As Fases 5 e 6 consolidam observabilidade e os recursos de plataforma listados como V2 (seção 5.2 e 7).

## 19. Riscos e Premissas

### 19.1 Riscos

| Risco | Impacto | Observação |
|---|---|---|
| Fidelidade da conversão OpenAPI → MCP | Alto | Especificações Swagger mal formadas ou incompletas podem gerar Tools com schemas incorretos, exigindo o Editor Visual (Módulo 8) como mitigação |
| Segurança de credenciais de terceiros | Alto | Armazenar e usar credenciais de APIs externas exige criptografia robusta e isolamento rígido por projeto (secrets isolados, Módulo 4) |
| Custos e contenção do runtime multi-tenant | Médio | Um projeto com alto volume de requisições pode impactar outros tenants no mesmo runtime compartilhado; requer rate limiting e isolamento de recursos |
| Evolução do protocolo MCP | Médio | O MCP é um protocolo em evolução; mudanças de especificação podem exigir atualizações no Conversor (Módulo 7) e no Runtime (Módulo 9) |
| Confiabilidade de APIs externas | Médio | APIs de terceiros fora do ar ou instáveis afetam diretamente a experiência dos servidores MCP publicados |
| Adoção de clientes MCP | Baixo/Médio | O crescimento do produto depende da adoção contínua do MCP por ferramentas como Claude, Cursor e outras |

### 19.2 Premissas

- Os clientes MCP (Claude, Cursor, VS Code, OpenAI, Windsurf, etc.) manterão compatibilidade com o protocolo MCP padrão.
- A maioria das APIs-alvo possui documentação OpenAPI/Swagger disponível ou pode ser cadastrada manualmente.
- Supabase é suficiente como backend principal (Auth, Postgres, Storage, Edge Functions) para o volume esperado no MVP e fase inicial de V2.
- O modelo de runtime multi-tenant é aceitável para os clientes iniciais; deploy dedicado (isolado) fica reservado ao plano Enterprise.

## 20. Integrações Futuras (candidatas)

GitHub, GitLab, Linear, Jira, Notion, Slack, Discord, Supabase, Firebase, PostgreSQL, MySQL, MongoDB, Stripe, Shopify, HubSpot, Salesforce, n8n, Flowise, Langflow, Open WebUI, OpenAI, Claude, Gemini.

## 21. Visão de Longo Prazo

Transformar o API2MCP na principal plataforma de integração entre sistemas e agentes de IA, oferecendo um ecossistema completo para criação, gerenciamento, publicação, monitoramento e comercialização de servidores MCP. A plataforma deverá evoluir de um simples conversor de APIs para uma **camada universal de integração**, permitindo que empresas exponham qualquer serviço como ferramentas compatíveis com IA de forma segura, escalável e governada.
