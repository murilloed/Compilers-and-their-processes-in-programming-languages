# Bootstrapping, Linguagem Host E Cross-Compiler Na Compilação Go

## Objetivo Da Aula

Entender como a linguagem Go chegou ao ponto de ter um compilador escrito em Go, o que significa **linguagem host**, o que é **bootstrapping**, como funcionam os **Diagramas em T**, o que é um **compilador self-hosted** e como isso se conecta com **cross-compilation** e **CGO**.

---

## 1. O Problema Inicial: Como Criar Um Compilador?

Quando uma nova linguagem nasce, surge um problema clássico:

> Se a linguagem ainda não tem compilador, como escrever o compilador dela usando ela mesma?

Imagine uma linguagem chamada **X**.

Queremos chegar aqui:

```text
Compilador da Linguagem X escrito em X
```

Mas ainda não existe nenhum compilador capaz de entender código X.

Esse é o dilema do “ovo e da galinha” em compiladores:

```text
Para compilar X, preciso de um compilador de X.
Mas para criar um compilador de X escrito em X, preciso compilar código X.
```

A solução é começar usando uma linguagem que já existe.

---

## 2. O Que É Linguagem Host?

A **linguagem host**, ou linguagem hospedeira, é a linguagem já existente usada para escrever a primeira versão do compilador de uma nova linguagem.

Exemplo:

```text
Nova linguagem: X
Primeiro compilador de X: escrito em C
Linguagem host: C
```

Ou seja:

> A linguagem host é a ponte inicial que permite uma nova linguagem nascer.

Ela é usada porque o computador ainda não entende a nova linguagem diretamente. Então usamos uma linguagem já madura, como:

```text
C
C++
Java
Python
Go
Rust
```

No caso histórico de Go, as primeiras versões do compilador tiveram forte dependência de C.

---

## 3. Visão Geral Do Bootstrapping Em Go

A figura abaixo resume as principais ideias desta aula: linguagem host, bootstrapping, compilador self-hosted, transição do Go de C para Go, cross-compiler e CGO.

<img width="1536" height="1024" alt="go-bootstrapping-visao-geral" src="https://github.com/user-attachments/assets/0b0b8cb0-0e39-4c6d-8e4a-3c42c3214e80" /><img width="1536" height="1024" alt="go-bootstrapping-diagrama-t" src="https://github.com/user-attachments/assets/92dcb458-64f9-4736-bc67-4f54a43a4707" />


Use essa imagem como mapa mental.

Ela mostra que:

- primeiro uma linguagem precisa de ajuda externa;
- depois ela pode reescrever seu próprio compilador;
- quando consegue compilar a si mesma, torna-se self-hosted;
- Go passou por esse caminho;
- Go também facilita compilação cruzada;
- CGO permite integração opcional com C.

---

## 4. O Que É Bootstrapping?

**Bootstrapping**, em compiladores, é o processo de levar uma linguagem até o ponto em que ela consegue compilar o próprio compilador.

A ideia geral é:

```text
1. Escrevo o primeiro compilador em outra linguagem.
2. Reescrevo o compilador na própria linguagem nova.
3. Uso o compilador antigo para compilar o novo.
4. O novo compilador passa a compilar a si mesmo.
```

Quando isso funciona, dizemos que a linguagem atingiu um estágio **self-hosted**, ou autohospedado.

Analogia:

> É como construir uma máquina simples que ajuda a construir uma máquina melhor. Depois, a máquina melhor passa a construir versões futuras dela mesma.

---

## 5. Processo Geral Do Bootstrapping

Imagine novamente a Linguagem X.

### Fase 1 — Compilador Inicial

Criamos um compilador da Linguagem X usando C.

```text
Compilador de X escrito em C
```

Fluxo:

```text
codigo.x
 ↓
compilador_x_escrito_em_C
 ↓
programa executável
```

Aqui, C é a linguagem host.

### Fase 2 — Reescrever O Compilador Em X

Agora escrevemos uma nova versão do compilador:

```text
Compilador de X escrito em X
```

Mas ainda existe um problema: esse compilador está escrito em X, e o computador ainda não compila X sozinho.

### Fase 3 — Usar O Compilador Antigo

Usamos o compilador antigo, feito em C, para compilar o novo compilador escrito em X.

```text
compilador_x_v1 feito em C
 ↓ compila
compilador_x_v2 escrito em X
 ↓ gera
novo compilador executável
```

Agora a linguagem começa a andar com as próprias pernas.

---

## 6. O Que É Um Compilador Self-Hosted?

Um compilador é **self-hosted** quando ele é escrito na própria linguagem que compila.

Exemplos:

```text
Compilador Go escrito em Go
Compilador Rust escrito em Rust
Compilador TypeScript escrito em TypeScript
```

No caso de Go:

> Desde o Go 1.5, o compilador passou a ser escrito majoritariamente em Go.

Isso representa maturidade da linguagem.

Antes:

```text
C ajuda Go a existir.
```

Depois:

```text
Go ajuda Go a evoluir.
```

---

## 7. Diagramas Em T No Bootstrapping

Os **Diagramas em T**, ou **T-diagrams**, ajudam a representar visualmente um compilador.

Eles mostram três elementos:

```text
1. Linguagem fonte: o que o compilador lê.
2. Linguagem alvo: o que o compilador gera.
3. Linguagem host: em qual linguagem o compilador foi escrito.
```

A figura abaixo mostra o bootstrapping usando Diagramas em T.

<img width="1536" height="1024" alt="go-bootstrapping-diagrama-t" src="https://github.com/user-attachments/assets/819997f6-f3ae-4a09-b1a8-d63875ff8505" />

### Como Ler A Figura

No compilador inicial:

```text
Fonte: Linguagem X
Alvo: Código de máquina
Host: C
```

Isso significa:

> O compilador lê X, gera código de máquina, mas foi escrito em C.

Depois do bootstrapping:

```text
Fonte: Linguagem X
Alvo: Código de máquina
Host: Linguagem X
```

Isso significa:

> O compilador lê X, gera código de máquina e foi escrito na própria Linguagem X.

Essa mudança é o coração do bootstrapping.

---

## 8. O Caso Da Linguagem Go

Go é um exemplo muito bom para explicar bootstrapping.

Historicamente, as primeiras versões do compilador Go tinham forte dependência de C.

Isso fazia sentido porque C era:

- maduro;
- rápido;
- disponível em várias plataformas;
- adequado para escrever ferramentas de sistema.

Fluxo inicial simplificado:

```text
Código do compilador Go escrito em C
 ↓
Compilador C
 ↓
Primeiro compilador Go funcional
```

Depois, o compilador foi reescrito em Go.

Fluxo posterior:

```text
Código do compilador Go escrito em Go
 ↓
Compilador Go anterior
 ↓
Novo compilador Go
```

Essa transição mostra o bootstrapping aplicado à linguagem Go.

---

## 9. Por Que Go Fez Esse Processo?

### 1. Autonomia

Go passou a depender menos da linguagem host original.

```text
Antes: C ajuda Go a existir.
Depois: Go ajuda Go a evoluir.
```

### 2. Evolução Mais Fácil

Se o compilador está escrito em Go, os próprios desenvolvedores da linguagem usam Go para melhorar Go.

Isso testa a linguagem em um projeto real, grande e complexo.

### 3. Validação Da Linguagem

Uma linguagem que consegue escrever seu próprio compilador mostra que é poderosa o suficiente para sistemas complexos.

Frase para sala:

> Quando uma linguagem consegue compilar a si mesma, ela mostra que deixou de ser apenas uma ideia e virou uma ferramenta madura.

---

## 10. Cross-Compiler: O Que É?

Um **compilador cruzado**, ou **cross-compiler**, é um compilador que roda em uma plataforma, mas gera código para outra.

Exemplo:

```text
Você compila no Windows x86.
Mas gera um binário para Linux ARM.
```

Ou:

```text
Você programa no notebook.
Mas gera código para um microcontrolador.
```

Conceitos importantes:

| Termo | Significado |
|---|---|
| Build | Máquina onde o compilador foi construído |
| Host | Máquina onde o compilador roda |
| Target | Máquina onde o programa final vai rodar |

Em compilação nativa:

```text
Host = Target
```

Em compilação cruzada:

```text
Host ≠ Target
```

---

## 11. Como O Cross-Compiler Funciona Por Dentro?

Um compilador moderno pode ser dividido assim:

```text
Código fonte
 ↓
Front-end
 ↓
IR
 ↓
Back-end
 ↓
Código de máquina
```

### Front-End

O front-end entende a linguagem.

Ele faz:

```text
análise léxica
análise sintática
AST
tipagem
análise semântica
```

### IR

A IR, ou representação intermediária, é uma forma neutra do programa.

Ela separa o entendimento da linguagem da geração de código para uma máquina específica.

### Back-End

O back-end entende a arquitetura alvo.

Ele gera código para:

```text
x86
ARM
RISC-V
WebAssembly
```

Ideia principal:

> O front-end entende Go. O back-end entende a máquina alvo.

---

## 12. Cross-Compilation Em Go

Go facilita muito a compilação cruzada.

Exemplo para gerar binário Linux em AMD64:

```bash
GOOS=linux GOARCH=amd64 go build
```

Exemplo para gerar executável Windows:

```bash
GOOS=windows GOARCH=amd64 go build
```

Exemplo para gerar binário Linux ARM64:

```bash
GOOS=linux GOARCH=arm64 go build
```

Isso é muito usado em:

```text
Docker
cloud
servidores
IoT
sistemas embarcados
ferramentas de linha de comando
```

Respondam:

> Por que compilar no notebook e executar em outro dispositivo pode ser melhor do que compilar direto no dispositivo final?




Resposta esperada:

> Porque o dispositivo final pode ter pouca memória, pouco processamento ou não ter ferramentas de desenvolvimento instaladas.

---

## 13. O Que É CGO?

**CGO** é o mecanismo que permite código Go chamar código C.

Exemplo conceitual:

```go
import "C"
```

Quando usamos CGO, o programa Go pode depender de:

- bibliotecas C;
- compilador C;
- headers C;
- ferramentas externas;
- bibliotecas dinâmicas do sistema.

Variável importante:

```bash
CGO_ENABLED
```

Valores comuns:

```text
CGO_ENABLED=1 → permite integração com C
CGO_ENABLED=0 → compila sem CGO
```

---

## 14. `CGO_ENABLED=0`

Quando usamos:

```bash
CGO_ENABLED=0 go build
```

estamos dizendo:

> Compile esse programa Go sem depender de integração com C.

Isso é comum em Docker:

```bash
CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o app
```

Esse tipo de build costuma ser usado para criar aplicações Go que rodam em containers minimalistas.

Motivos:

- reduz dependências externas;
- facilita distribuição;
- evita necessidade de bibliotecas C no ambiente final;
- melhora portabilidade.

---

## 15. CGO Não É O Mesmo Que Dependência Do Go Em C

Este ponto é essencial.

Go poder chamar C não significa que Go dependa de C para existir hoje.

São conceitos diferentes:

```text
Go self-hosted:
o compilador Go é escrito em Go.

CGO:
mecanismo opcional para chamar C a partir de Go.
```

Então:

```text
Go compila Go sem depender diretamente de C.
Mas Go pode conversar com C se o programador quiser.
```

---

## 16. Exemplo Didático: Go Puro Versus Go Chamando C

### Situação 1 — Go Puro

```go
package main

import "fmt"

func main() {
    fmt.Println("Olá Go")
}
```

Compilando:

```bash
CGO_ENABLED=0 go build
```

Aqui o programa não precisa chamar C.

### Situação 2 — Go Chamando C

```go
package main

/*
#include <stdio.h>
*/
import "C"

func main() {
    C.puts(C.CString("Olá C a partir do Go"))
}
```

Esse programa precisa de CGO.

Se tentar compilar com:

```bash
CGO_ENABLED=0 go build
```

pode falhar, porque o código usa:

```go
import "C"
```

---

## 17. Por Que Esse Conteúdo Importa Para Compiladores?

Este conteúdo conecta várias ideias centrais da disciplina:

```text
linguagem fonte
linguagem alvo
linguagem host
compilador
runtime
cross-compilation
self-hosting
dependências externas
```

Ele mostra que compiladores não são apenas programas que “traduzem código”.

Eles também estão ligados a:

- história das linguagens;
- arquitetura de computadores;
- sistemas operacionais;
- distribuição de software;
- portabilidade;
- independência tecnológica.

---

## 18. Resumo Geral

```text
Linguagem host:
linguagem usada para criar o primeiro compilador.

Bootstrapping:
processo de fazer uma linguagem compilar seu próprio compilador.

Self-hosted:
quando o compilador é escrito na própria linguagem que ele compila.

Cross-compiler:
compilador que roda em uma plataforma e gera código para outra.

CGO:
mecanismo que permite Go chamar código C.
```

---

## 19. Resumo Aplicado Ao Go

```text
Go começou com dependência de C.
Depois o compilador foi reescrito em Go.
Desde Go 1.5, Go é self-hosted.
Hoje Go consegue compilar Go usando Go.
Go também suporta cross-compilation facilmente.
CGO permite integração opcional com C.
```

---

## 20. Fechamento Da Aula

O bootstrapping mostra o momento em que uma linguagem deixa de depender da linguagem que a criou e passa a sustentar a si mesma.

No caso do Go, isso representa:

- maturidade;
- autonomia;
- portabilidade;
- simplicidade operacional;
- facilidade para compilar em diferentes plataformas.



> O bootstrapping é o ponto em que uma linguagem começa usando uma escada externa, mas depois aprende a construir sua própria escada.

