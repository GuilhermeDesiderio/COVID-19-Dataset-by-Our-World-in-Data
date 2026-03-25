# Análise Global da Pandemia COVID‑19 (OWID, 2020–2024) — README de Insights

## Contexto e objetivo
Este projeto analisa a evolução da COVID‑19 usando o dataset do **Our World in Data (OWID)** para construir uma narrativa orientada a dados sobre:
- **ondas e picos** (quando e onde a pandemia acelerou)
- **mudança de severidade** ao longo do tempo (CFR aproximado)
- **vacinação** e desigualdade (o que avançou e o que ficou para trás)

**Stack:** Python (Pandas) → SQLite → Power BI  
**Intervalo temporal (World / OWID_WRL):** **2020‑01‑05 a 2024‑08‑14**

---

## Referências do projeto (arquivos)
- `analise_covid.ipynb`: notebook de exploração/feature engineering
- `covid_dashboard.pbix`: dashboard Power BI
- `df_clean.csv`: dataset “limpo” (países + agregações OWID) — **429.435 linhas**
- `df_countries_clean.csv`: apenas países — **402.910 linhas**
- `df_agg_clean.csv`: agregações (mundo/continentes) — **26.525 linhas**

**Observação técnica:** os CSVs usam **`;` (separador)** e **`,` (decimal)**.

---

## Executive summary (1 minuto)
**2022 foi o ano do paradoxo:** recorde de casos, mas queda forte de mortes em relação a 2021. E o maior pico diário global aconteceu em **25/12/2022**, puxado principalmente pela China.

No agregado **World (OWID_WRL)** até **2024‑08‑14**:
- Casos acumulados (∑ `new_cases`): **775.935.057**
- Mortes acumuladas (∑ `new_deaths`): **7.060.988**
- Maior pico diário de novos casos: **44.236.227** em **2022‑12‑25** (média móvel 7d: **6.319.461**)

---

## Insight‑chave: 2022 redefiniu a relação “casos vs mortes”
Comparando **2021 vs 2022** (World / OWID_WRL):
- **Casos:** 200.298.480 → **424.017.377** (**2,12x**)
- **Mortes:** 3.549.359 → **1.249.137** (**‑64,8%**)
- **CFR aproximado (mortes/casos no ano):** **1,77% → 0,29%** (queda de **~6,0x**)

Leitura de negócio: vacinação, imunidade acumulada e mudanças de variante reduziram a letalidade média observada, mesmo com alta transmissão.

---

## Principais insights (com números)

### 1) “Natal Negro” (China) e pico global em 25/12/2022
Em **2022‑12‑25**:
- **World:** **44.236.227** novos casos (maior valor diário do período)
- **China:** **40.475.477** novos casos (pico diário do país)

### 2) Ondas/picos: padrão global, não exceção
No recorte de países (excluindo agregações OWID):
- **229 países** tiveram pelo menos um pico marcado
- **982 picos** com `is_real_peak=True` (associados a `wave_id`)

No agregado World:
- **4 picos** detectados, com intervalo médio de **~112 dias** entre picos (aprox.)

### 3) Onde doeu mais (mortes por milhão)
Top 10 países por `total_deaths_per_million` (último registro disponível de cada país):
1. Peru (**6.601**)
2. Bulgária (**5.670**)
3. Macedônia do Norte (**5.422**)
4. Bósnia e Herzegovina (**5.115**)
5. Hungria (**5.065**)
6. Croácia (**4.800**)
7. Eslovênia (**4.767**)
8. Geórgia (**4.519**)
9. Montenegro (**4.318**)
10. Moldávia (**4.028**)

### 4) Brasil e EUA (última data disponível no dataset)
**Brasil (2024‑08‑04):**
- Casos totais: **37.511.921** (≈ **178.368 por milhão**)
- Mortes totais: **702.116** (≈ **3.339 por milhão**)

**EUA (2024‑08‑04):**
- Casos totais: **103.436.829** (≈ **302.859 por milhão**)
- Mortes totais: **1.193.165** (≈ **3.494 por milhão**)

### 5) Vacinação: avanço global + “buraco” de atualização recente
No agregado World em **2024‑08‑14**:
- `people_fully_vaccinated_per_hundred`: **64,93**

Cobertura por país (existência de dados de vacinação):
- **215 de 237 países** têm ao menos um valor em `people_fully_vaccinated_per_hundred`

Ano do **último registro** de vacinação (por país):
- **2022:** 56 países
- **2023:** 139 países
- **2024:** 16 países

Implicação prática: para comparar países, use o **último valor disponível por país**, não a “foto no último dia do dataset”.

### 6) Desigualdade (renda x vacinação) aparece nos dados
Usando, para cada país, o **último valor disponível** de vacinação (com PIB per capita disponível):
- Correlação (Pearson) `gdp_per_capita` × `people_fully_vaccinated_per_hundred`: **0,572** (N=**188**)

Média de vacinação completa (último valor) por categoria de renda:
- **High income:** 68,52
- **Upper‑middle income:** 51,99
- **Lower‑middle income:** 37,88
- **Low income:** 28,61 (amostra pequena com ambas as variáveis)

---

## Linha do tempo (World): casos, mortes e CFR aproximado
Totais anuais (World / OWID_WRL):
- **2020:** 80.317.671 casos | 1.897.584 mortes | CFR ≈ 2,36%
- **2021:** 200.298.480 casos | 3.549.359 mortes | CFR ≈ 1,77%
- **2022:** 424.017.377 casos | 1.249.137 mortes | CFR ≈ 0,29%
- **2023:** 69.238.166 casos | 322.650 mortes | CFR ≈ 0,47%
- **2024 (até 14/08):** 2.063.363 casos | 42.258 mortes | CFR ≈ 2,05%

---

## O que foi “engenheirado” nos dados
Além das colunas originais, os CSVs limpos incluem variáveis prontas para análise/dashboard, por exemplo:
- `case_fatality_rate`, `new_cases_ma7`, `new_deaths_ma7`
- `is_real_peak`, `wave_id`, `days_since_last_peak`
- `covid_phase`, `pandemic_week`, `year_month`
- `income_category`, `density_category`, `age_category`

---

## Dashboard (Power BI)
Arquivo: `covid_dashboard.pbix`

Ideias de páginas (para deixar mais “portfólio”):
- Evolução global (casos/mortes + médias móveis + marcação de picos)
- “Paradoxo 2022” (casos vs mortes e CFR por ano)
- Ranking de mortalidade por milhão (último registro por país)
- Vacinação (último valor por país) vs PIB per capita (segmento por renda)

---

## Como reproduzir (mínimo)
1) Dependências:
```bash
pip install pandas numpy jupyter matplotlib seaborn
```

2) Execute o notebook:
```bash
jupyter notebook
```
e rode `analise_covid.ipynb`.

3) Leitura correta dos CSVs (exemplo):
```python
import pandas as pd
df = pd.read_csv("df_clean.csv", sep=";", decimal=",", parse_dates=["date"])
```

---

## Limitações e cuidados
- **Subnotificação e mudanças de critério**: séries são reportadas por países e variam no tempo.
- **Vacinação com atualização irregular**: muitas séries param em 2022/2023, então “foto no último dia” fica vazia.
- **Comparações entre países**: testagem, demografia, políticas e capacidade de reporte afetam a leitura.
- **CFR aproximado**: aqui é uma razão agregada (mortes/casos no ano), sem ajuste de defasagem entre infecção e óbito.

