# VerdeFácil Agro — do problema à arquitetura

Vertical agrícola do [VerdeFácil](https://github.com/sarynnec-spec/verdefacil-showcase).
Responde a uma pergunta que o pequeno agricultor português não consegue responder hoje:
**quanto ganho realmente em cada cultura?**

> ### Estado, com clareza
>
> Este projeto está na fase de **fundação (EPIC 0)**: investigação, conceito, decisão de
> arquitetura e protótipos **concluídos**; funcionalidade agrícola **ainda não
> implementada em código**. A plataforma que o suporta — autorização por capacidades,
> delegação a contabilistas, motor fiscal — já existe e está em produção no VerdeFácil.
>
> Está aqui porque o trabalho de levantar um problema, investigar o mercado, decidir uma
> arquitetura e saber dizer *não* a funcionalidades é metade da engenharia. A outra
> metade, neste caso, ainda está por escrever.

---

## O problema — validado em fontes oficiais, não suposto

O rendimento do agricultor português depende de apoios que ele não compreende, que
chegam tarde e que são tributados sem que ninguém lhe diga quanto lhe fica. A vida
financeira da exploração vive espalhada por Excel, IFAP, Finanças, banco, contabilista e
papel, **sem que nenhuma peça fale com a outra**.

| Facto | Fonte |
|---|---|
| Mais de **27,8%** do rendimento agrícola vem de subsídios | INE, 2024 |
| O IFAP paga **mais de 90%** entre novembro e junho — risco de tesouraria contra amortizações a 31/dez | CAP |
| Os subsídios são **rendimento tributável** (IRS Cat. B, coeficientes 0,10 e 0,30) | CIRS, art. 31.º |
| Compensação forfetária de IVA de **6%** perde-se por desconhecimento ou por falhar o prazo de 31 de março | OCC |
| Agricultor-tipo: **13,7 ha**, **52,5% com mais de 64 anos**, **53% só com formação prática** | INE, Recenseamento Agrícola 2019 |

Mercado: cerca de **290 mil explorações**.

## Porque é que isto ainda não existe

O problema vive entre três silos que não comunicam: o **Estado** (IFAP) só recebe
submissões, o **banco** só faz banca, e o **gabinete PAC** não escala. A *AgTech* olha
para a produção; a *fintech* olha para o credor. Ninguém é dono da vista *"quanto me
fica no bolso"*.

Os softwares agrícolas existentes (ISAGRI, CentralGest, Agroop) gerem **produção**; a
contabilidade genérica gere **documentos**. Falta a **camada de clareza financeira por
cultura, integrada com a fiscalidade** — e construí-la exige motor fiscal português
**e** domínio das regras do IFAP ao mesmo tempo.

---

## As decisões de desenho, e o que as obrigou

**O utilizador tem mais de 60 anos e baixa literacia digital.** Isto não é uma nota de
rodapé — decide a interface:

- **registo por voz**, porque o agricultor fala, não preenche formulários
- **missões** em vez de menus, com a ordem das tarefas decidida pelo sistema
- o resultado é sempre uma **estimativa clara**, nunca um número fiscal afirmado como
  certo — a exatidão do IRS depende do agregado familiar e isso confirma-se com um
  contabilista

**O critério para recusar funcionalidades.** Perante qualquer ideia nova:
*"isto aproxima ou afasta um agricultor de ver quanto ganhou por cultura?"* Se afasta,
vai para backlog. Está escrito, e é o que impede o produto de se transformar num ERP
agrícola — que é precisamente o que **não** é.

---

## A arquitetura — decidida antes da primeira linha de código

A decisão central está registada numa ADR:

> **O Agro não é um segundo produto técnico. É um segundo vertical do mesmo ecossistema.**

A alternativa — um segundo produto, com a sua base de dados e o seu motor fiscal —
duplicaria a parte mais cara e mais arriscada do sistema: a conformidade fiscal
portuguesa. Cada correção legal passaria a ter de ser feita duas vezes, e as duas
divergiriam.

O modelo separa quatro perguntas, e cada linha de código responde a **uma**:

| Camada | Pergunta |
|---|---|
| **Vertical** | como é apresentada a aplicação? |
| **Módulo** | que funcionalidades tem esta organização? |
| **Capability** | que operações pode este utilizador executar? |
| **Core Fiscal** | como cumprir a legislação fiscal portuguesa? |

Com o teste prático: *"se não sei a que pergunta esta linha responde, a arquitetura está
a degradar-se."*

Esta ADR é vinculativa — as decisões seguintes (ligação entre organizações, frota,
ecossistema, empresas) referenciam-na e declaram como a cumprem.

**O diferencial assenta em infraestrutura que já existe.** O ecossistema com
consentimento — o agricultor autoriza, e o banco, a cooperativa ou o contabilista veem
**apenas o que foi autorizado** — corre sobre o sistema de capacidades e delegação já
construído e em produção no VerdeFácil. É isso que o torna difícil de copiar, e é
também o que mantém o custo marginal do vertical baixo.

---

## O que está feito

| | |
|---|---|
| **Investigação** | mercado, concorrência, dores e oportunidades, com fontes oficiais |
| **Conceito** | proposta de valor, utilizador, limites do produto, critério de recusa |
| **Arquitetura** | ADR multi-vertical aprovada, modelo de dados, adaptação do CBAC, integração com o portal do contabilista |
| **Roadmap** | épicos definidos, com o que fica explicitamente fora de âmbito |
| **Protótipos** | deck de apresentação, protótipo do ecossistema e microdemo de registo por voz, todos em HTML |
| **Base técnica** | capacidades, delegação e motor fiscal — **já em produção** no VerdeFácil |
| **Funcionalidade agrícola** | **por implementar** — parcelas, culturas, custos por cultura, apoios IFAP |

---

## Documentação

Os documentos de fundação vivem no repositório privado do VerdeFácil:

```
docs/agro/AGRO_VISION.md           problema, utilizador, norte do MVP, limites
docs/agro/AGRO_ARCHITECTURE.md     modelo de dados, partilhado vs exclusivo,
                                   CBAC em organizações agrícolas, delegação
docs/agro/AGRO_ROADMAP.md          épicos e fora-de-âmbito
docs/agro/EPIC-01.md               o ciclo completo do MVP
docs/adr/ADR-010-multi-vertical.md a decisão de arquitetura e os seus princípios
```

Dez ADRs registam as decisões da plataforma — capacidades, cadeia de auditoria,
integridade do domínio fiscal, núcleo de acesso, e esta.

---

## Autoria

**Sarynne Coelho Ferreira** — conceito, investigação e arquitetura.
[LinkedIn](https://www.linkedin.com/in/sarynne-coelho-ferreira) ·
[VerdeFácil](https://github.com/sarynnec-spec/verdefacil-showcase)
