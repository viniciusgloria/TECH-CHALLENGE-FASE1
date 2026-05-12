# NPS Preditivo: Análise de Satisfação em E-commerce

**Tech Challenge Fase 1 | FIAP Pós Tech AI Scientist**

## Objetivo do Projeto

Uma empresa de e-commerce percebeu alta variabilidade no NPS (Net Promoter Score) entre seus clientes, mesmo com indicadores operacionais aparentemente semelhantes entre pedidos. O NPS é coletado apenas após o encerramento da jornada de compra, o que impede qualquer ação preventiva.

Este projeto analisa dados operacionais históricos de 2.500 pedidos para identificar quais fatores explicam a satisfação do cliente, com o objetivo de apoiar decisões proativas antes da pesquisa de NPS ser aplicada.

**Pergunta central:** quais fatores operacionais realmente influenciam o NPS e como a empresa pode agir antes do problema acontecer?

## Base de Dados

| Item | Detalhe |
|------|---------|
| Arquivo | `data/raw/desafio_nps_fase_1.csv` |
| Linhas | 2.500 registros de pedidos |
| Colunas | 19 variáveis |
| Target | `nps_score` (nota 0 a 10, coletada após a compra) |
| Valores faltantes | Nenhum |

### Dicionário das Variáveis

| Variável | Descrição |
|----------|-----------|
| `customer_id` | Identificador único do cliente |
| `order_id` | Identificador único do pedido |
| `customer_age` | Idade do cliente |
| `customer_region` | Região geográfica do cliente |
| `customer_tenure_months` | Tempo de relacionamento com a empresa (meses) |
| `order_value` | Valor total do pedido |
| `items_quantity` | Quantidade de itens no pedido |
| `discount_value` | Valor de desconto aplicado |
| `payment_installments` | Número de parcelas do pagamento |
| `delivery_time_days` | Tempo total de entrega (dias) |
| `delivery_delay_days` | Dias de atraso em relação ao prazo prometido |
| `freight_value` | Valor do frete |
| `delivery_attempts` | Tentativas de entrega |
| `customer_service_contacts` | Contatos do cliente com o atendimento |
| `resolution_time_days` | Tempo para resolução de problemas (dias) |
| `complaints_count` | Número de reclamações registradas |
| `repeat_purchase_30d` | Recompra em 30 dias (0=não, 1=sim) |
| `csat_internal_score` | Score interno de satisfação |
| `nps_score` | Nota NPS do cliente (0 a 10) — variável target |

## Metodologia

O projeto segue a estrutura CRISP-DM (Cross-Industry Standard Process for Data Mining):

1. **Entendimento do negócio:** definição do problema, importância do NPS para e-commerce e áreas impactadas pelos insights.
2. **Entendimento dos dados:** exploração inicial, qualidade, distribuições e correlações.
3. **Análise exploratória (EDA):** identificação dos fatores críticos, pontos de ruptura e perfis de cliente.
4. **Reflexão preditiva:** estratégia para antecipação do NPS com modelo de classificação.

## Principais Resultados

- NPS agregado da empresa na amostra: **-69,6** (84,4% de detratores, apenas 4,4% de promotores)
- Fator mais crítico: **atraso na entrega** (correlação Spearman de -0,59 com o NPS)
- Ponto de ruptura identificado: atrasos de 3+ dias derrubam o NPS médio de 5,1 para 2,9
- **Recompra:** 100% dos promotores voltaram a comprar em 30 dias; 0% dos detratores voltaram
- O tempo total de entrega não tem correlação com o NPS; o que importa é o cumprimento do prazo prometido

## Estrutura do Projeto

```
TECH CHALLENGE/
├── data/
│   ├── raw/                    CSV original da base de dados
│   └── processed/              CSV com colunas derivadas (gerado pelo notebook)
├── notebooks/
│   └── 01_eda_nps.ipynb       Análise exploratória completa
├── reports/
│   ├── 01_entendimento_do_negocio.md    Item 1: contexto e reflexão de negócio
│   ├── 02_definicao_target.md           Item 2: definição e riscos da target
│   ├── 03_eda_storytelling.md           Item 3: síntese para gestores
│   ├── 04_modelo_preditivo_reflexao.md  Item 4: estratégia preditiva
│   ├── slides_executivos.pdf            Apresentação para stakeholders
│   ├── roteiro_video.md                 Roteiro do vídeo executivo
│   └── fig_*.png                        Gráficos gerados pelo notebook
├── models/                     Reservado para implementação futura do modelo
├── README.md
├── requirements.txt
├── .gitignore
└── CLAUDE.md                   Contexto do projeto para ambiente de desenvolvimento
```

## Como Reproduzir

### Pré-requisitos

- Python 3.10 ou superior
- Jupyter Notebook ou JupyterLab

### Instalação

```bash
# Clone o repositório
git clone https://github.com/viniciusgloria/TECH-CHALLENGE-FASE1.git
cd TECH-CHALLENGE-FASE1

# Crie e ative um ambiente virtual (recomendado)
python -m venv .venv
source .venv/bin/activate        # Linux/Mac
.venv\Scripts\activate           # Windows

# Instale as dependências
pip install -r requirements.txt
```

### Executar a Análise

```bash
# Abra o Jupyter
jupyter notebook notebooks/01_eda_nps.ipynb
```

Execute as células em sequência. O notebook lê os dados de `data/raw/`, gera os gráficos em `reports/` e salva a base tratada em `data/processed/nps_tratado.csv`.

## Entregáveis

| Entregável | Localização |
|------------|-------------|
| Entendimento do negócio (Item 1) | `reports/01_entendimento_do_negocio.md` |
| Definição da target (Item 2) | `reports/02_definicao_target.md` |
| Análise exploratória (Item 3) | `notebooks/01_eda_nps.ipynb` |
| Síntese para gestores (Item 3) | `reports/03_eda_storytelling.md` |
| Reflexão sobre modelo preditivo (Item 4) | `reports/04_modelo_preditivo_reflexao.md` |
| Slides executivos | `reports/slides_executivos.pdf` |
| Vídeo executivo | [Link do vídeo] |

## Vídeo Executivo

> Link: [a ser adicionado após a gravação]

---

**Autor:** Vinícius Gomes Machado Gloria
**Programa:** FIAP Pós Tech AI Scientist - Fase 1
