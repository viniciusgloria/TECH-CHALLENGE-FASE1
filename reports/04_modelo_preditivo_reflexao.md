# Reflexão sobre Modelo Preditivo para NPS

## Por que um modelo preditivo faz sentido aqui?

O problema central deste case é que o NPS só é coletado depois que a jornada de compra termina. Nesse momento, a empresa não pode mais agir para melhorar aquela experiência específica.

Os dados disponíveis no dataset mostram que há variáveis operacionais com correlação forte com o NPS, como atraso na entrega (-0,59), número de reclamações (-0,49) e contatos com o atendimento (-0,34). Essas variáveis são geradas durante a jornada de compra, antes de a pesquisa de NPS ser enviada.

Isso abre a possibilidade de construir um modelo que use os dados operacionais disponíveis em tempo real para estimar qual será o NPS de um cliente, ou pelo menos se ele está em risco de se tornar um detrator, antes de a pesquisa ser disparada. Com essa informação, a empresa pode acionar intervenções proativas, como um contato de suporte preventivo ou uma ação de recuperação de experiência.

## Comparação entre as abordagens possíveis

### Abordagem 1: Regressão para estimar a nota contínua

Nesta abordagem, o modelo tentaria prever o valor exato do `nps_score`, tratando-o como uma variável contínua entre 0 e 10.

A vantagem é que a empresa teria uma estimativa numérica precisa e poderia criar diferentes faixas de alerta com thresholds customizados.

A limitação é que prever uma nota com precisão decimal é um problema mais difícil do que classificar um grupo. Além disso, do ponto de vista de negócio, saber que um cliente provavelmente dará 2,8 ou 3,1 faz pouca diferença prática. O que importa para a tomada de decisão operacional é saber se ele está no grupo de risco.

Modelos adequados para esta abordagem: regressão linear múltipla, Random Forest Regressor, Gradient Boosting Regressor.

Métricas de avaliação: MAE (Erro Absoluto Médio) e RMSE (Raiz do Erro Quadrático Médio) mediriam o quão longe as previsões estão do valor real.

### Abordagem 2: Classificação por categoria NPS

Nesta abordagem, o modelo classificaria o cliente em uma das três categorias: Promotor, Neutro ou Detrator. Ou, em uma versão mais simples, em apenas duas: satisfeito (Promotor/Neutro) e insatisfeito (Detrator).

A vantagem é que o output é diretamente acionável. O modelo não entrega um número abstrato, mas uma resposta que a operação consegue usar imediatamente: "esse pedido tem alta chance de gerar um detrator".

A limitação é o desbalanceamento de classes. Nos dados analisados, 84,4% são detratores. Um modelo ingênuo que classificasse todo mundo como detrator acertaria 84% do tempo sem aprender nada útil. É necessário usar técnicas de balanceamento (como SMOTE ou ajuste de pesos de classe) e métricas que penalizem esse tipo de comportamento, como F1-score balanceado ou ROC AUC.

Modelos adequados: Regressão Logística, Random Forest Classifier, XGBoost, LightGBM.

Métricas de avaliação: Acurácia balanceada, F1-score por classe, ROC AUC, Matriz de Confusão.

## Estratégia recomendada

A recomendação é começar pela classificação binária: Detrator vs. Não-Detrator.

O raciocínio é prático. A empresa tem 84,4% de detratores. Identificar corretamente quem vai virar detrator antes da pesquisa já teria impacto operacional imediato, porque esse é o grupo que mais precisa de intervenção e que tem taxa de recompra zero.

A classificação binária também é mais fácil de explicar e de operar: o time de atendimento recebe uma lista de pedidos com alta probabilidade de gerar insatisfação e age sobre eles. Não precisa interpretar um número contínuo.

Numa segunda fase, após validar o modelo binário em produção, seria possível evoluir para a classificação multiclasse (Promotor/Neutro/Detrator) ou até para a regressão, dependendo das necessidades específicas de cada área.

## Variáveis de entrada candidatas

Com base nas correlações encontradas, as variáveis mais promissoras como features de um modelo são:

- `delivery_delay_days` (correlação -0,59 com NPS)
- `csat_internal_score` (correlação +0,56, mas com atenção ao risco de leakage)
- `complaints_count` (correlação -0,49)
- `customer_service_contacts` (correlação -0,34)
- `resolution_time_days` (correlação -0,19)
- `delivery_attempts` (correlação baixa, mas pode capturar padrões combinados)
- `freight_value` e `order_value` (correlação baixa individualmente, mas podem interagir com outras variáveis)

Uma atenção especial deve ser dada ao `csat_internal_score`. Ele tem correlação alta com o NPS, o que o tornaria um feature valioso. Porém, se ele for coletado no mesmo momento que o NPS, usá-lo seria vazamento de informação (data leakage). Antes de incluir essa variável, é necessário confirmar o timing exato de coleta.

## Estratégia de validação

A validação deve ser temporal, não aleatória. Isso significa separar os dados de forma que o modelo seja treinado em pedidos mais antigos e testado em pedidos mais recentes, simulando o comportamento em produção.

A divisão aleatória pode parecer tecnicamente válida, mas falha em capturar sazonalidade e evolução do comportamento do cliente ao longo do tempo. Uma divisão temporal elimina esse risco.

Proporção sugerida: 70% dos dados mais antigos para treino, 30% mais recentes para teste.

## Como a empresa usaria isso na prática

Em produção, o modelo seria integrado ao sistema operacional da empresa da seguinte forma:

1. A cada pedido que alcança determinado estágio da jornada (por exemplo, quando o prazo de entrega é iminente e já há sinal de atraso), o sistema alimenta o modelo com os dados operacionais disponíveis até aquele momento.

2. O modelo retorna uma probabilidade de esse cliente se tornar detrator.

3. Pedidos com probabilidade acima de um threshold definido pela operação, como 70%, entram em uma fila de atenção prioritária.

4. O time de atendimento contata proativamente esses clientes antes da pesquisa de NPS ser enviada, oferecendo suporte, informação sobre o pedido ou uma compensação quando aplicável.

Com o padrão identificado nos dados, especialmente a ruptura a partir de 3 dias de atraso e o comportamento dos contatos com atendimento, um modelo simples como Regressão Logística com as principais features já teria poder preditivo suficiente para começar a gerar valor operacional. Modelos mais complexos podem ser introduzidos em iterações posteriores, conforme a empresa acumula feedback de produção.
