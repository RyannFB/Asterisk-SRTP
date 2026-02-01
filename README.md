# Asterisk com SRTP utilizando Docker

## 📌 Visão Geral

Este projeto implementa um **servidor VoIP Asterisk com suporte a SRTP (Secure RTP)**, utilizando **Docker e Docker Compose**, permitindo chamadas SIP com **sinalização e mídia protegidas**.

O objetivo é demonstrar, de forma prática, a aplicação de conceitos de **VoIP**, **SIP**, **PJSIP**, **SRTP**, **NAT** e **conteinerização**, integrando teoria e prática em um ambiente controlado e reproduzível.

O cenário foi validado com os softphones **Linphone** e **Zoiper**, realizando chamadas entre ramais internos e testes de interoperabilidade.

---

## 🎯 Objetivos do Projeto

* Implementar um PBX Asterisk funcional em container
* Configurar ramais SIP utilizando **PJSIP**
* Garantir **criptografia de mídia com SRTP**
* Permitir chamadas entre ramais internos (ex: 1001 ↔ 1002)
* Documentar problemas reais e suas soluções
* Fornecer um ambiente simples, reproduzível e acadêmico

---

## 🧱 Arquitetura da Solução

* **Asterisk 18** executando em container Docker
* Configuração via arquivos montados por volume
* Transporte SIP utilizando **UDP**
* Mídia protegida com **SRTP**
* Softphones externos conectando-se ao IP da VM

---

## 📁 Estrutura de Diretórios

```bash
Asterisk-SRTP/
├── docker-compose.yml
└── asterisk/
    ├── pjsip.conf
    ├── extensions.conf
    ├── modules.conf
    └── rtp.conf
```

Todos os arquivos de configuração do Asterisk são mantidos fora do container e montados via **bind mount**, facilitando edição, versionamento e troubleshooting.

---

## ⚙️ Pré-requisitos

```bash
- Docker
- Docker Compose
- Sistema Linux (testado em Debian 12 / Ubuntu)
- Softphone SIP (Linphone ou Zoiper)
```

---

## 🚀 Subindo o Ambiente

```bash
sudo docker compose up -d
```

Verificar se o container está em execução:

```bash
sudo docker ps
```

Acessar o console do Asterisk:

```bash
sudo docker exec -it asterisk_srtp asterisk -rvvv
```

---

## 📞 Configuração dos Ramais

Os ramais são definidos no arquivo `pjsip.conf` utilizando:

* Endpoint
* Auth
* AOR
* Transporte UDP
* SRTP habilitado

Exemplo de ramais configurados:

```bash
1001
1002
```

---

## 📡 Plano de Discagem

O arquivo `extensions.conf` define o plano de discagem básico, permitindo chamadas diretas entre os ramais internos.

Exemplo:

```bash
1001 → 1002
1002 → 1001
```

---

## 🔐 SRTP (Secure RTP)

A criptografia da mídia é garantida por:

* Módulo `res_srtp.so`
* Configuração correta no `pjsip.conf`
* Range RTP definido no `rtp.conf`

Verificação do módulo:

```bash
module show like srtp
```

---

## 🧪 Testes Realizados

* Registro de múltiplos ramais SIP
* Chamadas entre dois softphones
* Testes com Linphone e Zoiper
* Validação de SRTP ativo
* Análise de falhas reais de NAT e porta

---

## ⚠️ Problemas Encontrados e Soluções

### 🔴 Conflito de Porta 5060

```bash
Causa:
- SIP local ativo no softphone

Solução:
- Desativar SIP local / SIP Helper / SIP ALG
- Garantir que apenas o Asterisk utilize a porta 5060
```

### 🔴 Chamadas Encerrando Imediatamente

```bash
Causa:
- Configuração incorreta de SRTP ou NAT

Solução:
- Ajuste do rtp.conf
- Correção do transporte SIP
- Verificação do IP e mídia
```

### 🔴 Ramais não Registravam

```bash
Causa:
- Transporte ou credenciais incorretas

Solução:
- Revisão de usuário, senha e AOR
- Verificação do transport-udp
```

---

## 🧰 Tecnologias Utilizadas

```bash
Asterisk        # PBX Open Source
PJSIP           # Stack SIP moderna
SRTP            # Criptografia de mídia
Docker          # Conteinerização
Docker Compose  # Orquestração
Linphone        # Softphone SIP
Zoiper          # Softphone SIP
```

---

## 🛠️ Comandos Úteis

```bash
# Subir o ambiente
sudo docker compose up -d

# Reiniciar o container
sudo docker compose restart

# Acessar o Asterisk
sudo docker exec -it asterisk_srtp asterisk -rvvv

# Ver endpoints
pjsip show endpoints

# Ver transportes
pjsip show transports

# Ver módulos SRTP
module show like srtp
```

---




Projeto desenvolvido para **fins acadêmicos**, integrando conceitos de:

* Redes de Computadores
* VoIP
* Segurança da Informação
* Criptografia de Mídia
* Ambientes Conteinerizados

---
