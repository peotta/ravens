# Laboratório: Enviando e-mail via SMTP do Google (STARTTLS)

## 0) Pré-requisitos

- Conta Google **com 2FA ativado**.
    
- **App Password** (senha de app) gerado em: Conta Google → Segurança → Senhas de app.
    
    > ⚠️ O Gmail **não aceita** senha “normal” via SMTP; use **App Password**.
    
- Terminal com `openssl` e `base64` (Linux/macOS; no Windows, use WSL ou PowerShell).
    
- Um e-mail de destino (pode ser o próprio).
    

---

## 1) Codifique credenciais em Base64

Crie os valores em base64 para o fluxo `AUTH LOGIN`:

### Substitua pelo seu e-mail e sua App Password (16 caracteres, sem espaços)

`printf 'seu_usuario@gmail.com' | base64`
`printf 'SUA_APP_PASSWORD_AQUI' | base64`

Anote os dois resultados:

- `USER_B64` ← base64 do seu e-mail
    
- `PASS_B64` ← base64 da sua app password
    

> No PowerShell:
> 
> `[Convert]::ToBase64String([Text.Encoding]::ASCII.GetBytes("seu_usuario@gmail.com")) [Convert]::ToBase64String([Text.Encoding]::ASCII.GetBytes("SUA_APP_PASSWORD_AQUI"))`

---

## 2) Abra conexão segura com o SMTP do Google

Usaremos **STARTTLS** na porta **587**:

`openssl s_client -starttls smtp -connect smtp.gmail.com:587 -crlf -ign_eof

Você deve ver algo como:

`220 smtp.gmail.com ESMTP ...`

---

## 3) Faça o handshake SMTP + AUTH LOGIN

Digite (você mesmo) os comandos abaixo no terminal **já dentro do `openssl`**:

`EHLO exemplo.com`

Espere respostas `250-...` com as capacidades (incluindo `AUTH`).

Inicie autenticação:

`AUTH LOGIN`

O servidor responderá com `334 VXNlcm5hbWU6` (base64 de “Username:”).  
Envie **o base64 do seu e-mail** (USER_B64), por exemplo:

`c2V1X3VzdWFyaW9AZ21haWwuY29t`

Ele responderá `334 UGFzc3dvcmQ6` (base64 de “Password:”).  
Envie **o base64 da App Password** (PASS_B64):

`QUJDRERFRkdISUpLTE1OT1BRUlNUVVZXWFk=`

Se tudo OK:

`235 2.7.0 Accepted`

---

## 4) Envie a mensagem

Troca SMTP padrão:

`MAIL FROM:<seu_usuario@gmail.com> RCPT TO:<destinatario@exemplo.com> DATA Subject: Teste SMTP via OpenSSL From: Seu Nome <seu_usuario@gmail.com> To: Destino <destinatario@exemplo.com>  Olá, Esta é uma mensagem de teste enviada via SMTP do Google usando OpenSSL + STARTTLS.  Att, Seu Nome . QUIT`

Observe as respostas `250 200 OK` após o `DATA` (ao finalizar com um ponto sozinho na linha) e `221` após `QUIT`.

> Dicas:
> 
> - O `-crlf` no `openssl` garante final de linha correto (`\r\n`) para SMTP.
>     
> - Se preferir **TLS implícito** na **porta 465** (sem STARTTLS), use:
>     
>     `openssl s_client -connect smtp.gmail.com:465 -crlf`
>     

---

## 5) (Opcional) Teste “one-liner” com ferramenta `swaks` 

Se tiver `swaks` instalado:

`swaks --to destinatario@exemplo.com \       --from seu_usuario@gmail.com \       --server smtp.gmail.com --port 587 \       --auth LOGIN --auth-user seu_usuario@gmail.com \       --auth-password 'SUA_APP_PASSWORD_AQUI' \       --tls --header "Subject: Teste via swaks"`

---

## 6) Solução de problemas

- **535 5.7.8 Username and Password not accepted**  
    → Use **App Password** (não a senha normal). Confirme e-mail correto.
    
- **530 5.7.0 Authentication Required**  
    → Faça `AUTH LOGIN` corretamente **antes** de `MAIL FROM`.
    
- **Falha de handshake TLS**  
    → Verifique data/hora do sistema, firewall/proxy, tente `-tls1_2`.
    
- **Porta 25 bloqueada**  
    → Use **587** (STARTTLS) ou **465** (TLS implícito).
    

---

## 7) Boas práticas de segurança

- **Nunca** compartilhe App Password em claro.
    
- Use conta de **teste** para laboratórios.
    
- Revogue App Passwords não utilizados (Conta Google → Segurança).
    
- Evite digitar credenciais em ambientes gravados/logados.
    

---

### Resultado esperado

Você autenticou via `AUTH LOGIN` com **App Password**, enviou um e-mail real pelo **smtp.gmail.com** usando **TLS**, e observou o fluxo SMTP completo (EHLO, AUTH, MAIL/RCPT/DATA/QUIT).
