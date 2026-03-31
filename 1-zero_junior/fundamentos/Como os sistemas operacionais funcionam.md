# Como os sistemas operacionais funcionam

[[Fundamentos|<- Voltar para Fundamentos]]

[[Como os processadores funcionam|<- Voltar tópico]] | [[Diferença entre linguagem compilada e interpretada|Próximo tópico ->]]

---

## 1. O que é “SO” na prática (camadas)

Um SO não é “um programa só”, é um conjunto de componentes. Uma visão útil em camadas:

- **Aplicações (user space)**: navegador, editor, servidor, IDE.
- **Bibliotecas e runtimes**: libc, frameworks, JVM/CLR etc. (muitas vezes “escondem” detalhes do SO).
- **Kernel (núcleo do SO)**: parte central que controla recursos e impõe proteção.
- **Drivers**: “tradutores” entre o kernel e dispositivos.
- **Hardware**: CPU, RAM, disco, placa de rede, GPU, controladores.

O SO existe porque:

- vários programas competem por recursos (CPU, memória, I/O) e alguém precisa arbitrar isso,
- precisamos de **abstrações estáveis** (arquivos, processos, sockets) para que aplicações não precisem conhecer cada detalhe do hardware.

## 2. Kernel, modo usuário e proteção

Um ponto-chave para entender “como funciona” é que a CPU normalmente opera em dois modos:

- **Modo usuário (user mode)**: código de aplicação tem poderes limitados; o hardware impede certas instruções/acessos.
- **Modo kernel/supervisor (kernel mode)**: o kernel tem privilégios totais para configurar hardware, mapear memória, lidar com interrupções etc.

**Por que isso importa?**
Porque é a base do isolamento: um programa não deveria conseguir:

- ler/escrever memória de outro,
- controlar o disco “direto”,
- monopolizar a CPU,
- derrubar o sistema inteiro com facilidade.

## 3. Chamadas de sistema (syscalls): a “porta” de entrada para o kernel

Aplicações executam **direto na CPU** quase o tempo todo, mas quando precisam de um serviço privilegiado (I/O, criar processo, abrir arquivo), elas fazem uma **system call**.

Exemplos clássicos de syscalls (conceitualmente):

- abrir/ler/escrever arquivos,
- criar processos/threads,
- alocar memória virtual,
- enviar/receber dados na rede.

### Fluxo mental:

![[diagram.png]]

Isso explica a frase “o SO é intermediário”: o app **pede**, o kernel **decide e faz**.

## 4. Processos e threads: como programas “existem” para o SO

### 4.1 Processo

Quando você executa um programa, o kernel cria um **processo**: define identidade, recursos, prioridade, memória, e inicia a execução.  

O kernel mantém metadados (conceitualmente um **PCB – Process Control Block**) para saber:

- estado (rodando, pronto, bloqueado),
- registradores salvos,
- mapa de memória,
- arquivos abertos, permissões, etc.  

### 4.2 Thread

Uma **thread** é a unidade de execução escalonada; threads:

- têm **PC (program counter)**, conjunto de registradores e pilha própria,  
- mas podem compartilhar **código e heap** com outras threads do mesmo processo.  

Por isso, criar thread costuma ser “mais leve” do que criar processo (conceitualmente).

## 5. Escalonamento (scheduling) e troca de contexto (context switch)

Como vários processos/threads parecem rodar ao mesmo tempo?

- O kernel decide **quem usa a CPU e por quanto tempo** (timeslice).  
- Para alternar, faz **troca de contexto**:
    - salva registradores/estado da thread atual,
    - carrega registradores/estado da próxima.  

Quando há mais threads do que núcleos, o kernel alterna ao longo do tempo; com múltiplos núcleos, dá para executar em paralelo.  

**Detalhe importante:** interrupções e eventos de I/O influenciam o escalonamento — uma thread pode ser bloqueada esperando disco/rede/teclado e outra assume a CPU.

## 6. Interrupções: como o hardware “chama” o SO

Dispositivos são muito mais lentos que CPU. Se a CPU tivesse que ficar checando (“polling”), seria desperdício. Então usamos **interrupções**:

- Um dispositivo sinaliza: “terminei” ou “preciso de atenção”.
- A CPU desvia para um **tratador de interrupção (ISR)** no kernel.
- Isso pode causar uma **troca de contexto**

### DMA (Direct Memory Access)

Para transferências grandes, o SO pode configurar **DMA** para o dispositivo copiar dados **direto entre dispositivo e RAM**, e só interromper no final (em vez de interromper por byte/palavra).

## 7. Gerenciamento de memória: do isolamento à memória virtual

Essa é uma das partes mais “núcleo” do SO moderno.

### 7.1 Por que o SO gerencia memória?

- garantir que processos não invadam memória uns dos outros (proteção),
- manter vários programas residentes,
- lidar com falta de RAM usando disco (swap),
- oferecer a ilusão de que cada processo tem um espaço “contínuo e privado”.  

### 7.2 Memória virtual (paging/segmentation)

Com **memória virtual**, o kernel pode mapear os endereços que o processo usa para frames de RAM (e, se necessário, para disco).  

- Um acesso inválido gera uma exceção tipo **segmentation violation (segfault)** e o kernel intervém (muitas vezes encerrando o processo).  
- Um acesso a uma página ainda não carregada pode gerar **page fault**; o kernel então carrega/mapa a página necessária.  

### 7.3 Swapping

Quando a RAM aperta, páginas pouco usadas podem ser movidas para disco temporariamente (**swapping**).  

> *O “pulo do gato” conceitual: memória virtual dá ao kernel poder de decidir onde a memória “mora” (RAM vs disco) e quem pode acessar quais regiões.*

## 8. Sistema de arquivos e armazenamento: bytes com significado

Disco/SSD é só um repositório de blocos. O SO organiza isso via **filesystem** para fornecer:

- nomes (caminhos), diretórios,
- metadados (tamanho, datas),
- permissões,
- operações padronizadas: criar, abrir, ler, escrever, fechar.  

As syscalls de arquivo permitem que apps trabalhem com “arquivos” em vez de “blocos crus”.

## 9. I/O e drivers: como o SO fala com periféricos

O kernel não quer conhecer cada modelo de impressora, SSD, placa de rede. Então ele usa **drivers**, que traduzem:

- “quero escrever estes bytes” (pedido do kernel)
- para os comandos específicos do dispositivo.

Isso permite que o SO suporte milhares de combinações de hardware sem que aplicações mudem.

## 10. Segurança e controle de acesso

O kernel impõe:

- **isolamento entre aplicações** (evitar que falhas/malware afetem outros),  
- **controle de recursos** (evitar monopolização de CPU/memória),  
- **mecanismos de permissões/autenticação** (tema grande, mas o princípio é o kernel ser o “porteiro”).

## 11. Boot (bem resumido, só para contexto do SO “nascer”)

Ao ligar:

- firmware (BIOS/UEFI) inicia e verifica hardware,
- carrega um bootloader,
- bootloader carrega o kernel na memória,
- kernel inicializa drivers/serviços e o sistema passa a operar.  

(Detalhar boot é opcional para arquitetura de software, mas ajuda a fechar a história “como o SO começa”.)

---

# Navegação

[[Fundamentos|<- Voltar para Fundamentos]]

[[Como os processadores funcionam|<- Voltar tópico]] | [[Diferença entre linguagem compilada e interpretada|Próximo tópico ->]]