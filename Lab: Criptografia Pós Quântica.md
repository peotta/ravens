#labsslquantum

**Ravens - Research on Vulnerability Exploration in Network Security**
**Professor Laerte Peotta**
laerte.melo@unb.br

Este roteiro descreve passo a passo como realizar operações de criptografia Pós-Quântica com **OQS-OpenSSL** e assinatura digital usando **OpenSSL 3.x**.
# Objetivo  

Laboratório – Testando Criptografia Pós-Quântica com OQS-OpenSSL

- Aluno compreende como instalar e usar o **OQS-OpenSSL**.  
- Consegue listar e identificar algoritmos PQC disponíveis.  
- Executa uma sessão TLS 1.3 usando **Kyber + ECC**, simulando proteção contra ataques quânticos futuros.  
# Relatório (Entrega do Aluno) 

1. O que significa uma conexão **híbrida PQC**?  
2. Quais algoritmos pós-quânticos foram listados no `openssl list`?  
3. Por que o NIST escolheu **Kyber** como padrão para KEM?  
4. Compare o tamanho da chave/certificado PQC com RSA e ECC.  
5. Data entrega: às 23:59 do dia anterior a aula
6. Formato: PDF
7. Envio Individual: Tarefa no Teams

>**Observações: Inclua prints, respostas e conclusões organizados em seções.**

---
# Explicação Inicial

## OpenSSL e Criptografia Pós-Quântica (PQC)

O **OpenSSL** tradicionalmente implementa criptografia baseada em algoritmos clássicos (**RSA, DSA, DH, ECDSA, ECDH** etc.). Contudo, com os avanços da **criptografia pós-quântica (PQC)**,  o projeto começou a incorporar suporte experimental a novos algoritmos.  

---

## Situação atual (até 2025)

- O **OpenSSL 3.x** não possui nativamente suporte completo a algoritmos pós-quânticos.  
- O que existe são integrações experimentais por meio do **OQS-OpenSSL** (fork do projeto [Open Quantum Safe](https://openquantumsafe.org)).  

Esse fork adiciona algoritmos do **NIST PQC Standardization Project**, como:  
- **Kyber** (KEM – Key Encapsulation Mechanism, já padronizado pelo NIST em 2024).  
- **Dilithium, Falcon e SPHINCS+** (assinaturas digitais).  

Ele permite usar esses algoritmos em:  
- Conexões **TLS 1.3**.  
- **Geração de certificados**.  

---
## Principais usos no OQS-OpenSSL

- **TLS 1.3 com PQC híbrido** → combina um algoritmo clássico (ex.: ECDH) com um pós-quântico (ex.: Kyber), garantindo segurança mesmo contra ataques quânticos futuros.  
- **Geração de certificados** com chaves pós-quânticas.  
- **Testes de compatibilidade** para adoção gradual das soluções PQC.  


>A expectativa é que algoritmos como **Kyber e Dilithium** sejam gradualmente incorporados nas versões futuras do OpenSSL, conforme avançar a padronização do NIST.  

---
## Pré-requisitos  

- Kali Linux atualizado.  
- Pacotes básicos de compilação.  

```bash
sudo apt update && sudo apt upgrade -y
```

- `apt update` → atualiza a lista de pacotes disponíveis.  
- `apt upgrade -y` → instala atualizações (-y confirma automaticamente).  

```bash
sudo apt install -y build-essential cmake git
```

- `build-essential` → inclui compilador GCC e ferramentas básicas.  
- `cmake` → ferramenta para configurar projetos de compilação.  
- `git` → necessário para clonar os repositórios.  
- `-y` → confirma a instalação automaticamente.  

## Passo a Passo  

### 1. Clonar os repositórios do OQS [open-quantum-safe](https://github.com/open-quantum-safe)

```bash
cd ~
git clone https://github.com/open-quantum-safe/liboqs.git
git clone https://github.com/open-quantum-safe/oqs-provider.git
```
- `cd ~` → garante que você está no diretório home.  
- `git clone` → baixa o código dos repositórios.  
  - `liboqs` → biblioteca de algoritmos PQC.  
  - `oqs-openssl` → fork do OpenSSL integrado ao liboqs.

---

### 2. Instalar o **liboqs** (biblioteca PQC)  

```bash
cd ~/liboqs
mkdir build && cd build
cmake -DCMAKE_INSTALL_PREFIX=/usr/local ..
make -j$(nproc)
sudo make install
```
- `mkdir build && cd build` → cria pasta de compilação isolada.  
- `cmake -DCMAKE_INSTALL_PREFIX=/usr/local ..`  
  - `cmake` → prepara os arquivos de compilação.  
  - `-DCMAKE_INSTALL_PREFIX=/usr/local` → define o local de instalação.  
  - `..` → indica que os arquivos do projeto estão no diretório acima.  
- `make -j$(nproc)` → compila o código usando todos os núcleos da CPU (`$(nproc)`).  
- `sudo make install` → instala a biblioteca no sistema.  

---

### 3. Compilar o **OQS-OpenSSL** 

```bash
cd ~/oqs-provider
./Configure linux-x86_64 --prefix=/usr/local/oqs-provider no-shared
make -j$(nproc)
sudo make install
```
- `./Configure linux-x86_64` → prepara a compilação para Linux 64 bits.  
- `--prefix=/usr/local/oqs-openssl` → instala em diretório separado para não sobrescrever o OpenSSL original.  
- `no-shared` → compila bibliotecas de forma estática (mais simples para testes).  
- `make -j$(nproc)` → compila em paralelo.  
- `sudo make install` → instala no diretório definido no `--prefix`.

---

### 4. Verificar algoritmos disponíveis  

- Listar algoritmos de **KEM (Key Encapsulation Mechanisms):**  
```bash
/usr/local/oqs-openssl/bin/openssl list -kem-algorithms
```
- `openssl list` → lista algoritmos suportados.  
- `-kem-algorithms` → mostra apenas os algoritmos de encapsulamento de chave (ex.: Kyber).  

- Listar algoritmos de **assinatura digital:**  
```bash
/usr/local/oqs-openssl/bin/openssl list -signature-algorithms
```
- `-signature-algorithms` → mostra apenas algoritmos de assinatura (ex.: Dilithium, Falcon, SPHINCS+).  

>Deve aparecer **Kyber, Dilithium, Falcon, SPHINCS+**, entre outros.  

---

### 5. Gerar certificados de teste  

1. Gerar uma chave e certificado autoassinado (usando algoritmo clássico para o teste):  
```bash
/usr/local/oqs-openssl/bin/openssl req -x509 -newkey rsa:2048 -keyout key.pem -out cert.pem -days 365 -nodes -subj "/CN=localhost"
```
- `req` → comando para requisições de certificados.  
- `-x509` → gera um certificado autoassinado.  
- `-newkey rsa:2048` → cria uma nova chave RSA de 2048 bits.  
- `-keyout key.pem` → salva a chave privada no arquivo `key.pem`.  
- `-out cert.pem` → salva o certificado em `cert.pem`.  
- `-days 365` → validade de 1 ano.  
- `-nodes` → não criptografa a chave privada (sem senha).  
- `-subj "/CN=localhost"` → define o campo **Common Name** como `localhost`.  
---

### 6. Iniciar um servidor TLS 1.3 com PQC 

```bash
/usr/local/oqs-openssl/bin/openssl s_server -cert cert.pem -key key.pem -www -tls1_3 -groups kyber512:p256
```
- `s_server` → inicia um servidor TLS para testes.  
- `-cert cert.pem` → certificado a ser usado.  
- `-key key.pem` → chave privada correspondente.  
- `-www` → retorna uma página simples para confirmar a conexão.  
- `-tls1_3` → força o uso do protocolo TLS 1.3.  
- `-groups kyber512:p256` → define o grupo de troca de chaves híbrido (Kyber512 + curva P-256).

> O servidor ficará escutando na porta padrão `4433`.  
> O parâmetro `-groups kyber512:p256` indica uso **híbrido** (Kyber512 + curva elíptica P-256).  

---

### 7. Conectar como cliente  

Em outro terminal:  
```bash
/usr/local/oqs-openssl/bin/openssl s_client -connect localhost:4433 -groups kyber512:p256
```
- `s_client` → conecta a um servidor TLS.  
- `-connect localhost:4433` → conecta ao servidor local na porta 4433.  
- `-groups kyber512:p256` → força uso do grupo híbrido Kyber512 + P-256. 

>Se a conexão for bem-sucedida, a saída mostrará algo como:  

```
New, TLSv1.3, Cipher is TLS_AES_256_GCM_SHA384
Server Temp Key: OQS-Kyber512x25519
```

---


