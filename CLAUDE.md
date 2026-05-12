# Contexto do Projeto: Tech Challenge Fase 1 - NPS Preditivo

## CUIDADOS
- Antes de atingir o limite de tokens, avise que o limite está próximo para que possamos ajustar o conteúdo e garantir que os itens essenciais sejam entregues.

## Objetivo
Analisar dados operacionais de um e-commerce para entender o que impacta a satisfação do cliente (NPS) e propor como prever essa satisfação antes da pesquisa. Projeto individual para a FIAP Pós Tech AI Scientist.

## Estilo de Escrita Obrigatório
- Português brasileiro, linguagem humana e natural.
- Sem travessões (—), sem emojis, sem formatação excessiva.
- Sem padrões típicos de texto gerado por IA (listas desnecessárias, conclusões óbvias, palavras como "robusto", "abrangente", "desvendar", "mergulhar").
- Textos de negócio devem ser acessíveis para gestores sem background técnico.
- Comentários no código: só onde o raciocínio não é óbvio.

## Base de Dados
- Arquivo: `data/raw/desafio_nps_fase_1.csv`
- 2.500 linhas, 19 colunas
- Target: `nps_score` (0 a 10, coletado após a compra)
- Derivada da target: promotores (9-10), neutros (7-8), detratores (0-6)

## Estrutura do Projeto
```
TECH CHALLENGE/
├── data/raw/          CSV original
├── data/processed/    CSV tratado (gerado pelo notebook)
├── notebooks/         EDA em Jupyter
├── reports/           Textos dos itens 1, 2, 3, 4 + slides + roteiro
├── models/            Reservado para modelo preditivo (opcional)
├── README.md
├── requirements.txt
└── .gitignore
```

## Repositório
https://github.com/viniciusgloria/TECH-CHALLENGE-FASE1.git

## Entregáveis Obrigatórios (prazo: hoje 18:00)
1. `reports/01_entendimento_do_negocio.md`
2. `reports/02_definicao_target.md`
3. `notebooks/01_eda_nps.ipynb`
4. `reports/03_eda_storytelling.md`
5. `reports/04_modelo_preditivo_reflexao.md` (reflexão, não implementação)
6. Slides via Gamma (exportar como PDF em `reports/slides_executivos.pdf`)
7. Vídeo executivo de 5 min + roteiro em `reports/roteiro_video.md`
8. `README.md` completo
