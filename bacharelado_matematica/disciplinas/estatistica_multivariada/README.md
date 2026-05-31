# Análise Multivariada de Sinistros de Trânsito com Vítimas Fatais no Distrito Federal (2022–2024)

> **Centro Universitário Internacional – UNINTER | Bacharelado em Matemática**
> Disciplina: Estatística Multivariada · Atividade Locorregional
> Brasília – DF, 2025

---

## Sumário

- [Visão Geral](#visão-geral)
- [Estrutura do Repositório](#estrutura-do-repositório)
- [Dados](#dados)
- [Metodologia](#metodologia)
- [Principais Resultados](#principais-resultados)
- [Como Reproduzir](#como-reproduzir)
- [Dependências](#dependências)
- [Figuras Produzidas](#figuras-produzidas)
- [Referências](#referências)
- [Licença](#licença)

---

## Visão Geral

Este repositório contém o código, os dados e o relatório completo de uma pesquisa científica que aplica **Análise Estatística Multivariada** ao estudo dos sinistros de trânsito com vítimas fatais no **Distrito Federal (DF)** entre 2022 e 2024.

O DF registrou **288 óbitos em 2022**, **248 em 2023** e **223 em 2024** — redução acumulada de **19,7%** —, mas com distribuição espacial e perfis de risco heterogêneos entre as 33 Regiões Administrativas. O estudo investiga padrões latentes desse fenômeno usando três técnicas multivariadas complementares.

### Problema de Pesquisa

> Quais padrões multivariados subjazem à distribuição espacial e temporal dos sinistros fatais no DF entre 2022 e 2024, e quais combinações de fatores explicam a concentração da mortalidade em determinadas Regiões Administrativas e grupos de vítimas?

### Hipóteses Testadas

| ID | Hipótese | Técnica | Resultado |
|----|----------|---------|-----------|
| H₁ | Existência de estrutura latente de baixa dimensionalidade nas variáveis de risco | ACP | ✅ Corroborada |
| H₂ | As 33 RAs agrupam-se em perfis homogêneos de sinistralidade | Cluster | ✅ Corroborada |
| H₃ | Associação não aleatória entre tipo de vítima, tipo de via e fator de risco | Análise de Correspondência | ✅ Corroborada |

---

## Estrutura do Repositório

```
.
├── analise_multivariada_sinistros_df.ipynb   # Notebook principal (executado)
├── Relatorio_Final_Estatistica_Multivariada_Transito_DF.docx  # Relatório ABNT
├── README.md                                  # Este arquivo
└── figs/
    ├── fig_01_evolucao_obitos.png             # Série anual, mensal e perfil de vítimas
    ├── fig_02_boxplots.png                    # Boxplots das variáveis por RA
    ├── fig_03_correlacao.png                  # Matriz de correlação de Pearson
    ├── fig_04_screeplot.png                   # Scree plot e variância acumulada (ACP)
    ├── fig_05_loadings.png                    # Cargas fatoriais (heatmap)
    ├── fig_06_biplot.png                      # Biplot CP1 × CP2
    ├── fig_07_dendrograma.png                 # Dendrograma hierárquico (Ward)
    ├── fig_08_otimo_clusters.png              # Elbow + silhueta
    ├── fig_09_radar_clusters.png              # Radar chart dos clusters
    ├── fig_10_clusters_acp.png                # Clusters no espaço da ACP
    ├── fig_11_ac_vitima_via.png               # Análise de correspondência: Vítima × Via
    ├── fig_12_ac_vitima_risco.png             # Análise de correspondência: Vítima × Risco
    ├── fig_13_ac_risco_periodo.png            # Análise de correspondência: Risco × Período
    ├── fig_14_freq_relativa.png               # Frequências relativas (heatmaps)
    └── fig_15_painel_politicas.png            # Painel analítico integrado
```

---

## Dados

### Fonte Principal

Os dados primários foram extraídos dos **Boletins Anuais de Sinistros de Trânsito com Vítimas Fatais** do **Detran-DF / GEREST** (Gerência de Estatística de Acidentes de Trânsito) e do **Sistema Infovidas**, todos de acesso público.

| Fonte | Portal | Dados disponibilizados |
|-------|--------|------------------------|
| Detran-DF / GEREST | [detran.df.gov.br](https://www.detran.df.gov.br/category/servicos-mais-procurados/estatisticas-do-transito/) | Boletins anuais 2022–2024, perfil de vítimas, tipo de via, fator de risco |
| Sistema Infovidas | Detran-DF (lançado mar/2024) | Painel estratégico de segurança viária |
| PRF – Dados Abertos | [gov.br/prf](https://www.gov.br/prf/pt-br/acesso-a-informacao/dados-abertos/dados-abertos-da-prf) | Boletins de ocorrência em rodovias federais (CSV) |
| SES-DF | [saude.df.gov.br](https://www.saude.df.gov.br/vigilancia-em-acidentes) | Mapa de calor e boletins epidemiológicos (CID V01–V99) |
| IPEA | [repositorio.ipea.gov.br](https://repositorio.ipea.gov.br) | Custos dos acidentes de trânsito – TD n.º 2.565 |

### Nota Metodológica sobre os Dados

Como os microdados individuais por ocorrência não são disponibilizados publicamente em formato estruturado (apenas em relatórios PDF), o dataset utilizado no notebook foi construído a partir dos **totais anuais oficialmente publicados**, com desagregação mensal e por RA estimada por repartição proporcional, preservando todas as margens e proporções dos boletins oficiais. As seguintes **âncoras fixas** foram respeitadas em todo o modelo:

- Total de óbitos por ano: 288 (2022), 248 (2023), 223 (2024)
- Proporção de motociclistas: 26%, 27% e 32% nos respectivos anos
- Proporção de pedestres: 32%, 34% e 36%
- Proporção de vítimas masculinas: 81%, 81% e 85%
- Casos de álcool detectados em 2024: 28 condutores

### Variáveis

O estudo operacionaliza **10 variáveis** por Região Administrativa:

| Cód. | Variável | Tipo |
|------|----------|------|
| V1 | N.º de sinistros fatais | Quantitativa discreta |
| V2 | N.º de vítimas fatais | Quantitativa discreta |
| V3 | Tipo de vítima | Qualitativa nominal |
| V4 | Sexo da vítima | Qualitativa nominal |
| V5 | Faixa etária da vítima | Qualitativa ordinal |
| V6 | Tipo de via | Qualitativa nominal |
| V7 | Fator de risco associado | Qualitativa nominal |
| V8 | Dia da semana | Qualitativa ordinal |
| V9 | Período do dia | Qualitativa ordinal |
| V10 | Região Administrativa | Qualitativa nominal |

---

## Metodologia

O notebook executa o seguinte pipeline analítico:

```
Dados brutos (Detran-DF)
        │
        ▼
  [Etapa 1] Revisão bibliográfica e definição do problema
        │
        ▼
  [Etapa 2] Tabulação – dataset por RA (33 × 10 variáveis)
        │
        ▼
  [Etapa 3] Pré-processamento
            ├── Verificação de ausentes e outliers (IQR)
            └── Padronização (escore z)
        │
        ▼
  [Etapa 4] Análise exploratória univariada
            ├── Estatísticas descritivas
            ├── Boxplots
            └── Matriz de correlação
        │
        ▼
  [Etapa 5] Análise de Componentes Principais (ACP)
            ├── Autovalores / scree plot
            ├── Cargas fatoriais (loadings)
            └── Biplot CP1 × CP2
        │
        ▼
  [Etapa 6] Análise de Agrupamentos (Cluster)
            ├── Dendrograma hierárquico (Ward + euclidiana)
            ├── Critério do cotovelo + coeficiente de silhueta
            └── Radar chart dos clusters
        │
        ▼
  [Etapa 7] Análise de Correspondência
            ├── Tabelas de contingência + testes χ²
            └── Mapas de correspondência (SVD)
        │
        ▼
  [Etapas 8–9] Interpretação integrada + Relatório final
```

### Técnicas e Parâmetros

| Técnica | Parâmetro / Critério | Valor adotado |
|---------|----------------------|---------------|
| ACP | Seleção de componentes | Critério de Kaiser (λ > 1) |
| ACP | Padronização | Escore *z* (média 0, DP 1) |
| Cluster | Método de ligação | Ward |
| Cluster | Métrica de distância | Euclidiana |
| Cluster | Número ótimo de clusters | Silhueta máxima |
| Correspondência | Decomposição | SVD da matriz de resíduos χ² |
| Correspondência | Teste de significância | χ² de Pearson (α = 0,05) |

---

## Principais Resultados

### Análise de Componentes Principais

Três componentes principais foram retidas pelo critério de Kaiser, explicando **78,93%** da variância total:

| Componente | Autovalor (λ) | Variância (%) | Variância acum. (%) | Interpretação |
|------------|--------------|---------------|---------------------|---------------|
| CP1 | 3,7407 | 36,27 | 36,27 | Eixo de Risco Rodoviário-Motorizado |
| CP2 | 3,1206 | 30,26 | 66,53 | Eixo de Magnitude Absoluta de Óbitos |
| CP3 | 1,2778 | 12,39 | 78,93 | Eixo de Perfil de Masculinização |

**CP1** (cargas dominantes): `pct_pedestre` (+0,443) · `pct_rodovia` (−0,438) · `pct_motociclista` (−0,433) · `pct_noite_madrugada` (−0,424)

**CP2** (cargas dominantes): `obitos_2022` (+0,544) · `obitos_2023` (+0,550) · `obitos_2024` (+0,546)

### Análise de Agrupamentos

O coeficiente de silhueta indicou **k = 3** como número ótimo de clusters (silhueta = 0,317):

| Cluster | Perfil | Nº de RAs | Óbitos 2024 | % Motocicl. | % Pedestre | % Rodovia | Var. 2022→2024 |
|---------|--------|-----------|-------------|-------------|------------|-----------|----------------|
| **1** | Rodoviário | 9 | 60 | 41,4% | 26,5% | 76,5% | −24,1% |
| **2** | Urbano – baixo volume | 20 | 95 | 26,4% | 37,5% | 24,9% | −20,8% |
| **3** | Urbano – alto volume | 4 | 64 | 24,8% | 39,7% | 28,4% | −23,8% |

**Composição dos clusters:**

- **Cluster 1 (rodoviário):** Brazlândia, Sobradinho, Planaltina, Paranoá, São Sebastião, Park Way, Sobradinho II, Jardim Botânico, Fercal
- **Cluster 2 (urbano baixo vol.):** Gama, Núcleo Bandeirante, Guará, Cruzeiro, Santa Maria, Recanto das Emas, Lago Sul, Riacho Fundo, Lago Norte, Candangolândia, Águas Claras, Riacho Fundo II, Sudoeste/Octogonal, Varjão, SCIA/Estrutural, Itapoã, SIA, Vicente Pires, Sol Nascente/Pôr do Sol, Arniqueira
- **Cluster 3 (urbano alto vol.):** Brasília (Plano Piloto), Taguatinga, Ceilândia, Samambaia

### Análise de Correspondência

| Par de variáveis | χ² | g.l. | p-valor | Conclusão |
|------------------|----|------|---------|-----------|
| Tipo de Vítima × Tipo de Via | 27,791 | 8 | 0,0005 | Assoc. altamente significativa |
| Tipo de Vítima × Fator de Risco | 35,669 | 12 | 0,0004 | Assoc. altamente significativa |
| Fator de Risco × Período do Dia | ~22,4 | 9 | < 0,01 | Assoc. significativa |

**Associações mais fortes identificadas nos mapas de correspondência:**
- Pedestres ↔ Vias Urbanas / Imprudência–Desatenção
- Motociclistas ↔ Rodovias Federais e Distritais / Velocidade Excessiva
- Condutores ↔ Álcool–Drogas
- Álcool–Drogas ↔ Período Noturno / Madrugada

---

## Como Reproduzir

### 1. Clonar o repositório

```bash
git clone https://github.com/<usuario>/analise-transito-df-multivariada.git
cd analise-transito-df-multivariada
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
jupyter notebook analise_multivariada_sinistros_df.ipynb
```

Ou para executar via linha de comando e exportar todas as saídas:

```bash
jupyter nbconvert --to notebook --execute \
  analise_multivariada_sinistros_df.ipynb \
  --output analise_multivariada_sinistros_df_executed.ipynb
```

> **Tempo estimado de execução:** 30–60 segundos em hardware moderno.  
> Todas as figuras são salvas automaticamente no diretório `figs/`.

---

## Dependências

### Python

```
numpy>=1.24
pandas>=2.0
matplotlib>=3.7
seaborn>=0.12
scikit-learn>=1.3
scipy>=1.11
nbformat>=5.9
jupyter>=1.0
```

Arquivo `requirements.txt` para instalação direta:

```
numpy
pandas
matplotlib
seaborn
scikit-learn
scipy
nbformat
jupyter
```

### Versão do Python

Testado com **Python 3.10+**. Compatível com 3.9 e 3.11.

---

## Figuras Produzidas

| Figura | Descrição | Etapa |
|--------|-----------|-------|
| `fig_01_evolucao_obitos.png` | Série anual de óbitos, distribuição mensal (2022–2024) e perfil de vítimas 2024 | Exploratória |
| `fig_02_boxplots.png` | Boxplots das 10 variáveis por Região Administrativa | Exploratória |
| `fig_03_correlacao.png` | Matriz de correlação de Pearson | Exploratória |
| `fig_04_screeplot.png` | Scree plot com critério de Kaiser e variância acumulada | ACP |
| `fig_05_loadings.png` | Heatmap das cargas fatoriais nas CPs retidas | ACP |
| `fig_06_biplot.png` | Biplot CP1 × CP2: escores das RAs e vetores de variáveis | ACP |
| `fig_07_dendrograma.png` | Dendrograma hierárquico (método de Ward) | Cluster |
| `fig_08_otimo_clusters.png` | Critério do cotovelo (inércia) e coeficiente de silhueta | Cluster |
| `fig_09_radar_clusters.png` | Radar chart do perfil multivariado dos 3 clusters | Cluster |
| `fig_10_clusters_acp.png` | Clusters no espaço CP1 × CP2 | Cluster |
| `fig_11_ac_vitima_via.png` | Mapa de correspondência: Tipo de Vítima × Tipo de Via | Correspondência |
| `fig_12_ac_vitima_risco.png` | Mapa de correspondência: Tipo de Vítima × Fator de Risco | Correspondência |
| `fig_13_ac_risco_periodo.png` | Mapa de correspondência: Fator de Risco × Período do Dia | Correspondência |
| `fig_14_freq_relativa.png` | Heatmaps de frequências relativas (tabelas de contingência) | Correspondência |
| `fig_15_painel_politicas.png` | Painel analítico integrado para subsídio de políticas públicas | Integração |

---

## Referências

### Referências Obrigatórias da Disciplina

- CARVALHO, L. H. *A Estatística Multivariada*. Rio de Janeiro: UFRRJ, 2001.
- FERREIRA, Daniel Furtado. *Análise Multivariada*. Lavras: UFMG, 1996.
- SEHABER, Vanessa F.; VARGAS, Marina. *Estatística Multivariada: Rota de Aprendizagem*. Curitiba: Intersaberes, 2023.
- WANGENHEIM, A. von. *Análise de Agrupamentos*. Florianópolis: UFSC, 2000.

### Dados Oficiais

- DETRAN-DF / GEREST. *Boletins Anuais de Sinistros de Trânsito com Vítimas Fatais – 2022, 2023 e 2024*. Brasília: Detran-DF, 2025.
- BRASIL. *Plano Nacional de Redução de Mortes e Lesões no Trânsito (Pnatrans) 2021–2030*. Brasília: SENATRAN, 2021.
- IPEA. *Custos dos Acidentes de Trânsito no Brasil*. Texto para Discussão n.º 2.565. Rio de Janeiro: IPEA, 2019.
- OMS. *Global Status Report on Road Safety 2023*. Genebra: OMS, 2023.

### Literatura Metodológica e Aplicada

- HAIR JR., J. F. et al. *Análise Multivariada de Dados*. 5. ed. Porto Alegre: Bookman, 2005.
- JOHNSON, R. A.; WICHERN, D. W. *Applied Multivariate Statistical Analysis*. 6. ed. Pearson, 2007.
- PEREIRA, V. S. P. et al. Técnicas Estatísticas Multivariadas aplicadas ao estudo de vítimas fatais em acidentes de trânsito em Belém-PA. *Anais do SPOLM*, Rio de Janeiro: Escola de Guerra Naval, 2007.
- RAMOS, E. M. L. S. et al. Políticas Públicas de Redução dos Acidentes de Trânsito: Análise Multivariada na BR-101/AL. *Redalyc*, 2018.
- ROUSSEEUW, P. J. Silhouettes: A Graphical Aid to the Interpretation and Validation of Cluster Analysis. *Journal of Computational and Applied Mathematics*, v. 20, p. 53–65, 1987.
- TEIXEIRA et al. Óbitos por Acidentes de Trânsito no Brasil: Caracterização e Análise Espacial. *Periódicos Brasil. Pesquisa Científica*, v. 5, n. 1, p. 1298–1312, 2026.

---

## Licença

Este repositório é destinado exclusivamente a fins acadêmicos, como parte da Atividade Locorregional da disciplina de Estatística Multivariada do Bacharelado em Matemática da UNINTER. Os dados utilizados são de domínio público, obtidos de fontes oficiais do governo brasileiro.

---

*Brasília – DF, 2025*
