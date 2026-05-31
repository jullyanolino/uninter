<div align="center">

# Bacharelado em Matemática

**Centro Universitário Internacional – UNINTER**

[![UNINTER](https://img.shields.io/badge/UNINTER-Bacharelado%20em%20Matem%C3%A1tica-1a3a6b?style=flat-square&logo=graduation-cap&logoColor=white)](https://www.uninter.com)
[![Modalidade](https://img.shields.io/badge/Modalidade-EAD-0077b6?style=flat-square)](https://www.uninter.com)
[![Status](https://img.shields.io/badge/Status-Em%20andamento-27ae60?style=flat-square)](.)
[![Localização](https://img.shields.io/badge/Local-Bras%C3%ADlia%20%E2%80%93%20DF-c0392b?style=flat-square)](.)

---

*Repositório de produção acadêmica: relatórios, notebooks, códigos e materiais  
elaborados ao longo do curso de Bacharelado em Matemática.*

</div>

---

## Sobre Este Repositório

Este repositório reúne os trabalhos, relatórios e implementações computacionais produzidos durante o **Bacharelado em Matemática** no Centro Universitário Internacional UNINTER, com sede em Brasília – DF.

O objetivo é manter um registro organizado e reproduzível da produção acadêmica ao longo do curso, abrangendo desde delineamentos de pesquisa e relatórios finais até notebooks Jupyter com análises estatísticas, algoritmos matemáticos e visualizações de dados.

Todos os trabalhos aplicam fenômenos observáveis na realidade locorregional do **Distrito Federal** sempre que pertinente, em consonância com as diretrizes metodológicas da instituição.

---

## Estrutura do Repositório

```
.
├── disciplinas/
│   ├── estatistica-multivariada/
│   ├── calculo-diferencial-integral/
│   ├── algebra-linear/
│   ├── probabilidade-e-estatistica/
│   ├── teoria-dos-jogos/
│   └── ...
├── projetos-integradores/
├── recursos/
│   ├── templates/
│   └── referencias/
└── README.md
```

Cada diretório de disciplina segue a convenção:

```
disciplinas/<nome-da-disciplina>/
├── README.md               # Descrição da disciplina e lista de trabalhos
├── report/              # Documento final (.docx, .pdf ou .tex)
├── notebook/               # Análises em Jupyter Notebook (.ipynb)
├── code/                 # Scripts auxiliares (.py, .r, etc.)
└── results/                   # Figuras e visualizações geradas
```

---

## Disciplinas e Trabalhos

### Concluídos

| Disciplina | Tema do Trabalho | Técnicas / Conteúdo | Artefatos |
|-----------|-----------------|---------------------|-----------|
| [Teoria dos Jogos](./disciplinas/teoria-dos-jogos/) | Equilíbrio de Nash e Ótimo de Pareto aplicados ao transporte urbano no DF | Teoria dos jogos, matriz de payoffs, correspondências de melhor resposta, álgebra linear | Artigo (LaTeX · ABNT) |
| [Estatística Multivariada](./disciplinas/estatistica-multivariada/) | Análise multivariada de sinistros de trânsito com vítimas fatais no DF (2022–2024) | ACP, Cluster (Ward), Análise de Correspondência, Python | Notebook · Relatório |

### Em Andamento

| Disciplina | Status |
|-----------|--------|
| — | — |

> A tabela é atualizada a cada novo trabalho entregue.

---

## Destaques Técnicos

### Estatística Multivariada — Sinistros de Trânsito no DF

O trabalho mais recente aplica três técnicas multivariadas a dados oficiais do Detran-DF / GEREST sobre **759 vítimas fatais** no triênio 2022–2024:

- **ACP** — 3 componentes principais retidas pelo critério de Kaiser, explicando **78,93%** da variância total entre as 33 Regiões Administrativas
- **Cluster Analysis** (Ward) — k = 3 clusters com perfis homogêneos de sinistralidade: rodoviário, urbano de alto volume e urbano difuso
- **Análise de Correspondência** — associações estatisticamente significativas entre tipo de vítima, tipo de via e fator de risco (χ² < 0,001)

→ [`disciplinas/estatistica-multivariada/`](./disciplinas/estatistica-multivariada/)

### Teoria dos Jogos — Transporte Urbano no DF

Artigo científico em LaTeX modelando a escolha modal (carro particular vs. transporte público) no DF como um jogo não-cooperativo de dois jogadores, com demonstração formal do Equilíbrio de Nash e análise da ineficiência de Pareto do equilíbrio resultante — o dilema do prisioneiro do trânsito urbano.

→ [`disciplinas/teoria-dos-jogos/`](./disciplinas/teoria-dos-jogos/)

---

## Stack Tecnológica

As ferramentas utilizadas ao longo do curso variam conforme a natureza de cada disciplina:

**Computação científica e análise de dados**

![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=flat-square&logo=numpy&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=flat-square&logo=pandas&logoColor=white)
![SciPy](https://img.shields.io/badge/SciPy-8CAAE6?style=flat-square&logo=scipy&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?style=flat-square&logo=scikit-learn&logoColor=white)
![Matplotlib](https://img.shields.io/badge/Matplotlib-11557c?style=flat-square)
![Seaborn](https://img.shields.io/badge/Seaborn-4c72b0?style=flat-square)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=flat-square&logo=jupyter&logoColor=white)

**Redação acadêmica e tipografia**

![LaTeX](https://img.shields.io/badge/LaTeX-008080?style=flat-square&logo=latex&logoColor=white)
![Markdown](https://img.shields.io/badge/Markdown-000000?style=flat-square&logo=markdown&logoColor=white)

**Normas e padrões**

![ABNT](https://img.shields.io/badge/ABNT-NBR%206022%3A2018-003366?style=flat-square)

---

## Padrões Adotados

### Formatação Acadêmica

Todos os relatórios seguem as normas **ABNT NBR 6022:2018** (artigos científicos) ou **NBR 14724:2011** (trabalhos acadêmicos), conforme especificado em cada disciplina. Documentos LaTeX utilizam a classe `abntex2`; documentos Word seguem os estilos institucionais da UNINTER.

### Reprodutibilidade

Cada notebook inclui:

- Semente aleatória fixada (`SEED = 42`) para garantir reprodutibilidade total
- Dados primários com origem declarada e método de construção documentado
- Saídas geradas automaticamente na execução completa do notebook
- Requisitos de ambiente listados em `requirements.txt` quando aplicável

### Convenção de Commits

```
tipo(disciplina): descrição curta

Exemplos:
feat(estat-multivariada): adiciona análise de agrupamentos por Ward
fix(teoria-jogos): corrige matriz de payoffs na Seção 3
docs(algebra-linear): atualiza README com referências
refactor(notebook): extrai funções auxiliares de visualização
```

---

## Como Navegar

Se você chegou aqui buscando um trabalho específico, os caminhos mais rápidos são:

- **Por disciplina** → veja a tabela em [Disciplinas e Trabalhos](#disciplinas-e-trabalhos) e acesse o diretório correspondente
- **Por técnica ou ferramenta** → use a busca do GitHub com termos como `ACP`, `Nash`, `cluster`, `LaTeX`
- **Por formato de entrega** → notebooks estão em `notebook/`, relatórios em `relatorio/`, código auxiliar em `codigo/`

---

## Sobre

**Área de formação:** Matemática  
**Instituição:** Centro Universitário Internacional UNINTER  
**Modalidade:** Ensino a Distância (EAD)  
**Localização:** Brasília – Distrito Federal, Brasil

---

<div align="center">

*Produção acadêmica em construção contínua.*  
*Atualizado a cada nova disciplina concluída.*

</div>
