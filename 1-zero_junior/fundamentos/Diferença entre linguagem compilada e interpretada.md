# Diferença entre linguagem compilada e interpretada

[[Fundamentos|<- Voltar para Fundamentos]]

[[Como os sistemas operacionais funcionam|<- Voltar tópico]] | [[Linux (Ubuntu ou Debian)|Próximo tópico ->]]

---

## 1. Primeiro ajuste mental: “linguagem” vs “implementação”

É comum dizer “Python é interpretado” e “C é compilado”, mas tecnicamente o mais correto é:

- A **linguagem**: regras (sintaxe/semântica) do que é válido escrever.
- A **implementação**: o conjunto de ferramentas que executa aquela linguagem (compilador, interpretador, VM, runtime).

Por isso, “uma linguagem pode ter implementações compiladas e interpretadas”, e muitos ecossistemas modernos são híbridos.

## 2. O que é compilação (modelo clássico)

### Definição

**Compilar** é traduzir o código-fonte (texto) para uma forma que a máquina consiga executar, tipicamente gerando um **binário nativo** (executável) ou código objeto que será linkado.

### Pipeline típico (visão de alto nível)

1. **Frontend**: lê o código e entende a estrutura
   - análise léxica (tokens), parsing, AST, checagens
2. **Middle-end**: otimizações em representação intermediária (IR)
3. **Backend**: gera código de máquina (para x86/ARM etc.)
4. **Linkedição (linker)**: junta bibliotecas e resolve símbolos → executável final

Esse “pacote final” é o que o sistema operacional carrega em memória para executar.

### Características práticas

- **Performance**: tende a ser alta porque a tradução pesada ocorre antes e pode ser bem otimizada.  
- **Erros detectados cedo**: muitos problemas aparecem no build (sintaxe, tipos, símbolos).  
- **Dependência de plataforma**: o binário costuma ser específico para ISA/OS (ex.: ARM vs x86; Linux vs Windows).

Exemplos comuns (modelo clássico): C, C++, Rust, Go (geralmente AOT para nativo).

## 3. O que é interpretação (modelo clássico)

### Definição

Na **interpretação**, um **interpretador** é um programa que lê o código (ou uma forma intermediária dele) e executa “por cima” de uma camada de software, em tempo de execução.

Há dois estilos frequentes:

- **Interpretador direto (AST-walk)**: executa caminhando pela árvore sintática.
- **Bytecode + VM**: compila para bytecode e uma máquina virtual executa esse bytecode.

As fontes destacam a ideia “linha a linha”, mas na prática muitos interpretadores modernos não fazem literalmente linha a linha; eles interpretam estruturas já analisadas/transformadas. O ponto é: **a tradução/execução ocorre durante o runtime**.

### Características práticas

- **Portabilidade**: o mesmo código roda onde houver interpretador/VM compatível.  
- **Ciclo rápido de desenvolvimento**: editar e rodar sem etapa de build pesada (ótimo para scripts e prototipagem).  
- **Overhead**: há custo extra em runtime (interpretação, checagens dinâmicas, dispatch), então tende a ser mais lento que nativo — embora isso varie muito com JIT e com o tipo de aplicação.

**Exemplos comuns**: Python, Ruby, PHP; JavaScript (muito associado a runtimes com JIT).

## 4. O “mundo real”: bytecode, VM e JIT (o motivo da confusão)

Muitos ecossistemas não são “100% compilado” nem “100% interpretado”.

### 4.1 Bytecode + VM

Fluxo típico: **Fonte → (compilador) → bytecode → (VM) → execução**

- Java compila para ``.class`` (bytecode) e executa na JVM; essa execução pode envolver interpretação e/ou JIT.
- Python frequentemente compila para bytecode (ex.: ``.pyc``) e executa em uma VM/interpreter (ex.: ``CPython``).

### 4.2 JIT (Just-in-Time)

**JIT** compila partes do programa durante a execução, geralmente focando em “hotspots” (trechos muito usados), para obter desempenho próximo do nativo depois de um tempo.

Consequências importantes:

- **Warm-up time**: o programa pode começar mais lento e acelerar depois.
- O runtime pode otimizar com base no comportamento real do programa (profiling).

## 5. Comparação direta (tabela objetiva)

| Aspecto | Compilado (AOT p/ nativo) | Interpretado/VM (sem JIT) | VM com JIT |
|---------|---------------------------|-----------------------------|------------|
| Tradução para máquina | Antes de rodar | Durante a execução (ou bytecode) | Parte antes (bytecode) + parte durante (JIT) |
| Desempenho típico | Alto e previsível | Menor (mais overhead) | Pode ficar alto após warm-up |
| Portabilidade | Menor (recompilar por plataforma) | Alta (onde tiver runtime) | Alta (onde tiver VM) |
| Feedback de erros | Muitos no build | Muitos em runtime | Misturado (depende do runtime e checks) |
| Deploy | Entrega binário | Entrega fonte/bytecode + runtime | Entrega bytecode + VM |

## 6. O que considerar para “compreensão completa” (pontos que realmente importam)

1. **Quem executa no fim é a CPU**: tudo vira instrução de máquina, seja antes (AOT) ou durante (JIT/interpretação).  
2. **Performance não depende só disso**: acesso a memória, I/O, algoritmos e concorrência podem dominar (mesmo com código “rápido”).
3. **Trade-off de produto**:
   - sistemas de alto desempenho/baixo nível: AOT e controle fino ajudam
   - scripts, automação, protótipos: interpretação/VM acelera iteração  
4. **Segurança e distribuição**:
   - binário nativo costuma ocultar mais o código-fonte (embora engenharia reversa exista)
   - scripts geralmente distribuem fonte (ou bytecode), mudando o modelo de distribuição.

---

# Navegação

[[Fundamentos|<- Voltar para Fundamentos]]

[[Como os sistemas operacionais funcionam|<- Voltar tópico]] | [[Linux (Ubuntu ou Debian)|Próximo tópico ->]]