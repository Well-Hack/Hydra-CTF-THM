# Hydra
 Video explicativo de utilização do Hydra pelo THM.

 https://www.youtube.com/watch?v=kG_dnP3mmKo

 # TryHackMe - Hydra Room Writeup

Este repositório contém as notas, comandos e a resolução da sala **Hydra** na plataforma [TryHackMe](https://tryhackme.com/room/hydra). O objetivo deste laboratório é aprender a utilizar o THC-Hydra, uma das ferramentas de *brute-forcing* de credenciais de rede mais rápidas e populares do mundo.

## 📝 Visão Geral da Sala
* **Dificuldade:** Fácil
* **Tecnologias/Ferramentas Utilizadas:** Hydra, Linux (VirtualBox),OpenVpn, SSH, Protocolo HTTP (POST)
* **Objetivo:** Compreender a sintaxe do Hydra e utilizá-lo para comprometer serviços de rede (SSH) e formulários web (HTTP-POST) através de ataques de dicionário (*wordlists*).

---

## 🛠️ O que é o Hydra?
O **Hydra** é um crackeador de logins de rede paralelizado, muito rápido e flexível. Suporta inúmeros protocolos de comunicação (como SSH, FTP, Telnet, HTTP, HTTPS, SMB, pop3, etc.) e permite testar combinações de utilizadores e palavras-passe contra serviços ativos.

---

## 🚀 Resolução dos Desafios

### 1. Ataque de Força Bruta contra Formulário Web (HTTP-POST)
O primeiro objetivo prático consiste em descobrir a password web da utilizadora `molly`.

* **Análise da requisição (ex: usando DevTools ou Burp Suite):**
  * **Método:** `POST`
  * **Página de login:** `/`
  * **Corpo do formulário (Payload):** `username=^USER^&password=^PASS^`
  * **Mensagem de erro em caso de falha:** `incorrect`

* **Comando Utilizado:**
  ```bash
  hydra -l molly -P passwords.txt <IP_ALVO> http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -V

  Explicação dos Argumentos:

        -l molly: Especifica o nome de utilizador fixo (molly).

        -P passwords.txt: Define a lista de palavras-passe (wordlist) a testar.

        <IP_ALVO>: O endereço IP da máquina instanciada no TryHackMe.

        http-post-form: O módulo do Hydra para formulários HTTP POST.

        "/:...:F=incorrect": Formato obrigatório dividido por dois pontos:

            A rota (/).

            O corpo dos parâmetros onde ^USER^ e ^PASS^ serão substituídos dinamicamente.

            A condição de falha (F=incorrect), indicando que se a palavra "incorrect" aparecer na resposta, o login falhou.

        -V: Ativa o modo verboso para exibir os testes em tempo real.

🔓 Flag 1 encontrada: [Insira a Flag 1 aqui]
2. Ataque de Força Bruta contra SSH

O segundo objetivo envolve realizar o brute-force contra o serviço SSH utilizando o utilizador root.

    Comando Utilizado:

Bash

hydra -l root -P passwords.txt <IP_ALVO> -t 4 ssh

    Explicação dos Argumentos:

        -l root: Define o utilizador como root.

        -P passwords.txt: Especifica a lista de palavras-passe.

        -t 4: Define o número de threads paralelas como 4 (ideal para não sobrecarregar ou bloquear o serviço SSH).

        ssh: O protocolo/serviço que está a ser atacado (porta padrão 22).

🔓 Flag 2 encontrada: [Insira a Flag 2 aqui]
🧠 Conclusões e Boas Práticas (Mitigação)

A execução deste laboratório demonstra o perigo da utilização de credenciais fracas ou reutilizadas. Para mitigar ataques de força bruta realizados por ferramentas como o Hydra, devem ser implementadas as seguintes defesas:

    Políticas de Palavras-passe Fortes: Exigir comprimento mínimo elevado, uso de maiúsculas, minúsculas, números e caracteres especiais.

    Mecanismos de Bloqueio de Conta (Lockout): Bloquear temporariamente a conta ou o IP de origem após um número consecutivo de tentativas falhadas (ex: usando Fail2ban no Linux).

    Autenticação Multi-Fator (MFA): Adicionar uma camada extra de proteção que invalida o sucesso de um ataque baseado apenas em password.

    Desativar Logins Diretos como Root: No caso do SSH, configurar /etc/ssh/sshd_config com PermitRootLogin no.

Aviso: Este repositório tem fins estritamente educativos. Nunca utilize estas ferramentas em sistemas ou redes para os quais não possui autorização explícita por escrito.
