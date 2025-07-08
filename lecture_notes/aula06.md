
## Aula 06: 10/06/2025

### Parte 1: Oficina de Código em R – Geração e Avaliação de Previsões Fora da Amostra

Esta parte da aula é dedicada a uma demonstração prática em R, mostrando o processo de geração de previsões fora da amostra de forma recursiva e a subsequente comparação de modelos. O código apresentado serve como base para exercícios e trabalhos de previsão.

#### 1. Configuração do Exercício e Dados
* **Geração de Dados:** Para o exercício, os dados não são reais, mas gerados pelo computador a partir de um processo autorregressivo de ordem 2 (AR(2)). Na prática, os alunos usarão bases de dados reais.
* **Divisão da Amostra (In-Sample e Out-of-Sample):**
    * A amostra total, com tamanho T, é dividida em duas partes.
    * Uma parte inicial para estimação, com R observações (ex: 800).
    * Uma parte final para avaliação, com P observações (ex: 200), que são deixadas "fora da amostra". A relação é $T = R + P$.

#### 2. O Loop Recursivo para Previsão
* A estimação recursiva é sempre implementada na forma de um `loop`.
* **Janela Recursiva:** O processo de estimação é recursivo, o que significa que a janela de dados usados para estimar o modelo aumenta a cada passo.
    * **Passo 1:** Estima-se o modelo usando os dados de 1 a R (ex: 1 a 800) e prevê-se a observação R+1 (801).
    * **Passo 2:** Reestima-se o modelo usando os dados de 1 a R+1 (1 a 801) e prevê-se a observação R+2 (802).
    * **Passo 3 em diante:** O processo continua, atualizando a amostra de estimação a cada passo, até que todas as P previsões fora da amostra sejam geradas.
* **Implementação no Loop:** O loop itera P vezes (o número de previsões a serem feitas), e a cada iteração i, a janela de dados para a estimação é atualizada.

#### 3. Geração de Previsões: Modelos e a Função `forecast`
* **Modelos de Previsão:** Vários modelos são estimados e comparados dentro da estrutura do loop:
    * **Modelo Naive:** Um modelo simples que contém apenas o intercepto (média histórica). Sua previsão tende a ser constante e suave ("smooth").
    * **Modelos ARIMA:** Modelos como AR(1), AR(2), MA(1), MA(2) e ARMA(1,1) são facilmente implementados na função `Arima`, apenas alterando os parâmetros de ordem. As previsões desses modelos apresentam mais variância que a do modelo Naive.
* **Função `forecast`:**
    * Após estimar um modelo, a função `forecast` é usada para gerar a previsão pontual.
    * A previsão ótima, neste contexto, é a média condicional ($E[Y_{t+h} | I_t]$), que minimiza o Erro Quadrático Médio (MSE).
    * O objeto retornado pela função `forecast` contém vários elementos, incluindo a previsão pontual, intervalos de confiança, etc.
    * Para extrair apenas a previsão pontual (a média), utiliza-se o operador `$` (ex: `resultado_forecast$mean`).

#### 4. Avaliação de Modelos: Métricas e Funções Customizadas
* **Erro de Previsão:** A variável fundamental para comparar a capacidade preditiva de modelos é o erro de previsão ($Y_{t+h} - \hat{Y}_{t+h|t}$). Uma matriz de erros é gerada, onde cada coluna representa os erros de um modelo específico para o período fora da amostra.
* **Criação de Funções em R:** Uma das vantagens do R é a capacidade de criar funções próprias para calcular métricas de avaliação.
    * **Função para RMSE (Raiz do Erro Quadrático Médio):** Uma função chamada `RMSE` é criada para calcular a raiz quadrada da média dos erros ao quadrado para cada modelo.
    * **Função para MAE (Erro Absoluto Médio):** De forma análoga, uma função para o MAE é criada, mudando apenas a operação interna de erro ao quadrado (`error^2`) para o valor absoluto do erro (`abs(error)`).
    * **Out-of-Sample R² (OOS R²):** Essa métrica compara o modelo de interesse (modelo J) com um modelo de referência (benchmark), que geralmente é o modelo Naive. A fórmula é:
        $$OOS \ R^2 = 1 - \frac{\sum (erro_{\text{modelo benchmark}})^2}{\sum (erro_{\text{modelo } J})^2}$$
        O código implementa isso usando a primeira coluna da matriz de erros (que corresponde ao modelo Naive) como o denominador.

#### 5. Combinação de Previsões
* **Motivação:** A combinação de previsões de diferentes modelos é uma estratégia para diversificar e reduzir o risco, de forma análoga à diversificação de portfólios em finanças.
* **Implementação:** Um novo vetor de previsões é criado simplesmente calculando a média, a cada período, das previsões geradas por todos os modelos individuais. Isso cria um novo "modelo combinado" que também pode ser avaliado. É possível também usar pesos diferentes para cada modelo na combinação.

---

### Parte 2: Introdução ao Método do Controle Sintético

Esta parte da aula introduz um método para avaliação de políticas públicas em cenários onde apenas uma unidade agregada (ex: um estado, uma cidade, um país) sofre uma intervenção.

#### 1. Motivação: Avaliação de Políticas e o Contrafactual
* **Problema Central:** Para avaliar o efeito de uma política pública, é necessário saber qual seria o valor de uma variável de interesse (ex: renda, criminalidade) se essa política não tivesse sido implementada.
* **Previsão do Contrafactual:** Esse cenário hipotético é chamado de **contrafactual**. O objetivo de métodos como o Controle Sintético é prever o contrafactual.
* **O Efeito da Política:** O efeito da intervenção é a diferença entre o valor observado na unidade que sofreu a política e o valor do seu contrafactual previsto.

    $$ \mathrm{Efeito}_{\mathrm{politica}} = Y_{\mathrm{observado}} - Y_{\mathrm{contrafactual\_previsto}} $$

#### 2. Estrutura dos Dados e Cenário de Aplicação
* O Controle Sintético é aplicado a um tipo específico de base de dados.
* **Uma Unidade Tratada:** Apenas uma unidade agregada (um estado, uma cidade, uma escola) é exposta à política ou intervenção. Esta é chamada de "unidade tratada".
* **Múltiplas Unidades de Controle:** As outras unidades que não sofreram a intervenção servem como o grupo de controle (ou "doadoras").
* **Dados em Painel:** A análise requer dados de séries temporais para todas as unidades, tanto para o período antes da intervenção (`pré-tratamento`) quanto para o período após a intervenção (`pós-tratamento`).

#### 3. Formalização e Variáveis
* **Unidades:** Assume-se um total de `J+1` unidades, onde a unidade 1 é a tratada (ex: o estado de Roraima sofrendo um choque migratório) e as unidades 2 a `J+1` são as unidades de controle (os outros estados brasileiros).
* **Variável de Interesse (Y):** É a variável cujo efeito da política se quer medir (ex: salário médio, taxa de desemprego, criminalidade). Ela deve ser observada em todas as unidades e em todos os períodos.
* **Preditores (X):** São variáveis que ajudam a prever a variável de interesse Y (ex: gastos com educação, número de universidades). Os mesmos preditores devem ser observados em todas as unidades. Lags da própria variável dependente também podem ser usados como preditores.

#### 4. Exemplos de Aplicação
O método do Controle Sintético é usado para responder a uma variedade de perguntas sobre o impacto de políticas e eventos únicos:
* **EUA:** O efeito de leis de porte de arma na Califórnia sobre crimes ou a legalização da prostituição em um estado sobre crimes contra a mulher.
* **Brasil:** O impacto do choque migratório de venezuelanos no estado de Roraima sobre variáveis socioeconômicas como salários e emprego.
* **Cuba:** Qual seria o estado das variáveis socioeconômicas de Cuba se a Revolução de 1959 não tivesse ocorrido? O método permite criar uma "Cuba Sintética" para comparação.
* **Argentina:** Qual o impacto da implementação de políticas de controle de capitais sobre o investimento no país?