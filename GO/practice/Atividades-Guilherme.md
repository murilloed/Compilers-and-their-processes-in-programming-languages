# Atividades — Aula De Compiladores Go / Golang

## Organização Da Aula

Tempo total: **1h40min**, ou **100 minutos**.

Timetable:

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
1. Código .go
2. Lexer
3. Parser
4. Type checking
5. AST
6. SSA
7. Código de máquina
8. Linker
9. Executável nativo
10. Runtime Go
```

### Pergunta Extra

Em qual etapa o compilador descobre que `"texto"` não pode ser atribuído a uma variável do tipo `int`?

Resposta:

```text
Type checking -> Análise semântica.
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
Sintático
```

Justificativa:

```text
Falta o fechamento dos parenteses da função main, isso faz com que gere um erro na gramática da linguagem.
```

### Item 2

```go
var x int
x = "abc"
```

Classificação:

```text
Semântico / Tipagem
```

Justificativa:

```text
Pois a variável X que antes foi declarada como um inteiro não suporta a atribuição de um tipo String.
```

### Item 3

```go
func main() {
    x := 10
}
```

Classificação:

```text
Semântica / Tipagem
```

Justificativa:

```text
Em GO, toda variavel localmente atribuida deve ser necessáriamente utilizada.
```

### Item 4

```go
fmt.Println("Olá"
```

Classificação:

```text
Sintático
```

Justificativa:

```text
Falta o fechamento dos parenteses.
```

### Item 5

```go
var p *Pessoa
fmt.Println(p.Nome)
```

Classificação:

```text
Runtime
```

Justificativa:

```text
Pois ele tenta desreferenciar um ponteiro nulo, pois a variavel nao foi inicializada.
```

### Pergunta Final

Por que Go costuma encontrar mais erros antes da execução do que Python?

Resposta:

```text
Pois o python se trata de uma linguagem interpretada enquanto GO é uma linguagem compilada.
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
(X) Stack
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
(X) Heap
```

Justificativa:

```text
Pois a função retorna o apontamento para a variavel X, se fosse pra Stack assim que a função cria() terminasse a variavel seria apagada e retornaria nulo ou vazio
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
(X) Stack
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
(X) Heap
```

Justificativa:

```text
Ponteiro aponta para a variavel P
```

---

## Atividade 4 — Verdadeiro Ou Falso

### Objetivo

Revisar os principais conceitos da aula.

### Instrução

Marque **V** para verdadeiro ou **F** para falso. Depois, justifique cada resposta em uma frase.

| Item | Afirmação | V/F | Justificativa |
|---|---|---|---|
| 1 | Go normalmente compila para bytecode. | F |  |
| 2 | Go gera executável nativo. | V |  |
| 3 | O runtime Go cuida do garbage collector. | V |  |
| 4 | Goroutines são sempre iguais a threads do sistema operacional. | F |  |
| 5 | SSA ajuda o compilador a otimizar o código. | V |  |
| 6 | O linker junta pacotes, runtime e código compilado. | V |  |
| 7 | Go é dinamicamente tipado como Python. | F |  |
| 8 | Escape analysis ajuda a decidir stack ou heap. | V |  |

---

## Questão Reflexiva Final

Responda em um parágrafo:

> Qual é a principal vantagem de Go gerar um executável nativo, mas ainda carregar um runtime próprio?

Resposta:

```text
Go consegue unir o máximo de desempenho e portabilidade, tendo seus recursos sendo gerenciados pelo runtime embutido e com o gerenciamento de memória automática ( Garbage colllector ). Gerando no final um executável extremamente rápido e de fácil distribuição.
```
