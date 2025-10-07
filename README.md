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
