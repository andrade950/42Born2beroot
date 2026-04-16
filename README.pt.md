<div align="center">

# 🖥️ Born2beRoot

[Read in English](README.md)

**Um projeto de Administração de Sistemas da 42 School**

*Configura o teu primeiro servidor virtual — e compreende cada decisão que tomas.*

[![Tutorial](https://img.shields.io/badge/📖_Tutorial-Português-4A90D9?style=for-the-badge)](born2beroot.tutorial.pt.md)
[![Guia de Avaliação](https://img.shields.io/badge/✅_Guia_de_Avaliação-Português-2ECC71?style=for-the-badge)](born2beroot.evaluation.guide.pt.md)
[![Enunciado PDF](https://img.shields.io/badge/📄_Enunciado-PDF-E74C3C?style=for-the-badge)](en.subject.pdf)

</div>

---

## 📌 Visão Geral

O **Born2beRoot** é um projeto de administração de sistemas que te introduz ao mundo da **virtualização**. Irás criar e configurar uma máquina virtual do zero, instalar um servidor Linux seguro, e demonstrar que compreendes genuinamente como o sistema funciona — não apenas que funciona.

> *"Podes fazer tudo o que quiseres fazer. Esta máquina virtual é o teu mundo."*

---

## 🎯 Objetivos

Ao concluir este projeto, serás capaz de:

- Configurar uma **máquina virtual** com VirtualBox (ou UTM)
- Instalar e configurar um servidor **Debian** ou **Rocky Linux** sem interface gráfica
- Implementar **partições LVM encriptadas**
- Configurar e proteger **SSH**, **UFW/Firewalld** e **sudo**
- Aplicar uma **política de palavras-passe** rigorosa
- Escrever um **script bash de monitorização** que difunde informação do sistema a cada 10 minutos
- Compreender e explicar cada componente da tua configuração durante a defesa

---

## 🗂️ Estrutura do Repositório

```
repositório/
└── signature.txt     ← Assinatura SHA1 do disco virtual (.vdi)
```

> ⚠️ **Não deves** submeter a máquina virtual em si. Apenas o ficheiro `signature.txt` vai para o repositório.

---

## ⚙️ Requisitos Obrigatórios

### Sistema Operativo
Escolhe um:
- **Debian** *(recomendado para iniciantes)* — deve ter **AppArmor** em execução no arranque
- **Rocky Linux** — deve ter **SELinux** em execução no arranque (KDump não é necessário)

Não é permitida qualquer interface gráfica. Instalar X.org ou equivalente resulta numa **nota de 0**.

---

### Particionamento
Deves criar **pelo menos 2 partições LVM encriptadas**. Exemplo de estrutura:

```
sda
├── sda1       487M   /boot
├── sda2         1K
└── sda5       7.5G
    └── sda5_crypt  (LVM)
        ├── root    2.8G   /
        ├── swap    976M   [SWAP]
        └── home    3.8G   /home
```

---

### SSH
- Corre exclusivamente na **porta 4242**
- **Login root via SSH está desativado**
- Testado durante a avaliação ligando com um utilizador criado na hora

---

### Firewall (UFW / Firewalld)
- Deve estar **ativo no arranque**
- Apenas a **porta 4242** aberta por defeito
- Rocky Linux utiliza **Firewalld**; Debian utiliza **UFW**

---

### Utilizador e Grupos
- Deve existir um utilizador não-root com o teu login
- Esse utilizador deve pertencer aos grupos `sudo` e `user42`

---

### Política de Palavras-passe

| Regra | Valor |
|---|---|
| Validade máxima da palavra-passe | 30 dias |
| Dias mínimos antes de alteração | 2 dias |
| Aviso antes da expiração | 7 dias |
| Comprimento mínimo | 10 caracteres |
| Deve conter | maiúscula, minúscula, dígito |
| Máximo de caracteres iguais consecutivos | 3 |
| Não pode conter | o nome do utilizador |
| Deve diferir da palavra-passe anterior em | 7 caracteres *(apenas não-root)* |

---

### Configuração do Sudo

| Configuração | Valor |
|---|---|
| Tentativas de palavra-passe | máx. 3 |
| Mensagem de palavra-passe errada | Mensagem personalizada |
| Localização dos logs | `/var/log/sudo/` |
| Modo TTY | Ativado |
| Caminhos restritos | `/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin` |

---

### Script de Monitorização (`monitoring.sh`)

Um script bash que difunde as seguintes informações para todos os terminais a cada **10 minutos** via `wall`:

- Arquitetura do SO e versão do kernel
- Número de processadores físicos e virtuais
- Uso de RAM (usado / total / percentagem)
- Uso de disco (usado / total / percentagem)
- Percentagem de carga do CPU
- Data e hora do último reinício
- Se o LVM está ativo
- Número de ligações TCP ativas
- Número de utilizadores com sessão iniciada
- Endereço IPv4 e endereço MAC
- Número de comandos `sudo` executados

**Exemplo de saída:**
```
#Architecture: Linux wil 4.19.0-16-amd64 #1 SMP Debian 4.19.181-1 x86_64 GNU/Linux
#CPU physical : 1
#vCPU : 1
#Memory Usage: 74/987MB (7.50%)
#Disk Usage: 1009/2Gb (49%)
#CPU load: 6.7%
#Last boot: 2021-04-25 14:45
#LVM use: yes
#Connections TCP : 1 ESTABLISHED
#User log: 1
#Network: IP 10.0.2.15 (08:00:27:51:9b:a5)
#Sudo : 42 cmd
```

> Durante a avaliação, serás questionado sobre como **o script funciona** e terás de o **parar sem o modificar** (dica: usa o `cron`).

---

## 🔏 Submissão

1. Desliga a tua VM
2. Gera a assinatura SHA1 do teu ficheiro `.vdi`:
   ```bash
   # Linux / macOS
   sha1sum nome_da_vm.vdi
   # ou
   shasum nome_da_vm.vdi
   ```
3. Guarda o resultado no ficheiro `signature.txt` na raiz do teu repositório:
   ```bash
   echo <a_tua_assinatura> > signature.txt
   ```
4. **Clona ou faz snapshot** da VM antes de qualquer avaliação para preservar a assinatura

---

## 📚 Recursos

| Recurso | Ligação |
|---|---|
| 📖 Tutorial passo a passo (PT) | [born2beroot_tutorial_pt.md](born2beroot.tutorial.pt.md) |
| ✅ Guia de Avaliação (PT) | [born2beroot_evaluation_guide_pt.md](born2beroot.evaluation.guide.pt.md) |
| 📄 Enunciado Oficial (PDF) | [en.subject.pdf](en.subject.pdf) |

---

## 🧠 Conceitos-Chave para a Defesa

- O que é uma **máquina virtual** e para que serve?
- Diferenças entre **Debian** e **Rocky**
- **apt** vs **aptitude** (Debian) / **SELinux** e **DNF** (Rocky)
- O que é o **AppArmor**?
- Como funciona o **LVM**?
- O que é o **cron** e como se configura?
- Como funciona o **UFW** / **Firewalld**?
- Como funciona o **SSH** e porquê desativar o login root?

---
