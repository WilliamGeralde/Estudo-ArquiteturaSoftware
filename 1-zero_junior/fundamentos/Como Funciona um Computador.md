# Como Funciona um Computador

## 1. O que é um computador?

De forma bem direta:

- Um computador é uma máquina eletrônica que:
  - recebe **entradas** (input),
  - **processa** essas informações segundo **instruções** (programas),
  - **armazena** dados,
  - gera **saídas** (output).

Ele é programável: você pode mudar o programa e ele passa a fazer coisas diferentes (um editor de texto, um jogo, um servidor, etc.), sem trocar o hardware.

## 2. Arquitetura Básica (modelo Von Neumann)

Quase todos os computadores modernos seguem uma variação da **arquitetura de Von Neumann**. Nessa arquitetura, um computador é composto basicamente por quatro grandes blocos:

### 2.1. CPU (UCP – Unidade Central de Processamento)

- Dentro dela temos:
    - **ULA (Unidade Lógica e Aritmética)** - faz contas e operações lógicas.
    - **UC (Unidade de Controle)** - coordena o que a CPU faz, busca instruções na memória, controla o fluxo de execução.
    - **Registradores** - pequenas memórias muito rápidas dentro da CPU.

### 2.2. Memória Principal (RAM)

- Onde ficam instruções (programas) e dados enquanto o computador está executando algo.

### 2.3. Dispositivos de Entrada e Saída (E/S ou I/O)

- Entrada: teclado, mouse, microfone, câmera, scanner etc.
- Saída: monitor, impressora, caixas de som etc.
- Muitos dispositivos podem ser de entrada e saída ao mesmo tempo (placa de rede, disco, multifuncional).

### 2.4. Barramentos (Buses)

- Conjuntos de fios/canais que transportam:
    - **dados** (barramento de dados),
    - **endereços de memória** (barramento de endereço),
    - **sinais de controle** (barramento de controle).

A ideia central: **programas e dados ficam juntos na memória**, e a CPU busca instruções da memória, uma por vez (ou algumas por vez, em arquiteturas modernas), executa e repete isso em loop.

## 3. Principais componentes de hardware

### 3.1. CPU (Processador)

Função: é o “cérebro” do computador. Ele:

- Busca instruções na memória (RAM).
- Decodifica essas instruções (descobre o que fazer).
- Executa operações:
  - contas aritméticas (somar, subtrair, multiplicar, etc.),
  - operações lógicas (comparar valores, AND, OR, NOT),
  - acesso à memória (ler/escrever dados),
  - controle de I/O (pedir dados para um disco, por exemplo).
- Escreve resultados de volta em registradores ou na memória.

Componentes internos importantes:

- **Registradores** - células de memória extremamente rápidas dentro da CPU, usadas para guardar:
    - dados intermediários,
    - endereços de memória,
    - o **PC (Program Counter)**, que aponta para a próxima instrução.
- **ULA (Unidade Lógica e Aritmética)** - unidade que realmente faz as contas e as operações lógicas.
- **UC (Unidade de Controle)** - coordena tudo:
    - lê instruções de memória,
    - decodifica,
    - manda sinais para ULA, para memória e para I/O,
    - atualiza o PC (próxima instrução, salto, loop, etc.).

A CPU trabalha em ciclos de clock (GHz = bilhões de ciclos por segundo). Em cada ciclo (ou em alguns ciclos), ela pode buscar, decodificar ou executar uma instrução.

### 3.2. Memória

No nível lógico, você pode imaginar a memória como uma grande lista de células, cada célula com um endereço (0, 1, 2, 3, …) e contendo um valor (em bits/bytes).

Tipos principais:

#### 1. Memória RAM (volátil)

- Guarda instruções e dados de programas em execução.
- É rápida, mas o conteúdo **some quando o computador desliga**.  
- Exemplo: 8 GB, 16 GB de RAM em PCs.

#### 2. Memória Cache (dentro ou muito perto da CPU)

- Ainda mais rápida que a RAM.
- Guarda **dados e instruções usados com frequência**, para evitar acesso à RAM o tempo todo.
- Geralmente implementada com tecnologia SRAM, mais cara mas mais rápida.

#### 3. Memória de armazenamento permanente (não volátil)

- Disco rígido (HD) ou SSD.
- Conteúdo **permanece mesmo quando o computador é desligado**.  
- Guarda sistema operacional, programas, arquivos do usuário.

#### 4. ROM / Firmware

- Memória de somente leitura (ou preferencialmente de leitura) que guarda o **firmware** – código que roda logo ao ligar o computador (BIOS/UEFI, POST etc.).

### 3.3. Dispositivos de Entrada e Saída (E/S ou I/O)

Responsáveis por conectar o computador ao “mundo externo”.

- **Entrada**: teclado, mouse, scanner, microfone, câmera, interfaces de rede (recebendo dados).
- **Saída**: monitor, impressora, alto-falantes, interface de rede (enviando dados).
- **Entrada e Saída**: discos, pen drives, impressoras multifuncionais, placas de rede.

Todos eles têm em comum:

- **Entrada**: convertem sinais do mundo físico (teclas, som, luz) em dados digitais (bits) que o computador consegue processar.
- **Saída**: convertem bits em algo que humanos entendem (texto na tela, som, imagem impressa).

### 3.4. Placa-mãe (Motherboard) e Barramentos

- A **placa-mãe** é a placa principal que conecta:
    - CPU,
    - memória,
    - chips de suporte,
    - slots de expansão (placa de vídeo, som, rede),
    - conectores para I/O (USB, áudio, rede, etc.).
- Ela possui **barramentos** (conjuntos de trilhas/cabos) que carregam:
    - **dados** (barramento de dados),
    - **endereços** (barramento de endereços),
    - **sinais de controle** (barramento de controle).

É como o “sistema circulatório” que permite que tudo se comunique.

### 3.5. Fonte de Alimentação e Refrigeração

- A **fonte** pega energia da tomada (AC) e converte para tensões adequadas (DC) para CPU, memória, discos etc.  
- Há reguladores de tensão na placa-mãe para alimentar a CPU e outros componentes de forma estável.  
- **Refrigeração**:
  - Dissipadores, coolers e fans para evitar superaquecimento, principalmente da CPU e da GPU.

## 4. Como o computador funciona “em movimento”

Vamos ver o fluxo de forma cronológica, do momento em que você aperta o botão de ligar até rodar um programa.

### 4.1. Ao apertar o botão de ligar

1. A **fonte** começa a fornecer energia para a placa-mãe e demais componentes.
2. A placa-mãe executa o firmware (BIOS/UEFI) armazenado em ROM:  
    - Roda o **POST** (Power-On Self Test) para checar CPU, RAM, vídeo etc.
    - Se tudo ok, ela procura um dispositivo de boot (disco, SSD, etc.).
3. O firmware carrega o **bootloader**, que por sua vez carrega o sistema operacional (por exemplo, Linux) do disco para a memória RAM.

(Detalhes de SO será visto em outro tópico.)

### 4.2. Ciclo de busca-decodificação-execução (fetch-decode-execute)

Esse é o loop que a CPU faz o tempo todo enquanto o computador está ligado:

1. **Busca (Fetch)**
    - A unidade de controle lê o valor do PC (Program Counter) – o endereço da próxima instrução.
    - Através do barramento, a CPU pede à memória “me dê o conteúdo do endereço X”.
    - A memória responde com o código da instrução, que vai para um registrador da CPU.
2. **Decodificação (Decode)**
    - A unidade de controle interpreta o código:
        - “é uma soma?”
        - “é um salto condicional?”
        - “é uma instrução de leitura/escrita na memória?”
3. **Execução (Execute)**
    - Se for uma operação aritmética/lógica, a ALU faz o cálculo.
    - Se for leitura/escrita em memória, a CPU solicita isso através do barramento.
    - Se for uma operação de I/O, a CPU conversa com o dispositivo correspondente (usualmente mediada pelo sistema operacional).
4. **Atualização do PC**
    - Normalmente, o PC é incrementado para apontar para a próxima instrução.
    - Em instruções de desvio (`if`, `loops`, `chamadas de função`), o PC é alterado para outro endereço. Isso implementa **fluxo de controle** (condicionais, repetições, funções).

Isso acontece bilhões de vezes por segundo em CPUs modernas.

### 4.3. Entrada, processamento, saída

Colocando em termos funcionais:

- **Entrada**: você digita algo no teclado ou move o mouse.
    - O periférico converte isso em sinais elétricos, que são convertidos em códigos (scancodes, por exemplo).
    - Esses dados chegam à CPU (via controladores de I/O, placa-mãe, barramentos).
- **Processamento**:
    - A CPU recebe esses dados, um programa (por exemplo, um editor de texto) interpreta o que foi digitado, atualiza um buffer, recalcula o que precisa ser mostrado na tela.
- **Saída**:
    - O programa pede para o sistema operacional atualizar a tela.
    - A GPU/placa de vídeo recebe comandos e desenha novos pixels na tela.

Enquanto isso, o sistema operacional gerencia memória, processos, acesso a disco, rede etc. (tema de outro tópico).

## 5. Relação entre hardware e software (visão inicial)

Só para amarrar com os próximos tópicos:
- O **hardware** entende apenas instruções muito simples, codificadas em **linguagem de máquina** (bits).
- A **ISA (Instruction Set Architecture)** define esse conjunto de instruções (somar, comparar, carregar da memória, armazenar, saltar, etc.).
- **Assembly** é uma linguagem de baixo nível que representa praticamente 1:1 essas instruções da ISA.
- Linguagens de alto nível (C, Java, Python) são convertidas (compiladas ou interpretadas) em instruções que a CPU consegue executar.

Nos próximos tópicos (“como os programas rodam na memória”, “como o processador funciona”, “como o SO funciona” e “compilado vs interpretado”) vamos descendo degrau por degrau dessa pilha.