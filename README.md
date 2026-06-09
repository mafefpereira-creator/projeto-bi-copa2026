# ⚽ Copa do Mundo 2026 — Dashboard de BI

> **Quem tem mais chances de ganhar a Copa 2026?**  
> Um projeto de Business Intelligence end-to-end que responde essa pergunta com dados reais, Machine Learning e análises estratégicas.

---

## 📌 Sobre o projeto

Dashboard analítico interativo construído para prever o campeão da Copa do Mundo 2026, combinando dados históricos das Copas de 2014, 2018 e 2022 com atualização automática dos jogos de 2026 ao vivo.

**Desenvolvido por:** Maria Fernanda Pereira  
**Stack:** Python · Pandas · StatsBomb API · football-data.org · K-Means · Chart.js · HTML/JS  
**Deploy:** GitHub Pages / Vercel

---

## 🗂️ Estrutura do projeto

```
copa2026/
│
├── index.html              ← Dashboard completo (único arquivo para deploy)
├── README.md               ← Este arquivo
│
├── colab/
│   └── copa2026_final.ipynb ← Notebook Google Colab (coleta + análise + exportação)
│
└── data/                   ← CSVs gerados pelo notebook (não sobem pro GitHub)
    ├── jogos_historico.csv
    ├── jogadores_stats.csv
    ├── metricas_completo.csv
    └── ranking_2026.csv
```

---

## 📊 Fontes de dados

| Fonte | O que fornece | Custo |
|---|---|---|
| **StatsBomb Open Data** | Eventos por jogador das Copas 2018 e 2022: gols, xG, passes, dribles, assistências | Gratuito |
| **football-data.org** | Copa 2026 ao vivo — resultados atualizados automaticamente | Gratuito (plano free) |
| **Dados embutidos** | Copa 2014 — 64 jogos com resultados oficiais | — |

---

## 🧠 Metodologia

### Score Histórico Ponderado

Cada seleção recebe um **Score_Ano (0–10)** por Copa, calculado a partir de 4 sub-scores:

| Sub-score | Peso | O que mede |
|---|---|---|
| Ofensivo | 30% | xG (55%) + gols marcados (45%) |
| Defensivo | 25% | Inverso dos gols sofridos |
| Eficiência | 25% | % vitórias (60%) + saldo de gols (40%) |
| Criação | 20% | % passes certos + dribles + assistências |

Os scores das 3 Copas são combinados com pesos que priorizam o desempenho recente:

```
Score_2026 = (Score_2014 × 0.15) + (Score_2018 × 0.30) + (Score_2022 × 0.55)
```

> Seleções estreantes em 2026 recebem desconto de **–15%** por falta de histórico.

### K-Means Clustering (Machine Learning não-supervisionado)

O algoritmo agrupa automaticamente as 37 seleções em 4 perfis sem intervenção humana:

- 🏆 **Favoritos Históricos** — 13 seleções
- ⚡ **Fortes Candidatos** — 8 seleções
- ⚽ **Competitivos** — 11 seleções
- 🌱 **Estreantes / Azarões** — 5 seleções

### Atualização ao vivo (Copa 2026)

O dashboard faz uma chamada à API do football-data.org automaticamente ao carregar e a cada 10 minutos. Quando há jogos realizados, o score é ajustado:

```
Score_Ajustado = (Score_Histórico × 0.70) + (Score_Live × 0.30)
```

---

## 🚀 Como rodar localmente

### Opção A — Só abrir o HTML (mais simples)

```bash
# Baixe o index.html e abra no navegador
open index.html
```

Os dados históricos já estão embutidos. Para ativar os dados ao vivo da Copa 2026, configure a chave de API (veja abaixo).

### Opção B — Rodar o notebook e regenerar os dados

```bash
# 1. Abra o Google Colab
# 2. Faça upload de colab/copa2026_final.ipynb
# 3. Execute todas as células (Ctrl+F9)
# 4. Baixe os arquivos gerados (Excel + PDFs)
```

---

## 🔑 Configuração da API Key

Para ativar os dados ao vivo da Copa 2026:

**Vercel (recomendado para produção):**
1. No painel do Vercel → Settings → Environment Variables
2. Adicione: `FOOTBALL_API_KEY = sua_chave_aqui`

**Teste local:**

A chave já está configurada de forma segura no `index.html` via `atob()`. Para substituir sem expor no código, defina antes do script principal:

```html
<script>window.__WC_KEY = "sua_chave_aqui";</script>
```

> ⚠️ Nunca suba sua chave diretamente no GitHub. O `.gitignore` já está configurado para proteger arquivos `.env`.

---

## 📈 O que o dashboard mostra

### 7 seções interativas:

1. **Ranking 2026** — tabela completa com filtro por perfil (Favoritos, Fortes, Competitivos, Estreantes) e barra de score visual
2. **Probabilidades de título** — análise por times com histórico de títulos, sem títulos e estreantes/retornos
3. **Gráficos interativos** — Top 12, distribuição K-Means, evolução 2014→2026
4. **Artilheiros e xG** — top jogadores das Copas 2018 e 2022, filtrável por edição
5. **Copa 2026 Ao Vivo** — resultados em tempo real com atualização automática
6. **Metodologia** — SWOT, PESTEL, Ansoff, fórmula do score, pesos por Copa
7. **Estreias históricas** — os 4 países que jogam sua 1ª Copa em 2026 (Cabo Verde, Curaçau, Jordânia e Uzbequistão)

---

## 🌍 Estreias históricas em 2026

A Copa de 2026 é a primeira com **48 seleções**, o que viabilizou a estreia absoluta de:

| Seleção | Continente | Curiosidade |
|---|---|---|
| 🇨🇻 Cabo Verde | África | Futebol impulsionado pela diáspora em Portugal e Espanha |
| 🇨🇼 Curaçau | CONCACAF | Ilha de 150 mil habitantes — caso raro no futebol mundial |
| 🇯🇴 Jordânia | Ásia | Beneficiada pelas 8 vagas da AFC no novo formato |
| 🇺🇿 Uzbequistão | Ásia | 36 milhões de habitantes, liga doméstica em crescimento |

---

## 📋 Análises estratégicas incluídas

- **SWOT** — forças, fraquezas, oportunidades e ameaças da seleção favorita (França)
- **PESTEL** — fatores externos: político, econômico, social, tecnológico, ambiental e novas regras FIFA (substituições, atendimento médico, lateral com o pé, tiro de meta)
- **Ansoff** — posicionamento estratégico das seleções por perfil de crescimento
- **Probabilidades** — modelo estatístico combinando score + tradição + experiência

---

## ⚠️ Disclaimer

As probabilidades e previsões são baseadas em modelo estatístico e têm fins analíticos e educacionais. Futebol é imprevisível — lesões, sorteio de chaveamento, pênaltis e condições climáticas podem alterar completamente qualquer resultado.

---

## 📬 Contato

**Maria Fernanda Pereira**  
Analista de Dados & Business Intelligence  
[LinkedIn](https://www.linkedin.com/in/mariafernandapereiracarvalho/) · [GitHub](https://github.com/mafefpereira-creator)
