
# **PNETLab – Guia Essencial para Instalação e Uso**

## O que é o PNetLab?

PNetLab é uma plataforma de virtualização voltada para redes e cibersegurança, semelhante ao EVE-NG, mas com foco em:

- Suporte ampliado a imagens de dispositivos (Cisco, Fortinet, Palo Alto, Mikrotik, Linux, etc.)
    
- Interface simplificada
    
- Menor consumo de recursos
    
- Ambiente preparado para ensino e simulação de ataques/defesa
    

É ideal para **aulas de redes, simulação de ataques, testes de segurança, forense digital e laboratórios de incidentes**.


---

# ** Instalação do PNetLab**

Existem duas formas recomendadas:

---

## **Opção A – Instalação em VM (VMware/VirtualBox/Proxmox)**

**Passos:**

### **Baixar a imagem PNetLab OVA/ISO**

Disponível no site oficial ou repositório público do projeto. https://pnetlab.com/pages/download

### **Criar uma VM com os requisitos mínimos:**

|Componente|Recomendado|
|---|---|
|CPU|4 vCPUs (Intel VT-x ou AMD-V habilitado)|
|RAM|8–16 GB|
|Disco|60–120 GB|
|Rede|1 interface bridge + 1 NAT (opcional)|

### **Iniciar e configurar pelo console:**

- Definir senha root
    
- Configurar IP estático
    
- Testar acesso via browser:
    
    `https://<IP-do-PNetLab>`
    

### **Criar o usuário administrador na primeira inicialização**

- Utilizando o arquivo `.ova` usuário root senha pnet
- dica: verifique se a virtualização está ativada corretamente  **VT-x (Intel) ou AMD-V (AMD) está ativado** no BIOS para suporte a virtualização.
- No prompt linux execute `kvm-ok` para testar
- execute o seguinte comando para virtualbox:  
	- `\virtualbox>VBoxManage list vms`
	- `\virtualbox>VBoxManage modifyvm "PNetLab" --nested-hw-virt on` onde PNetlab é o nome da sua máquina virtual e tem que ser identica
<img width="897" height="517" alt="image" src="https://github.com/user-attachments/assets/881eafc3-1d27-4e09-b7c4-a25db92f2283" />


---

## **Opção B – Instalação em Servidor (bare metal)**

Recomendado para:

- Laboratórios complexos
    
- Simulações pesadas em cibersegurança
    

Instalação é similar: boot pela ISO → seguir assistente → configurar rede.

---

# **Estrutura Interna do PNetLab**

O PNetLab é composto por:

- **Engine de virtualização** baseado em QEMU/KVM
    
- **Gerenciador web** para criação e execução de topologias
    
- **Biblioteca de imagens** (Cisco, Fortigate, Linux, Windows, etc.)
    
- **Sistema de redes virtuais** baseado em bridges internas
    

---

# **Como usar – Criando o primeiro laboratório**

### **Passos básicos:**

1. Acessar o painel web
    
2. Criar um novo _Lab_ (arquivo `.unl`)
    
3. Adicionar nós (routers, Linux, firewalls, hosts, etc.)
    
4. Conectar os nós com links virtuais
    
5. Iniciar os equipamentos
    
6. Acessar via console integrado HTML5
    
7. Salvar projeto
    

---

# **Como adicionar imagens (Cisco, Linux, Firewalls, etc.)**

Imagens vão para:

`/opt/unetlab/addons/`

Tipos comuns:

|Tipo|Caminho dentro do addons|
|---|---|
|QEMU|/opt/unetlab/addons/qemu/<nome>|
|Docker|/opt/unetlab/addons/docker/<nome>|

Passos:

1. Subir imagem via SCP (WinSCP ou scp)
    
2. Descompactar dentro da diretório correto
    
3. Ajustar permissões:
    
    `/opt/unetlab/wrappers/unl_wrapper -a fixpermissions`
    

---

# **Uso do PNetLab em Cibersegurança**

Ótimo para:

### **Red Team:**

- Simular ataques MITRE ATT&CK
    
- Explorar vulnerabilidades em ambientes isolados
    
- Criar cenários com Kali Linux e Metasploitable
    

### **Blue Team:**

- Montar SOC virtual (ELK/Splunk/Sigma)
    
- Testar detecção e resposta
    
- Reproduzir comportamentos de malware
    

### **Threat Hunting:**

- Geração de tráfego malicioso
    
- Execução de beaconing C2 controlado
    
- Investigação e coleta de vestígios
    

### **Forense:**

- Análise de memória
    
- Coleta de discos virtuais
    
- Simulação de incidentes reais
    

---





