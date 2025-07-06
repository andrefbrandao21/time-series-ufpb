## Aula 04: Previsão Quantílica, VaR, CoVaR, Stress Test e Introdução à Previsão com Big Data/Machine Learning

### **Limitações da Previsão pela Média e Introdução à Previsão Quantílica**

* Quando há pouca incerteza sobre o valor futuro de $Y_{t+h}$ (a curva de densidade é estreita), a média condicional $E[Y_{t+h} | I_t]$ é uma boa representação para a previsão. 
* Em cenários de alta incerteza ou volatilidade (comuns em variáveis financeiras), a curva de densidade se alarga. Nesses casos, a média pode não ser suficiente, e o mercado passa a se interessar por previsões de eventos em outras partes da distribuição. 
* Surge o interesse em previsões do tipo: "Qual o valor $\hat{Y}$ tal que a probabilidade de $Y_{t+h}$ ser menor que $\hat{Y}$ seja de 5%?".  Isso leva diretamente ao conceito de quantil. 

### **Value at Risk (VaR)**

* O **Value at Risk (VaR)** representa a perda potencial máxima de um portfólio para um dado nível de probabilidade (ex: 5%) e um horizonte de tempo.
    * É uma medida de risco crucial para instituições financeiras.
* **Importância Regulatória:** O Banco Central utiliza o VaR para regular as instituições financeiras, exigindo que provisionem capital para cobrir essas perdas potenciais.  Quanto maior o VaR reportado, mais capital o banco precisa deixar imobilizado.
* **VaR como Previsão Quantílica:** O VaR nada mais é do que uma previsão do $\tau$-ésimo quantil da distribuição de retornos futuros do portfólio $Y_{t+h}$ (ou $R_{t+h}$), condicionado à informação $I_t$. 
    * Para uma probabilidade de 5%, o VaR corresponde ao quantil $\tau = 0.05$: 
        $$\text{VaR}_{t+h|t} = Q_{0.05}(Y_{t+h} | I_t)$$
       
* **Desafios na Fiscalização do VaR:**
    * Havia um incentivo para os bancos subestimarem o VaR para reduzir a necessidade de provisionamento de capital e aumentar a capacidade de investimento. 
    * A fiscalização dos modelos internos de VaR dos bancos pelo Banco Central era complexa, muitas vezes com assimetria de conhecimento entre a equipe do BC e os estatísticos dos bancos.
* **Teste de Adequação do VaR (Engel & Manganelli):**
    * Uma forma de testar se o VaR reportado por um banco é adequado, sem precisar auditar o modelo interno complexo do banco.
    * O Banco Central solicita duas séries temporais do banco comercial: a série dos retornos realizados do portfólio ($R_{t+h}$) e a série das previsões do VaR para o dia seguinte, feitas no dia $t$ (VaR$_{t+h|t}$). 
    * Estima-se uma regressão quantílica para o quantil $\tau=0.05$ (ou o nível do VaR): 
        $$Q_{0.05}(R_{t+h} | \text{VaR}_{t+h|t}) = \alpha_{0.05} + \beta_{0.05} \cdot \text{VaR}_{t+h|t}$$
       
    * **Hipótese Nula para um VaR bem especificado:** Se o modelo de VaR do banco é correto, espera-se que: 
        * $\alpha_{0.05} = 0$ 
        * $\beta_{0.05} = 1$ 
       
    * Se essas hipóteses não forem rejeitadas, o VaR do banco é considerado adequado.  Este teste simplifica a fiscalização. 
    * Graficamente, espera-se que o retorno do portfólio cruze (fique abaixo) da linha do VaR em aproximadamente 5% das vezes. 

### **CoVaR (Conditional Value at Risk) – Medida de Contágio**

* O CoVaR é uma medida de risco sistêmico que avalia como o VaR de uma instituição/mercado é afetado quando outra instituição/mercado está sob estresse. 
* **Exemplo:** Calcular o VaR do Ibovespa ($R_M$) condicionado a diferentes níveis de retorno da Vale ($R_i$). 
    * Estima-se uma regressão quantílica do retorno do Ibovespa sobre o retorno da Vale: 
        $$Q_{\tau}(R_{M,t} | R_{i,t}) = \alpha(\tau) + \beta(\tau) R_{i,t}$$
       
    * Avalia-se essa regressão para $\tau = 0.05$ (para obter o VaR do Ibovespa) em dois cenários para o retorno da Vale ($R_{i,t}$): 
        1.  **Cenário A (Normal):** $R_{i,t} = \text{Mediana}(R_{i,t})$ 
        2.  **Cenário B (Estresse):** $R_{i,t} = Q_{0.05}(R_{i,t})$ (o 5º percentil do retorno da Vale) 
    * O VaR do Ibovespa em cada cenário será: 
        * $\text{VaR}_{M|A} = \hat{\alpha}(0.05) + \hat{\beta}(0.05) \cdot \text{Mediana}(R_{i,t})$ 
        * $\text{VaR}_{M|B} = \hat{\alpha}(0.05) + \hat{\beta}(0.05) \cdot Q_{0.05}(R_{i,t})$ 
    * A diferença, $\Delta \text{CoVaR} = \text{VaR}_{M|A} - \text{VaR}_{M|B}$ (ou, mais diretamente, o impacto da mudança no retorno da Vale multiplicado por $\hat{\beta}(0.05)$) mede o contágio.
    * Se $\hat{\beta}(0.05)$ for estatisticamente igual a zero, conclui-se que não há contágio (a crise na Vale não afeta o VaR do Ibovespa). 
* **Aplicação Sugerida:** Analisar o efeito da flexibilização da Lei das Estatais no risco de contágio, comparando o CoVaR antes e depois da mudança, ou entre empresas afetadas e não afetadas.  Mudanças na regulação são oportunidades de pesquisa. 

### **Stress Test – Medida de Resiliência da Firma**

* O teste de estresse avalia a vulnerabilidade de uma firma específica a uma crise sistêmica no mercado. 
* Inverte-se a regressão anterior: a variável dependente é o retorno da firma ($R_i$, ex: Vale) e a independente é o retorno do mercado ($R_M$, ex: Ibovespa). 
    $$Q_{\tau}(R_{i,t} | R_{M,t}) = \alpha(\tau) + \beta(\tau) R_{M,t}$$
* Calcula-se o VaR da firma (para $\tau=0.05$) em dois cenários para o retorno do mercado ($R_{M,t}$):
    1.  **Cenário Normal:** $R_{M,t} = \text{Mediana}(R_{M,t})$ 
    2.  **Cenário de Estresse:** $R_{M,t} = Q_{0.05}(R_{M,t})$ (o 5º percentil do retorno do mercado)
* A mudança no VaR da firma devido à crise no mercado é dada pela diferença entre o VaR nos dois cenários (ou $\hat{\beta}(0.05)$ multiplicado pela queda no retorno do mercado). 
* Se $\hat{\beta}(0.05)$ for estatisticamente zero, a firma é considerada resiliente a crises de mercado.  Quanto maior o $\hat{\beta}(0.05)$, maior o estresse transmitido para a firma. 
* Pode-se usar para comparar a resiliência de diferentes firmas ou da mesma firma antes e depois de eventos como a flexibilização da Lei das Estatais. 

### **Previsão de Densidade (Density Forecasting)**

* Se é possível prever múltiplos quantis de uma distribuição futura $Y_{t+h}$ (usando regressão quantílica para diferentes valores de $\tau$), pode-se interpolar esses quantis para obter uma aproximação da densidade completa prevista $f(Y_{t+h}|I_t)$.
* Com a densidade prevista, pode-se calcular a probabilidade de qualquer evento. 
    * Exemplo: Dada a densidade prevista para a variação da taxa de câmbio ($\Delta S_{t+h}$), pode-se calcular a probabilidade de depreciação do real, $P(\Delta S_{t+h} > 0)$, integrando a área sob a curva da densidade para valores positivos. 
* Essas previsões de probabilidade podem ser usadas para tomar decisões de investimento (trading).
* Aplicações macroeconômicas incluem prever a probabilidade de recessão, como no paper de Adrian et al. (2019) sobre "Vulnerable Growth".

### **Previsão com Big Data e Introdução ao Machine Learning (Supervised Learning)**

* **Dilema Viés-Variância:** 
    * **Modelo Simples (Underfitting):** Ex: Regressão apenas com intercepto. A previsão é a média histórica. Tem baixa variância (previsão suave), mas alto viés (pode não capturar bem a dinâmica da série). 
    * **Modelo Complexo (Overfitting):** Ex: Regressão com muitos preditores, potencialmente um para cada observação. Tem baixo viés (dentro da amostra, ajusta-se bem aos dados), mas alta variância (desempenho ruim fora da amostra).
    * Machine Learning (Supervised) visa encontrar um modelo que otimize esse trade-off entre viés e variância. 
* **Desafio com Big Data:** Muitas vezes, o número de potenciais preditores ($K$) é maior que o número de observações ($T$), tornando a estimação por Mínimos Quadrados Ordinários (MQO) inviável. 
* **Resultado de Tibshirani (1996):** 
    * O Erro Quadrático Médio (MSE) de uma previsão está relacionado ao MSE do estimador dos coeficientes ($\hat{\beta}$) do modelo: 
        $$\text{MSE}(\text{previsão}) = E[(\hat{\beta}-\beta)^2] + \sigma^2_{\epsilon} = \text{MSE}(\hat{\beta}) + \sigma^2_{\epsilon}$$
        onde $\sigma^2_{\epsilon}$ é a variância irredutível do erro do modelo (ruído). 
    * Sabendo que $\text{MSE}(\hat{\beta}) = (\text{Viés}(\hat{\beta}))^2 + \text{Variância}(\hat{\beta})$. 
    * Para minimizar o MSE da previsão, é preciso encontrar um estimador $\hat{\beta}$ que minimize seu próprio MSE. Este estimador pode ser viesado, desde que a redução na variância compense o aumento no viés ao quadrado.
    * O objetivo do machine learning é encontrar um conjunto ótimo de $K^*$ preditores que minimize o MSE da previsão. 
* **Elastic Net:** Uma técnica de regularização para seleção de variáveis e estimação em contextos de Big Data. 
    * Minimiza a soma dos quadrados dos resíduos (termo de MQO) adicionando duas penalidades aos coeficientes $\beta_j$: 
        $$\min_{\beta_0, \beta} \left( \sum_{t=1}^{R} (Y_{t+h} - (\beta_0 + \sum_{j=1}^{K} \beta_j X_{j,t}))^2 \right) \text{ sujeito a:}$$
        1.  **Penalidade Lasso (L1):** $\sum_{j=1}^{K} |\beta_j| \le s_1$ [cite: 2]
        2.  **Penalidade Ridge (L2):** $\sum_{j=1}^{K} \beta_j^2 \le s_2$
    * **Casos Especiais:** 
        * **Ridge Regression ($s_1 \to \infty$):** Apenas a penalidade L2 é ativa.  Encolhe os coeficientes (shrinkage), útil para multicolinearidade, mas não zera coeficientes (não faz seleção de variáveis). 
        * **Lasso Regression ($s_2 \to \infty$):** Apenas a penalidade L1 é ativa.  Pode zerar coeficientes, realizando seleção de variáveis e encontrando um $K^*$.  Um problema é que se $K > T$, o Lasso seleciona no máximo $T$ variáveis. 
        * **Elastic Net ($s_1, s_2$ finitos):** Combina ambas as penalidades.  Faz seleção de variáveis e pode lidar com $K > T$ de forma mais eficaz que o Lasso puro, agrupando variáveis correlacionadas. 
* **Aplicação do Elastic Net em Dados Textuais:** 
    * Palavras ou n-gramas de documentos (ex: artigos de jornais) são transformadas em variáveis numéricas (ex: frequência, normalizada). 
    * Isso gera um número massivo de potenciais preditores ($K$ muito grande). 
    * Elastic Net é usado para selecionar o subconjunto de palavras ($K^*$) que são úteis para prever uma variável de interesse $Y$. 
    * O sinal do coeficiente $\beta_j$ associado a uma palavra pode indicar se ela carrega "notícia positiva" ou "negativa" para $Y$.  Palavras usadas aleatoriamente (ruído) terão coeficientes zerados. 
* **Elastic Net para Regressão Quantílica:** 
    * A função de perda quadrática no problema de minimização do Elastic Net é substituída pela "check function" (função perda quantílica), que depende do quantil $\tau$ de interesse. 
    * Isso permite identificar diferentes conjuntos de palavras ($K^*(\tau)$) que são relevantes para prever diferentes quantis da variável $Y$.
    * Exemplo Câmbio: Palavras ligadas à recessão nos EUA podem ser preditivas para o *downside risk* do dólar (apreciação do real, $\tau$ baixo), enquanto palavras sobre conflitos geopolíticos podem ser preditivas para o *upside risk* do dólar (depreciação do real, dólar como "safe haven", $\tau$ alto).
* **Teste da Metodologia com Dados Textuais:**
    * Criar "textos falsos" embaralhando aleatoriamente o conteúdo dos textos originais (destruindo a estrutura temporal e semântica). 
    * Se o modelo Elastic Net, aplicado a esses textos embaralhados, não encontrar poder preditivo significativo, isso valida a capacidade do modelo de distinguir informação de ruído. 