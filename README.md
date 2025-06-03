# 📱 n8n-whatsapp-sync-ai-reports

Automação desenvolvida com [n8n](https://n8n.io) para:
1. Capturar mensagens do WhatsApp via API personalizada.
2. Armazenar no Microsoft SQL Server.
3. Gerar relatórios personalizados com uso de Inteligência Artificial (OpenAI).
4. Enviar relatórios automaticamente por e-mail.

---

## 📦 Funcionalidades

### 🔁 1. Sincronização de Mensagens WhatsApp

Automação que realiza:
- Consulta do `last_id` no banco com base no `chat_id`.
- Requisições paginadas para a API local do WhatsApp (`WPPConnect` ou `open-wa`).
- Tratamento e deduplicação de mensagens.
- Inserção segura no banco (`IF NOT EXISTS`).
- Paginação até final das mensagens antigas (via `last_id`).

> Implementações paralelas com dois modelos:
> - Via **Merge + IF Loop** (modo iterativo)
> - Via **encadeamento simples com fetch único** (para performance)

### 🧠 2. Análise e Geração de Relatórios com IA

Fluxo adicional:
- Recebe input de texto com pergunta sobre conversas.
- Gera consulta SQL usando **GPT-4**.
- Executa a query no banco e coleta mensagens.
- Repassa contexto completo para nova análise com **GPT-4**.
- Gera relatório estruturado em HTML com IA.
- Envia e-mail com relatório renderizado.

---

## 🧰 Tecnologias

- **n8n** (automatizador de processos)
- **WPPConnect Server** ou **open-wa** (API do WhatsApp)
- **Microsoft SQL Server** (armazenamento)
- **OpenAI GPT-4** (análise de contexto e geração de relatório)
- **SMTP (Gmail/Outlook)** (envio de relatórios por email)

---

## ⚙️ Pré-requisitos

- Node.js e Docker instalados
- Conta na [OpenAI](https://platform.openai.com)
- Servidor do WhatsApp (via [WPPConnect](https://github.com/wppconnect-team/wppconnect-server))
- Acesso ao banco Microsoft SQL
- Conta SMTP configurada (Gmail, Outlook etc)

---

## 🚀 Instalação

```bash
# Clone o repositório
git clone https://github.com/sua-conta/n8n-whatsapp-sync-ai-reports.git

# Inicie o ambiente n8n (via Docker ou local)
n8n start

# Importe os dois fluxos (JSONs exportados do n8n)
```

---

## 📂 Estrutura dos Fluxos

### 🔹 Fluxo 1 – `whatsapp-sync`
- `Webhook (POST /whatsapp-sync)`
- `SQL Query (last_id)`
- `HTTP Request` para mensagens
- `JS Code` para tratamento e extração de dados
- `Insert` no SQL Server
- Loop com Merge + IF (quando necessário)

### 🔹 Fluxo 2 – `report-generator`
- `ChatTrigger` recebe mensagem com pergunta
- `GPT-4` transforma pergunta em SQL
- `SQL Execution`
- `GPT-4` analisa contexto retornado
- `GPT-4` gera HTML de relatório
- `Send Email` via SMTP

---

## 📥 Exemplo de Input para Análise

**Usuário:**
> Quais mensagens o cliente 5511989912822 enviou ontem com reclamação?

**Resposta esperada (IA):**
> Foram identificadas 3 mensagens do número informado no dia anterior contendo termos relacionados a reclamações...

---

## 🔐 Segurança

- Tokens de autenticação para API do WhatsApp
- Credenciais do banco e SMTP gerenciadas via **n8n Credentials**
- Consulta SQL segura com deduplicação via `IF NOT EXISTS`



## 📧 Exemplo de E-mail

> Assunto: `Relatório de Mensagens WhatsApp`
> Corpo: HTML personalizado com resultado da análise

---

## 👨‍💻 Autor

**Matheus Pedroso**
- GitHub: [@matheuspedroso](https://github.com/matheuspedroso)

---
