# Como os programas rodam na memória
[[Fundamentos|<- Voltar para Fundamentos]]

[[Como Funciona um Computador|<- Voltar tópico]] | [[Como os processadores funcionam|Próximo tópico ->]]

---

## 1. Programa x Processo

Antes de falar de memória, é importante separar duas ideias:

### Programa

- É o arquivo no disco (HD/SSD): um .exe, um binário ELF no Linux, um .jar, etc.
- É algo estático, só código e dados armazenados.

### Processo

- É o programa em execução.
- Quando você “roda” um programa, o sistema operacional cria um processo: carrega o código para a memória, cria estruturas de controle, reserva espaço para variáveis, pilha etc.
- É o processo que realmente “ocupa memória RAM”.

Então, sempre que falarmos de “um programa na memória”, tecnicamente estamos falando do processo correspondente.

## 2. Do disco para a memória RAM

O fluxo básico é:

1. Você dá um comando para executar um programa (clique duplo, comando no terminal, etc)

2. O sistema operacional:
    - localiza o arquivo do programa no disco
    - lê o código e dados iniciais desse programa
    - copia (carrega) esse conteúdo para a memória RAM
    - cria um processo, com um espaço de endereçamento próprio 

3. O processador (CPU) começa a executar as instruções desse processo, buscando-as na RAM.

Ponto importante:

- Para ser executado, o programa precisa estar na RAM (pelo menos a parte que está sendo usada naquele momento). 
- O disco é armazenamento lento; a RAM é onde acontece a execução “de verdade”.

## 3. Espaço de endereçamento de um processo

Quando o processo é criado, o sistema operacional reserva para ele um espaço de endereçamento: um conjunto de endereços de memória que “pertencem” àquele processo.

Visto de forma simplificada, esse espaço se divide em regiões (nomes podem variar, mas a ideia é sempre parecida):

### 3.1. Segmento de código (text/code segment)

- Contém as **instruções do programa** (o código de máquina).
- É normalmente **somente leitura** (o programa não pode escrever aqui, só ler e executar). 
- É o “arquivo executável” carregado na memória.

### 3.2. Segmento de dados estáticos (data / bss)

- Guarda variáveis **estáticas e globais**, constantes e dados inicializados em tempo de compilação. 
- Tamanho conhecido antes da execução.

### 3.3. Heap

- Região usada para **memória dinâmica**, alocada em tempo de execução (malloc/free em C, new/delete em C++, objetos em Java, estruturas dinâmicas, etc.).
- Cresce ou diminui conforme o programa aloca e libera memória.
- Gerenciada pelo runtime da linguagem / bibliotecas / sistema operacional.

### 3.4. Pilha (stack)

- Usada para **chamadas de função**:
  - endereços de retorno (para onde voltar depois de uma função),
  - parâmetros de funções,
  - variáveis locais. 
- Normalmente cresce e encolhe conforme funções são chamadas e retornam.

Os detalhes dependem um pouco da linguagem e do sistema, mas quase todo programa em C, C++, Java, Python, etc. vai acabar com algo equivalente a:

- **Código**
- **Dados estáticos**
- **Heap**
- **Pilha**

Todos esses pedaços ficam dentro do **espaço de endereçamento do processo**.

## 4. Papel do PC e ciclo de execução (do ponto de vista da memória)

Lembrando o que a CPU faz, agora ligado à memória:

### 4.1. PC (Program Counter / Instruction Pointer)

- Registrador da CPU que guarda o **endereço da próxima instrução** do processo a ser executada.
- Esse endereço aponta para o **segmento de código** do processo na memória.

### 4.2. Ciclo básico: buscar–decodificar–executar

- **Buscar (fetch):** a CPU lê, da RAM, a instrução no endereço indicado pelo PC.
- **Decodificar (decode):** a CPU interpreta qual operação é.
- **Executar (execute):** a CPU realiza a operação:
  - pode ler/escrever dados da RAM (heap, dados, stack),
  - pode fazer chamadas de função (mexendo na pilha),
  - pode acessar dispositivos via sistema operacional.

### 4.3. Atualização do PC

- Normalmente: PC = PC + tamanho_da_instrução (segue em frente).
- Em desvios (if, loops, chamadas de função/retornos), o PC é alterado para outro endereço do **mesmo espaço de endereçamento do processo** (salvo quando se muda de processo/execução).

**Conexão importante**:

- A memória RAM guarda:
  - o código do programa (segmento de código),
  - dados (estáticos + heap + stack).
- A CPU só sabe executar instruções lendo esses bytes da RAM.
- O PC é o “ponto atual” dentro desse espaço de memória que está sendo executado.

## 5. Visão do sistema operacional: vários programas na memória

Na prática, quase nunca há só um programa em execução. O sistema operacional faz:

### 5.1. Carrega vários processos na memória

- Cada processo tem seu próprio **espaço de endereçamento virtual** (como se cada um tivesse sua própria “RAM particular”).
- O SO e o hardware (MMU, tabela de páginas) traduzem endereços virtuais em endereços físicos na RAM real.

### 5.2. Gerenciamento de memória

- **Alocação**: quando um processo pede memória (por exemplo, cria uma variável, aloca um array, um objeto), o SO ou o runtime reserva um **bloco de memória** para ele.
- **Reciclagem**: quando o processo libera memória (ou termina), esses blocos podem ser reutilizados.
- Lida com problemas como:
  - fragmentação interna e externa,
  - partições fixas/variáveis,
  - memória virtual, swap, etc. (assunto mais profundo de SO).

### 5.3. Multitarefa / multiprogramação

- Mantendo vários processos na memória, o SO pode alternar entre eles (troca de contexto), aumentando o uso da CPU.
- Mesmo com uma única CPU, parece que vários programas estão rodando ao mesmo tempo.

O ponto principal para você agora:

> *Cada processo enxerga um espaço de memória contínuo, com código, dados estáticos, heap e pilha. O sistema operacional cuida de mapear isso para a RAM física e, se necessário, para o disco (memória virtual, swap).*

## 6. Pequena conexão com outros tópicos (sem entrar em detalhe)

Resumindo como isso se conecta aos demais tópicos do seu estudo:

- **Como funciona um computador**

    - A RAM é o local onde ficam instruções e dados em uso.
    - A CPU busca instruções da RAM e lê/escreve dados nela.

- **Como os processadores funcionam**

    - Vamos detalhar: registradores, PC, ALU, pipeline, caches, etc., e como eles acessam a memória para buscar instruções e manipular dados.

- **Como os sistemas operacionais funcionam**

    - Vamos aprofundar nesses temas:
        - gerenciamento de memória (heap, stack, memória virtual, partições, swapping, etc.); 
        - criação e destruição de processos;
        - isolamento entre processos, proteção de memória.

- **Compilado vs interpretado**

    - Vamos ver como diferentes ambientes de execução usam a memória:
        - código nativo carregado direto,
        - bytecode + máquina virtual,
        - interpretadores que leem scripts e dados.

---

## Navegação
[[Fundamentos|<- Voltar para Fundamentos]]

[[Como Funciona um Computador|<- Voltar tópico]] | [[Como os processadores funcionam|Próximo tópico ->]]