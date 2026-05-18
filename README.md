# 🤖 Tohka Bot — Sistema Avançado de Automação e Gestão para WhatsApp

<div align="center">
  <img src="https://upload.tohka.com.br/storage/C8P6HEEloXGPyvfNCQYj.jpeg" width="180" height="180" alt="Tohka Bot Logo" style="border-radius: 50%;" />
  <p><em>A solução definitiva em automação, moderação inteligente e entretenimento para WhatsApp.</em></p>
  
  <p>
    <img src="https://img.shields.io/badge/Node.js-v18+-green?style=for-the-badge&logo=node.js" alt="Node.js version" />
    <img src="https://img.shields.io/badge/Baileys-WA__Socket-blue?style=for-the-badge" alt="Baileys Core" />
    <img src="https://img.shields.io/badge/Ambiente-Linux%20%2F%20Docker-orange?style=for-the-badge&logo=linux" alt="Linux Guard" />
    <img src="https://img.shields.io/badge/Desenvolvido_por-Tohka_T.I-7700ff?style=for-the-badge" alt="Tohka T.I" />
  </p>
</div>

---

## 📑 Índice
- [🚀 Sobre o Projeto](#-sobre-o-projeto)
- [⚙️ Arquitetura e Tecnologias](#️-arquitetura-e-tecnologias)
- [🛡️ Sistemas de Segurança Centrais](#️-sistemas-de-segurança-centrais)
  - [1. Motor de Interceptação Antiflood](#1-motor-de-interceptação-antiflood)
  - [2. Sistema Híbrido de Mute via API](#2-sistema-híbrido-de-mute-via-api)
  - [3. Filtros de Mídia de Segurança (Anti e Only)](#3-filtros-de-mídia-de-segurança-anti-e-only)
- [🛠️ Pré-requisitos e Instalação](#️-pré-requisitos-e-instalação)
- [📈 Guia de Conexão Móvel](#-guia-de-conexão-móvel)
- [💾 Sincronização Dinâmica com API](#-sincronização-dinâmica-com-api)
- [📝 Estrutura de Logs Visuais no Console](#-estrutura-de-logs-visuais-no-console)
- [👑 Níveis de Permissão](#-níveis-de-permissão)
- [INFO - Resumo da atualização](#-resumo)

---

## 🚀 Sobre o Projeto

O **Tohka Bot** é um motor de automação robusto e inteligente para o WhatsApp, desenvolvido pela **Tohka T.I**. Criado sob uma arquitetura resiliente baseada na biblioteca `baileys`, o sistema vai além de um simples bot de respostas: ele atua como um administrador automatizado de grupos em tempo real, integrado a APIs externas para gestão de planos premium, segurança proativa contra invasões, e com suporte a ferramentas de Inteligência Artificial para processamento inteligente de interações.

---

## ⚙️ Arquitetura e Tecnologias

A infraestrutura foi construída focando em performance, baixo consumo de memória e modularidade:

* **Motor Principal:** `baileys` — Conexão nativa e otimizada via WebSocket com servidores do WhatsApp.
* **Controlo de Fluxo e Estado:** `useMultiFileAuthState` para persistência segura das credenciais de autenticação (geradas na pasta `auth_info`).
* **Gerenciamento de Tempo:** `moment-timezone` configurado para sincronização estrita com o fuso horário `America/Sao_Paulo`.
* **Tratamento Gráfico de Console:** `chalk` e `colors` para formatação visual e diagnósticos em tempo real no terminal.
* **Comunicação I/O:** `axios` e `node-fetch` para requisições assíncronas de alta performance com o backend da **Tohka**.

---

## 🛡️ Sistemas de Segurança Centrais

### 1. Motor de Interceptação Antiflood
O sistema possui uma proteção interna baseada em memória transitória (`Map`) que analisa e mitiga envios massivos de mensagens por usuários mal-intencionados.

* **Comando:** `!antiflood` (Exclusivo para Admins / Donos)
* **Sintaxe de Ativação:** `!antiflood 1 [tempo em segundos] [limite de msgs] [ação]`
* **Ações Configuráveis:**
    * `apagar`: Remove as mensagens do infrator automaticamente e emite um alerta visual.
    * `mutar1`: Aplica um bloqueio silencioso de 10 minutos (todas as mensagens enviadas no período serão apagadas).
    * `mutar2`: Aplica um bloqueio severo de 10 minutos (se o usuário tentar enviar qualquer conteúdo, será banido na hora).
    * `banir`: Expulsão imediata do participante do grupo.

### 2. Sistema Híbrido de Mute via API
Integração via webhook com o ecossistema `bot.tohka.com.br` para listagem e aplicação de silenciamento persistente:
* `!mutar @usuário [Ação: 1 ou 2] [Tempo em minutos]` — Envia a instrução de castigo direto para o banco de dados centralizado.
* `!desmutar @usuário` — Revoga a punição na API e restaura os privilégios de fala do usuário no grupo.
* `!mutados` — Exibe a listagem completa dos participantes que estão sob restrição de escrita.

### 3. Filtros de Mídia de Segurança (Anti e Only)
Gerenciamento granular e em tempo real do tipo de mídia que pode circular nos grupos:
* **Modo Anti:** Bloqueia mídias indesejadas específicas. Se ativado para nível 1 (`Ação 1`), apenas deleta a mídia (Imagem, Vídeo, Áudio ou Sticker). Se ativado para nível 2 (`Ação 2`), deleta o arquivo e bane o remetente.
* **Modo Only:** Restringe o grupo a aceitar **apenas** um tipo de formato (Ex: `OnlyFig` permite apenas figurinhas, apagando ou banindo quem enviar texto ou outra mídia).

---

## 🛠️ Pré-requisitos e Instalação

O bot foi validado e otimizado para rodar em ambientes Linux (BigLinux/Ubuntu/Debian) e servidores de nuvem de alta disponibilidade (Railway, VPS, Docker).

### Instalação de Dependências do Sistema

Certifique-se de possuir o Node.js v18 ou superior instalado. No terminal Linux, execute:

```bash

git clone https://github.com/riqtownley/tohka.git
cd tohka

# Instale os pacotes e dependências listadas no package.json
npm install
📈 Guia de Conexão Móvel
A Tohka Bot oferece flexibilidade total para sincronização inicial. Ao iniciar com num start, se nenhuma sessão anterior for encontrada na pasta auth_info, o sistema disponibilizará um menu interativo de escolha no terminal:

Plaintext
====== TOHKA BOT - CONEXÃO ======
[0] Conectar via QR CODE
[1] Conectar via PAIRING CODE (Número de Telefone)
Opção [0] (QR Code): Gera um código QR miniaturizado diretamente no terminal através da biblioteca qrcode-terminal. Basta escanear com a câmera do WhatsApp em "Aparelhos Conectados".

Opção [1] (Pairing Code): Permite inserir o número do Bot no formato internacional (ex: 5516999999999). O bot irá solicitar um código alfa-numérico de 8 dígitos à API do WhatsApp e exibirá no terminal para digitação manual no telemóvel, ideal para servidores VPS que não renderizam bem o QR Code.

💾 Sincronização Dinâmica com API
Para conseguir a sua chave, acesse https://bot.tohka.com.br e crie uma conta!

O bot inicializa fazendo um handshake de autenticação e carregamento de configurações com o backend em ./database/src/api/auth.js e atualizações de grupos através do módulo GroupService.

As chaves do ecossistema e credenciais mestre de validação de licenças residem em ./database/dono/dono.json. Certifique-se de que o arquivo esteja preenchido corretamente:

JSON
{
  "key": "SUA_API_KEY_AQUI"
}

📝 Estrutura de Logs Visuais no Console
As interações que passam pelo gateway de execução geram logs estilizados com delimitação de bordas hexadecimais (#7700ff), facilitando o monitoramento de atividade:

Exemplo de Comando Recebido:

╭────〔 👥 COMANDO EM GRUPO 〕────╮
│ Comando: antiflood
│ Usuário: Henrique (Townley)
│ Número: 5516994588660
│ Grupo: Suporte Comercial Tohka T.I
│ Hora: 14:32:10
│ Dono: Sim
╰──────────────────────────────╯

👑 Níveis de Permissão
Para garantir a integridade dos comandos, o sistema segmenta as chamadas sob uma hierarquia estrita de controle:

Criador/Desenvolvedor (Criador): Acesso irrestrito a todas as funções de infraestrutura profunda do código.

Dono do Bot (MeuNumero / Dono1 ao Dono4): Permissões administrativas globais para ligar/desligar o bot (BotOff) e ignorar restrições de grupos.

Usuários Premium/VIP (isVip): Liberação para execução de comandos restritos configurados na tabela comandosPremium do painel web.

Administradores do Grupo (isSenderAdmin): Controlo total sobre ferramentas de moderação interna do grupo local (Mute, Antiflood, Ban, Filtros).

Membros Gerais: Acesso exclusivo a comandos de entretenimento, utilitários e interações básicas autorizadas.

Resumo da atualização: 1.2.1
Nessa atualização, foi adicionado o sistema de AntiPV e foi arrumado o sistema de conexão.
