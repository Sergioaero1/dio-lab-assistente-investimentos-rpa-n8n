# Assistente Inteligente de Investimentos com RPA, n8n e Gemini
## 📌 Sobre o Projeto
Este projeto foi desenvolvido como desafio prático utilizando RPA (Robotic Process Automation), Python, n8n e Inteligência Artificial Generativa (Google Gemini).

O objetivo é automatizar a análise de clientes investidores, identificar oportunidades compatíveis com seu perfil de risco e enviar recomendações personalizadas por e-mail.

# N8N AUTOMAÇÃO:

<img width="1696" height="791" alt="image" src="https://github.com/user-attachments/assets/b9a595a5-f454-442f-a6a4-04969dea526e" />


## 🚀 Tecnologias Utilizadas
Python
Requests, 
BeautifulSoup, 
Google Colab, 
GitHub, 
GitHub Pages, 
CSV, 
n8n, 
Docker, 
Ngrok, 
Google Gemini AI, 
Gmail API, 
## 🏗️ Arquitetura da Solução
Python (RPA)
      ↓
Webhook n8n
      ↓
HTTP Request
      ↓
Leitura CSV de investimentos
      ↓
Tratamento dos dados
      ↓
Recomendação personalizada
      ↓
Loop Over Items
      ↓
Google Gemini
      ↓
Padronização do e-mail
      ↓
Gmail
      ↓
Resposta do Webhook
## 📋 Fluxo Completo da Automação
### 1. Coleta dos Clientes
Foi desenvolvido um script Python responsável por:

acessar uma página HTML hospedada no GitHub Pages;
realizar Web Scraping;
capturar:
nome;
e-mail;
saldo;
perfil do investidor;
montar um JSON com os clientes;
enviar os dados para o n8n através de um Webhook.
Exemplo de cliente
{
  "nome": "Ana Silva",
  "email": "ana@email.com",
  "saldo": "R$ 12.500,00",
  "perfil": "Conservador"
}

### 2. Recebimento dos Dados no n8n
O Webhook recebe os clientes enviados pelo script Python.

### 3. Consulta da Base de Investimentos
A automação realiza um HTTP Request para consumir um arquivo CSV hospedado no GitHub.

A base contém:

perfil do investidor;
produto recomendado;
valor mínimo;
rentabilidade estimada.
Exemplo:

perfil,produto,minimo,rentabilidade
Conservador,CDB Liquidez Diária,1000,12.5%
Moderado,Fundo Multimercado,5000,16.0%
Arrojado,ETF de Ações,10000,22.0%
### 4. Processamento do CSV
Foi utilizado um nó Code para:

converter CSV em JSON;
normalizar os campos;
transformar o valor mínimo em número;
gerar uma estrutura utilizável pelo fluxo.
### 5. Cruzamento dos Dados
Utilizando o nó Merge:

clientes;
investimentos;
foram combinados para identificar a aplicação adequada para cada perfil.

### 6. Geração da Recomendação
Um nó Code realiza:

análise do perfil;
validação de valor mínimo;
recomendação do produto;
criação de mensagens personalizadas;
geração de um briefing para IA.

### 7. Uso de Inteligência Artificial
Foi integrado o Google Gemini para gerar e-mails personalizados.

## Prompt utilizado:

Você é um assessor de investimentos.

Com base nos dados abaixo gere um e-mail.

{{$json.briefing}}

Retorne EXATAMENTE neste formato:

ASSUNTO: texto do assunto

CORPO:
texto do e-mail
### 8. Controle de Limite do Gemini
Durante os testes ocorreu erro de Rate Limit:

The service is receiving too many requests from you
O problema foi resolvido utilizando:

Loop Over Items
Processando:

1 cliente por vez
em vez de enviar todos simultaneamente para a IA.

### 9. Padronização dos E-mails
Após a resposta do Gemini foi criado um nó Code responsável por:

extrair assunto;
extrair corpo do e-mail;
normalizar o formato;
preparar a mensagem para envio no Gmail.

### 10. Envio Automático
Os e-mails são enviados automaticamente utilizando:

Gmail OAuth2 API
Cada cliente recebe uma recomendação personalizada baseada em:

perfil;
saldo disponível;
produto recomendado;
rentabilidade estimada.

## 🔧 Problemas Encontrados e Soluções
Problema 1
Erro:

### Connection Refused
Causa
O Google Colab tentava acessar:

localhost:5678
que existe apenas na máquina local.

### Solução
Foi utilizado o Ngrok para expor o n8n à internet.

### Problema 2
Erro:

Expected property name or '}'
Causa
JSON inválido configurado no node Respond to Webhook.

### Solução
Configuração correta do Response Body utilizando Expression.

### Problema 3
Erro:

$html retornando no processamento CSV
Causa
Uso da URL do GitHub:

github.com/.../blob/...
### Solução
Troca para URL RAW:

raw.githubusercontent.com
### Problema 4
Erro:

$json.clientes is not iterable
Causa
Estrutura incorreta do JSON recebido.

### Solução
Alteração para:

$json.body.clientes
### Problema 5
Erro:

The service is receiving too many requests from you
Causa
10 clientes sendo enviados simultaneamente ao Gemini.

### Solução
Implementação do:
Loop Over Items
Processando um cliente por vez.

✅ Resultados Obtidos
Captura automática de clientes;
Integração com Webhook;
Consumo de CSV remoto;
Recomendação de investimento;
Integração com Google Gemini;
Geração automática de e-mails;
Envio via Gmail;
Fluxo totalmente automatizado.
📚 Aprendizados
Durante o desenvolvimento foram praticados conceitos de:

# RPA;
Web Scraping;
APIs;
Webhooks;
Integração de sistemas;
Manipulação de CSV;
Docker;
n8n;
Engenharia de Prompt;
Inteligência Artificial Generativa;
Automação de processos.
```
# 👨‍💻 Autor
Sergio Aero

Projeto desenvolvido como atividade prática da formação em Automação e IA utilizando n8n.
