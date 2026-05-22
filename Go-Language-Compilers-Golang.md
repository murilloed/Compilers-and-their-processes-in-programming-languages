# Aula — Compiladores Da Linguagem Go / Golang
<img width="889" height="499" alt="image" src="https://github.com/user-attachments/assets/7bf55331-6c14-4715-9970-2cc2c18cacca" />
### Prof.Dr. Murillo Carvalho
## Compiladores

## Linguagem Go / Golang

### Go Compiler • SSA • Runtime • Garbage Collector • Goroutines • Linker

---

## 1. Introdução

Quando escrevemos Go:

```go
package main

import "fmt"

func main() {
    fmt.Println("Olá Go")
}
```

Para humanos, isso é código simples, legível e de alto nível.

Para a máquina, ainda não é executável diretamente. O computador não entende `fmt.Println`, `func main`, `package` ou `import`. Tudo isso precisa ser analisado, transformado, otimizado e convertido em código de máquina.

A diferença importante é que Go, diferente de Python e Java, normalmente gera um **executável nativo**.

---

## 2. Grande Diferença Entre C, Java, Python E Go

| Linguagem | Modelo principal |
|---|---|
| C | Compila para código nativo |
| Java | Compila para bytecode JVM |
| Python | Compila para bytecode Python e interpreta |
| Go | Compila para executável nativo com runtime embutido |

A ideia central:

> Go é uma linguagem compilada para código nativo, mas carrega junto um runtime próprio para gerenciar goroutines, garbage collector, stacks e scheduler.

---

## 3. Pipeline Da Compilação Go

Fluxo simplificado:

```text
main.go
 ↓
go build
 ↓
Scanner / Lexer
 ↓
Parser
 ↓
AST
 ↓
Type Checking
 ↓
IR
 ↓
SSA
 ↓
Otimizações
 ↓
Geração de código
 ↓
Assembler
 ↓
Linker
 ↓
Executável nativo
 ↓
CPU
```

Esse pipeline mostra que Go não executa o `.go` diretamente. O código passa por várias fases internas até virar binário.

---

## 4. Go Gera Bytecode?

Na implementação padrão, não.

Go normalmente não gera bytecode como Java ou Python. Ele gera código de máquina nativo para uma arquitetura específica.

Exemplo:

```text
Windows x86-64 → .exe
Linux x86-64   → ELF
macOS          → Mach-O
```

Então, quando compilamos Go, o resultado final é um executável real do sistema operacional.

---

## 5. O Que É `go build`?

`go build` é o comando que organiza o processo de compilação.

Exemplo:

```bash
go build main.go
```

Ele faz mais do que chamar o compilador. Ele:

- lê módulos e pacotes;
- resolve imports;
- compila pacotes;
- chama o assembler;
- chama o linker;
- gera o executável final.

Em Go, o comando `go` funciona como uma ferramenta de construção completa.

---

## 6. O Compilador Go

O compilador principal da implementação oficial fica no toolchain do Go.

Ele é conhecido como:

```text
cmd/compile
```

Ele recebe código Go e transforma em código objeto.

Fluxo:

```text
.go
 ↓
cmd/compile
 ↓
arquivo objeto
 ↓
linker
 ↓
executável
```

O compilador Go moderno é escrito majoritariamente em Go.

---

## 7. Etapa 1 — Scanner / Lexer

O lexer transforma texto em tokens.

Código:

```go
idade := 20
```

Tokens:

| Token | Tipo |
|---|---|
| `idade` | identificador |
| `:=` | operador de declaração curta |
| `20` | literal inteiro |

O compilador não trabalha diretamente com texto cru. Primeiro ele quebra o código em unidades significativas.

---

## 8. Etapa 2 — Parser

O parser verifica se os tokens formam uma estrutura válida da linguagem.

Exemplo inválido:

```go
func main( {
    fmt.Println("erro")
}
```

Aqui há erro sintático, porque a declaração da função está malformada.

O parser responde:

> Essa sequência de tokens respeita a gramática da linguagem Go?

---

## 9. Etapa 3 — AST

AST significa **Abstract Syntax Tree**, ou árvore sintática abstrata.

Código:

```go
x := 2 + 3
```

Representação conceitual:

```text
Declaração :=
 ├── variável: x
 └── expressão +
      ├── 2
      └── 3
```

A AST não guarda apenas texto. Ela guarda a estrutura lógica do programa.

---

## 10. Etapa 4 — Análise Semântica E Tipagem

Go é uma linguagem estaticamente tipada.

Isso significa que muitos erros são encontrados antes da execução.

Exemplo:

```go
var x int
x = "texto"
```

Erro:

```text
cannot use "texto" as int
```

O compilador verifica:

- tipos;
- escopos;
- imports;
- uso de variáveis;
- interfaces;
- chamadas de função;
- retornos;
- atribuições.

Em Go, variável declarada e não usada gera erro de compilação.

---

## 11. Go E Tipagem Estática

Em Python:

```python
x = "texto"
x = 10
```

Isso é permitido.

Em Go:

```go
x := "texto"
x = 10
```

Isso gera erro, porque `x` foi inferido como `string`.

Go tem inferência de tipo, mas não é dinamicamente tipado.

---

## 12. Etapa 5 — IR

IR significa **Intermediate Representation**, ou representação intermediária.

Depois que o compilador entende o programa, ele transforma a AST em uma forma mais adequada para otimização.

A IR é uma ponte entre:

```text
Código Go de alto nível
 ↓
Representação interna do compilador
 ↓
Código de máquina
```

O compilador não otimiza diretamente o texto original. Ele otimiza representações internas.

---

## 13. Etapa 6 — SSA

Go usa SSA em sua pipeline de otimização.

SSA significa:

```text
Static Single Assignment
```

A ideia é que cada variável intermediária receba valor uma única vez.

Exemplo conceitual:

```go
x = 10
x = x + 5
```

Forma SSA:

```text
x1 = 10
x2 = x1 + 5
```

Isso ajuda o compilador a entender fluxo de dados, eliminar código inútil e otimizar registradores.

---

## 14. Otimizações Do Compilador Go

O compilador pode realizar otimizações como:

- constant folding;
- dead code elimination;
- inlining;
- escape analysis;
- bounds check elimination;
- register allocation;
- otimização de chamadas;
- otimizações em laços.

Exemplo:

```go
x := 2 + 3
```

Pode virar:

```go
x := 5
```

Antes mesmo do programa rodar.

---

## 15. Escape Analysis

Escape analysis é uma das partes mais importantes do Go.

Ela decide se uma variável pode ficar na stack ou precisa ir para o heap.

Exemplo:

```go
func cria() *int {
    x := 10
    return &x
}
```

Como `x` é retornado por referência, ele não pode morrer ao final da função. Então provavelmente “escapa” para o heap.

Comando para visualizar:

```bash
go build -gcflags="-m" main.go
```

Mensagem típica:

```text
x escapes to heap
```

Isso é importante para performance e uso de memória.

---

## 16. Stack E Heap Em Go

Em Go:

- stack guarda chamadas de função e variáveis locais;
- heap guarda objetos que precisam sobreviver além do escopo local;
- o garbage collector limpa objetos do heap que não são mais usados.

Go possui stacks de goroutines que começam pequenas e crescem conforme necessário.

Essa é uma diferença importante em relação a threads tradicionais.

---

## 17. Garbage Collector Em Go

Go tem garbage collector automático.

Isso significa que o programador normalmente não usa:

```c
malloc()
free()
```

Em Go:

```go
p := &Pessoa{}
```

O runtime decide quando liberar memória que não é mais alcançável.

O GC de Go é projetado para baixa latência, o que é importante em servidores, APIs, sistemas web e serviços concorrentes.

---

## 18. Go Runtime

O executável Go contém mais do que o seu código.

Ele também carrega partes do runtime.

O runtime é responsável por:

- garbage collector;
- scheduler de goroutines;
- gerenciamento de stacks;
- canais;
- timers;
- mapas;
- panic/recover;
- interface com o sistema operacional.

Então Go gera executável nativo, mas esse executável inclui uma infraestrutura de execução.

---

## 19. Goroutines

Goroutines são unidades leves de concorrência.

Exemplo:

```go
go tarefa()
```

Isso cria uma goroutine.

Ela é mais leve que uma thread do sistema operacional.

A grande ideia:

```text
Muitas goroutines
 ↓
Poucas threads do SO
 ↓
Scheduler do Go organiza tudo
```

O runtime Go decide quais goroutines executam em quais threads.

---

## 20. Scheduler Do Go

O scheduler do Go trabalha com o modelo M:N.

De forma simplificada:

```text
G = goroutine
M = machine/thread do SO
P = processor lógico do runtime
```

Modelo:

```text
Goroutines → Scheduler Go → Threads do SO → CPU
```

Isso permite que milhares de goroutines sejam executadas de forma eficiente.

---

## 21. Channels

Channels são estruturas usadas para comunicação entre goroutines.

Exemplo:

```go
canal := make(chan int)

go func() {
    canal <- 10
}()

valor := <-canal
fmt.Println(valor)
```

Channels conectam compilador, runtime e concorrência.

Eles não são apenas uma estrutura da linguagem. Eles dependem do runtime para sincronização e comunicação.

---

## 22. Geração De Código

Depois das otimizações, o compilador gera código específico para a arquitetura.

Exemplo:

```text
amd64
arm64
386
wasm
riscv64
```

Go consegue compilar para várias plataformas usando variáveis como:

```bash
GOOS=linux
GOARCH=amd64
go build
```

Isso é chamado de cross-compilation.

---

## 23. Assembler E Linker

Depois da geração de código, entram o assembler e o linker.

Fluxo:

```text
Código Go
 ↓
Compilador
 ↓
Código objeto
 ↓
Assembler
 ↓
Linker
 ↓
Executável
```

O linker junta:

- seu código;
- pacotes importados;
- runtime Go;
- metadados;
- símbolos;
- informações necessárias para execução.

---

## 24. Executável Go

O resultado final costuma ser um binário único.

Exemplo:

```bash
go build main.go
```

No Windows:

```text
main.exe
```

No Linux:

```text
main
```

Esse executável pode rodar sem instalar uma VM como JVM ou PVM.

Mas ele não é “mágico”: ele contém código do runtime Go junto.

---

## 25. Comparação Go, Java E Python

| Característica | Go | Java | Python |
|---|---|---|---|
| Gera executável nativo | Sim | Normalmente não | Normalmente não |
| Usa VM tradicional | Não | Sim, JVM | Sim, PVM |
| Tem GC | Sim | Sim | Sim |
| Tipagem | Estática | Estática | Dinâmica |
| Concorrência | Goroutines | Threads/virtual threads | Threads/processos/async |
| Startup | Geralmente rápido | Depende da JVM | Geralmente rápido, mas interpretado |
| Performance | Alta | Alta | Menor no CPython puro |

---

## 26. Go Não É Apenas “C Mais Simples”

Go parece simples, mas internamente envolve muita engenharia.

Ele tem:

- compilador moderno;
- análise de tipos;
- SSA;
- runtime próprio;
- garbage collector;
- scheduler;
- suporte nativo a concorrência;
- linker próprio;
- cross-compilation simples.

A simplicidade da linguagem esconde uma infraestrutura sofisticada.

---

## 27. Visualizando Assembly Em Go

Comando:

```bash
go tool compile -S main.go
```

Ou:

```bash
go build -gcflags="-S" main.go
```

Isso permite ver o assembly gerado.

Exemplo didático:

```go
func soma(a int, b int) int {
    return a + b
}
```

Pode virar instruções de máquina como:

```asm
ADDQ BX, AX
RET
```

A sintaxe exata depende da arquitetura e da versão do compilador.

---

## 28. Visualizando Otimizações

Para ver decisões do compilador:

```bash
go build -gcflags="-m" main.go
```

Esse comando mostra informações como:

- função foi inline;
- variável escapou para heap;
- variável ficou na stack;
- alocação foi necessária.

Exemplo:

```text
can inline soma
x escapes to heap
```

Isso é excelente para aula prática.

---

## 29. Visualizando SSA

Go permite inspecionar SSA com:

```bash
GOSSAFUNC=soma go build
```

Isso gera um arquivo HTML com as fases internas da SSA para a função indicada.

Exemplo:

```bash
GOSSAFUNC=soma go build main.go
```

Esse é um ótimo recurso para mostrar aos alunos que o compilador possui várias transformações internas.

---

## 30. Código Real Do Compilador Go

No código-fonte do Go, os alunos podem pesquisar partes como:

```text
cmd/compile
cmd/compile/internal/syntax
cmd/compile/internal/types2
cmd/compile/internal/ir
cmd/compile/internal/ssa
cmd/link
runtime
```

Essas pastas representam partes reais do compilador, linker e runtime.

A frase para aula:

> O compilador Go não é uma caixa mágica. Ele é um programa real, organizado em módulos, que transforma código fonte em binário executável.

---

## 31. Pipeline Completo Do Go

```text
main.go
 ↓
go command
 ↓
scanner
 ↓
parser
 ↓
AST
 ↓
type checker
 ↓
IR
 ↓
SSA
 ↓
otimizações
 ↓
machine code
 ↓
assembler
 ↓
object file
 ↓
linker
 ↓
executável nativo
 ↓
Go runtime
 ↓
sistema operacional
 ↓
CPU
```

Esse é o mapa principal da aula.

---

## 32. Erros Em Go

Erro léxico/sintático:

```go
func main( {
}
```

Erro semântico/tipagem:

```go
var x int
x = "texto"
```

Erro de compilação por variável não usada:

```go
func main() {
    x := 10
}
```

Erro em execução:

```go
var p *Pessoa
fmt.Println(p.Nome)
```

Pode causar panic por acesso inválido, dependendo do caso.

---

## 33. Go E Performance

Go foi pensado para:

- compilação rápida;
- execução eficiente;
- concorrência simples;
- binários fáceis de distribuir;
- produtividade em servidores.

Ele não tenta ser tão baixo nível quanto C, nem tão dinâmico quanto Python.

Go ocupa um meio-termo:

```text
simplicidade + compilação nativa + runtime moderno
```

---

## 34. Go E Sistema Operacional

Um programa Go conversa com o sistema operacional para:

- criar threads;
- abrir arquivos;
- usar rede;
- alocar memória;
- criar timers;
- fazer chamadas de sistema.

O runtime Go fica entre seu programa e o sistema operacional em várias situações.

Fluxo:

```text
Código Go
 ↓
Runtime Go
 ↓
Sistema Operacional
 ↓
Hardware
```

---

## 35. Go É Interpretado?

Não, no uso tradicional.

Go é compilado para código nativo.

Mas existem ambientes específicos, como playgrounds, interpretadores experimentais ou TinyGo/WebAssembly, que podem ter fluxos diferentes.

Para a aula principal:

> Go é uma linguagem compilada estaticamente para executável nativo, com runtime embutido.

---

## 36. Go Vs C

| Característica | C | Go |
|---|---|---|
| Compila para nativo | Sim | Sim |
| GC automático | Não | Sim |
| Ponteiros | Sim | Sim, mas mais restritos |
| Concorrência nativa | Não | Sim, goroutines/channels |
| Runtime embutido | Pequeno ou inexistente | Sim |
| Gerência de memória | Manual | Automática |
| Compilação rápida | Depende | Forte objetivo da linguagem |

---

## 37. Go Vs Python

| Característica | Python | Go |
|---|---|---|
| Execução padrão | Bytecode + PVM | Binário nativo |
| Tipagem | Dinâmica | Estática |
| Performance | Menor no CPython puro | Maior |
| Concorrência CPU-bound | Limitada pelo GIL no CPython | Melhor suporte via goroutines |
| Distribuição | Depende do interpretador | Binário único |
| Sintaxe | Muito flexível | Simples e rígida |

---

## 38. Go Vs Java

| Característica | Java | Go |
|---|---|---|
| Saída principal | Bytecode `.class` | Executável nativo |
| Runtime | JVM externa | Runtime embutido |
| JIT | Sim | Não no modelo tradicional |
| GC | Sim | Sim |
| Concorrência | Threads/virtual threads | Goroutines |
| Portabilidade | JVM | Cross-compilation |

---

## 39. Demonstração Prática Em Sala

Crie `main.go`:

```go
package main

import "fmt"

func soma(a int, b int) int {
    return a + b
}

func main() {
    x := soma(2, 3)
    fmt.Println(x)
}
```

Executar:

```bash
go run main.go
```

Compilar:

```bash
go build main.go
```

Ver otimizações:

```bash
go build -gcflags="-m" main.go
```

Ver assembly:

```bash
go tool compile -S main.go
```

Ver SSA:

```bash
GOSSAFUNC=soma go build
```

---

## 40. Atividade Para Os Alunos

### Parte 1 — Pesquisa

1. O que é SSA e por que ela ajuda o compilador Go?
2. O que é escape analysis em Go?
3. Qual a diferença entre stack e heap em Go?
4. Como o garbage collector do Go funciona de forma geral?
5. O que são goroutines e como diferem de threads?
6. Como o Go consegue fazer cross-compilation?

### Parte 2 — Análise Crítica

Os alunos deverão analisar o pipeline:

```text
.go
 ↓
parser
 ↓
AST
 ↓
type checking
 ↓
IR
 ↓
SSA
 ↓
otimizações
 ↓
código nativo
 ↓
linker
 ↓
executável
```

E responder:

- Quais são os pontos positivos desse modelo?
- Quais são os pontos negativos?
- O que Go ganha ao gerar binário nativo?
- O que Go perde por não usar JIT como Java?
- Como o runtime Go ajuda e ao mesmo tempo adiciona complexidade?

---

## 41. Pontos Positivos Do Pipeline Go

- Gera executável nativo.
- Tem compilação rápida.
- Facilita distribuição.
- Tem tipagem estática.
- Detecta muitos erros antes da execução.
- Tem GC automático.
- Tem concorrência nativa.
- Possui cross-compilation simples.
- Produz binários adequados para servidores e cloud.

---

## 42. Pontos Negativos Do Pipeline Go

- Binários podem ser maiores por causa do runtime.
- GC pode impactar latência.
- Não possui JIT adaptativo como Java.
- Menos controle manual de memória que C.
- Algumas otimizações são menos agressivas que compiladores C/C++ muito maduros.
- O modelo de simplicidade da linguagem limita certos recursos avançados.

---

## 43. Frase Forte Para Fechar A Aula

> Em Go, o programador escreve código simples; o compilador transforma esse código em executável nativo; e o runtime organiza memória, concorrência e execução para que o programa funcione com eficiência.

---

## 44. Resumo Final

```text
Go não é interpretado como Python.
Go não roda em uma VM externa como Java.
Go compila para código nativo como C.
Mas, diferente de C, Go leva junto um runtime com GC, scheduler e suporte a goroutines.
```

A grande ideia:

> Go combina compilação nativa com runtime moderno. Essa mistura explica sua força em servidores, cloud, APIs, ferramentas de linha de comando e sistemas concorrentes.
