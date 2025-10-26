
## Objetivos

- Entender como o DNS resolve nomes em endereços IP.
- Identificar registros DNS (A, AAAA, CNAME, MX, NS) e TTL.
- Diferenciar consultas recursivas e iterativas.
- Explorar ferramentas práticas de consulta DNS.

## Pré-requisitos

- Acesso a terminal (Linux/WSL, Windows).
- Conexão à internet.
- Ferramentas: `nslookup`, `dig`, `host`, (`drill` opcional), ou `Resolve-DnsName` no Windows PowerShell.

---

## Parte 1 – Introdução

**Ferramenta 1: `nslookup`**

1. Resolver um nome e identificar servidor que respondeu:
```bash
nslookup www.google.com
```

2. Consultar tipo MX (servidores de e-mail):
```bash
nslookup -type=MX gmail.com
```

3. Trocar o servidor DNS (ex.: Cloudflare):
```bash
nslookup www.unb.br 1.1.1.1
```

---

## Parte 2 – Explorando mais campos
**Ferramenta 2: `dig`**

1. Consultar registros A e observar TTL:
```bash
dig www.unb.br
```

2. Consultar IPv6 (AAAA) e saída curta:
```bash
dig +short www.google.com AAAA
```

3. Consultar registros NS:
```bash
dig unb.br NS
```

4. Consultar CNAME:
```bash
dig mail.google.com CNAME
```

5. Consultar usando servidor público (Google DNS):
```bash
dig www.exemplo.com @8.8.8.8
```

---

## Parte 3 – Hierarquia e iteração

**Ferramenta 3: `dig +trace`**

1. Seguir a cadeia de resolução:
```bash
dig +trace www.unb.br
```

Se falhar (firewall/ISP), usar:

```bash
dig +trace -4 www.unb.br
dig www.unb.br @1.1.1.1
```

---

## Parte 4 – Ferramentas alternativas

**Ferramenta 4: `host`**

1. Resolução simples:
```bash
host www.unb.br
```

2. Consultar MX:
```bash
host -t MX gmail.com
```

3. Reverse lookup (PTR):
```bash
host 8.8.8.8
```

**Ferramenta 5: `drill` (opcional)**

1. Consulta simples:
```bash
drill www.google.com
```

2. Consultar NS:
```bash
drill -t NS unb.br
```

**Alternativa Windows (PowerShell):**
```powershell
Resolve-DnsName www.unb.br
Resolve-DnsName gmail.com -Type MX
Resolve-DnsName 8.8.8.8 -Type PTR
```

---

## Parte 5 – DNSSEC e TCP

1. Verificar DNSSEC:
```bash
dig dnssec-failed.org +dnssec
```

2. Forçar TCP em vez de UDP:
```bash
dig www.unb.br +tcp
```

---

## Checklist de Entrega

- Exemplos de registros (A, AAAA, MX, CNAME, NS).
- Saída de um reverse lookup (PTR).
- Captura de um fluxo iterativo (`dig +trace`).
- Observações sobre TTL e cache.
- Comparação entre respostas de resolvers diferentes.

---

## Questões de Reflexão
1. Como o TTL afeta o tempo de propagação de mudanças?  
2. Qual a diferença entre consulta recursiva e iterativa?  
3. Por que resolvedores públicos podem ser mais rápidos?  
4. Para que serve o reverse lookup (PTR)?  


