**Ravens - Research on Vulnerability Exploration in Network Security**
**Professor Laerte Peotta**
laerte.melo@unb.br


---

## 1. Preparação do Ambiente

1. **Baixar e instalar o VirtualBox:**
   - Acessar: [https://www.virtualbox.org](https://www.virtualbox.org)
   - Fazer download da versão do sistema operacional Windows).
   - Instalar seguindo o assistente padrão.
   - Instalar o [VirtualBox Extension Pack](https://www.virtualbox.org/wiki/Downloads) este pacote facilita a integração com o Sistema Operacional Host.


2. **Baixar a imagem do Kali Linux para VirtualBox:**
   - Acessar: [https://www.kali.org/get-kali/](https://www.kali.org/get-kali/#kali-virtual-machines)
   - Selecionar a versão *Kali Linux VirtualBox* (64-bit).
   - Descompactar.


---

## 2. Criando a Máquina Virtual

1. Abrir o **VirtualBox**.
2. Clicar em **Open**.
3. Navegar até o local onde descompactou o arquivo Kali Linux para VirtualBox

<img width="946" height="533" alt="Pasted image 20250914140845" src="https://github.com/user-attachments/assets/14292110-53da-4b98-8749-911f3098a513" />


> Quando clicar no arquivo com a extensão `vdi` a máquina será criada automaticamente, não necessitando outras configurações.


A tela abaixo mostra como fica depois de executado os passos acima.

<img width="962" height="751" alt="Pasted image 20250914175705" src="https://github.com/user-attachments/assets/9cad700b-98f1-421c-996d-7d1074ec007b" />


---

## 3. Pós-instalação

1. Fazer login:

```
Username: kali
Password: kali
```

2. Atualizar pacotes:

   ```bash
   sudo apt update && sudo apt upgrade -y
   ```

  ---

## 4  Próximos Passos para o Laboratório

- Criar **snapshots** do estado inicial.
- Instalar ferramentas extras do Kali (caso não venham por padrão):

  ```bash
  sudo apt install -y kali-linux-headless
  sudo apt install -y kali-linux-large
  ```
- Documentar cada etapa em relatório pelos alunos.
- Configurar redes virtuais para práticas de ataque/defesa (ex.: uma VM vítima com Metasploitable 2). 
