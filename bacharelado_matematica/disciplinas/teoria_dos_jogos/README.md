# Equilíbrio de Nash e Ótimo de Pareto no Transporte Urbano do Distrito Federal

> **Centro Universitário Internacional – UNINTER | Bacharelado em Matemática**
> Disciplina: Teoria dos Jogos · Atividade Prática Locorregional
> Brasília – DF, 2025

---

## Sumário

- [Visão Geral](#visão-geral)
- [Estrutura do Repositório](#estrutura-do-repositório)
- [Modelagem do Jogo](#modelagem-do-jogo)
- [Metodologia Computacional](#metodologia-computacional)
- [Principais Resultados](#principais-resultados)
- [Como Reproduzir](#como-reproduzir)
- [Dependências](#dependências)
- [Figuras Produzidas](#figuras-produzidas)
- [Referências](#referências)
- [Licença](#licença)

---

## Visão Geral

Este repositório contém o notebook de análise computacional e as figuras produzidas como insumo para o artigo científico que aplica **Teoria dos Jogos** ao problema de mobilidade urbana no **Distrito Federal (DF)**.

A situação é formalizada como um **jogo estratégico em forma normal** de dois jogadores — motoristas particulares e o Governo do Distrito Federal (GDF) — com o objetivo de comparar o **Equilíbrio de Nash** e o **Ótimo de Pareto** do jogo, quantificar a divergência entre ambos por meio do **Preço da Anarquia** (PoA) e extrair recomendações de política pública fundamentadas na análise formal.

O DF apresenta uma das maiores taxas de motorização per capita do país, com frota crescendo **62% entre 2010 e 2022** enquanto a população cresceu apenas **10%** no mesmo período. O Metrô-DF opera com **27 estações** e **42,38 km** de extensão, com **74,8% dos passageiros** gastando entre 31 minutos e 2 horas no deslocamento diário ao trabalho — evidência empírica da ineficiência do equilíbrio observado.

### Questão Central

> O equilíbrio de mobilidade urbana observado no DF é eficiente do ponto de vista social? Qual é o custo, em termos de bem-estar agregado, da falha de coordenação entre motoristas e governo?

### Proposições Verificadas

| ID | Proposição | Método | Resultado |
|----|-----------|--------|-----------|
| P₁ | O único EN em estratégias puras é `(TP, INV)` com payoff `(4, 4)` | Verificação exaustiva + `nashpy` | ✅ Confirmada |
| P₂ | `INV` é estratégia estritamente dominante para o GDF para todo `p ∈ [0,1]` | Análise de EU em estratégias mistas | ✅ Confirmada |
| P₃ | O único Ótimo de Pareto do jogo é `(TP, INV)`, coincidindo com o EN formal | Verificação de dominância de Pareto | ✅ Confirmada |
| P₄ | O equilíbrio empírico `(CP, PARC)` produz PoA = 2,0 (perda social de 50%) | Cálculo direto do PoA | ✅ Confirmada |

---

## Estrutura do Repositório

```
teoria_dos_jogos/
├── README.md                                        # Este arquivo
│
├── notebook/
│   └── analise_teoria_jogos_df.ipynb                # Notebook principal (executado)
│
└── results/
    ├── fig1_bimatriz_anotada.png                    # Bimatriz com BR e EN marcados
    ├── fig2_espaco_payoffs_pareto.png               # Espaço (u₁, u₂) e fronteira de Pareto
    ├── fig3_estrategias_mistas.png                  # Dominância em estratégias mistas
    ├── fig4_bem_estar_social.png                    # Comparação de W por perfil
    ├── fig5_dinamica_melhores_respostas.png         # Diagrama de fluxo de BR
    └── fig6_sensibilidade_PoA.png                  # Robustez do EN e Preço da Anarquia
```

---

## Modelagem do Jogo

O jogo é definido formalmente como a tripla:

$$G = \bigl(N,\; \{S_i\}_{i \in N},\; \{u_i\}_{i \in N}\bigr)$$

### Jogadores

| Jogador | Representação | Decisão estratégica |
|---------|--------------|---------------------|
| $J_1$ | Motoristas particulares | Modal de deslocamento diário |
| $J_2$ | Governo do Distrito Federal (GDF) | Nível de investimento em transporte público |

### Espaços de Estratégias

```
S₁ = { TP  (Transporte Público),  CP  (Carro Particular) }
S₂ = { INV (Investimento amplo),  PARC (Parcial),  NÃO (Sem investimento) }

|S| = |S₁ × S₂| = 6 perfis de estratégias
```

### Bimatriz de Payoffs

Payoffs em escala ordinal discreta `{1, 2, 3, 4}` — maior valor = maior utilidade.

|  | **INV** | **PARC** | **NÃO** |
|--|---------|----------|---------|
| **TP** | **(4, 4)** ★ | (3, 3) | (1, 2) |
| **CP** | (2, 3) | **(2, 2)** † | (1, 1) |

> ★ Equilíbrio de Nash = Ótimo de Pareto &nbsp;&nbsp;&nbsp; † Equilíbrio empírico observado no DF

---

## Metodologia Computacional

O notebook executa o seguinte pipeline analítico:

```
Definição formal do jogo G
        │
        ▼
  [Célula 2] Bimatriz: matrizes A₁ e A₂ (numpy arrays)
        │
        ▼
  [Célula 3] Correspondências de melhor resposta BR₁ e BR₂
             └── Cálculo algébrico via argmax por coluna/linha
        │
        ▼
  [Célula 4] Equilíbrios de Nash em estratégias puras
             ├── Verificação exaustiva dos 6 perfis
             └── Confirmação via nashpy (support enumeration)
        │
        ▼
  [Célula 5] Fronteira de Pareto e dominância
             ├── Verificação par-a-par de dominância de Pareto
             └── Cálculo do Preço da Anarquia (PoA)
        │
        ▼
  [Células 6–7] Figuras: bimatriz anotada e espaço de payoffs
        │
        ▼
  [Célula 8] Análise de estratégias mistas
             ├── EU₁(TP) − EU₁(CP) = 1 + q > 0  ∀q ∈ [0,1]
             └── EU₂(INV) − EU₂(PARC) = 1 > 0   ∀p ∈ [0,1]
        │
        ▼
  [Células 9–12] Figuras: bem-estar, dinâmica de BR, sensibilidade
        │
        ▼
  [Célula 13] Relatório analítico consolidado (TXT)
```

### Técnicas e Parâmetros

| Técnica | Implementação | Parâmetro / Critério |
|---------|--------------|----------------------|
| Melhor resposta | `numpy.argmax` por eixo | Máximo de cada coluna/linha |
| EN em estratégias puras | Verificação exaustiva manual | Condição de dupla BR |
| EN em estratégias mistas | `nashpy` — `support_enumeration()` | Todos os suportes |
| Dominância de Pareto | Comparação par-a-par | `u_i(s') ≥ u_i(s)` com pelo menos um estrito |
| Preço da Anarquia | Cálculo direto | `PoA = W* / W_emp` |
| Análise de sensibilidade | Varredura paramétrica | `u₁(CP, INV) ∈ [0.5, 5]` |

---

## Principais Resultados

### Equilíbrio de Nash

A verificação exaustiva e a confirmação via `nashpy` (algoritmo de enumeração de suporte) identificaram **um único Equilíbrio de Nash** do jogo, em estratégias puras e mistas:

$$s^* = (\mathrm{TP},\; \mathrm{INV}) \qquad u(s^*) = (4,\; 4)$$

A unicidade decorre da existência de **estratégias estritamente dominantes** para ambos os jogadores:

| Jogador | Estratégia dominante | Margem de dominância |
|---------|---------------------|----------------------|
| $J_1$ (Motoristas) | TP | $EU_1(\mathrm{TP}) - EU_1(\mathrm{CP}) = 1 + q > 0 \;\forall q$ |
| $J_2$ (GDF) | INV | $EU_2(\mathrm{INV}) - EU_2(\mathrm{PARC}) = 1 > 0 \;\forall p$ |

### Fronteira de Pareto

| Perfil | $u_1$ | $u_2$ | $W = u_1 + u_2$ | Pareto-ótimo? | Dominado por |
|--------|-------|-------|-----------------|--------------|--------------|
| `(TP, INV)` | 4 | 4 | **8** | **Sim ★** | — |
| `(TP, PARC)` | 3 | 3 | 6 | Não | `(TP, INV)` |
| `(CP, INV)` | 2 | 3 | 5 | Não | `(TP, INV)` |
| `(CP, PARC)` | 2 | 2 | 4 | Não | `(TP, INV)`, `(TP, PARC)`, `(CP, INV)` |
| `(TP, NÃO)` | 1 | 2 | 3 | Não | `(TP, INV)`, … |
| `(CP, NÃO)` | 1 | 1 | 2 | Não | todos os demais |

### Preço da Anarquia

$$\mathrm{PoA} = \frac{W^*}{W_{\mathrm{emp}}} = \frac{u_1(\mathrm{TP},\mathrm{INV}) + u_2(\mathrm{TP},\mathrm{INV})}{u_1(\mathrm{CP},\mathrm{PARC}) + u_2(\mathrm{CP},\mathrm{PARC})} = \frac{4+4}{2+2} = \frac{8}{4} = \mathbf{2{,}0}$$

O equilíbrio empiricamente observado no DF representa **apenas 50% do bem-estar social potencial máximo**. A falha de coordenação — sustentada por expectativas pessimistas autorreferentes e pelo problema de credibilidade do GDF — custa metade do bem-estar possível.

---

## Como Reproduzir

### 1. Clonar o repositório

```bash
git clone https://github.com/jullyano/teoria_dos_jogos.git
cd teoria_dos_jogos
```

### 2. Criar ambiente virtual e instalar dependências

```bash
python -m venv .venv
source .venv/bin/activate        # Linux / macOS
# .venv\Scripts\activate         # Windows

pip install -r requirements.txt
```

### 3. Executar o notebook

```bash
jupyter notebook notebook/analise_teoria_jogos_df.ipynb
```

Ou para executar via linha de comando e exportar todas as saídas:

```bash
jupyter nbconvert --to notebook --execute \
  notebook/analise_teoria_jogos_df.ipynb \
  --output notebook/analise_teoria_jogos_df_executed.ipynb
```

> **Tempo estimado de execução:** 15–30 segundos em hardware moderno.
> Todas as figuras são salvas automaticamente no diretório `results/`.

---

## Dependências

### Python

```
numpy>=1.24
pandas>=2.0
matplotlib>=3.7
scipy>=1.11
nashpy>=0.0.17
jupyter>=1.0
nbconvert>=7.0
```

Arquivo `requirements.txt` para instalação direta:

```
numpy
pandas
matplotlib
scipy
nashpy
jupyter
nbconvert
```

### Versão do Python

Testado com **Python 3.11**. Compatível com 3.9+.

---

## Figuras Produzidas

Todas as figuras são salvas em `results/` com resolução **300 dpi**, prontas para inserção no artigo científico em LaTeX.

| Arquivo | Descrição | Seção do artigo |
|---------|-----------|-----------------|
| `fig1_bimatriz_anotada.png` | Bimatriz do jogo com melhores respostas sublinhadas (BR₁ vermelho, BR₂ azul pontilhado), EN e equilíbrio empírico marcados | Análise Computacional |
| `fig2_espaco_payoffs_pareto.png` | Espaço de payoffs `(u₁, u₂)` com fronteira de Pareto, vetor de melhoria social e PoA | Ótimo de Pareto |
| `fig3_estrategias_mistas.png` | Utilidade esperada dos jogadores em função de `q = Pr(INV)` e `p = Pr(TP)`, evidenciando dominância estrita | Análise Computacional |
| `fig4_bem_estar_social.png` | Bem-estar social `W = u₁ + u₂` por perfil de estratégias (barras) e decomposição individual `u₁`, `u₂` | Comparação e PoA |
| `fig5_dinamica_melhores_respostas.png` | Diagrama de fluxo no espaço de perfis: setas de desvio unilateral de cada jogador, convergindo para o EN único | Identificação do EN |
| `fig6_sensibilidade_PoA.png` | Robustez do EN a perturbações em `u₁(CP, INV)` e PoA como função do bem-estar empírico | Comparação e PoA |

---

## Referências

### Referências da Disciplina

- BIERMAN, H. Scott; FERNANDEZ, Luis F. *Teoria dos Jogos*. 2. ed. São Paulo: Pearson Prentice Hall, 2011.
- FIANI, Ronaldo. *Teoria dos Jogos: para cursos de administração e economia*. Rio de Janeiro: Elsevier/Campus, 2004.
- GIBBONS, Robert. *Game Theory for Applied Economists*. Princeton: Princeton University Press, 1992.
- RASMUSSEN, Eric. *Games and Information: An Introduction to Game Theory*. Cambridge: Blackwell, 1989.

### Literatura Canônica

- NASH, John Forbes. Equilibrium Points in N-Person Games. *Proceedings of the National Academy of Sciences*, v. 36, n. 1, p. 48–49, 1950.

### Dados Empíricos Oficiais

- DISTRITO FEDERAL. Secretaria de Estado de Transporte e Mobilidade (SEMOB). *Plano Diretor de Transporte Urbano e Mobilidade do Distrito Federal — PDTU/DF*. Brasília: SEMOB/GDF, 2010.
- METRÔ-DF (Companhia do Metropolitano do Distrito Federal). *Relatório de Gestão 2022*. Brasília: Metrô-DF, 2022.
- SECRETARIA NACIONAL DE TRÂNSITO (SENATRAN). *Frota de Veículos — Dezembro de 2023*. Brasília: Ministério dos Transportes, 2023.
- SECRETARIA DE ESTADO DE GOVERNO DO DISTRITO FEDERAL (SEGOV). Novo PDTU busca adaptar transporte e mobilidade ao crescimento do DF. *Agência Brasília*, 2024.
- SECRETARIA DE ESTADO DE TRANSPORTE E MOBILIDADE DO DISTRITO FEDERAL (SEMOB-DF). Faixas e corredores exclusivos garantem viagens mais rápidas no transporte público. *Jornal de Brasília*, 2025.

---

## Licença

Este repositório é destinado exclusivamente a fins acadêmicos, como parte da Atividade Prática Locorregional da disciplina de Teoria dos Jogos do Bacharelado em Matemática da UNINTER. Os dados empíricos utilizados são de domínio público, obtidos de fontes oficiais do governo do Distrito Federal.

---

*Brasília – DF, 2026*
