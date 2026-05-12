# Prompt para Geração dos Slides no Gamma

Cole o conteúdo abaixo no Gamma (modo "Generate from text" ou "Paste content").
Após gerar, ajuste o tema visual, revise os textos e exporte como PDF.
Salve o PDF como `slides_executivos.pdf` nesta mesma pasta.

---

Crie uma apresentação profissional de 10 slides chamada "NPS Preditivo: O que os dados revelam sobre a satisfação dos clientes". O público é de gestores e líderes de negócio, sem background técnico. O tom deve ser direto e executivo. Use linguagem simples. Sem jargão estatístico.

---

SLIDE 1: CAPA
Título: NPS Preditivo
Subtítulo: O que os dados revelam sobre a satisfação dos clientes
Contexto: E-commerce | Análise de Dados Operacionais | Tech Challenge Fase 1

---

SLIDE 2: O PROBLEMA
Título: O NPS chega tarde demais

A empresa coleta o NPS apenas depois que a jornada de compra termina. Quando o resultado aparece, a oportunidade de agir já passou.

Resultado atual (amostra de 2.500 pedidos):
- NPS da empresa: -69,6
- 84% dos clientes são detratores
- Apenas 4% são promotores

---

SLIDE 3: A PERGUNTA
Título: O que precisamos saber

Pergunta central em destaque:
"Quais fatores operacionais disponíveis durante a jornada explicam a satisfação do cliente?"

Objetivo: agir antes da pesquisa ser enviada.

---

SLIDE 4: A BASE DE DADOS
Título: De onde vieram os insights

Analisamos 2.500 pedidos históricos com 18 variáveis operacionais por pedido:
- Dados do pedido: valor, itens, forma de pagamento
- Dados de logística: tempo de entrega, dias de atraso, tentativas
- Dados de atendimento: contatos, reclamações, tempo de resolução
- Nota NPS do cliente ao final da compra

---

SLIDE 5: PRINCIPAL ACHADO
Título: O atraso na entrega é o maior vilão

A correlação entre atraso e NPS é a mais forte de toda a análise.

Situação | NPS médio
Entregue no prazo | 6,9
1 a 2 dias de atraso | 5,1
3 a 5 dias de atraso | 2,9
6 ou mais dias de atraso | 0,8

Destaque: o tempo total de entrega não importa. O que importa é cumprir o prazo prometido.

---

SLIDE 6: OUTROS FATORES CRÍTICOS
Título: Atendimento e reclamações amplificam a insatisfação

Comparando detratores e promotores:
- Reclamações: detratores acumulam em média 4,4 | promotores, 2,3
- Contatos com atendimento: detratores fazem em média 1,6 | promotores, 0,7

Cada contato adicional com o atendimento sem resolução reduz mais o NPS.
Cliente que nunca precisou ligar: NPS médio de 5,5.
Cliente que ligou 5 vezes: NPS médio de 2,2.

---

SLIDE 7: IMPACTO NA RECEITA
Título: Satisfação não é só imagem. É receita.

Taxa de recompra em 30 dias:
- Promotores: 100%
- Neutros: 38%
- Detratores: 0%

Nenhum cliente insatisfeito voltou a comprar no período analisado.
Cada experiência ruim representa a perda de um cliente e de toda a receita futura que ele geraria.

---

SLIDE 8: RECOMENDAÇÕES PRÁTICAS
Título: O que fazer agora

1. Monitoramento de prazo em tempo real: identificar pedidos em risco antes do prazo estourar.
2. Comunicação proativa: avisar o cliente sobre atrasos antes que ele perceba sozinho.
3. Priorização no atendimento: clientes com mais de um contato sem resolução devem ser atendidos com urgência.
4. Confiabilidade acima de velocidade: cumprir o prazo prometido vale mais do que entregar rápido e atrasar.

---

SLIDE 9: PRÓXIMO PASSO ESTRATÉGICO
Título: Prevendo a insatisfação antes que ela aconteça

Os dados operacionais que já existem permitem construir um modelo que preveja quais clientes estão em risco de virar detratores durante a jornada de compra.

Com isso, a empresa pode:
- Criar uma fila de atenção prioritária para pedidos de alto risco
- Acionar o atendimento antes da pesquisa ser enviada
- Transformar potenciais detratores em clientes recuperados

---

SLIDE 10: LIMITAÇÕES E CUIDADOS
Título: O que esta análise não resolve sozinha

- A amostra pode ter viés: clientes insatisfeitos respondem pesquisas com mais frequência.
- Correlação não é causalidade: os padrões encontrados indicam associação, mas validações adicionais são necessárias antes de decisões estruturais.
- Sazonalidade: os resultados refletem um período específico e podem variar em datas de alta demanda.
- Antes de implementar um modelo em produção, validar com dados de outros períodos é essencial.
