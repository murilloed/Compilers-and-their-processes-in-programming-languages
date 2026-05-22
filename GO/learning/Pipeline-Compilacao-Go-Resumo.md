# Pipeline De Compilação Da Linguagem Go

Go é uma linguagem de alto nível, compilada para executável binário, tipada estaticamente e projetada para ser simples, eficiente e produtiva.

Ela possui recursos importantes como:

- concorrência avançada com **goroutines**;
- comunicação entre goroutines com **channels**;
- APIs de rede na biblioteca padrão;
- pouco código repetitivo, ou seja, pouco **boilerplate**;
- biblioteca padrão rica, no estilo **batteries included**;
- testes integrados;
- formatação automática com `gofmt`;
- geração de executáveis nativos.

Go pode ser usada para:

1. Desenvolvimento web, como MVC e APIs REST.
2. Serviços de nuvem e rede.
3. DevOps e confiabilidade de sites, incluindo criação de ferramentas e orquestradores.
4. Interfaces de linha de comando.

---

## Como O Compilador Go Transforma Código Em Executável

Um compilador Go traduz o código-fonte escrito na linguagem Go em código de máquina que o computador consegue executar.

Esse processo envolve várias etapas:

```text
Código Go
 ↓
Análise léxica
 ↓
Análise sintática
 ↓
Análise semântica
 ↓
Representação intermediária
 ↓
SSA e otimizações
 ↓
Geração de código
 ↓
Vinculação
 ↓
Executável
 ↓
Execução
```

---

## 1. Análise Léxica

A primeira etapa é decompor o código-fonte em **tokens**.

Esse processo também é chamado de:

```text
análise léxica
scanner
varredura léxica
```

O compilador identifica elementos como:

- palavras-chave;
- identificadores;
- literais;
- operadores;
- delimitadores;
- símbolos da linguagem.

Exemplo:

```go
idade := 20
```

Pode ser dividido em tokens como:

| Token | Tipo |
|---|---|
| `idade` | identificador |
| `:=` | operador de declaração curta |
| `20` | literal inteiro |

Durante essa etapa, espaços em branco e comentários geralmente são descartados, pois não são necessários para o processamento posterior.

---

## 2. Análise Sintática

Depois da análise léxica, o compilador realiza a **análise sintática**.

Essa etapa verifica se os tokens estão organizados de acordo com a gramática da linguagem Go.

O resultado é uma estrutura como:

```text
árvore de análise sintática
ou
AST — Abstract Syntax Tree
```

A AST representa a estrutura do programa e mostra como os elementos da linguagem estão relacionados.

Exemplo:

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

Essa árvore permite que o compilador entenda a organização lógica do código.

---

## 3. Análise Semântica

Na análise semântica, o compilador verifica se o código faz sentido de acordo com as regras da linguagem.

Ele verifica, por exemplo:

- declarações de variáveis;
- tipos;
- escopos;
- chamadas de função;
- retornos;
- imports;
- uso correto de identificadores;
- compatibilidade entre tipos.

Exemplo de erro semântico:

```go
var idade int
idade = "vinte"
```

O código pode até parecer bem estruturado sintaticamente, mas está semanticamente errado, porque uma `string` está sendo atribuída a uma variável `int`.

---

## 4. Representação Intermediária

Após a análise semântica, o compilador gera uma **representação intermediária**, também chamada de **RI** ou **IR**.

Essa representação é uma forma abstrata do programa, mais adequada para análise e otimização.

Ela fica entre:

```text
Código Go de alto nível
 ↓
Representação intermediária
 ↓
Código de máquina
```

A representação intermediária ajuda o compilador a aplicar otimizações sem depender diretamente do texto original do programa.

Algumas otimizações ou análises associadas a essa fase incluem:

1. Eliminação de código morto.
2. Inserção de chamadas de função em linha, ou **inlining**.
3. Desvirtualização de funções.
4. Análise de escape.

---

## 5. SSA — Static Single Assignment

SSA significa:

```text
Static Single Assignment
```

Em português:

```text
Atribuição Única Estática
```

Nessa forma, cada variável intermediária recebe valor apenas uma vez.

Exemplo conceitual:

```go
x = 10
x = x + 5
```

Pode virar:

```text
x1 = 10
x2 = x1 + 5
```

Isso facilita o trabalho do compilador, pois torna mais claro o fluxo dos dados.

Com SSA, o compilador consegue realizar otimizações como:

- redução de constantes;
- otimização de loops;
- eliminação de código morto;
- melhor uso de registradores;
- análise de dependências entre valores.

---

## 6. Geração De Código

Depois das otimizações, a representação intermediária é traduzida para código de máquina da plataforma de destino.

Essa plataforma pode ser, por exemplo:

```text
amd64
arm64
386
riscv64
wasm
```

O compilador pode gerar código assembly ou produzir diretamente código de máquina, dependendo da arquitetura e da etapa interna do toolchain.

Exemplo simplificado:

```go
func soma(a int, b int) int {
    return a + b
}
```

Pode ser convertido em instruções de baixo nível que somam valores em registradores e retornam o resultado.

---

## 7. Vinculação

A etapa de **vinculação**, ou **linkedição**, é realizada pelo linker.

Se o programa tiver vários arquivos, pacotes ou bibliotecas, o linker combina tudo em um único executável.

Ele resolve:

- referências entre partes do programa;
- chamadas de funções;
- símbolos;
- pacotes importados;
- partes necessárias do runtime Go;
- metadados do executável.

Fluxo simplificado:

```text
Arquivos objeto
Pacotes compilados
Runtime Go
 ↓
Linker
 ↓
Executável final
```

O resultado é um binário executável completo.

---

## 8. Execução

Depois da vinculação, o executável final pode ser executado na máquina de destino.

Ele executa a lógica especificada no código-fonte Go original.

Fluxo:

```text
Executável Go
 ↓
Sistema operacional
 ↓
CPU
```

O programa final não é mais apenas código-fonte. Ele virou um binário que o sistema operacional consegue carregar e executar.

---

## Garbage Collector E Runtime Go

Go utiliza um **garbage collector**, ou coletor de lixo.

Isso significa que a memória é gerenciada automaticamente.

Em linguagens como C, o programador precisa liberar memória manualmente:

```c
malloc()
free()
```

Em Go, o runtime ajuda a gerenciar a memória automaticamente.

O compilador e o ambiente de execução trabalham em conjunto para lidar com a coleta de lixo de forma eficiente.

---

## Concorrência Em Go

Go também oferece suporte nativo à programação concorrente por meio de:

- **goroutines**;
- **channels**.

As goroutines permitem executar tarefas concorrentes de forma leve.

Exemplo:

```go
go tarefa()
```

Os channels permitem comunicação entre goroutines.

O runtime Go fornece recursos como:

- agendamento;
- sincronização;
- gerenciamento de goroutines;
- suporte à execução concorrente.

---

## Resumo Final

O compilador Go transforma código-fonte em executável por meio de várias etapas:

```text
1. Análise léxica
2. Análise sintática
3. Análise semântica
4. Representação intermediária
5. SSA e otimizações
6. Geração de código
7. Vinculação
8. Execução
```

A grande ideia é:

> Go combina compilação nativa, tipagem estática, runtime moderno, garbage collector e suporte nativo à concorrência para gerar programas eficientes e fáceis de distribuir.
