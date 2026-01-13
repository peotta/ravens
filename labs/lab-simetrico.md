**Ravens - Research on Vulnerability Exploration in Network Security**
**Professor Laerte Peotta**
laerte.melo@unb.br

Este roteiro descreve passo a passo como realizar operações criptográficas utilizando cifras simétricas com **openssl**
Atualizado em: 07/09/2025

---
# Objetivo

Explorar o funcionamento do AES em todos os modos de operação disponíveis no OpenSSL, além de comparar com o algoritmo moderno ChaCha20. Avaliar diferenças de segurança, desempenho e aplicabilidade em redes de comunicação.

---
# Prazos

- Data da entrega: até as 23:59 do dia anterior da próxima aula
- Formato da entrega: PDF
- Entrega pelo Teams

> **Evidências da execução**: Inclua prints, respostas e conclusões organizados em seções.

---

# Parte 1 - Preparação

1. Crie um arquivo simples chamado `texto.txt`:
   ```txt
   Segurança de Redes de Computadores
   Teste prático com AES e ChaCha20
   Criptografia Simétrica
   ```
2. Certifique-se de que o **OpenSSL** está instalado no computador (Linux, macOS ou Windows com WSL).

---
# Estrutura geral do comando

```bash
openssl enc -algoritmo -opções -in entrada -out saida
```

## **Parâmetros principais**

- **`enc`** → indica ao OpenSSL que será usado para **encryption** (cifragem).  
- **`-aes-256-cbc`** → escolhe o algoritmo:  
  - `aes` = AES (Advanced Encryption Standard).  
  - `256` = tamanho da chave em bits (256 bits = alta segurança).  
  - `cbc` = modo de operação (Cipher Block Chaining).  
- **`-chacha20`** → escolhe o algoritmo de cifra de fluxo ChaCha20. 
- **`-salt`** → adiciona um valor aleatório (*salt*) para aumentar a segurança contra ataques de dicionário.  
- **`-in`** → define o arquivo de **entrada** a ser cifrado.  
- **`-out`** → define o arquivo de **saída** (resultado da cifragem ou decifragem).  
- **`-d`** → ativa o modo de **decifragem** (decrypt).  

## **Resultado esperado**

Após cifrar e decifrar corretamente:  

- O arquivo **decifrado** (`*_dec.txt`) deve ser **idêntico** ao arquivo original (`texto.txt`).  
- O arquivo **cifrado** (`*.enc`) conterá **dados ilegíveis** se aberto em um editor de texto.  

---

## Parte 2 – AES em todos os modos de operação

### 1. AES-256-ECB (Electronic Codebook)


```bash
openssl enc -aes-256-ecb -in texto.txt -out texto_ecb.enc
openssl enc -aes-256-ecb -d -in texto_ecb.enc -out texto_ecb_dec.txt
```
	- Usa **AES-256** no modo **ECB**.  
- Lê o conteúdo de `texto.txt`.  
- Gera o arquivo cifrado `texto_ebc.enc`.  
- Solicita uma senha (chave secreta).  
- O parâmetro `-d` indica decifragem.  
- Recupera o conteúdo original no arquivo `texto_ebc_dec.txt`.

👉 Simples, mas inseguro. Padrões podem ser revelados.

### 2. AES-256-CBC (Cipher Block Chaining)

```bash
openssl enc -aes-256-cbc -salt -in texto.txt -out texto_cbc.enc
openssl enc -aes-256-cbc -d -in texto_cbc.enc -out texto_cbc_dec.txt
```
 👉 Mais seguro que ECB, amplamente usado em VPNs e criptografia de disco.

### 3. AES-256-CFB (Cipher Feedback)
```bash
openssl enc -aes-256-cfb -in texto.txt -out texto_cfb.enc
openssl enc -aes-256-cfb -d -in texto_cfb.enc -out texto_cfb_dec.txt
```
👉 Converte AES em cifra de fluxo, útil em canais interativos.

### 4. AES-256-OFB (Output Feedback)
```bash
openssl enc -aes-256-ofb -in texto.txt -out texto_ofb.enc
openssl enc -aes-256-ofb -d -in texto_ofb.enc -out texto_ofb_dec.txt
```
👉 Similar ao CFB, mas gera fluxo independente do texto original.

### 5. AES-256-CTR (Counter Mode)
```bash
openssl enc -aes-256-ctr -in texto.txt -out texto_ctr.enc
openssl enc -aes-256-ctr -d -in texto_ctr.enc -out texto_ctr_dec.txt
```
👉 Altamente eficiente, permite paralelismo. Padrão moderno em IPsec e TLS.

---

## Parte 3 – ChaCha20
```bash
openssl enc -chacha20 -in texto.txt -out texto_chacha.enc
openssl enc -chacha20 -d -in texto_chacha.enc -out texto_chacha_dec.txt
```
- Usa a cifra de fluxo **ChaCha20**.  
- Cria o arquivo cifrado `texto_chacha.enc`.  
- Com o parâmetro `-d`, recupera o arquivo original em `texto_chacha_dec.txt`.  

👉 Rápido e seguro, adotado no **TLS 1.3** e no **WireGuard (VPN)**.

---

## Parte 4 – Experimento de Performance
1. Gere um arquivo grande de teste (100 MB):
   ```bash
   dd if=/dev/urandom of=arquivo100M.txt bs=1M count=100
   ```
2. Meça tempo de cifragem em cada modo:
   ```bash
   time openssl enc -aes-256-ecb -in arquivo100M.txt -out teste_ecb.enc
   time openssl enc -aes-256-cbc -in arquivo100M.txt -out teste_cbc.enc
   time openssl enc -aes-256-cfb -in arquivo100M.txt -out teste_cfb.enc
   time openssl enc -aes-256-ofb -in arquivo100M.txt -out teste_ofb.enc
   time openssl enc -aes-256-ctr -in arquivo100M.txt -out teste_ctr.enc
   time openssl enc -chacha20 -in arquivo100M.txt -out teste_chacha.enc
   ```
3. Anote os tempos e compare eficiência.

---

## Questões 

1. Qual modo de operação apresentou melhor desempenho?
2. Por que o ECB é considerado inseguro mesmo sendo rápido?
3. Quais modos são mais adequados para redes de comunicação?
4. O que diferencia o ChaCha20 do AES em termos de eficiência e segurança?
5. Como essa prática se relaciona com protocolos como TLS, IPsec e VPNs modernas?
