NPS Preditivo: Análise de Satisfação em E-commerce

Tech Challenge Fase 1 | FIAP Pós Tech AI Scientist


Objetivo do Projeto
-------------------

A empresa fictícia de e-commerce 'GNF' identificou alta variabilidade no NPS (Net Promoter Score) entre seus clientes, mesmo com indicadores operacionais aparentemente semelhantes. O NPS é coletado apenas após o encerramento da jornada de compra, o que impede qualquer ação preventiva.

Este projeto analisa dados operacionais históricos de 2.500 pedidos para identificar quais fatores explicam a satisfação do cliente e como prever o risco de insatisfação antes da pesquisa de NPS ser aplicada.

A pergunta que guia o trabalho é: quais fatores operacionais influenciam o NPS e como a empresa pode agir antes do problema acontecer?


Base de Dados
-------------

| Item | Detalhe |
|------|---------|
| Arquivo | `data/raw/desafio_nps_fase_1.csv` |
| Registros | 2.500 pedidos |
| Variáveis | 19 colunas |
| Target | `nps_score` (nota de 0 a 10, coletada após a compra) |
| Valores faltantes | Nenhum |

Dicionário das variáveis:

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
| `delivery_attempts` | Número de tentativas de entrega |
| `customer_service_contacts` | Contatos do cliente com o atendimento |
| `resolution_time_days` | Tempo para resolução de problemas (dias) |
| `complaints_count` | Número de reclamações registradas |
| `repeat_purchase_30d` | Recompra em 30 dias (0=não, 1=sim) |
| `csat_internal_score` | Score interno de satisfação |
| `nps_score` | Nota NPS do cliente (0 a 10), variável target |


Metodologia
-----------

O projeto segue a estrutura CRISP-DM e cobre os quatro itens do Tech Challenge:

1. Entendimento do negócio (Item 1): definição do problema, importância do NPS para e-commerce, áreas impactadas e reflexão sobre recompra, boca a boca e market share.
2. Definição da target (Item 2): identificação da variável alvo, justificativa da escolha, momento de coleta e riscos de uso inadequado.
3. Análise exploratória (Item 3): identificação dos fatores críticos, ponto de ruptura na experiência do cliente e perfis de promotores e detratores, com narrativa acessível para gestores.
4. Modelo preditivo (Item 4): classificação binária Detrator vs. Não-Detrator usando Regressão Logística e Random Forest com `class_weight='balanced'`, com avaliação por classification report, matriz de confusão e ROC AUC.


Principais Resultados
---------------------

O NPS agregado da empresa na amostra é de -69,6, com aproximadamente 74% de detratores, 22% de neutros e 4% de promotores.

O principal fator associado à insatisfação é o atraso na entrega, que apresentou correlação de Spearman de -0,59 com o NPS. O ponto de ruptura identificado está em 3 dias de atraso, momento em que o NPS médio cai de 5,1 para 2,9.

A taxa de recompra em 30 dias é de 100% entre promotores e 0% entre detratores, o que liga a satisfação diretamente à receita futura.

O modelo escolhido foi um Random Forest com `class_weight='balanced'`, que atingiu acurácia de 83% e ROC AUC de 0,87 no conjunto de teste, com recall de 94% para a classe Detrator.


Estrutura do Projeto
--------------------

```
TECH CHALLENGE/
├── data/
│   ├── raw/                    CSV original da base de dados
│   └── processed/              CSV com colunas derivadas (gerado pelo notebook 01)
├── notebooks/
│   ├── 01_eda_nps.ipynb        Análise exploratória completa
│   └── 02_modelo_preditivo.ipynb  Modelo preditivo de classificação
├── reports/
│   ├── 01_entendimento_do_negocio.md    Respostas do Item 1
│   ├── 02_definicao_target.md           Respostas do Item 2
│   ├── 03_eda_storytelling.md           Síntese da EDA para gestores
│   ├── 04_modelo_preditivo_reflexao.md  Estratégia e justificativa do Item 4
│   ├── slides_executivos.pdf            Apresentação para stakeholders
│   └── fig_*.png                        Gráficos gerados pelos notebooks
├── models/
│   └── modelo_nps.pkl          Modelo Random Forest treinado
├── README.md
├── requirements.txt
└── .gitignore
```


Como Reproduzir
---------------

Pré-requisitos: Python 3.10 ou superior e Jupyter Notebook.

```bash
# Clone o repositório
git clone https://github.com/viniciusgloria/TECH-CHALLENGE-FASE1.git
cd TECH-CHALLENGE-FASE1

# (Opcional) Crie um ambiente virtual
python -m venv .venv
.venv\Scripts\activate           # Windows
source .venv/bin/activate        # Linux/Mac

# Instale as dependências
pip install -r requirements.txt

# Abra o Jupyter
python -m jupyter notebook
```

Execute os notebooks na ordem:

1. `notebooks/01_eda_nps.ipynb`, que lê `data/raw/`, gera os gráficos em `reports/` e salva a base tratada em `data/processed/nps_tratado.csv`.
2. `notebooks/02_modelo_preditivo.ipynb`, que lê `data/processed/`, treina os modelos, salva o melhor em `models/modelo_nps.pkl` e gera os gráficos do modelo em `reports/`.

Troubleshooting: se o comando `jupyter notebook` não for reconhecido pelo Windows, use `python -m jupyter notebook`. Esse comando funciona mesmo quando o executável `jupyter` não está no PATH do sistema.

Os notebooks usam caminhos relativos como `../data/`. Para que funcionem corretamente, é importante abrir o Jupyter a partir da raiz do projeto (`TECH CHALLENGE/`) e navegar até a pasta `notebooks/` na interface, não abrir os notebooks diretamente por outro caminho.


Entregáveis
-----------

| Item | Entregável | Localização |
|------|------------|-------------|
| 1 | Entendimento do negócio | `reports/01_entendimento_do_negocio.md` |
| 2 | Definição da target | `reports/02_definicao_target.md` |
| 3 | Análise exploratória | `notebooks/01_eda_nps.ipynb` |
| 3 | Síntese para gestores | `reports/03_eda_storytelling.md` |
| 4 | Reflexão sobre o modelo | `reports/04_modelo_preditivo_reflexao.md` |
| 4 | Modelo preditivo em Python | `notebooks/02_modelo_preditivo.ipynb` |
| | Slides executivos | `reports/slides_executivos.pdf` |
| | Vídeo executivo | https://www.loom.com/share/66f91fc9da484d1b9604ab509373005f |


---

Autor: Vinícius Gomes Machado Gloria
Instituição: FIAP Pós Tech
Curso: AI Scientist - Fase 1
