
# Ravens — Laboratórios de Redes & Segurança

<p align="center">
  <img src="https://github.com/user-attachments/assets/0d39a308-fe59-42da-91a2-78bba0a0936a" 
       alt="Ravens-Logo" 
       width="250">
</p>

---

## Grupo de Pesquisa RAVENS

O **RAVENS - Research Group on Advanced Vulnerabilities, Networks and Security** é um grupo de pesquisa dedicado ao estudo avançado de **cibersegurança**, **redes de computadores**, **vulnerabilidades**, **defesa cibernética** e **análise de ameaças**.

O grupo atua na interseção entre **pesquisa acadêmica**, **formação de recursos humanos** e **aplicações práticas**, desenvolvendo estudos, projetos e experimentos nas seguintes áreas:

- Segurança em redes de computadores;
- Detecção e mitigação de ataques cibernéticos;
- Análise de malware e ameaças avançadas;
- Cyber Threat Intelligence e Threat Hunting;
- Criptografia aplicada e protocolos seguros;
- Fraudes digitais e segurança bancária.

### Objetivos do RAVENS

- Produzir pesquisa científica aplicada em cibersegurança;
- Formar alunos de graduação e pós-graduação em temas avançados;
- Desenvolver soluções experimentais e provas de conceito;
- Integrar ensino, pesquisa e extensão;
- Contribuir para o fortalecimento da segurança cibernética nacional.

### 🔗 Repositórios e Recursos

- Repositórios de pesquisa e experimentos:  https://github.com/peotta/ravens

> Os laboratórios e projetos desenvolvidos no âmbito das disciplinas e da pesquisa científica estão alinhados às atividades do grupo **RAVENS**.


---

## Sumário
- [Visão Geral](#visão-geral)
- [Estrutura do Repositório](#estrutura-do-repositório)
- [Pré‑requisitos](#pré-requisitos)
- [Como usar](#como-usar)
- [Laboratórios](#laboratórios)
  - [Lab 1 — Enviando e‑mail via SMTP do Google](./labs/lab-dns.md)
  - [Lab — Criptografia Simétrica](./labs/lab-simetrica.md)
  - [Lab — Criptografia Assimétrica com OpenSSL](./labs/lab-assimetrica.md)
  - [Lab — DNS](#lab--dns)
- [Boas práticas & segurança](#boas-práticas--segurança)
- [Referências recomendadas](#referências-recomendadas)
- [Como contribuir](#como-contribuir)
- [Licença](#licença)

---

## Visão Geral
Este repositório reúne **roteiros de laboratório** usados em disciplinas de graduação e pós‑graduação. Os exercícios priorizam ferramentas disponíveis em qualquer Linux/macOS (ou WSL), como `openssl`, `dig`/`nslookup` e bibliotecas padrão de envio de e‑mail.

> *Contexto histórico:* DNS surge nos anos 1980 para resolver nomes na ARPANET; os algoritmos assimétricos (Diffie–Hellman/RSA) datam dos anos 1970; os esquemas simétricos modernos (como AES) consolidam‑se no fim dos anos 1990. Compreender esse percurso ajuda a conectar tecnologia e História da Computação.

---

## Estrutura do Repositório
```
ravens/
├── Lab1:Laboratório: Enviando e-mail via SMTP do Google.md
├── Lab: Criptografia Simétrica.md
├── Laboratório Criptografia Assimétrica com OpenSSL.md
└── Lab:DNS.md
```

Links diretos:
- [SMTP/Gmail](Lab1%3ALaborat%C3%B3rio%3A%20Enviando%20e-mail%20via%20SMTP%20do%20Google.md)
- [Criptografia Simétrica](Lab%3A%20Criptografia%20Sim%C3%A9trica.md)
- [Criptografia Assimétrica com OpenSSL](Laborat%C3%B3rio%20Criptografia%20Assim%C3%A9trica%20com%20OpenSSL.md)
- [DNS](Lab%3ADNS.md)

---

## Pré‑requisitos
- **Sistema:** Linux, macOS ou Windows 10/11 com **WSL**.
- **Ferramentas:** `openssl`, `bind9-dnsutils`, `python3` (opcional)
- **Conta Google** com autenticação em 2 etapas e **senha de app** para o laboratório de SMTP.

Instalação rápida (Debian/Ubuntu):
```bash
sudo apt update && sudo apt install -y openssl dnsutils python3
```

---

## Como usar
1. Clone o repositório:
   ```bash
   git clone https://github.com/peotta/ravens.git
   cd ravens
   ```
2. Escolha o roteiro desejado e siga as instruções.
3. Registre evidências (saídas de comandos, prints, chaves, certificados etc.).

---

## Laboratórios

### Lab 1 — Enviando e‑mail via SMTP do Google
Arquivo: `Lab1:Laboratório: Enviando e-mail via SMTP do Google.md`

**Objetivo:** compreender o fluxo SMTP autenticado (STARTTLS) e boas práticas no uso do Gmail.

**Conteúdos:** criação de senha de app, handshake TLS, cabeçalhos SMTP, envio automatizado e verificação de autenticação.

---

### Lab — Criptografia Simétrica
Arquivo: `Lab: Criptografia Simétrica.md`

**Objetivo:** compreender o uso de AES e modos de operação (CBC, GCM).

**Conteúdos:** geração de chaves, IVs, PBKDF2, padding, integridade e riscos de reutilização de IV.

---

### Lab — Criptografia Assimétrica com OpenSSL
Arquivo: `Laboratório Criptografia Assimétrica com OpenSSL.md`

**Objetivo:** aplicar RSA e curvas elípticas em geração de chaves, assinatura e certificados.

**Conteúdos:** geração de chaves com `openssl genpkey`, assinatura/verificação, criação de CSR e certificado autoassinado.

---

### Lab — DNS
Arquivo: `Lab:DNS.md`

**Objetivo:** compreender resolução de nomes e registros DNS, além de práticas seguras.

**Conteúdos:** consultas com `dig`, TTL, DNSSEC, CAA, e segurança de servidores (zone transfer, amplification).

---

## Boas práticas & segurança
- Nunca publique senhas ou chaves privadas.
- Use RSA ≥ 3072 bits ou curvas elípticas modernas.
- Documente versões de ferramentas e parâmetros.
- Evite expor serviços internos via DNS público.

---

## Referências recomendadas
- Diffie & Hellman (1976); Rivest, Shamir & Adleman (1978).
- Stevens — *TCP/IP Illustrated*; Stallings; Kurose & Ross.
- NIST SP 800‑38D, SP 800‑56A/B.
- RFCs: 1034/1035 (DNS), 8446 (TLS 1.3), 5321/5322 (SMTP), 6376 (DKIM), 7489 (DMARC).

---

## Como contribuir
1. Abra uma *issue* com sua sugestão.
2. Faça um *fork* e envie um *pull request* com as modificações.
3. Mantenha o conteúdo em português e reprodutível.

Sugestões: laboratórios de TLS/mTLS, Zero Trust, Wireshark/tcpdump e forense.

---

## Licença
Conteúdo sob **CC BY‑NC 4.0**, salvo indicação contrária.

