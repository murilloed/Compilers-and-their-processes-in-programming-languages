# Aula: Compiladores e COBOL

Tema: Do programa COBOL ao executavel: como uma linguagem legada ainda sustenta sistemas criticos

Duracao: 50 minutos

Publico-alvo: alunos de Compiladores, Arquitetura, Sistemas Legados ou Engenharia de Software

## 1. O que e COBOL?

COBOL significa Common Business-Oriented Language. E uma linguagem criada para aplicacoes de negocio, especialmente sistemas bancarios, folha de pagamento, governo, seguros, contabilidade, processamento em lote e transacoes corporativas.

Diferente de linguagens voltadas para calculo cientifico ou sistemas operacionais, COBOL foi pensado para representar regras de negocio de forma parecida com texto em ingles. Por isso seus programas costumam ter uma estrutura bem declarativa, com secoes como IDENTIFICATION DIVISION, ENVIRONMENT DIVISION, DATA DIVISION e PROCEDURE DIVISION.

Um exemplo simples:

```cobol
IDENTIFICATION DIVISION.
PROGRAM-ID. HELLO.

PROCEDURE DIVISION.
    DISPLAY "OLA, COBOL".
    STOP RUN.
```

A ideia mais importante para os alunos: COBOL nao e apenas uma linguagem antiga. Ele e parte de um ecossistema de execucao corporativa que envolve compiladores, arquivos, bancos de dados, mainframes, jobs, transacoes e regras de negocio acumuladas por decadas.

## 2. Quais modelos de COBOL existem hoje?

Hoje, COBOL aparece em varios modelos de uso:

| Modelo | Onde aparece | Caracteristica |
|---|---|---|
| COBOL em mainframe | IBM z/OS, CICS, IMS, Db2, JCL | Muito usado em bancos, governo e grandes empresas |
| COBOL em ambiente distribuido | Linux, Windows, Unix | Pode ser compilado fora do mainframe, geralmente com Micro Focus/OpenText, GnuCOBOL, Fujitsu ou Veryant |
| COBOL orientado a objetos | Alguns compiladores comerciais | Adiciona classes, objetos e integracao com Java/.NET em alguns ambientes |
| COBOL interoperavel | Integra com C, Java, .NET, bancos e servicos | Usado em modernizacao gradual |
| COBOL migrado/modernizado | Traducao para Java, C#, cloud ou APIs | Usado quando empresas querem reduzir dependencia de legado |

COBOL moderno tambem pode trabalhar com XML, JSON, APIs, bancos relacionais e ferramentas DevOps, dependendo do compilador e do ambiente.

## 3. Compiladores COBOL atuais

Alguns compiladores relevantes hoje:

| Compilador | Tipo | Observacao |
|---|---|---|
| IBM Enterprise COBOL for z/OS | Comercial/mainframe | Forte em ambientes IBM Z, CICS, IMS e Db2 |
| GnuCOBOL | Livre/open source | Traduz COBOL para C e usa um compilador C nativo |
| OpenText Visual COBOL / Micro Focus COBOL | Comercial | Muito usado para modernizar e executar COBOL fora do mainframe |
| Veryant isCOBOL | Comercial | Gera codigo orientado a objeto que roda sobre Java Runtime Environment |
| Fujitsu NetCOBOL | Comercial | Compilador COBOL para ambientes corporativos |

Para aula e laboratorio, o mais acessivel costuma ser o GnuCOBOL, porque e gratuito e pode ser usado em Linux, Windows, macOS e simuladores online.

## 4. Pontos fortes da linguagem COBOL

- Sintaxe muito legivel para regras de negocio.
- Excelente para processamento de registros, arquivos e dados financeiros.
- Alta estabilidade em sistemas criticos.
- Decadas de codigo testado em producao.
- Boa compatibilidade com ambientes corporativos tradicionais.
- Forte presenca em bancos, governo, seguradoras e grandes operacoes.
- Separacao clara entre dados e procedimentos.

## 5. Pontos fracos da linguagem COBOL

- Sintaxe verbosa.
- Menor quantidade de profissionais novos.
- Ecossistema fragmentado por dialetos e fornecedores.
- Dificuldade de integracao com praticas modernas quando o sistema e muito antigo.
- Dependencia de mainframe, JCL, CICS, IMS, VSAM ou copybooks em muitos ambientes.
- Modernizacao pode ser cara e arriscada.
- Codigo legado pode conter regras de negocio implicitas e pouco documentadas.

## 6. Pontos fortes da compilacao COBOL

- Gera sistemas estaveis e eficientes para processamento corporativo.
- Em mainframe, integra bem com runtime, arquivos, transacoes e banco de dados.
- Em GnuCOBOL, pode gerar executaveis nativos por meio de C.
- Suporta modelos tradicionais de compilacao: fonte, objeto, link-edicao e executavel/load module.
- Permite reaproveitamento de copybooks para padronizar estruturas de dados.

## 7. Pontos fracos da compilacao COBOL

- O processo pode depender muito do ambiente.
- Em mainframe, compilar pode envolver JCL, bibliotecas, datasets e procedimentos catalogados.
- Dialetos diferentes podem gerar incompatibilidades.
- Programas com CICS, Db2, IMS ou arquivos VSAM podem exigir pre-processamento ou configuracoes especificas.
- Mensagens de erro podem ser mais dificeis para iniciantes.
- O build muitas vezes nao e apenas "compilar um arquivo"; ele envolve dependencias historicas do sistema.

## 8. Como esta estruturada a compilacao COBOL?

Pipeline conceitual:

```text
Programa .cob / .cbl
   ↓
COPY / copybooks
   ↓
Pre-processamento
   ↓
Compilador COBOL
   ↓
Codigo objeto
   ↓
Link-editor / linker
   ↓
Executavel / load module
   ↓
Runtime COBOL
   ↓
Arquivos, banco, transacoes, relatorios, mainframe ou sistema operacional
```

Em um ambiente mainframe, a compilacao pode ser feita com JCL, TSO, CLISTs ou paineis ISPF. Em um ambiente com GnuCOBOL, o fluxo fica mais simples:

```text
hello.cob
   ↓
cobc
   ↓
C intermediario
   ↓
gcc ou outro compilador C
   ↓
executavel nativo
```

## 9. Como compilar COBOL na pratica com GnuCOBOL?

Arquivo `hello.cob`:

```cobol
IDENTIFICATION DIVISION.
PROGRAM-ID. HELLO.

PROCEDURE DIVISION.
    DISPLAY "OLA, COBOL".
    STOP RUN.
```

Compilar e gerar executavel:

```bash
cobc -x hello.cob
```

Executar:

```bash
./hello
```

Gerar apenas o C intermediario:

```bash
cobc -C hello.cob
```

Isso e excelente para aula de compiladores, porque os alunos conseguem enxergar que uma linguagem pode ser compilada indiretamente: COBOL vira C, e depois C vira codigo nativo.

## 10. O compilador COBOL e escrito em COBOL ou C?

Depende do compilador.

No caso do GnuCOBOL, o compilador `cobc` e implementado principalmente em C e traduz programas COBOL para C. Depois, esse C intermediario e compilado por um compilador C nativo, normalmente GCC.

Compiladores comerciais, como IBM Enterprise COBOL, OpenText/Micro Focus, Fujitsu e Veryant, possuem implementacoes proprietarias. Nao e correto afirmar genericamente que "todo compilador COBOL e escrito em COBOL" ou "todo compilador COBOL e escrito em C". O ponto didatico mais seguro e:

- GnuCOBOL: compilador aberto, implementado em C, gera C intermediario.
- IBM Enterprise COBOL: compilador proprietario para z/OS.
- Veryant isCOBOL: gera codigo orientado a objeto para rodar sobre JRE.
- Outros fornecedores podem usar arquiteturas internas diferentes.

## 11. GitHub e codigo-fonte de compilador COBOL

O principal compilador open source para aula e experimentacao e o GnuCOBOL.

Links uteis:

- Projeto oficial GnuCOBOL: https://gnucobol.sourceforge.io/
- Download no SourceForge: https://sourceforge.net/projects/gnucobol/
- Repositorio SourceForge: https://sourceforge.net/p/gnucobol/code/
- Mirror no GitHub mantido pela OCamlPro: https://github.com/OCamlPro/gnucobol
- Documentacao tecnica do codigo-fonte: https://gnucobol.sourceforge.io/doxygen/gnucobol-2/

Importante para os alunos: o GitHub acima e um clone/mirror util para navegacao e estudo, mas a origem historica/oficial do projeto fica no SourceForge.

## 12. Onde os alunos podem simular COBOL online?

Opcoes praticas para sala:

| Plataforma | Link | Uso em aula |
|---|---|---|
| OnlineGDB | https://www.onlinegdb.com/online_cobol_compiler | Escrever, compilar e executar COBOL no navegador |
| JDoodle COBOL | https://www.jdoodle.com/execute-cobol-online | IDE online simples para testes rapidos |

Sugestao de atividade online:

1. Abrir um compilador COBOL online.
2. Copiar o programa `HELLO`.
3. Executar.
4. Alterar a mensagem.
5. Criar duas variaveis numericas.
6. Somar e exibir o resultado.
7. Anotar quais erros de compilacao apareceram quando erraram a sintaxe.

## 13. Aula de 50 minutos

Tema da aula: Do COBOL ao executavel: como um programa legado vira processamento real

Objetivo da aula: fazer o aluno entender como um programa COBOL e compilado, ligado a bibliotecas, executado em ambientes corporativos/mainframe e por que ainda e tao importante.

## 14. Roteiro da aula

| Tempo | Momento | O que acontece |
|---:|---|---|
| 0-5 min | Provocacao | Pergunta: por que uma linguagem de 1959 ainda processa dinheiro, folha de pagamento e sistemas bancarios? |
| 5-10 min | Mini explicacao | Mostrar o fluxo `.cob -> copybooks -> compilador -> objeto -> linker -> executavel/load module -> runtime` |
| 10-20 min | Atividade 1: Pipeline Humano | Grupos representam etapas da compilacao COBOL |
| 20-32 min | Atividade 2: Pesquisa Relampago | Alunos pesquisam COBOL moderno, mainframe, copybook, JCL, VSAM, GnuCOBOL e migracao |
| 32-42 min | Atividade 3: Producao | Cada grupo cria mapa visual, tabela ou analogia |
| 42-48 min | Debate | Vale modernizar COBOL ou manter sistemas legados funcionando? |
| 48-50 min | Fechamento | Exit ticket individual |

## 15. Atividade 1: Pipeline Humano

Divida a turma em grupos:

1. Codigo-fonte COBOL
2. COPY / copybook
3. Pre-processamento
4. Compilador COBOL
5. Codigo objeto
6. Linker / link-editor
7. Runtime / mainframe

Missao de cada grupo:

> Voce e uma etapa do pipeline COBOL. Explique o que recebe, o que transforma e o que entrega para a proxima etapa.

Exemplo de resposta:

> COPY/copybook: recebo trechos reutilizaveis de definicao de dados e insiro no programa antes da compilacao.

Produto final: cada grupo escreve uma frase no quadro. Ao final, a turma monta o pipeline completo.

## 16. Atividade 2: Pesquisa Relampago

Cada grupo pesquisa uma pergunta:

1. O que e um copybook em COBOL?
2. O que e JCL e qual sua relacao com COBOL em mainframes?
3. O que e VSAM e por que aparece tanto em sistemas COBOL?
4. COBOL ainda e usado em bancos? Por que?
5. Qual a diferenca entre compilar COBOL em mainframe e em ambiente distribuido?
6. Quais sao os desafios de migrar COBOL para Java, C# ou cloud?
7. Como o GnuCOBOL transforma COBOL em executavel?
8. O que muda quando COBOL se integra com C, Java ou banco de dados?

Entrega:

- explicacao simples;
- um exemplo real;
- uma vantagem;
- um problema ou risco;
- uma fonte consultada.

## 17. Atividade 3: Producao

Cada grupo escolhe um produto:

- mapa visual do pipeline COBOL;
- tabela "COBOL legado vs linguagem moderna";
- analogia: "COBOL como cartorio bancario";
- checklist de riscos de migracao;
- mini apresentacao: "por que COBOL ainda nao morreu?";
- comparacao entre GnuCOBOL e COBOL de mainframe;
- diagrama mostrando COBOL, copybook, JCL, arquivos e banco.

Entrega oral: 1 minuto por grupo.

## 18. Debate instigador

Pergunta central:

> Se COBOL funciona ha decadas, trocar e evolucao ou risco?

Divida a sala:

- Lado A: defender modernizacao.
- Lado B: defender manutencao do legado.

Argumentos que devem aparecer:

- custo;
- risco operacional;
- falta de profissionais;
- seguranca;
- desempenho;
- dependencia tecnologica;
- continuidade do negocio;
- risco de perder regras de negocio historicas.

## 19. Mini laboratorio opcional

Se houver internet, usar OnlineGDB ou JDoodle.

Codigo base:

```cobol
IDENTIFICATION DIVISION.
PROGRAM-ID. SOMA.

DATA DIVISION.
WORKING-STORAGE SECTION.
01 A PIC 9(2) VALUE 10.
01 B PIC 9(2) VALUE 20.
01 R PIC 9(3).

PROCEDURE DIVISION.
    ADD A TO B GIVING R.
    DISPLAY "RESULTADO = " R.
    STOP RUN.
```

Desafios:

1. Trocar os valores de A e B.
2. Criar uma subtracao.
3. Fazer uma multiplicacao.
4. Provocar um erro de sintaxe e interpretar a mensagem do compilador.
5. Explicar o que significa `PIC 9(2)`.

## 20. Fechamento

Frase para encerrar:

> COBOL mostra que compiladores nao sao apenas teoria: eles sustentam sistemas reais, criticos e bilionarios.

Exit ticket:

> Em uma frase, explique como um programa COBOL sai do codigo-fonte e chega a execucao real.

Resposta esperada:

> Um programa COBOL passa por copybooks/pre-processamento, compilacao, geracao de codigo objeto, link-edicao e execucao em um runtime ou ambiente mainframe.

## 21. Avaliacao rapida

Use 3 criterios:

| Criterio | Excelente | Parcial | Precisa melhorar |
|---|---|---|---|
| Entendeu o pipeline | Explica todas as etapas | Explica algumas etapas | Confunde compilacao e execucao |
| Pesquisou com qualidade | Usa fonte e exemplo | Usa apenas definicao | Nao apresenta fonte |
| Produziu algo claro | Diagrama/tabela bem explicado | Produto incompleto | Produto confuso |

## 22. Referencias consultadas

- GnuCOBOL, projeto oficial: https://gnucobol.sourceforge.io/
- GnuCOBOL no SourceForge: https://sourceforge.net/projects/gnucobol/
- Manual GnuCOBOL: https://gnucobol.sourceforge.io/doc/gnucobol.html
- FAQ GnuCOBOL: https://gnucobol.sourceforge.io/faq/gcfaq.html
- Mirror GitHub OCamlPro/GnuCOBOL: https://github.com/OCamlPro/gnucobol
- IBM Enterprise COBOL for z/OS: https://www.ibm.com/products/cobol-compiler-zos
- IBM Docs, compilando COBOL no z/OS: https://www.ibm.com/docs/en/cobol-zos/6.4?topic=program-compiling-under-zos
- IBM COBOL Compiler Family: https://www.ibm.com/products/cobol-compiler-family
- Veryant isCOBOL Compiler: https://www.veryant.com/products/iscobol/iscobolcompiler.html
- Fujitsu NetCOBOL: https://www.fujitsu.com/global/products/software/developer-tool/netcobol/
- OnlineGDB COBOL: https://www.onlinegdb.com/online_cobol_compiler
- JDoodle COBOL: https://www.jdoodle.com/execute-cobol-online
