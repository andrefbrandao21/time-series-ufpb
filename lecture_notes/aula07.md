
## Aula 07: 11/06/2025

### Parte 1: Aprofundamento em Controle Sintético e a Extensão Quantílica

Esta seção da aula revisa os fundamentos do método de controle sintético e introduz uma extensão moderna para analisar os efeitos distribuicionais de uma intervenção.

#### 1. Revisão: O Método do Controle Sintético
* **Cenário de Aplicação:** O método é ideal para situações em que apenas uma unidade agregada (como um estado, cidade ou universidade) é "tratada" ou exposta a uma política, enquanto as outras unidades formam um grupo de controle. A presença de uma única unidade tratada e múltiplas unidades de controle é um forte indicativo para o uso desta metodologia.
* **Objetivo Central:** O grande desafio é prever o contrafactual, ou seja, o que teria acontecido com a unidade tratada se a política não tivesse sido implementada.
* **Efeito da Política:** O efeito da intervenção é calculado como a diferença entre a variável de resposta observada (após a política) e a previsão do seu contrafactual.
* **Dados Necessários:** O método requer dados de séries temporais para a variável de interesse (Y) e seus preditores (X). É crucial ter um período de tempo suficiente antes da intervenção (pré-tratamento) para construir um controle sintético robusto.

#### 2. A Estimação dos Pesos: Do Naive à Minimização de Distância
O desafio fundamental do controle sintético é estimar os pesos (W) a serem atribuídos às unidades do grupo de controle para criar a versão sintética da unidade tratada.
* **Método Naive 1 (Média Simples):** Atribuir o mesmo peso para todos os estados do grupo de controle.
* **Método Naive 2 (Ponderado pela População):** Atribuir pesos com base na população de cada unidade de controle em relação à população total do grupo de controle. Isso já representa um avanço, pois trata os estados de forma diferenciada.
* **Método Sofisticado (Minimização de Distância):** Esta é a abordagem padrão. Os pesos são escolhidos de forma a minimizar a distância (geralmente Euclidiana) entre o vetor de preditores (X) da unidade tratada e o vetor de preditores ponderado das unidades de controle, no período pré-intervenção. A ideia é encontrar a combinação de unidades de controle que mais se assemelha à unidade tratada antes do evento.

#### 3. Limitações do Método Tradicional
* **Inferência Estatística:** O método original de Abadie et al. fornecia apenas uma estimativa pontual do efeito da política (tau), sem um intervalo de confiança, o que dificultava a avaliação da significância estatística.
* **Foco no Efeito Médio:** A abordagem tradicional foca apenas no efeito médio da política sobre a variável de interesse (ex: o salário médio), não sendo capaz de capturar como a intervenção afeta diferentes partes da distribuição.

#### 4. Extensão: O Controle Sintético Quantílico para Análise Distribuicional
* **Motivação:** Toda política gera "vencedores e perdedores". O efeito agregado (na média) pode esconder impactos muito diferentes em subgrupos da população. É preciso analisar como a intervenção afeta toda a distribuição da variável de resposta.
* **Ideia Central:** Em vez de estimar o contrafactual da média, este método estima o contrafactual para cada quantil da distribuição. O quantil contrafactual da unidade tratada é modelado como uma média ponderada dos quantis correspondentes das unidades de controle.
* **Estimação dos Pesos:** A lógica é análoga, mas em vez de minimizar a distância entre vetores de preditores, minimiza-se a distância entre as funções de quantis.
* **Requisito de Dados:** Para calcular os quantis em cada unidade, o método exige dados granulares (microdados), como informações de salários individuais de pesquisas domiciliares (ex: PNAD).
* **Vantagens:** Ao estimar a densidade contrafactual completa, torna-se possível analisar o efeito da política sobre qualquer medida de distribuição, como o Índice de Gini, a desigualdade, ou o efeito em faixas de renda específicas.

---

### Parte 2: Estudo de Caso – O Impacto do Choque Migratório em Roraima

Esta seção aplica os conceitos de controle sintético (tradicional e quantílico) para analisar os efeitos do choque migratório venezuelano no estado de Roraima.

#### 1. Contexto da Pesquisa: A Migração Venezuelana
* **Choque Migratório Exógeno:** A partir de 2013-2015, uma crise econômica e social na Venezuela gerou uma emigração em massa, com o Brasil, e especialmente o estado de Roraima, sendo um dos principais destinos.
* **Isolamento de Roraima:** O isolamento geográfico de Roraima concentrou o impacto inicial do fluxo migratório, tornando-o um caso ideal para análise de choque exógeno.
* **Dados:** A pesquisa utiliza dados da PNAD Contínua para analisar variáveis do mercado de trabalho (salário e emprego) nos setores formal e informal.

#### 2. Metodologia Aplicada e Resultados Pontuais (Efeitos na Média)
Utilizando o método de controle sintético com uma abordagem Bayesiana para calcular os intervalos de confiança, o estudo encontrou os seguintes efeitos médios do choque migratório:
* **Salário Agregado:** Queda estatisticamente significante.
* **Emprego Formal:** Nenhum impacto estatisticamente significante.
* **Emprego Informal:** Aumento estatisticamente significante.
* **Conclusão Preliminar:** O ajuste do mercado de trabalho ao choque migratório ocorreu principalmente via setor informal.

#### 3. Resultados Distribuicionais (Efeitos nos Quantis e na Desigualdade)
O controle sintético quantílico é usado para contextualizar e explicar a queda do salário médio, revelando efeitos que estavam escondidos na análise agregada.
* **Trabalhadores de Baixa Renda:** Os efeitos adversos do choque se concentraram nos trabalhadores de baixa renda. Houve uma queda de salário mais forte para aqueles abaixo da mediana (ex: no quantil 25).
* **Trabalhadores de Alta Renda:** Não houve mudança significativa no salário para quem recebia acima da mediana.
* **Aumento da Desigualdade:** Como resultado, o choque migratório aumentou a desigualdade de renda em Roraima, o que foi confirmado pela análise do Índice de Gini contrafactual.

#### 4. Análise sobre Criminalidade e Conclusões Gerais
* **Criminalidade:** A análise também mostrou que o choque migratório esteve associado a um aumento nas taxas de crimes como homicídios e roubos em Roraima. O método, no entanto, não permite identificar se os crimes foram cometidos por imigrantes ou por nativos afetados economicamente pela crise.
* **Contribuição do Estudo:** O trabalho unifica um debate na literatura. Ao mostrar que a imigração teve um efeito negativo nos salários de baixa renda, mas não nos de alta renda, ele concilia estudos anteriores que encontravam resultados aparentemente contraditórios (alguns achavam efeito negativo, outros, nenhum efeito). A incorporação de dados do setor informal foi crucial para explicar os mecanismos de ajuste.