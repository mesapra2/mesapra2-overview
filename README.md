# Mesapra2

**Social dining com curadoria gastronômica.** Conecta pessoas através de eventos em restaurantes parceiros — matching social + agenda de encontros em grupo + benefícios com curadoria de restaurantes, num produto só.

> 🔒 Este repositório é uma vitrine pública da arquitetura do produto. **Não contém código-fonte** — o app em produção vive num repositório privado. O objetivo aqui é mostrar como o sistema é organizado, não como ele é implementado.

---

## O que é

O Mesapra2 cruza três coisas que hoje existem separadas: matching social (tipo Tinder), agenda de encontros em grupo (tipo Partiful) e um clube de benefícios com restaurantes parceiros. O usuário cria ou entra num evento, é aprovado pelo anfitrião, conversa no chat do evento, participa — e sai dali com reputação e recompensas resgatáveis nos parceiros.

- 📱 Apps nativos (Android + iOS) via Capacitor, mais webapp
- 🌎 Multi-país desde o desenho (Brasil, Argentina, Uruguai, Colômbia em rollout)
- 🛡️ Compliance como diferencial: KYC obrigatório, modo de segurança em encontros, trust score
- 💳 Dois modelos de assinatura (social Premium / Club de benefícios) + monetização com parceiros

## Arquitetura em 5 grupos

O sistema é organizado em 16 domínios funcionais, agrupados em 5 clusters. O **Núcleo** (Eventos + Rede Social) é o motivo do app existir — os outros quatro grupos existem para sustentar, monetizar, divulgar e operar o núcleo.

\`\`\`mermaid
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
\`\`\`

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

## Stack

**Cliente:** React + Vite, Capacitor (Android/iOS), TailwindCSS, i18next (pt-BR/es/en)
**Backend:** Node.js (funções serverless), PostgreSQL via Supabase (Auth + RLS)
**Infra:** Vercel (deploy + edge functions), CI/CD bloqueante (build, contratos, testes, cobertura)
**Integrações:** Mercado Pago, RevenueCat (IAP Google Play/Apple), KYC via Didit, Google Maps/Places

## Status

Em produção — apps publicados na Play Store e App Store, operando no Brasil com expansão em andamento para Argentina, Uruguai e Colômbia.

## Sobre este repositório

Mantido como referência pública de arquitetura. Issues e PRs não são aceitos aqui — para contato, veja [mesapra2.com](https://mesapra2.com).
