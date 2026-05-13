# Reflexão sobre o Modelo Preditivo

## Por que um modelo preditivo faz sentido aqui?

O problema central deste case é que o NPS só é coletado depois que a jornada de compra termina. Nesse momento, a empresa não pode mais agir para melhorar aquela experiência específica.

Os dados disponíveis no dataset mostram que há variáveis operacionais com correlação forte com o NPS, como atraso na entrega (-0,59), número de reclamações (-0,49) e contatos com o atendimento (-0,34). Essas variáveis são geradas durante a jornada de compra, antes de a pesquisa de NPS ser enviada.

Isso abre a possibilidade de construir um modelo que use os dados operacionais disponíveis em tempo real para estimar qual será o NPS de um cliente, ou pelo menos se ele está em risco de se tornar um detrator, antes de a pesquisa ser disparada. Com essa informação, a empresa pode acionar intervenções proativas, como um contato de suporte preventivo ou uma ação de recuperação de experiência.

## Comparação entre as abordagens possíveis

A primeira abordagem é a regressão, onde o modelo tentaria prever o valor exato do `nps_score`, tratando-o como uma variável contínua entre 0 e 10. A vantagem é que a empresa teria uma estimativa numérica precisa e poderia criar diferentes faixas de alerta com thresholds customizados. A limitação é que prever uma nota com precisão decimal é um problema mais difícil do que classificar um grupo. Além disso, do ponto de vista de negócio, saber que um cliente provavelmente dará 2,8 ou 3,1 faz pouca diferença prática. O que importa para a tomada de decisão operacional é saber se ele está no grupo de risco. Modelos que serviriam para essa abordagem seriam regressão linear múltipla, Random Forest Regressor ou Gradient Boosting Regressor, avaliados por MAE e RMSE.

A segunda abordagem é a classificação por categoria NPS. Aqui o modelo classifica o cliente em uma das três categorias (Promotor, Neutro, Detrator) ou, numa versão mais simples, em apenas duas: detrator ou não-detrator. A vantagem é que o output é diretamente acionável. O modelo não entrega um número abstrato, mas uma resposta que a operação consegue usar imediatamente. A limitação é o desbalanceamento de classes. Como cerca de 74% da base são detratores, um modelo ingênuo que classificasse todo mundo como detrator acertaria 74% do tempo sem aprender nada útil. É necessário usar técnicas de balanceamento, como `class_weight='balanced'` ou SMOTE, e métricas que penalizem esse tipo de comportamento, como F1-score balanceado, ROC AUC e recall por classe.

## Estratégia adotada

A estratégia escolhida foi a classificação binária Detrator vs. Não-Detrator. O raciocínio é prático. A empresa tem muito mais detratores do que outros grupos. Identificar corretamente quem vai virar detrator antes da pesquisa já tem impacto operacional imediato, porque esse é o grupo que mais precisa de intervenção e que tem taxa de recompra zero.

A classificação binária também é mais fácil de explicar e de operar. O time de atendimento recebe uma lista de pedidos com alta probabilidade de gerar insatisfação e age sobre eles. Não precisa interpretar um número contínuo.

Numa segunda fase, após validar o modelo binário em produção, seria possível evoluir para a classificação multiclasse (Promotor, Neutro, Detrator) ou até para a regressão, dependendo das necessidades específicas de cada área.

## Variáveis de entrada usadas

Com base nas correlações encontradas na EDA, as variáveis selecionadas foram as relacionadas a logística (delivery_delay_days, delivery_time_days, delivery_attempts), atendimento (complaints_count, customer_service_contacts, resolution_time_days), pedido (freight_value, order_value, items_quantity, payment_installments, discount_value), e perfil do cliente (customer_age, customer_tenure_months, customer_region).

Decidi não usar o `csat_internal_score` mesmo tendo correlação alta com o NPS. O motivo é que, sem confirmação clara sobre quando esse score é coletado, há risco de ser uma variável que só existe junto com o NPS, o que caracterizaria data leakage. Em produção, o modelo precisa rodar antes da pesquisa, então features que só aparecem depois da jornada não podem entrar.

Também ficaram de fora `customer_id`, `order_id` (identificadores), `nps_score` (origem da target), `nps_categoria` e `faixa_atraso` (derivadas da target), e `repeat_purchase_30d` (informação que só existe depois da compra).

A variável `customer_region` foi transformada em variáveis dummy usando `pd.get_dummies`.

## Estratégia de validação

A validação ideal seria temporal, com treino em pedidos mais antigos e teste em pedidos recentes, simulando o comportamento em produção. A divisão aleatória pode parecer tecnicamente válida, mas falha em capturar sazonalidade e evolução do comportamento do cliente ao longo do tempo.

Como o dataset não tem informação de data, usei split aleatório com proporção 80% treino e 20% teste, com `random_state=42` para reprodutibilidade e `stratify=y` para manter a mesma proporção de detratores nos dois conjuntos.

## Modelos testados e métricas

Foram treinados dois modelos: Regressão Logística como baseline e Random Forest como modelo mais flexível. Ambos usaram `class_weight='balanced'` para lidar com o desbalanceamento da base.

A Regressão Logística teve AUC de 0,875 e acurácia de 80%, com recall de 79% para detratores e 80% para não-detratores. O Random Forest teve AUC de 0,868 e acurácia de 83%, com recall de 94% para detratores e 50% para não-detratores.

Os dois modelos têm desempenho similar em termos de capacidade de separação (AUC), mas com perfis diferentes. O Random Forest é melhor para capturar detratores (recall mais alto), o que é exatamente o que o negócio quer. A Regressão Logística é mais equilibrada entre as duas classes.

Para uso operacional, com foco em identificar detratores para ação preventiva, o Random Forest é a escolha mais adequada e foi o modelo salvo em `models/modelo_nps.pkl`.

## Como a empresa usaria isso na prática

Em produção, o modelo seria integrado ao sistema operacional da empresa. A cada pedido que alcança determinado estágio da jornada, por exemplo quando o prazo de entrega é iminente e já há sinal de atraso, o sistema alimentaria o modelo com os dados operacionais disponíveis até aquele momento. O modelo retornaria uma probabilidade de esse cliente se tornar detrator. Pedidos com probabilidade acima de um threshold definido pela operação, por exemplo 70%, entrariam em uma fila de atenção prioritária. O time de atendimento contataria proativamente esses clientes antes da pesquisa de NPS ser enviada, oferecendo suporte, informação sobre o pedido ou uma compensação quando aplicável.

Com o padrão identificado nos dados, especialmente a ruptura a partir de 3 dias de atraso e o comportamento dos contatos com atendimento, mesmo um modelo simples como Regressão Logística com as principais features já teria poder preditivo suficiente para começar a gerar valor operacional. Modelos mais complexos podem ser introduzidos em iterações posteriores, conforme a empresa acumula feedback de produção.
