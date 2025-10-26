#Roteiro00-kali


**Ravens - Research on Vulnerability Exploration in Network Security**
**Professor Laerte Peotta**
laerte.melo@unb.br

## 🔹 Nível 1 – Fundamentos
1. **Instalação do Kali Linux no VirtualBox** – configuração inicial, snapshots e atualização do sistema.
2. **Exploração do ambiente Kali** – conhecer as categorias de ferramentas, terminal e permissões.
3. **Gerenciamento de usuários e permissões** – boas práticas de segurança em Linux.
4. **Configuração de redes virtuais** – NAT, Bridge e Host-Only no VirtualBox para simulação de ambientes de ataque/defesa.

---

## 🔹 Nível 2 – Reconhecimento e Varredura
5. **Scanner de rede com Nmap** – identificar hosts, portas e serviços.
6. **Enumeração de serviços** – usar ferramentas como `enum4linux`, `nbtscan`, `snmpwalk`.
7. **Fingerprinting de sistemas operacionais** – detecção de versões de OS e serviços vulneráveis.
8. **Mapeamento de vulnerabilidades** – introdução ao Nessus/OpenVAS.

---

## 🔹 Nível 3 – Exploração e Acesso
9. **Exploits com Metasploit Framework** – exploração controlada de vulnerabilidades conhecidas.
10. **Ataques de dicionário e força bruta** – uso do Hydra contra protocolos como SSH e FTP.
11. **Phishing básico com SET (Social Engineering Toolkit)** – criação de páginas falsas em ambiente controlado.
12. **Escalada de privilégios** – simulação de vulnerabilidades locais em sistemas Linux e Windows.

---

## 🔹 Nível 4 – Persistência e Movimentação
13. **Backdoors e shells reversos** – uso do Netcat e Metasploit para persistência.
14. **Keylogging e captura de credenciais** – técnicas em ambientes simulados.
15. **Pivoting em redes virtuais** – acessar sub-redes internas a partir de máquinas comprometidas.

---

## 🔹 Nível 5 – Análise e Defesa
16. **Sniffing e análise de tráfego** – uso do Wireshark e tcpdump para capturar pacotes.
17. **Ataques MITM (Man-in-the-Middle)** – ARP spoofing com Ettercap e mitigação.
18. **Detecção de intrusões com IDS** – integração do Kali com Snort ou Suricata.
19. **Forense básica** – recuperação de arquivos deletados, análise de logs e imagens de disco.
20. **Red Team vs Blue Team** – exercício final com ataque (Ravens Red) e defesa (Ravens Blue) em laboratório controlado.

---

**Observação:** cada laboratório deve ser documentado em relatório, incluindo descrição da prática, resultados obtidos e reflexões críticas sobre os riscos e defesas associadas.
