## Aula 02: 29/05/2025

### **Fundamentos da Teoria da Decisão e Utilidade**
* **Introdução:** A aula faz uma introdução à previsão usando a teoria econômica, focando em previsão e teoria da decisão.
* **Base Econômica:** Em economia, uma decisão é avaliada com base na utilidade que ela gera para o indivíduo.
    * Exemplos: Comprar um sorvete ou fazer um doutorado são decisões tomadas porque geram utilidade positiva, mesmo que essa utilidade seja sentida no futuro.
    * Consome-se um bem se ele proporciona utilidade positiva; não se consome se a utilidade é negativa.
* **Incerteza e Futuro:** A decisão de consumir ou comprar algo envolve incerteza sobre o que vai acontecer no futuro. O objetivo é maximizar a utilidade esperada.

### **O Papel da Previsão na Tomada de Decisão**
* **Consulta à Previsão:** Antes de tomar uma decisão que depende de um evento futuro incerto, consulta-se uma previsão.
    * Exemplo do Guarda-Chuva: A decisão de levar ou não um guarda-chuva depende da previsão do tempo. Se chover e levou, a decisão foi ótima; se não chover e levou, gerou desutilidade.
* **Avaliação da Decisão:** Só é possível saber se a decisão foi a melhor após o evento futuro ocorrer, comparando a decisão com o que de fato aconteceu.
* **Proximidade da Previsão à Realidade:** Quanto mais próxima a previsão estiver do que realmente acontecerá, melhores serão as decisões tomadas.

### **Formalização da Teoria da Decisão**
* **Função de Utilidade:** Um agente (indivíduo) possui uma função de utilidade U, que depende de duas variáveis: $X$ e $\alpha$.
    * **X:** Uma variável incerta no tempo $t$ (hoje), que só será conhecida/observada no futuro $(t+h)$.
    * **$\alpha$:** Uma ação ou decisão tomada no tempo $t$.
* **Escolha da Ação Baseada na Previsão:** O indivíduo escolhe a ação $\alpha$ no tempo T baseando-se numa previsão de $X$ (denotada como $X^{F}$).
    * $X^{F}$ é o valor esperado de $X$ em $t+h$, condicionado à informação disponível em $t$.
    
    $$X^F = E[X_{t+h} | I_t]$$

    * Essa informação pode ser, por exemplo, a previsão do tempo na televisão.
    * A decisão $\alpha$ é uma função da previsão XF.
* **Decisão Ótima:** A decisão ótima $\alpha^*$ é aquela que maximiza a utilidade esperada no tempo $t$, dada a previsão $X^F$.
* **Utilidade Ex-Post:** Em $t+h$, após $X$ ser observado, calcula-se a utilidade realizada (ex-post), que é $U(X, \alpha)$, onde $X$ é o valor realizado e $\alpha$ foi a ação tomada com base na previsão $X^F$.

### **Perda de Utilidade e Erro de Previsão**
* **Perda de Utilidade (Utility Loss):** É a diferença entre a utilidade que seria obtida com uma previsão perfeita (onde $X^F = X$) e a utilidade obtida com a previsão $X^F$ que foi de fato utilizada.
    * Utilidade com previsão perfeita: $U(X, \alpha(X))$ (decisão baseada no valor real de $X$).
    * Utilidade com a previsão $X^F: U(X, \alpha(X^F))$ (decisão baseada na previsão $X^F$, mas avaliada com o $X$ realizado).
    * Utility Loss = $U(X, \alpha(X)) - U(X, \alpha(X^F))$. 
* **Decisões Erradas e Previsões Erradas:** A lição principal é que decisões erradas são baseadas em previsões erradas.
* **Minimizando a Perda de Utilidade:** Para minimizar a perda de utilidade, é preciso minimizar o erro de previsão.
* **Erro de Previsão:** Definido como o valor realizado $(X)$ menos o valor previsto $(X^F)$. Esta é uma variável essencial do curso e será usada para comparar modelos.
* **Frustração:** A distância entre o que se espera $(X^F)$ e a realidade $(X)$ pode ser interpretada como frustração ("missed expectations"). Previsões descoladas da realidade levam a decisões que geram perda de utilidade.

### **Importância dos Fundamentos**
* É crucial manter a mente concentrada nos fundamentos da econometria e da teoria da decisão, em vez de apenas decorar conceitos.
* Saber responder e aplicar os fundamentos diferencia aqueles que realmente entendem a matéria.

### **Introdução à Geração de Previsões**
* Uma série temporal observada de um ponto inicial até o tempo $t$ é usada como base.
* O período futuro a ser previsto é $t+h$.
* Existe uma incerteza sobre o valor futuro da variável em $t+h$, representada por uma curva de densidade.

### **Tipos de Previsão**
* **Previsão Pontual:**
    * Inicialmente, a previsão é definida como a média da variável em $t+h$.
    * Essa média representa o elemento mais provável, o valor esperado condicional da variável, dada a informação disponível até o tempo $t$ (o "information set").
    * O "information set" deve incluir toda a informação relevante até o momento da previsão para evitar dependência e indicar que o modelo está bem especificado.
    * A previsão pontual, como por exemplo o dólar a 5,90, não associa a incerteza a esse valor.
* **Previsão Quantílica (ou de Densidade):**
    * Posteriormente, a ideia de que a melhor previsão é a esperança (média) será abandonada em favor da previsão do quantil.
    * Prever a "curva inteira" (a distribuição de densidade completa) permite calcular probabilidades de eventos específicos (ex: dólar ultrapassar R$6,00, ocorrência de uma recessão).
    * Essa abordagem é chamada de previsão quantílica e é vantajosa por permitir a construção de intervalos de confiança para a previsão pontual.
    * A aula focará inicialmente na previsão pontual (valor esperado).

### **Avaliação de Modelos de Previsão e Geração de Erros**
* Para avaliar modelos, é necessário gerar erros de previsão.
* Se queremos prever $h$ períodos à frente $(t+h)$, precisamos gerar $h$ valores de erro de previsão.
* **Metodologia "Pseudo Out-of-Sample Forecasting" (Previsão Fora da Amostra):**
    * A série temporal é dividida em duas partes: uma parte para estimação inicial (ex: de $1$ a $t$) e uma parte deixada "de fora" para avaliação (out-of-sample).
    * O modelo é estimado usando apenas a primeira parte da amostra (ex: de $1$ a $t$).
    * Usa-se o modelo estimado para prever a primeira observação fora da amostra $(t+1)$.
    * O erro de previsão para $t+1$ é calculado como a diferença entre o valor observado $(Y_{t+1})$ e o valor previsto $(\hat{Y}_{t+1})$.
    * O modelo é então reestimado, incluindo a observação $t+1$ na amostra de estimação (agora de $1$ a $t+1$).
    * Usa-se o modelo reestimado para prever $t+2$, e o processo se repete.
    * Este processo gera um vetor de erros de previsão fora da amostra, que será usado para comparar diferentes modelos (indexados por $i$).
    * Por exemplo, se tivermos um modelo AR(1) e um MA(1), todo o processo é feito para cada um, gerando um vetor de erros para cada modelo.
* O número de observações fora da amostra é $P$. Se a amostra total é 100, pode-se usar $R=60$ para estimar e $P=40$ para avaliação fora da amostra. A primeira estimação usa 60 observações, a segunda 61, e assim por diante, até a penúltima observação para prever a última.

### **Métricas Estatísticas para Comparação de Modelos (Fora da Amostra)**
* **Erro Quadrático Médio (MSE - Mean Squared Error):**
    * Calculado como:
    $$ \text{MSE} = \frac{1}{P} \sum_{t=1}^{P} (erro_t)^2 $$
    * Onde $P$ é o número de observações fora da amostra.
    * O melhor modelo é aquele com o menor MSE, pois indica menor erro.
* **Erro Absoluto Médio (MAE - Mean Absolute Error):**
    * Calculado usando o valor absoluto dos erros em vez do quadrado:
    $$ \text{MAE} = \frac{1}{P} \sum_{t=1}^{P} |erro_t| $$
    * É mais robusto a outliers, pois a função quadrática do MSE pode penalizar excessivamente um modelo por um único erro grande, mesmo que ele seja melhor na maioria dos períodos.
* **R² Fora da Amostra (Out-of-Sample R² ou OOS R²):**
    * Fórmula:
    $$\text{OOS R}^2 = 1 - \frac{\sum (erro_{\text{modelo } i})^2}{\sum (erro_{\text{modelo benchmark}})^2}$$
    * O somatório dos erros é feito sobre as observações fora da amostra.
    * Compara o modelo (modelo $i$) com um modelo de referência (benchmark), que pode ser um modelo ingênuo (ex: que prevê apenas a média histórica ou um modelo ARMA simples).
    * Se o OOS R² é próximo de zero, o modelo não prevê melhor que o modelo simples/benchmark.
    * Se o OOS R² é maior que zero, a previsão do modelo é melhor que a do modelo simples.

### **Valor Econômico da Previsão**
* Além da comparação estatística, os modelos podem ser comparados com base no valor econômico que suas previsões geram.
* **Exercício de Riqueza Acumulada:**
    * Participantes recebem um valor inicial (ex: $1) e devem prever o retorno de um ativo financeiro usando seus modelos.
    * Regra de decisão: Se a previsão do retorno em $t+1$ (feita em $t$) for positiva, compra-se o ativo. O resultado em $t+1$ será $1 \times (1 + r)$, onde $r$ é o retorno realizado.
    * Se prever queda, vende-se (ou fica-se de fora/vende a descoberto).
    * O exercício é repetido por um período determinado (ex: 30 dias, o período fora da amostra).
    * Ao final, plota-se a riqueza acumulada para cada modelo/participante. Um modelo que resulta em perda de valor não gerou riqueza. Um modelo que aumenta o valor inicial demonstra valor econômico.
* **Aplicação em Finanças (Portfólios Ativos):**
    * Em vez de um único ativo, pode-se prever o retorno de múltiplas empresas (cross-section de retornos).
    * Um portfólio ativo utiliza previsões para decidir quais ações comprar (previsão de retorno positivo) e quais vender/vender a descoberto (previsão de retorno negativo).
    * Isso é contrastado com um portfólio passivo (ex: index fund, que replica um índice como o Ibovespa).
    * Compara-se a riqueza acumulada por diferentes estratégias de portfólio ativo baseadas em diferentes modelos de previsão.
    * A literatura tem explorado o uso de **dados textuais** (de relatórios como 10-K) e **previsão quantílica** (para prever volatilidade) para melhorar a performance de portfólios.
    * Gráficos de riqueza acumulada de diferentes portfólios (alguns usando informação textual, outros não) ilustram o desempenho.

### **Considerações Finais e Próximos Passos**
* Previsão e decisão são duas partes da mesma moeda; a previsão alimenta a decisão.
* **Próxima Aula:** Testes de hipóteses para comparar modelos.
    * Mesmo que dois modelos tenham MSEs numericamente diferentes, é preciso testar se essa diferença é estatisticamente significativa.
    * Será abordado como testar a hipótese de que os MSEs de dois modelos são estatisticamente iguais.