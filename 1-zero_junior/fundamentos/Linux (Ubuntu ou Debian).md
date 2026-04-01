# Linux (Ubuntu ou Debian)

[[Fundamentos|<- Voltar para Fundamentos]]

---

## 1. O que exatamente é “Linux” nesse contexto

**Linux = kernel**: a parte central que fala com o hardware e oferece mecanismos como:

- escalonamento de CPU (processos/threads),
- gerenciamento de memória,
- drivers,
- system calls,
- rede e sistema de arquivos (no nível do kernel).

**Debian/Ubuntu = distribuições**: um “pacote” completo que inclui:

- kernel Linux,
- ferramentas GNU (shell, utilitários),
- bibliotecas,
- instalador,
- gerenciamento de pacotes,
- init system (hoje, geralmente **systemd**),
- e (opcionalmente) ambiente gráfico.

Uma visão em camadas (bem útil para arquitetura mental):

1. **Hardware**
2. **Kernel Linux**
3. **System utilities + package management (APT/dpkg)**
4. **Aplicações / Desktop / Serviços**

## 2. Debian vs Ubuntu: relação e diferenças práticas

### Debian (características típicas)

- Forte reputação de **estabilidade e confiabilidade**; muito usado em servidores.
- Mantém ramos (branches) como **stable, testing e unstable (sid)**.
- Cultura “Unix-like” e foco em boas práticas (usuário normal no dia a dia, root só quando necessário).
- Filosofia de software livre bem central (DFSG e repositórios organizados).

### Ubuntu (características típicas)

- **Derivado do Debian**, com foco forte em **usabilidade** e ritmo de releases mais agressivo; possui edições Desktop e Server.
- Em operações e infraestrutura, entender o “miolo Debian” ajuda muito a diagnosticar comportamentos e conflitos de pacotes/serviços.

### Tradução honesta para o dia a dia

- Se você quer um sistema “conservador e previsível”: Debian stable costuma ser o caminho.
- Se você quer “pronto para uso” e com integração/UX mais trabalhada (especialmente desktop): Ubuntu costuma ser mais amigável.

## 3. Terminal, shell e consoles virtuais (o habitat natural do Linux)

Em Debian/Ubuntu, mesmo com GUI, o terminal é ferramenta de primeira classe.

- **Shell prompt**: é onde você digita comandos e automatiza tarefas.
- **Consoles virtuais (TTYs)**: no Debian, existem múltiplos consoles de texto (VTs). Você pode alternar com ``Ctrl``+``Alt``+``F1``..``F6`` (varia conforme a distro/GUI, mas o conceito é o mesmo).

Por que isso importa?

- Administração remota, troubleshooting e automação dependem disso.
- Você consegue trabalhar mesmo quando o ambiente gráfico falha (o que é um “superpoder” de sysadmin).

## 4. Usuários, root, sudo e permissões

### Root e sudo

- **root** é o superusuário: pode ler/escrever/remover qualquer arquivo e alterar configurações críticas.
- Em workstation, é comum usar **sudo** para executar tarefas administrativas com a senha do usuário (menos risco do que ficar logado como root).

### Permissões e a ideia “tudo é arquivo”

- Arquivos e diretórios têm permissões (r/w/x) e donos (usuário/grupo).
- Dispositivos também aparecem como arquivos em ``/dev`` (ex.: discos, terminais), seguindo a tradição Unix de unificar acesso a recursos.

Isso conecta diretamente com segurança: um sistema bem configurado impede que um usuário normal “quebre” o sistema sem passar por privilégios.

## 5. Sistema de arquivos: árvore única, montagem e conceitos

### Pontos fundamentais

- Existe uma **raiz**: ``/`` (root directory). Não confundir com o diretório do usuário root: ``/root``.
- **Nomes são case-sensitive**: ``MeuArquivo`` ≠ ``meuarquivo``.
- Discos/partições e outros dispositivos entram na árvore via mount/umount (montagem/desmontagem).
- Informações do sistema e processos aparecem no filesystem (por exemplo, via ``/proc``), reforçando o estilo Linux/Unix de “expor estado como arquivos”.

Também vale uma regra de ouro operacional: desligar “no botão” pode corromper dados; o sistema usa cache e precisa de shutdown correto para descarregar alterações no disco.

## 6. Gerenciamento de pacotes: APT e dpkg (o “app store” do Debian/Ubuntu)

### Ferramentas

- **APT** (alto nível): instala, remove, atualiza e resolve dependências.
- **dpkg** (baixo nível): gerencia pacotes ``.deb`` diretamente.
- Configurações importantes do APT ficam em ``/etc/apt/`` (ex.: ``sources.list``).

### Comandos essenciais (mínimos e úteis)

```bash
sudo apt update
sudo apt upgrade
sudo apt install <pacote>
sudo apt remove <pacote>
sudo apt autoremove
```

Esses comandos viram o “pão com manteiga” de manter o sistema seguro e atualizado.

## 7. systemd: inicialização e serviços (especialmente em servidores)

Em Debian/Ubuntu modernos, systemd costuma ser o init system e gerenciador de serviços.

### Ideias centrais:

- Serviços são definidos por unit files (arquivos ``.service``, etc.).
- Existe diferença entre:
  - units fornecidas pela distro (ex.: em ``/lib/systemd/system/``)
  - overrides locais (ex.: em ``/etc/systemd/system/``)

Comandos comuns:

```bash
systemctl status <serviço>
systemctl start <serviço>
systemctl stop <serviço>
systemctl enable <serviço>   # sobe no boot
systemctl cat <serviço>      # ver unit + overrides
```

Isso é crucial para operar SSH, web servers, bancos de dados e jobs de infraestrutura.

## 8. Logs e diagnóstico: journalctl, dmesg e “olhar o que o sistema está dizendo”

- **journald/journalctl**: logs centralizados por serviço e por boot.

Exemplos:

```bash
journalctl -u sshd
journalctl -b
```

- **dmesg**: mensagens do kernel (muito útil para hardware, drivers e falhas baixas).

A habilidade de “ler logs” é uma das diferenças entre usar Linux e dominar Linux.

## 9. Segurança e hardening (noções iniciais)

As fontes citam práticas comuns em Ubuntu/Debian para produção:

- manter o sistema atualizado (``apt update && apt upgrade``),
- firewall simples (**ufw**),
- MAC (controle de acesso mandatário) com **AppArmor**,
- proteção contra brute-force com **fail2ban**,
- auditoria com **auditd** (dependendo do cenário).

Não é para “decorar ferramentas”, e sim entender o princípio: **reduzir superfície de ataque e limitar dano**.

---

# Navegação

[[Fundamentos|<- Voltar para Fundamentos]] | [[Linux (Ubuntu ou Debian)|Próximo tópico ->]]