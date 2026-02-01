# Asterisk-SRTP
Projeto direcionado à disciplina da Redes Convergentes com intuito de realizar a configuração do asterisk utilizando SRTP para o SIP

# Asterisk com SRTP utilizando Docker

Este projeto implementa um ambiente de **telefonia IP segura** utilizando **Asterisk ** com **SRTP (Secure RTP)**, executado em contêiner Docker.  
O objetivo é demonstrar, de forma prática, a comunicação SIP segura entre softphones, integrando conceitos de **VoIP, criptografia de mídia e conteinerização**.

---

## Objetivo

Permitir o aprofundamento prático nos conceitos de VoIP e segurança da informação por meio da configuração e validação de:

- SIP utilizando PJSIP
- Criptografia de mídia com SRTP
- Ambiente isolado e reproduzível com Docker
- Testes de chamadas entre dois endpoints SIP

---

## Arquitetura do Ambiente

- **Servidor VoIP:** Asterisk 18
- **Sinalização:** SIP (UDP)
- **Mídia:** RTP / SRTP
- **Softphones:** Linphone e Zoiper
- **Containerização:** Docker e Docker Compose
- **Sistema operacional:** Debian 12 / Ubuntu

---

## Estrutura de Diretórios
``
Asterisk-SRTP/
├── docker-compose.yml
└── asterisk/
├── pjsip.conf
├── extensions.conf
├── modules.conf
└── rtp.conf


---

## Docker

O Asterisk é executado utilizando a imagem do Docker Hub:

andrius/asterisk

yaml

Portas utilizadas:
- **5060/UDP** – SIP
- **10000–20000/UDP** – RTP / SRTP

---

## Endpoints SIP Configurados

| Ramal | Usuário | Descrição |
|-----|--------|-----------|
| 1001 | 1001 | Softphone A |
| 1002 | 1002 | Softphone B |

Cada endpoint utiliza:
- Autenticação por usuário e senha
- Transporte UDP
- Criptografia de mídia via SRTP

---

## SRTP

O SRTP é utilizado para proteger o tráfego de áudio, garantindo confidencialidade e integridade da comunicação.  
O módulo `res_srtp.so` encontra-se carregado e em execução no Asterisk.

---

## Como Executar

### Subir o container
```bash
sudo docker compose up -d
Acessar o CLI do Asterisk
bash

sudo docker exec -it asterisk_srtp asterisk -rvvv

Verificar endpoints registrados
asterisk
Copiar código
pjsip show endpoints
Configuração do Linphone
Criar uma conta SIP com os seguintes parâmetros:

Usuário: 1001 ou 1002

Senha: conforme definido em pjsip.conf

Servidor: IP da VM (exemplo: 192.168.134.129)

Transporte: UDP

Criptografia: SRTP habilitado

Importante: desativar o SIP local do Linphone para evitar conflito com a porta 5060.

Testes Realizados
Registro simultâneo de dois softphones

Chamada entre os ramais 1001 e 1002

Comunicação de áudio bidirecional

Validação de SRTP ativo

Problemas e Soluções
Conflito de porta 5060: causado por SIP local ativo no softphone

Chamadas encerrando imediatamente: ajustes de SRTP e NAT

Falha no registro SIP: correção de transporte e credenciais

Tecnologias Utilizadas
Asterisk 

PJSIP

SRTP

Docker

Docker Compose

Linphone

Zoiper

Autor
José Ryann
Projeto desenvolvido para fins acadêmicos, integrando teoria e prática em VoIP e Segurança da Informação.

