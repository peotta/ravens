**Ravens - Research on Vulnerability Exploration in Network Security**
**Professor Laerte Peotta**
laerte.melo@unb.br

Este roteiro descreve passo a passo como realizar operações Como instalar o Metasploitable 2 no VirtualBox

Atualizado em: 12/09/2025

[How to install Metasploitable 2 in VirtualBox – GeeksforGeeks](https://www.geeksforgeeks.org/linux-unix/how-to-install-metasploitable-2-in-virtualbox)

---

# Como instalar o Metasploitable2 no VirtualBox


Vamos primeiro discutir o que é o Metasploitable. Trata-se de um ambiente de testes muito útil para iniciantes que desejam praticar e testar suas habilidades em testes de penetração e pesquisa de segurança. Trata-se de uma máquina-alvo usada para descobrir e explorar vulnerabilidades, permitindo que o usuário tenha uma ideia de alvos e máquinas reais.

Em outras palavras, o Metasploitable é uma versão intencionalmente vulnerável do Ubuntu para máquinas virtuais, projetada para testar ferramentas de segurança e demonstrar vulnerabilidades comuns.

> Para instalar o **Metasploitable2** é preciso que você tenha o virtualbox.

## Instalação

***Etapa 1:***  [Baixe](https://translate.google.com/website?sl=en&tl=pt&hl=pt&client=srp&u=https://sourceforge.net/projects/metasploitable/files/Metasploitable2/) o arquivo Metasploitable 2. Ele vem em formato ZIP e contém um arquivo `.vmdk` (disco rígido virtual) – não um arquivo ISO.

![](https://media.geeksforgeeks.org/wp-content/uploads/20221115094635/image1.jpg)

***Passo 2:*** O arquivo inicialmente estará no formato zip, então precisamos extraí-lo. Após extrair o arquivo, abra o VirtualBox.

![](https://media.geeksforgeeks.org/wp-content/uploads/20221115094755/image2.jpg)

***Etapa 3:*** Agora, como mostrado na imagem acima, clique em **new** 

![](https://media.geeksforgeeks.org/wp-content/uploads/20221115095023/image3.jpg)

- Agora uma janela você deverá fornecer alguns detalhes, como o nome da sua máquina, caminho de instalação, tipo e versão.
- preencha os detalhes como:

Nome: ***Metasploitabe2***  
Caminho: ***deixe como recomendado***  
Tipo: ***Linux***  
Versão: ***outro (64 bits)***

> ***Observação*** : como o arquivo Metasploitable 2 não é um arquivo ISO, você não inicializará a partir de um instalador. Em vez disso, anexaremos o `.vmdk`arquivo existente como um disco rígido virtual, que contém o sistema operacional completo pré-instalado.

![](https://media.geeksforgeeks.org/wp-content/uploads/20221115095717/image4.jpg)

***Etapa 4:*** selecione a RAM que você deseja fornecer à máquina virtual. recomendado (512 MB).

![](https://media.geeksforgeeks.org/wp-content/uploads/20221115095742/image5.jpg)

***Passo 5:*** Agora, escolha a opção de usar um arquivo de disco rígido virtual existente. Clique no ícone da pasta, navegue e selecione o `.vmdk`arquivo extraído do arquivo ZIP do Metasploitable 2. Isso montará o disco rígido da VM diretamente, sem a necessidade de ISO.

![](https://media.geeksforgeeks.org/wp-content/uploads/20221115095822/image6.jpg)

- Agora localize o arquivo que extraímos.

***Etapa 6:** Agora salve o arquivo e você verá que a instância será criada com o nome **Metasploitable2**

![](https://media.geeksforgeeks.org/wp-content/uploads/20221115095857/image7.jpg)

- Estamos prontos para usar a máquina, basta pressionar o botão Iniciar na parte superior e esperar que ela inicie e carregue a instância.

![](https://media.geeksforgeeks.org/wp-content/uploads/20221115095919/image8.jpg)

**Etapa 7.** Assim que a instância for carregada, você será solicitado a fornecer um nome de usuário e uma senha. Por padrão, as credenciais são:

```
Login: msfadmin  
Senha: msfadmin
```

![](https://media.geeksforgeeks.org/wp-content/uploads/20221115095941/image9.jpg)

- Depois de fazer login com as credenciais, você será direcionado para a máquina e concluiremos o processo de instalação.

### Demonstração de teste de penetração com Metasploitable 2

**Etapa 1:** abra suas máquinas Metasploitable2 e Kali Linux lado a lado.

- Primeiro, precisamos executar ambas as instâncias ao mesmo tempo, lado a lado, para que possamos ver as alterações claramente. Inicie o Vbox e inicie o kali Linux e o Metasploitable2 lado a lado.

![](https://media.geeksforgeeks.org/wp-content/uploads/20221116005839/img1.jpg)

**Etapa 2:** vamos verificar os endereços IP de ambas as máquinas para obter uma visão geral da máquina de destino.

- Agora vamos abrir o terminal e verificar o endereço IP do Metasploitable2 no qual vamos realizar o ataque. use o seguinte comando:

```bash
msfadmin@metasploitable:~$ ifconfig
```
- na imagem acima, podemos ver que temos um endereço IP, ou seja, 192.168.10.5, da máquina de destino.

**Etapa 3:** agora realizaremos uma varredura de rede com a ajuda da ferramenta Nmap para ver quais serviços estão sendo executados no destino e quais estão dentro do destino.

- agora o primeiro passo é procurar por vulnerabilidades para que possamos explorar a máquina, para isso usaremos o Nmap scan em um terminal Linux. use o comando:

```bash
root-user-#/ $ nmap -sV -O 192.168.10.5
```

![](https://media.geeksforgeeks.org/wp-content/uploads/20221116010018/img3.jpg)

- no comando acima, -sV é usado para obter as versões dos serviços em execução na máquina de destino e -O é usado para detectar o sistema operacional na máquina de destino.
- agora podemos ver que temos muitas maneiras de exploração de vulnerabilidades, usaremos o exploit vsftpd_234_backdoor para explorar e obter acesso à máquina.

**Etapa 4:**  Agora que temos todas as informações relacionadas ao exploit que precisamos usar, ou seja, **vsftpd_backdoor**, podemos usar o Metasploit para explorar a máquina e obter acesso ao shell de comando, o que eventualmente nos dará acesso à máquina alvo.

- inicie o [Metasploit Framework](https://translate.google.com/website?sl=en&tl=pt&hl=pt&client=srp&u=+https://www.geeksforgeeks.org/linux-unix/what-is-metasploit/++) pelo comando mencionado abaixo:

```bash
root-user-#/ $ msfconsole
```
- Depois de seguir os comandos, vamos escolher o exploit vsftpd_backdoor e então definir o Rhost (IP de destino).

**Etapa 5:** Agora, tudo o que precisamos fazer é implantar o exploit na máquina de destino com a ajuda do msfconsole. Para isso, precisamos seguir alguns passos básicos que são:

- primeiro, vamos selecionar o exploit que vamos usar, neste caso é o vsftpd_backdoor, então usaremos o seguinte comando ****:****

```bash
msf6~/ use exploit/unix/ftp/vsftpd_234_backdoor
```
- Depois de selecionar o exploit, vamos configurar o alvo no qual iremos implantar o exploit.

```bash
msf6~/ (unix/ftp/vsftpd_234_backdoor): show options
```

![](https://media.geeksforgeeks.org/wp-content/uploads/20221116010105/img4.jpg)

- agora podemos ver que temos a opção de definir RHOST, que é o host receptor. Então, vamos defini-lo para o endereço IP da máquina de destino.

```bash
msf6~/ (unix/ftp/vsftpd_234_backdoor):set RHOST 192.168.10.5
```

**Etapa 6:** a etapa final é executar o exploit, pelo comando exploit.

```bash
msf6~/ (unix/ftp/vsftpd_234_backdoor): exploit
```

- depois de definir o RHOST, basta digitar o comando exploit e você verá que o shell de comando da máquina alvo será obtido.

![](https://media.geeksforgeeks.org/wp-content/uploads/20221116010157/img6.jpg)

- agora que tivemos sucesso obtendo um shell, você pode tentar comandos e verificar em ambas as máquinas ao mesmo tempo. 

**Etapa 7:** verifique usando alguns comandos do shell, como imprimir o diretório de trabalho ou ls em uma diretório.

```bash
pwd, ls -l, ls -a etc
```
- Então, analisamos com sucesso como o Metasploitable é útil para praticar habilidades de testes de invasão.
- Temos acesso root à máquina.

### Conclusão:

O Metasploitable 2 é uma ótima máquina para praticar e aprender sobre testes de invasão e hacking, apresenta várias vulnerabilidades e falhas que você pode continuar pesquisando e aprimorando suas habilidades. Atualmente, outra versão do Metasploitable também está disponível, e você pode usá-la. O processo de configuração e instalação é o mesmo do descrito acima.

Aprendemos como instalar o Metasploitable versão 2 com sucesso e vimos uma demonstração de exploração com o exploit mais famoso e básico, o vsftpd_backdoor. Há muitos outros exploits e técnicas para aprender e praticar.
