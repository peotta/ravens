- [ ] #labssl

**Ravens - Research on Vulnerability Exploration in Network Security**
**Professor Laerte Peotta**
laerte.melo@unb.br

Este roteiro descreve passo a passo como realizar operações de criptografia e assinatura digital usando **OpenSSL 3.x**.

---
# Entregável do Laboratório de Criptografia

## Objetivo

O objetivo desta atividade é que cada aluno produza um **documento em formato PDF** apresentando os resultados obtidos no laboratório de criptografia com OpenSSL.

---
## Estrutura do Documento

1. **Capa**  
   - Nome do aluno  
   - Título: *Laboratório de Criptografia com OpenSSL*  
   - Data da entrega: até as 23:59 do dia anterior da próxima aula
   - Formato da entrega: PDF
   - Entrega pelo Teams

>**Observações: Inclua prints, respostas e conclusões organizados em seções.**


2. **Evidências da execução**  
   - Capturas de tela (**prints**) ou trechos de terminal mostrando:  
     - Geração das chaves de Alice e Bob.  
     - Mensagem curta cifrada e depois decifrada.  
     - Mensagem curta assinada e verificada.  
     - Mensagem longa (híbrida) cifrada e decifrada.  

3. **Arquivos principais (listagem e evidência de uso)**  
   - `alice.public` e `bob.public`  
   - `message.encrypted` e `message.decrypted`  
   - `message.sig`  

4. **Respostas rápidas (3 a 5 linhas cada)**  
   5. Para que serve o padding **OAEP**?  
   6. Qual é a função do **IV**?  
   7. Por que usamos **cifra híbrida** para mensagens grandes?  

8. **Conclusão**  
   - Breve reflexão (5 a 8 linhas) sobre o que aprendeu no laboratório, principais dificuldades e importância prática da criptografia estudada.  

---

## 🔑 1) Geração de chaves RSA

### Comandos
```bash
openssl genpkey -algorithm RSA -pkeyopt rsa_keygen_bits:2048 -out alice.private
```
**Parâmetros:**
- `genpkey` → gera uma chave privada (PKCS#8).
- `-algorithm RSA` → algoritmo de chave assimétrica RSA.
- `-pkeyopt rsa_keygen_bits:2048` → define o tamanho da chave (2048 bits).
- `-out alice.private` → arquivo de saída com a chave privada.

---

```bash
openssl pkey -in alice.private -pubout -out alice.public
```
**Parâmetros:**
- `pkey` → manipula chaves (PKCS#8).
- `-in alice.private` → chave privada de entrada.
- `-pubout` → extrai chave pública.
- `-out alice.public` → arquivo de saída com a chave pública.

---

```bash
chmod 600 *.private
```
**Parâmetros:**
- `chmod` → altera permissões de arquivos.
- `600` → somente o dono pode ler/escrever.
- `*.private` → aplica a todos os arquivos `.private`.

---

Faça os mesmos passos para gerar as chaves para Bob. (Os comandos para Bob são idênticos, mudando os nomes dos arquivos.)

---

## 📩 2) Mensagem curta confidencial (Alice → Bob)

### Comando
```bash
openssl pkeyutl -encrypt -in message.plain -out message.encrypted \
  -pubin -inkey bob.public \
  -pkeyopt rsa_padding_mode:oaep \
  -pkeyopt rsa_oaep_md:sha256 \
  -pkeyopt rsa_mgf1_md:sha256
```
**Parâmetros:**
- `pkeyutl` → utilitário para operações com chaves.
- `-encrypt` → operação de cifragem.
- `-in message.plain` → arquivo de entrada (mensagem original).
- `-out message.encrypted` → arquivo de saída cifrado.
- `-pubin` → indica que `-inkey` é uma chave pública.
- `-inkey bob.public` → chave pública do Bob.
- `-pkeyopt rsa_padding_mode:oaep` → padding OAEP.
- `-pkeyopt rsa_oaep_md:sha256` → hash SHA-256 para OAEP.
- `-pkeyopt rsa_mgf1_md:sha256` → hash SHA-256 para MGF1.

---

```bash
openssl pkeyutl -decrypt -in message.encrypted -out message.decrypted \
  -inkey bob.private \
  -pkeyopt rsa_padding_mode:oaep \
  -pkeyopt rsa_oaep_md:sha256 \
  -pkeyopt rsa_mgf1_md:sha256
```
**Parâmetros:**
- `-decrypt` → operação de decifragem.
- `-in message.encrypted` → arquivo cifrado.
- `-out message.decrypted` → mensagem em claro.
- `-inkey bob.private` → chave privada do Bob.
- Demais parâmetros iguais ao de cifragem.

---

```bash
diff -s message.plain message.decrypted
```
**Parâmetros:**
- `diff` → compara arquivos.
- `-s` → mostra mensagem se forem idênticos.

---

## 🖊️ 3) Mensagem curta assinada (Bob → Alice)

### Comando
```bash
openssl dgst -sha256 -sign bob.private -out message.sig message.plain
```
**Parâmetros:**
- `dgst` → calcula hash/digest.
- `-sha256` → algoritmo de hash SHA-256.
- `-sign bob.private` → chave privada usada para assinar.
- `-out message.sig` → arquivo de saída com a assinatura.
- `message.plain` → arquivo assinado.

---

```bash
openssl dgst -sha256 -verify bob.public -signature message.sig message.plain
```
**Parâmetros:**
- `-verify bob.public` → verifica assinatura com a chave pública do Bob.
- `-signature message.sig` → arquivo da assinatura.
- `message.plain` → mensagem original.

---

## 📜 4) Mensagem grande - cifra híbrida (Alice → Bob)

# Explicação:

# RSA-OAEP (Optimal Asymmetric Encryption Padding)

É um esquema de preenchimento usado junto com o algoritmo **RSA** para **cifrar mensagens de forma mais segura**.

---

## 🔑 Resumo em pontos

- O **RSA “puro”** (sem padding) é inseguro, sujeito a ataques matemáticos.  
- **OAEP** insere **aleatoriedade controlada** antes de aplicar RSA: combina a mensagem com um valor aleatório (*seed*) e duas funções hash (tipicamente **SHA-256**).  
- Essa mistura cria um texto intermediário que **dificulta ataques de adivinhação** ou de **oráculo de padding**.  
- Assim, RSA-OAEP garante **confidencialidade robusta**, mesmo se um atacante conseguir submeter textos cifrados a um “decifrador oráculo”.  
- É considerado o **padrão moderno**, substituindo o antigo **PKCS#1 v1.5**.  

---

## 👉 Em termos práticos

- **Alice** cifra uma chave simétrica com a **pública de Bob** usando **RSA-OAEP**.  
- **Bob** só consegue decifrar corretamente se tiver a **chave privada**.  
- O uso da **semente aleatória** faz com que **cada cifragem produza um resultado diferente**, mesmo para a mesma mensagem.  

- ---

# IV (Initialization Vector) - Vetor de Inicialização

O **IV** é um valor adicional usado em algoritmos de criptografia simétrica (como **AES em CBC ou GCM**) para **garantir aleatoriedade e segurança**.  

---

## O que é

- Um **bloco de dados aleatórios ou pseudoaleatórios**, geralmente do mesmo tamanho do bloco do algoritmo (ex.: 16 bytes para AES).  
- Funciona como um **ponto de partida** para o processo de cifragem.  

---

## Por que é necessário

- Se duas mensagens **iguais** fossem cifradas apenas com a **mesma chave**, o resultado seria **idêntico**, revelando padrões.  
- Com o IV, mesmo que a chave e o texto sejam os mesmos, o **ciphertext sai diferente**.  
- Isso **aumenta a segurança** contra ataques que exploram repetições.  

---

## Como funciona

- Em modos como **CBC (Cipher Block Chaining)**, o IV é combinado com o **primeiro bloco** da mensagem antes da cifra.  
- Nos modos **AEAD** (como **GCM**), o IV também ajuda a garantir **integridade** além da confidencialidade.  

---

## ⚠️ Regras importantes

- **Aleatório/Único:** o IV deve ser diferente a cada cifragem com a mesma chave.  
- **Não é segredo:** pode ser transmitido junto com a mensagem cifrada.  
- **Reuso do IV com a mesma chave** compromete a segurança (abre espaço para ataques).  

---
### Comandos


```bash
openssl rand 16 > iv.bin
```
**Parâmetros:**
- `16` → bytes para IV de 128 bits.
- `> iv.bin` → salva em `iv.bin`.

---

```bash
openssl enc -aes-256-cbc -in message.plain -out message.encrypted \
  -K $(xxd -p key.bin | tr -d '\n') \
  -iv $(xxd -p iv.bin | tr -d '\n')
```
**Parâmetros:**
- `enc` → cifragem simétrica.
- `-aes-256-cbc` → algoritmo AES 256 bits, modo CBC.
- `-in message.plain` → arquivo original.
- `-out message.encrypted` → saída cifrada.
- `-K` → chave em hexadecimal.
- `-iv` → vetor de inicialização em hexadecimal.

---

```bash
openssl dgst -sha256 -sign alice.private -out message.sig message.plain
```
**Parâmetros:** assinatura da mensagem longa com SHA-256 e chave privada da Alice.

---

```bash
cat key.bin iv.bin > keyiv.bin
```
**Parâmetros:** concatena chave + IV em um único arquivo binário.

---

```bash
openssl pkeyutl -encrypt -in keyiv.bin -out keyiv.encrypted \
  -pubin -inkey bob.public \
  -pkeyopt rsa_padding_mode:oaep \
  -pkeyopt rsa_oaep_md:sha256 \
  -pkeyopt rsa_mgf1_md:sha256
```
**Parâmetros:** cifra chave+IV com a chave pública do Bob, usando RSA-OAEP.

---

```bash
openssl pkeyutl -decrypt -in keyiv.encrypted -out keyiv.decrypted \
  -inkey bob.private \
  -pkeyopt rsa_padding_mode:oaep \
  -pkeyopt rsa_oaep_md:sha256 \
  -pkeyopt rsa_mgf1_md:sha256
```
**Parâmetros:** Bob decifra chave+IV com sua chave privada.

---

```bash
dd if=keyiv.decrypted of=key.bin bs=1 count=32 status=none
```
**Parâmetros:** extrai os 32 primeiros bytes (chave AES).

---

```bash
dd if=keyiv.decrypted of=iv.bin bs=1 skip=32 count=16 status=none
```
**Parâmetros:** pula 32 bytes (chave) e copia os 16 seguintes (IV).

---

```bash
openssl enc -d -aes-256-cbc -in message.encrypted -out message.decrypted \
  -K $(xxd -p key.bin | tr -d '\n') \
  -iv $(xxd -p iv.bin | tr -d '\n')
```
**Parâmetros:**
- `-d` → decifragem.
- Demais parâmetros iguais ao de cifragem.

---

```bash
openssl dgst -sha256 -verify alice.public -signature message.sig message.decrypted
```
**Parâmetros:** verifica assinatura de Alice com a chave pública.

---

```bash
diff -s message.plain message.decrypted
```
**Parâmetros:** confirma que mensagem original e decifrada são idênticas.

---
### FIM

