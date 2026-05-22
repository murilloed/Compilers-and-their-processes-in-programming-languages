# Atividades — Aula De Compiladores Go / Golang

## Organização Da Aula

Tempo total: **1h40min**, ou **100 minutos**.

Modelo sugerido:

| Tempo | Momento |
|---|---|
| 0–20 min | Explicação 1: pipeline Go e comparação com C, Java e Python |
| 20–30 min | Atividade 1: monte o pipeline |
| 30–50 min | Explicação 2: lexer, parser, AST e tipagem |
| 50–60 min | Atividade 2: caça ao erro do compilador |
| 60–80 min | Explicação 3: SSA, otimização, stack, heap e escape analysis |
| 80–90 min | Atividade 3: stack ou heap? |
| 90–100 min | Atividade 4: verdadeiro ou falso |

---

## Atividade 1 — Monte O Pipeline

### Objetivo

Organizar corretamente as etapas do processo de compilação da linguagem Go.

### Instrução

Os itens abaixo estão embaralhados. Reorganize-os na ordem mais adequada do pipeline de compilação Go.

```text
Executável nativo
Parser
Código .go
SSA
Lexer
Linker
AST
Type checking
Código de máquina
Runtime Go
```

### Espaço Para Resposta

```text
1.
2.
3.
4.
5.
6.
7.
8.
9.
10.
```

### Pergunta Extra

Em qual etapa o compilador descobre que `"texto"` não pode ser atribuído a uma variável do tipo `int`?

Resposta:

```text

```

---

## Atividade 2 — Caça Ao Erro Do Compilador

### Objetivo

Identificar em qual fase o erro aparece: análise léxica, análise sintática, análise semântica/tipagem ou execução.

### Instrução

Classifique cada trecho de código usando uma das categorias:

```text
Léxico
Sintático
Semântico / Tipagem
Runtime
```

### Item 1

```go
func main( {
}
```

Classificação:

```text

```

Justificativa:

```text

```

### Item 2

```go
var x int
x = "abc"
```

Classificação:

```text

```

Justificativa:

```text

```

### Item 3

```go
func main() {
    x := 10
}
```

Classificação:

```text

```

Justificativa:

```text

```

### Item 4

```go
fmt.Println("Olá"
```

Classificação:

```text

```

Justificativa:

```text

```

### Item 5

```go
var p *Pessoa
fmt.Println(p.Nome)
```

Classificação:

```text

```

Justificativa:

```text

```

### Pergunta Final

Por que Go costuma encontrar mais erros antes da execução do que Python?

Resposta:

```text

```

---

## Atividade 3 — Stack Ou Heap?

### Objetivo

Raciocinar sobre escape analysis e entender quando uma variável pode ficar na stack ou precisa ir para o heap.

### Instrução

Para cada trecho de código, marque se a variável destacada provavelmente fica na **stack** ou escapa para o **heap**.

---

### Código A

```go
func soma() int {
    x := 10
    return x + 5
}
```

Variável analisada: `x`

Marque:

```text
( ) Stack
( ) Heap
```

Justificativa:

```text

```

---

### Código B

```go
func cria() *int {
    x := 10
    return &x
}
```

Variável analisada: `x`

Marque:

```text
( ) Stack
( ) Heap
```

Justificativa:

```text

```

---

### Código C

```go
func imprime() {
    nome := "Ana"
    println(nome)
}
```

Variável analisada: `nome`

Marque:

```text
( ) Stack
( ) Heap
```

Justificativa:

```text

```

---

### Código D

```go
type Pessoa struct {
    Nome string
}

func criaPessoa() *Pessoa {
    p := Pessoa{Nome: "Ana"}
    return &p
}
```

Variável analisada: `p`

Marque:

```text
( ) Stack
( ) Heap
```

Justificativa:

```text

```

---

## Atividade 4 — Verdadeiro Ou Falso

### Objetivo

Revisar os principais conceitos da aula.

### Instrução

Marque **V** para verdadeiro ou **F** para falso. Depois, justifique cada resposta em uma frase.

| Item | Afirmação | V/F | Justificativa |
|---|---|---|---|
| 1 | Go normalmente compila para bytecode. |  |  |
| 2 | Go gera executável nativo. |  |  |
| 3 | O runtime Go cuida do garbage collector. |  |  |
| 4 | Goroutines são sempre iguais a threads do sistema operacional. |  |  |
| 5 | SSA ajuda o compilador a otimizar o código. |  |  |
| 6 | O linker junta pacotes, runtime e código compilado. |  |  |
| 7 | Go é dinamicamente tipado como Python. |  |  |
| 8 | Escape analysis ajuda a decidir stack ou heap. |  |  |

---

## Questão Reflexiva Final

Responda em um parágrafo:

> Qual é a principal vantagem de Go gerar um executável nativo, mas ainda carregar um runtime próprio?

Resposta:

```text

```
