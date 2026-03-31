# Como os processadores funcionam
[[Fundamentos|<- Voltar para Fundamentos]]

[[Como os programas rodam na memória|<- Voltar tópico]] | [[Como os sistemas operacionais funcionam|Próximo tópico ->]]

---

Pense na CPU como uma fábrica de altíssima velocidade que recebe “ordens” (instruções) e “matéria-prima” (dados), e tenta manter todas as esteiras ocupadas o tempo todo.

## 1. O que a CPU faz (em alto nível)

Em essência, uma CPU:

- **Lê instruções** de alguma memória
- **Interpreta** o que cada instrução pede
- **Executa** operações aritméticas/lógicas e movimentações de dados
- **Decide desvios** (ifs/loops)
- **Grava resultados** em registradores ou memória

Isso vale desde CPUs simples até as modernas; o que muda é o quanto de paralelismo e otimizações são usadas por baixo dos panos.

## 2. Componentes fundamentais dentro da CPU

### 2.1 Unidade Lógica e Aritmética (ALU)

A ALU executa operações como soma, subtração, AND/OR/XOR, comparações etc.  
Algumas CPUs também têm unidades específicas para ponto flutuante (FPU) ou vetores/SIMD (conceito; detalhes variam por arquitetura). A ideia: mais unidades especializadas = mais tipos de cálculo eficientes.

### 2.2 Registradores

**Registradores** são pequenas memórias dentro da CPU, muito rápidas, usadas para:  

- guardar operandos (valores de entrada),
- guardar resultados,
- guardar endereços,
- controlar a execução.

Registradores clássicos/conceituais importantes:

- **PC (Program Counter)**: endereço da próxima instrução.  
- **IR (Instruction Register)**: onde a instrução buscada pode ser mantida para decodificação (conceito comum em CPUs didáticas).  
- **Flags/Status**: bits que indicam condições (zero, negativo, carry etc.) para instruções de desvio condicional (conceito comum; implementação varia).

### 2.3 Unidade de Controle (UC)

A **unidade de controle** coordena a CPU:

- manda buscar instruções,
- decodifica o opcode,
- gera sinais para mover dados entre registradores / ALU / memória,
- decide a sequência de microações necessárias para executar a instrução.

Em algumas arquiteturas, a decodificação/controle pode ser mais “hardwired” (fixo em lógica) ou mais “microprogramado” (conceito geral). O ponto principal: **controle orquestra o restante**.

### 2.4 Barramentos e sinais (visão interna/externa)

Para conversar com memória e I/O, a CPU usa caminhos equivalentes a:  

- **Barramento de endereços** (qual posição acessar),
- **Barramento de dados** (o que é lido/escrito),
- **Sinais de controle** (ler, escrever, pronto, etc.).

A descrição exata muda (em PCs modernos há controladores e protocolos mais complexos), mas o princípio permanece.

### 2.5 Clock (relógio)

A CPU é tipicamente um circuito síncrono: ela avança em “batidas” do clock, que define o ritmo de mudanças de estado internas.  
Nem toda instrução “termina em 1 batida”; o importante é: **o clock coordena quando dados mudam e quando são capturados**.

## 3. O ciclo de instrução (fetch → decode → execute)

Quase toda explicação de CPU começa aqui porque é a “unidade atômica” do funcionamento.  

### 3.1 Fetch (buscar a instrução)

1. O **PC** contém o endereço da próxima instrução.  
2. A **CPU** pede à memória o conteúdo daquele endereço.
3. A instrução chega e é colocada em um registrador interno (conceitualmente, **IR**).  
4. O **PC** é atualizado para apontar para a próxima instrução (ou preparado para isso, dependendo do fluxo).

**Problema clássico:** memória pode ser “lenta” comparada à CPU, causando **stall** (CPU esperando).

### 3.2 Decode (decodificar)

A CPU separa a instrução em:

- **opcode**: “o que fazer” (somar, carregar, comparar, pular…)
- **operandos**: “com o quê/onde” (quais registradores, endereços, imediatos).

Essa decodificação vira sinais internos que configuram o “caminho de dados” (data path) para a execução.

### 3.3 Execute (executar)

A execução varia conforme a instrução:

- **Aritmética/lógica**: ALU opera sobre registradores e coloca resultado em registrador.  
- **Load/Store**: a CPU calcula um endereço e lê/escreve memória.  
- **Branch/jump**: altera o PC (possivelmente baseado em flags/condições).  

Muitas CPUs têm uma unidade dedicada para calcular endereços, tipo **AGU (Address Generation Unit)**, para acelerar acessos à memória (ex.: arrays).

## 4. “CPU moderna”: por que ela é muito mais complexa que esse ciclo?

O ciclo acima descreve o que precisa acontecer. O desafio real é: **fazer isso muito rápido**, evitando ficar parado esperando memória e aproveitando paralelismo.

### 4.1 Cache (para não esperar a RAM o tempo todo)

Como a RAM é relativamente lenta, CPUs usam hierarquia de caches (L1, L2, L3…) para guardar dados/instruções acessados com frequência.

Quanto mais perto da CPU, **mais rápido e menor** tende a ser.  

### 4.2 Pipeline (linha de montagem)
Em vez de terminar uma instrução inteira antes de começar a próxima, a CPU sobrepõe etapas:

- enquanto uma instrução está executando, outra está sendo decodificada e outra está sendo buscada.  

Isso aumenta a “vazão” (throughput). A analogia é uma linha de produção: cada estação trabalha em uma etapa diferente ao mesmo tempo.

### 4.3 Superscalar (várias linhas de montagem)
Além do pipeline, algumas CPUs conseguem emitir/executar mais de uma instrução por ciclo (IPC > 1) quando não há dependências.  

### 4.4 Execução fora de ordem (Out-of-Order) e especulativa
Para evitar ociosidade, a CPU pode:

- **reordenar** a execução de instruções (mantendo o resultado final correto) para contornar dependências e esperas,  
- **executar especulativamente** caminhos prováveis (ex.: após um branch) e descartar se a previsão estiver errada.  

Isso melhora desempenho, mas aumenta muito a complexidade interna.

### 4.5 Multicore e SMT

- **Multicore**: um chip pode ter vários núcleos, cada um com seu próprio ciclo de instrução.  
- **SMT** (ex.: “hyper-threading” conceitualmente): um mesmo núcleo pode manter mais de um “fluxo” de execução para ocupar melhor as unidades internas.

## 5. Como a CPU “enxerga” programas e memória (conexão mínima com o tópico anterior)

Sem entrar fundo em sistema operacional ainda, a conexão essencial é:

- A CPU **executa instruções** que estão na memória, e manipula dados que também estão na memória (ou em registradores/caches).
- O **PC** aponta para onde está o “próximo passo” do programa.
- Quando a CPU precisa de algo que não está em cache, ela pode **parar (stall)** esperando — e isso é um dos motivos de existirem caches, pipeline, OoO etc.

---

## Navegação
[[Fundamentos|<- Voltar para Fundamentos]]

[[Como os programas rodam na memória|<- Voltar tópico]] | [[Como os sistemas operacionais funcionam|Próximo tópico ->]]