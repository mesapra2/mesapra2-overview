# Mesapra2

**Social dining com curadoria gastronômica.** Conecta pessoas através de eventos em restaurantes parceiros — matching social + agenda de encontros em grupo + benefícios com curadoria de restaurantes, num produto só.

> 🔒 Este repositório é uma vitrine pública da arquitetura do produto. **Não contém código-fonte** — o app em produção vive num repositório privado. O objetivo aqui é mostrar como o sistema é organizado e o que já está implementado, não como ele é codado.

![Status: Em Produção](https://img.shields.io/badge/Status-Em%20Produ%C3%A7%C3%A3o-22C55E?style=for-the-badge) ![Plataformas: Android · iOS · Web](https://img.shields.io/badge/Plataformas-Android%20%C2%B7%20iOS%20%C2%B7%20Web-3B82F6?style=for-the-badge) ![Países: 6 via Play Store](https://img.shields.io/badge/Pa%C3%ADses-6%20via%20Play%20Store-F59E0B?style=for-the-badge) ![Repositório: Vitrine Pública](https://img.shields.io/badge/Reposit%C3%B3rio-Vitrine%20P%C3%BAblica-64748B?style=for-the-badge)

![Mesapra2](mesapra2-screenshot.jpg)

---

## Sumário

- [Jornada](#jornada)
- [O que é](#o-que-é)
- [Stack principal](#stack-principal)
- [Arquitetura em 5 grupos](#arquitetura-em-5-grupos)
- [Domínios funcionais](#domínios-funcionais)
- [Fluxos em destaque](#fluxos-em-destaque)
- [Ecossistema de integrações](#ecossistema-de-integrações)
- [Confiabilidade e Compliance](#confiabilidade-e-compliance)
- [Documentos institucionais](#documentos-institucionais)
- [Status](#status)

## Jornada

Da concepção ao app publicado nas duas lojas — linha do tempo real, sem os números financeiros (esses ficam só para investidores).

| Quando | Marco |
|---|---|
| Jun 2025 | 💡 Concepção — a tese nasce: matching social + agenda de eventos em grupo + curadoria de restaurantes |
| Ago 2025 | 🧱 Início do desenvolvimento — site institucional mesapra2.com no ar |
| Out 2025 | 🚀 Product Hunt + primeiro commit (26/10/2025) — início do histórico público do código |
| Jan 2026 | 🏛️ CNPJ aberto — MESAS DO BRASIL (I.S.), Inova Simples (Lei Complementar 167/2019) |
| Jan 2026 | ™️ Marca depositada no INPI |
| Mar 2026 | 🏆 "Versão Dourada" (v1.0.61) — primeiras features principais consolidadas |
| Abr 2026 | 📱 Soft launch Android na Play Store (Google Play Billing integrado) |
| Jul 2026 | 💳 Monetização via RevenueCat + Google Play (Club) |
| Ago 2026 | ⚙️ Pipeline de release automatizado — build → e2e → AAB → Play → watcher |
| Set 2026 | 🍎 App publicado na App Store — Android e iOS no ar |
| Set 2026 | 🌎 App em espanhol — neutro para LatAm + variante rioplatense (AR/UY) |
| Set 2026 | ⚡ Entrada do app 5× mais rápida (6,85s → 1,29s em rede lenta) — acessibilidade 100/100 |
| Set 2026 | 🛡️ Segurança como infraestrutura — captcha em todos os pontos de entrada, isolamento de dados pessoais, rate limiting compartilhado |
| Set 2026 | 💬 Verificação por WhatsApp oficial (Meta, empresa verificada pelo CNPJ) |
| Set 2026 | 📧 E-mail corporativo com domínio autenticado (Google Workspace + SPF/DKIM/DMARC) |
| Set 2026 | 🔁 Android e iOS lançados na mesma versão (1.0.246) |
| Set 2026 | 🔒 Segurança em padrão internacional — CAA, MTA-STS, CSP sem exceções, LGPD + GDPR |
| Set 2026 | ✉️ E-mail transacional em produção no Amazon SES (saiu do sandbox) |
| Out 2026 | 🔍 Postura de segurança auditada — controle de acesso por linha confirmado em todo o banco, inventário de componentes (SBOM) gerado a cada build |

## O que é

O Mesapra2 cruza três coisas que hoje existem separadas: matching social (tipo Tinder), agenda de encontros em grupo (tipo Partiful) e um clube de benefícios com restaurantes parceiros. O usuário cria ou entra num evento, é aprovado pelo anfitrião, conversa no chat do evento, participa — e sai dali com reputação e recompensas resgatáveis nos parceiros.

- 📱 Apps nativos (Android + iOS) via Capacitor, mais webapp
- 🌎 App Android distribuído oficialmente em 6 países: Brasil, Argentina, Chile, Colômbia, Uruguai e Venezuela — iOS publicado na App Store
- 🛡️ Compliance como diferencial: KYC obrigatório, modo de segurança em encontros, trust score
- 💳 Dois modelos de assinatura (social Premium / Club de benefícios) + monetização com parceiros

## Stack principal

[![React](https://img.shields.io/badge/React-0D9488?style=for-the-badge&logo=react&logoColor=white)]() [![Vite](https://img.shields.io/badge/Vite-0D9488?style=for-the-badge&logo=vite&logoColor=white)]() [![Capacitor](https://img.shields.io/badge/Capacitor-0D9488?style=for-the-badge&logo=capacitor&logoColor=white)]() [![TailwindCSS](https://img.shields.io/badge/TailwindCSS-0D9488?style=for-the-badge&logo=tailwindcss&logoColor=white)]() [![Node js](https://img.shields.io/badge/Node%20js-0D9488?style=for-the-badge&logo=nodedotjs&logoColor=white)]() [![i18next](https://img.shields.io/badge/i18next-0D9488?style=for-the-badge&logo=i18next&logoColor=white)]()

**Backend:** Node.js (funções serverless) · PostgreSQL via Supabase (Auth + RLS)
**Infra:** Vercel (deploy + edge functions) · CI/CD bloqueante (build, contratos, testes, cobertura)

## Arquitetura em 5 grupos

O sistema é organizado em 16 domínios funcionais, agrupados em 5 clusters. O **Núcleo** (Eventos + Rede Social) é o motivo do app existir — os outros quatro grupos existem para sustentar, monetizar, divulgar e operar o núcleo.

```mermaid
flowchart TD
    subgraph Confianca["🛡️ Confiança"]
        Auth["Auth & KYC"]
        Seg["Segurança"]
        Gam["Gamificação & Reputação"]
    end

    subgraph Nucleo["🎯 Núcleo do produto"]
        Eventos["Eventos"]
        Social["Rede Social"]
    end

    subgraph Dinheiro["💰 Dinheiro"]
        Pag["Pagamentos & Assinaturas"]
        Partners["Partners (Restaurantes)"]
        Club["Club Social"]
    end

    subgraph Alcance["📣 Alcance"]
        Notif["Notificações & Marketing"]
        Growth["Growth & Analytics"]
        Blog["Blog institucional"]
    end

    subgraph Operacao["⚙️ Operação"]
        Admin["Admin interno"]
        Suporte["Suporte (IA + humano)"]
        Webhooks["Webhooks & Integrações"]
        QA["QA / Infra interna"]
    end

    Confianca -->|"só quem passou por KYC/reputação participa"| Nucleo
    Nucleo -->|"concluir evento gera recompensa resgatável"| Dinheiro
    Nucleo -->|"toda ação relevante dispara notificação"| Alcance
    Operacao -.->|"aprova, modera, observa"| Confianca
    Operacao -.-> Nucleo
    Operacao -.-> Dinheiro
    Operacao -.-> Alcance
```

## Domínios funcionais

| Cluster | Domínio | O que faz |
|---|---|---|
| 🎯 Núcleo | **Eventos** | Criar, candidatar-se, aprovar, chat do evento, conclusão e avaliação |
| 🎯 Núcleo | **Rede Social** | Conexão 1:1 fora de evento, com chat direto e prazo de validade |
| 🛡️ Confiança | **Auth & KYC** | Login social, verificação de telefone e de identidade (KYC) |
| 🛡️ Confiança | **Segurança** | Blacklist, modo de segurança com compartilhamento de local, rate limiting |
| 🛡️ Confiança | **Gamificação & Reputação** | XP, nível, conquistas, trust score com penalidade por no-show |
| 💰 Dinheiro | **Pagamentos & Assinaturas** | Checkout, webhooks de pagamento, planos e cupons |
| 💰 Dinheiro | **Partners** | Cadastro de restaurantes, cardápio, sistema de recompensas |
| 💰 Dinheiro | **Club Social** | Benefícios corporativos via empresa parceira |
| 📣 Alcance | **Notificações & Marketing** | Push, e-mail transacional, campanhas, banners |
| 📣 Alcance | **Growth & Analytics** | Atribuição de origem, testes A/B |
| 📣 Alcance | **Blog institucional** | Conteúdo para SEO e social proof |
| ⚙️ Operação | **Admin interno** | Kanban, aprovações, painel de risco |
| ⚙️ Operação | **Suporte** | Canal de atendimento com IA de primeira linha + escalonamento humano |
| ⚙️ Operação | **Webhooks & Integrações** | Cola com provedores externos de pagamento |
| ⚙️ Operação | **QA / Infra interna** | Ferramentas de teste e verificação |

## Fluxos em destaque

### 🍽️ Partner (restaurantes)
Parceiro se cadastra → admin aprova → monta cardápio → cria recompensa → usuário resgata no balcão.
- Cadastro, solicitação e aprovação de parceiro
- Cardápio por categorias/itens, com importação em lote
- Restaurantes favoritos do usuário
- Criar/desativar vantagem (recompensa) e validar o resgate no balcão

### 🤝 Social (Topando + Reservas)
Publicar intenção → demonstrar interesse → aceitar → chat direto com validade — ou ir direto pelo fluxo de reserva (/reservar), escolhendo o restaurante primeiro.
- Chat direto com prazo de expiração, avaliação e bloqueio
- Roleta de prêmios do Topando, resgatável no parceiro
- Reserva de mesa reservation-first: busca por restaurante/bairro/cidade
- Indicação de amigos (referral) com código próprio

### 🏢 Club Social (institucional / corporativo)
Empresa parceira cadastrada → funcionário entra com código corporativo → usa benefícios → acompanha economia acumulada.
- Programa de benefícios para empresas (não é rede social — só vantagens em restaurantes)
- Trial do Club e troca para o plano pago
- Códigos de resgate gerados em lote pelo admin
- Dashboard de uso por empresa

## Ecossistema de integrações

Mapeado direto do código (dependências reais do projeto + variáveis de ambiente), não é uma lista de intenção.

**💳 Pagamentos & Assinaturas**
[![Mercado Pago](https://img.shields.io/badge/Mercado%20Pago-0EA5E9?style=for-the-badge&logo=mercadopago&logoColor=white)]() [![RevenueCat](https://img.shields.io/badge/RevenueCat-0EA5E9?style=for-the-badge&logo=revenuecat&logoColor=white)]() [![Google Play Billing](https://img.shields.io/badge/Google%20Play%20Billing-0EA5E9?style=for-the-badge&logo=googleplay&logoColor=white)]() [![Apple In App Purchase](https://img.shields.io/badge/Apple%20In%20App%20Purchase-0EA5E9?style=for-the-badge&logo=apple&logoColor=white)]()

**🔐 Identidade & Segurança**
[![AWS Rekognition](https://img.shields.io/badge/AWS%20Rekognition-EF4444?style=for-the-badge&logo=amazonaws&logoColor=white)]() [![AWS Textract](https://img.shields.io/badge/AWS%20Textract-EF4444?style=for-the-badge&logo=amazonaws&logoColor=white)]() [![AWS Amplify Face Liveness](https://img.shields.io/badge/AWS%20Amplify%20Face%20Liveness-EF4444?style=for-the-badge&logo=awsamplify&logoColor=white)]() [![Google Cloud Vision](https://img.shields.io/badge/Google%20Cloud%20Vision-EF4444?style=for-the-badge&logo=googlecloud&logoColor=white)]() [![hCaptcha](https://img.shields.io/badge/hCaptcha-EF4444?style=for-the-badge&logo=hcaptcha&logoColor=white)]() [![Apple Sign In](https://img.shields.io/badge/Apple%20Sign%20In-EF4444?style=for-the-badge&logo=apple&logoColor=white)]() [![Google Sign In](https://img.shields.io/badge/Google%20Sign%20In-EF4444?style=for-the-badge&logo=google&logoColor=white)]() [![Facebook Login](https://img.shields.io/badge/Facebook%20Login-EF4444?style=for-the-badge&logo=facebook&logoColor=white)]() [![Didit KYC (fallback)](https://img.shields.io/badge/Didit%20KYC%20(fallback)-EF4444?style=for-the-badge)]()

**💬 Comunicação**
[![WhatsApp Business API](https://img.shields.io/badge/WhatsApp%20Business%20API-22C55E?style=for-the-badge&logo=whatsapp&logoColor=white)]() [![AWS SES](https://img.shields.io/badge/AWS%20SES-22C55E?style=for-the-badge&logo=amazonaws&logoColor=white)]() [![Firebase Cloud Messaging](https://img.shields.io/badge/Firebase%20Cloud%20Messaging-22C55E?style=for-the-badge&logo=firebase&logoColor=white)]() [![Twilio SMS (fallback)](https://img.shields.io/badge/Twilio%20SMS%20(fallback)-22C55E?style=for-the-badge&logo=twilio&logoColor=white)]() [![Nodemailer SMTP IMAP](https://img.shields.io/badge/Nodemailer%20SMTP%20IMAP-22C55E?style=for-the-badge)]()

**🗺️ Mapas & Localização**
[![Google Maps Places](https://img.shields.io/badge/Google%20Maps%20Places-F59E0B?style=for-the-badge&logo=googlemaps&logoColor=white)]() [![OpenStreetMap Nominatim](https://img.shields.io/badge/OpenStreetMap%20Nominatim-F59E0B?style=for-the-badge&logo=openstreetmap&logoColor=white)]() [![OSRM Routing](https://img.shields.io/badge/OSRM%20Routing-F59E0B?style=for-the-badge)]()

**🤖 Inteligência Artificial**
[![OpenAI](https://img.shields.io/badge/OpenAI-A855F7?style=for-the-badge&logo=openai&logoColor=white)]() [![Groq](https://img.shields.io/badge/Groq-A855F7?style=for-the-badge)]()

**🗄️ Dados & Infraestrutura**
[![Supabase](https://img.shields.io/badge/Supabase-64748B?style=for-the-badge&logo=supabase&logoColor=white)]() [![PostgreSQL](https://img.shields.io/badge/PostgreSQL-64748B?style=for-the-badge&logo=postgresql&logoColor=white)]() [![Upstash Redis](https://img.shields.io/badge/Upstash%20Redis-64748B?style=for-the-badge&logo=redis&logoColor=white)]() [![Vercel](https://img.shields.io/badge/Vercel-64748B?style=for-the-badge&logo=vercel&logoColor=white)]() [![Sentry](https://img.shields.io/badge/Sentry-64748B?style=for-the-badge&logo=sentry&logoColor=white)]() [![Firebase](https://img.shields.io/badge/Firebase-64748B?style=for-the-badge&logo=firebase&logoColor=white)]()

**🏢 Produtividade & Operação**
[![Google Workspace](https://img.shields.io/badge/Google%20Workspace-8B5CF6?style=for-the-badge&logo=googleworkspace&logoColor=white)]()

**📊 Analytics**
[![Google Analytics 4](https://img.shields.io/badge/Google%20Analytics%204-EC4899?style=for-the-badge&logo=googleanalytics&logoColor=white)]()

Login do Facebook e WhatsApp Business API rodam sob um app registrado na Meta (Meta Business Suite) — Login já publicado em produção, WhatsApp com webhook e caixa de mensagens ativos. AWS (Rekognition, Textract, Amplify, SES) é hoje o caminho principal para identidade e e-mail transacional; Didit e Twilio seguem ativos como fallback.

## Confiabilidade e Compliance

Alinhado na prática com 7 padrões de mercado de segurança e qualidade de software (selos públicos em mesapra2.com — "aligned in practice", não certificação formal, que depende de auditoria externa):

![ISO 25010:2023: Aligned in practice](https://img.shields.io/badge/ISO%2025010%3A2023-Aligned%20in%20practice-3B82F6?style=for-the-badge) ![NIST SSDF 1.1: Aligned in practice](https://img.shields.io/badge/NIST%20SSDF%201.1-Aligned%20in%20practice-3B82F6?style=for-the-badge) ![OWASP ASVS 4.0.3: Aligned in practice](https://img.shields.io/badge/OWASP%20ASVS%204.0.3-Aligned%20in%20practice-3B82F6?style=for-the-badge) ![CVSS v3.1: Implemented in production](https://img.shields.io/badge/CVSS%20v3.1-Implemented%20in%20production-10B981?style=for-the-badge) ![SLSA / SBOM: Implemented](https://img.shields.io/badge/SLSA%20%2F%20SBOM-Implemented-10B981?style=for-the-badge) ![LGPD: Aligned](https://img.shields.io/badge/LGPD-Aligned-3B82F6?style=for-the-badge) ![ISO 27001: Aligned](https://img.shields.io/badge/ISO%2027001-Aligned-3B82F6?style=for-the-badge)

| Padrão | Escopo | Status |
|---|---|---|
| ISO 25010:2023 | Qualidade de software | Aligned in practice |
| NIST SSDF 1.1 | Desenvolvimento seguro de software | Aligned in practice |
| OWASP ASVS 4.0.3 | Segurança de aplicação | Aligned in practice |
| CVSS v3.1 | Classificação de risco de vulnerabilidade | Implemented in production |
| SLSA / SBOM | Segurança da cadeia de suprimento de software | Implemented |
| LGPD | Dados pessoais e privacidade | Aligned |
| ISO 27001 | Prontidão de segurança da informação | Aligned |

## Documentos institucionais

![CNPJ: Ativo](https://img.shields.io/badge/CNPJ-Ativo-1E3A8A?style=for-the-badge) ![Inova Simples: Lei 167/2019](https://img.shields.io/badge/Inova%20Simples-Lei%20167%2F2019-1E3A8A?style=for-the-badge) ![INPI: Marca depositada](https://img.shields.io/badge/INPI-Marca%20depositada-009739?style=for-the-badge) ![D-U-N-S: Dun and Bradstreet](https://img.shields.io/badge/D--U--N--S-Dun%20and%20Bradstreet-1E3A8A?style=for-the-badge) ![Apple Developer: Program](https://img.shields.io/badge/Apple%20Developer-Program-1E3A8A?style=for-the-badge&logo=apple&logoColor=white)

Empresa formalizada, com marca **depositada** no INPI (consulta pública disponível na base do INPI — registro concedido é etapa posterior ao depósito).

## Status

Em produção — Android distribuído oficialmente em 6 países (Brasil, Argentina, Chile, Colômbia, Uruguai, Venezuela) via Play Store; iOS publicado na App Store. Catálogo com milhares de restaurantes mapeados em 7 países (a maior parte aguardando o dono reivindicar o perfil) — os números de parceiros com atividade real sobem junto com os ciclos de expansão comercial.

## Sobre este repositório

Mantido como referência pública de arquitetura. Issues e PRs não são aceitos aqui — para contato, veja [mesapra2.com](https://mesapra2.com).
