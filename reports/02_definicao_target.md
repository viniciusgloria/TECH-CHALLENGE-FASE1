# Definição da Target

## Qual variável representa a satisfação do cliente?

A variável que representa a satisfação do cliente neste dataset é o `nps_score`, uma nota de 0 a 10 coletada após a conclusão da experiência de compra. O nome já indica a origem: é a nota que dá base ao cálculo do Net Promoter Score.

Para fins analíticos e, futuramente, para a construção de um modelo preditivo, essa variável pode ser usada de duas formas. Tratada como contínua, ela é uma nota que vai de 0 a 10, útil quando o objetivo é estimar diretamente a nota que um cliente provavelmente vai dar. Tratada como categórica, ela é transformada em três grupos seguindo a lógica do NPS tradicional: detratores (notas 0 a 6), neutros (notas 7 e 8), e promotores (notas 9 e 10). Essa segunda forma é útil quando o objetivo é classificar o cliente e acionar uma resposta operacional específica para cada grupo.

Nos dados analisados, a média do `nps_score` é 4,38 e a mediana é 4,4, ambas na faixa de detrator. Ao categorizar os 2.500 registros, encontramos cerca de 74% de detratores, 22% de neutros e apenas 4% de promotores. O NPS agregado da empresa nesta amostra é de -69,6.

## Por que essa variável foi escolhida?

O `nps_score` foi escolhido porque é o único indicador no dataset que captura diretamente a percepção do cliente sobre a experiência vivida. As demais variáveis registram eventos operacionais, como quanto tempo levou a entrega, quantas vezes o cliente ligou, quantas reclamações foram registradas. Essas são as causas. O `nps_score` é o efeito.

Além disso, o NPS é um padrão amplamente adotado no mercado para medir lealdade e satisfação. Isso significa que os resultados são comparáveis com benchmarks externos, o que não seria possível com métricas internas genéricas.

Existe também uma variável chamada `csat_internal_score` no dataset, que é um score interno de satisfação. Essa variável tem correlação alta com o `nps_score` (Spearman de 0,56), o que indica que os dois medem aspectos relacionados, mas não idênticos. O CSAT tende a capturar a satisfação imediata com uma interação específica, enquanto o NPS captura a disposição geral de recomendar, que é uma avaliação mais integrada da experiência. Para o objetivo deste projeto, que é antecipar a satisfação geral do cliente com a jornada de compra, o `nps_score` é a escolha mais adequada.

## Em que momento da jornada essa informação é coletada?

O `nps_score` é coletado após o encerramento da jornada de compra. Isso significa que o cliente já recebeu o pedido, já teve (ou não) alguma interação com o atendimento, e já foi exposto a tudo que a empresa entregou naquela experiência.

Esse timing é relevante porque cria uma assimetria. A empresa só sabe como foi a experiência depois que ela terminou. É exatamente essa limitação que motiva o projeto. Se fosse possível prever qual nota um cliente daria com base nos dados operacionais disponíveis durante a jornada, a empresa poderia agir antes, priorizando casos de risco e, possivelmente, revertendo uma experiência negativa enquanto ainda há tempo.

## Existe algum risco de usar essa variável de forma inadequada?

Alguns riscos merecem atenção.

O primeiro é o viés de resposta. Nem todos os clientes respondem à pesquisa de NPS. Quem responde pode ter perfil diferente de quem não responde. Clientes muito satisfeitos ou muito insatisfeitos têm mais incentivo para responder, o que pode distorcer a distribuição observada. O fato de aproximadamente 74% dos registros serem detratores pode refletir tanto a realidade da empresa quanto uma amostra com viés de resposta de clientes frustrados.

Outro risco é o target leakage. Algumas variáveis no dataset podem conter informações que, em um cenário real, só estariam disponíveis depois da compra ser concluída. Por exemplo, `csat_internal_score` pode ser coletado no mesmo momento que o `nps_score`. Se isso for verdade, usar essa variável como entrada de um modelo preditivo seria enganoso, porque o modelo aprenderia a usar uma resposta do cliente para prever outra resposta do mesmo cliente, algo que não aconteceria em produção. Antes de construir qualquer modelo, é necessário validar o timing exato de coleta de cada variável.

Sazonalidade e contexto externo também precisam ser considerados. O dataset representa um período específico de operação da empresa. Variações sazonais, como datas comemorativas, greves de transportadoras ou crises logísticas regionais, podem ter impacto significativo no NPS e não estão representadas de forma equilibrada dependendo do período amostrado.

Existe também o risco de generalização indevida. Os padrões encontrados refletem o perfil dos clientes e das operações da empresa no período analisado. Usar esses resultados para tomar decisões em contextos muito diferentes, como novos mercados geográficos ou novos segmentos de produto, sem validação adicional, carrega risco de erro.

Por fim, um detalhe técnico importante: o `nps_score` no dataset tem casas decimais (notas como 4,4; 6,9; 2,4), o que sugere que pode ser uma média ou um score calculado, e não a nota bruta da pesquisa, que tradicionalmente é um número inteiro de 0 a 10. Isso impacta a escolha do método de análise e do modelo. Uma nota contínua pode ser tratada como regressão, enquanto a categorização padrão NPS permite tratamento como classificação. Na categorização adotada, considero detrator quem tem nota igual ou abaixo de 6, mantendo a definição clássica do indicador.
