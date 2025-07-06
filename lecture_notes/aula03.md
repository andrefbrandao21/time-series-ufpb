## Aula 03: Teste de Hipóteses para Comparação de Modelos e Introdução à Previsão Quantílica

### **Revisão e Introdução ao Teste de Hipóteses para Comparação de Modelos**

* Na aula passada, foram construídas medidas de comparação de modelos baseadas no erro de previsão.
* O erro de previsão, $e_{t+h|t} = Y_{t+h} - \hat{Y}_{t+h|t}$ (observado menos previsto), é a variável principal para essa análise.
* Um vetor de erros de previsão fora da amostra é gerado recursivamente: estima-se o modelo até $T$, prevê-se para $T+1$; reestima-se até $T+1$, prevê-se para $T+2$, e assim por diante.
* Com esse vetor de erros, constroem-se métricas como MSE (Erro Quadrático Médio) e MAE (Erro Absoluto Médio) para comparar diferentes modelos.
* **Problema:** Se as métricas (ex: MSE) de dois modelos são próximas, não se pode afirmar conclusivamente qual é o melhor apenas pelo valor numérico, pois estatisticamente podem ser iguais.
* **Objetivo da Aula:** Aprender a realizar testes de hipóteses para verificar se dois modelos têm, estatisticamente, a mesma capacidade preditiva, mesmo que suas métricas pontuais sejam diferentes.

### **Teste de Hipótese para Comparar a Capacidade Preditiva de Dois Modelos**

* Considera-se dois modelos (modelo 1 e modelo 2) que geram previsões ($\hat{Y}_{1t}$, $\hat{Y}_{2t}$) para a mesma variável ($Y_t$).
    * Exemplo: Modelo 1 pode ser um AR, e Modelo 2 um MA, ambos prevendo a inflação.
* Para cada modelo, obtém-se um vetor de erros de previsão fora da amostra ($\epsilon_{1t}$ e $\epsilon_{2t}$).
* **Função Perda (Loss Function):**
    * A acurácia da previsão de um modelo é medida por uma função perda, $g(Y_t, \hat{Y}_{it})$, onde $i$ denota o modelo.
    * Frequentemente, a função perda depende apenas do erro de previsão $e_{it} = Y_t - \hat{Y}_{it}$.
        * Em algumas áreas como meteorologia, a função perda pode depender também do valor efetivo da variável (ex: errar a categoria de um tornado forte é mais grave do que de um fraco). Em economia, geralmente foca-se no erro de previsão.
    * **Exemplos de Funções Perda:**
        * **Função Perda Quadrática:** $g(e_{it}) = e_{it}^2$. A média dessa função perda é o MSE.
            * Esta função é simétrica: penaliza erros positivos ($e_{it} > 0$) e negativos ($e_{it} < 0$) da mesma forma.
        * **Função Perda de Valor Absoluto:** $g(e_{it}) = |e_{it}|$. A média dessa função perda é o MAE.
            * Também é simétrica e mais robusta a outliers que a quadrática.
        * **Função Perda Assimétrica (Check Function/Pinball Loss Function - para Regressão Quantílica):**
            * Penaliza erros positivos e negativos de forma diferente.
            * Possui inclinação $\tau$ para erros positivos e $-(1-\tau)$ (ou $\tau-1$ no contexto da fala, dependendo da definição do erro) para erros negativos, onde $\tau \in (0,1)$ é o quantil de interesse.
            * Se $\tau = 0.5$, a função é simétrica e corresponde à perda por valor absoluto (relacionada à mediana).
            * Se $\tau = 0.9$, por exemplo, erros positivos (onde o valor realizado é maior que o previsto) são penalizados mais levemente que erros negativos (onde o valor realizado é menor que o previsto, dado $e_t = Y_t - \hat{Y}_t$), ou vice-versa dependendo da formulação exata da inclinação e do interesse do agente.
            * **Exemplo de Agente com Função Perda Assimétrica:** Um Banco Central com meta de inflação. Pode ter interesse em penalizar mais fortemente o erro de prever a inflação muito acima do que ela efetivamente será (erro negativo, $Y_t < \hat{Y}_t$), pois isso levaria a uma política monetária desnecessariamente contracionista. Ou, inversamente, pode penalizar mais o erro de prever abaixo (erro positivo, $Y_t > \hat{Y}_t$) se o objetivo principal é evitar surpresas inflacionárias.
            * Funções perda assimétricas levam a previsões ótimas que não são a média, mas sim um quantil específico da distribuição. Um Banco Central pode, por exemplo, sistematicamente publicar previsões "otimistas" (abaixo da média) se sua função perda assimétrica o induzir a isso.

* **Construção do Teste de Comparação (ex: Diebold-Mariano):**
    * Define-se uma série de diferenças das perdas: $D_t = g(Y_t, \hat{Y}_{1t}) - g(Y_t, \hat{Y}_{2t})$. Para cada período $t$ na amostra de avaliação, calcula-se essa diferença.
    * **Hipótese Nula ($H_0$):** Os dois modelos têm a mesma capacidade preditiva. Matematicamente, $E[D_t] = 0$. Isso significa que, na média, as perdas são iguais.
    * **Hipótese Alternativa ($H_1$):** As capacidades preditivas são diferentes, $E[D_t] \neq 0$.
        * Se $E[D_t] > 0$, significa que a perda do modelo 1 é, na média, maior que a do modelo 2, então o modelo 2 é melhor.
        * Se $E[D_t] < 0$, o modelo 1 é melhor.
    * **Estatística de Teste:** Semelhante a um teste $t$ para a média de $D_t$.
        * Calcula-se a média amostral $\bar{D} = \frac{1}{P} \sum_{t=1}^{P} D_t$, onde P é o número de previsões fora da amostra.
        * A estatística de teste é 
        $$T_{\text{stat}} = \frac{\bar{D}}{\text{std\_err}(\bar{D})}$$.

        * **Desafio em Séries Temporais:** A série $D_t$ pode apresentar autocorrelação (memória), o que viola a suposição de independência dos testes $t$ tradicionais.
        * O cálculo do erro padrão de $\bar{D}$ deve levar em conta essa autocorrelação usando a **Variância de Longo Prazo (Long-Run Variance - LRV)** de $D_t$.
        * **Estimador de Newey-West para a LRV:** Uma forma de estimar a LRV é:
            $$ \hat{\text{LRV}}(\bar{D}) = \frac{1}{P} \left( \hat{\gamma}_0 + 2 \sum_{k=1}^{M_P} w_k \hat{\gamma}_k \right) $$
            Onde $\hat{\gamma}_k = \text{Cov}(D_t, D_{t-k})$ é a k-ésima autocovariância amostral de $D_t$.
            Os pesos $w_k = 1 - \frac{k}{M_P+1}$ (pesos de Bartlett) dão menos importância a autocovariâncias de lags maiores.
            $M_P$ é um parâmetro de truncamento, que pode ser escolhido, por exemplo, como $M_P \approx \text{integer}[4(P/100)^{2/9}]$.
            Se $D_t$ fosse IID (sem memória), apenas $\hat{\gamma}_0$ (a variância amostral) seria considerada no denominador.
    * **Decisão do Teste:**
        * Compara-se o valor absoluto da estatística $T_{stat}$ calculada com um valor crítico da distribuição normal padrão (ex: 1.96 para um teste bilateral com 5% de nível de significância).
        * Se $|T_{stat}| > 1.96$, rejeita-se $H_0$, concluindo que há diferença estatisticamente significativa na capacidade preditiva dos modelos.
        * O sinal de $\bar{D}$ (e consequentemente de $T_{stat}$) indica qual modelo é superior. Se $T_{stat} = 2.5$ (positivo e > 1.96), rejeita-se $H_0$ e o modelo 2 é melhor (pois $D_t$ foi definido como perda do modelo 1 menos perda do modelo 2).
        * Analogia com um julgamento: $H_0$ é a presunção de inocência (mesma capacidade preditiva). Rejeita-se $H_0$ apenas se a evidência (estatística de teste) for suficientemente forte para cair na região crítica (extremos da distribuição).
        * Linguagem correta: Não se "aceita" $H_0$. Diz-se "não há evidência suficiente para rejeitar $H_0$".

### **Introdução à Previsão Quantílica**

* A previsão quantílica é um tema central na pesquisa do professor.
* O objetivo é transitar da ideia de previsão ótima como esperança condicional (para funções perda quadráticas) para a previsão baseada em regressão quantílica.
* **Nowcasting (Previsão para o Presente, H=0):**
    * Situação onde se prevê o valor de uma variável para o período corrente (T), cujo valor oficial só será divulgado no futuro.
    * Exemplo: Prever o PIB do primeiro trimestre no final do próprio primeiro trimestre, usando indicadores de alta frequência (mensais, diários como produção de papelão, consumo de energia) que já estão disponíveis, antes da divulgação oficial do PIB (que ocorre, por exemplo, 45 dias depois).
    * Utiliza modelos como MIDAS (Mixed Data Sampling) que lidam com dados de frequências mistas (ex: prever variável trimestral com dados mensais/diários).
    * Outra aplicação: prever uma previsão que ainda não é pública (ex: "Green Book Forecast" do Federal Reserve, que é divulgado com anos de defasagem).
* **Previsão Ótima e Função Perda (Revisão):**
    * A previsão ótima ($Y^*_{t+h|t}$) é aquela que minimiza a esperança da função perda, condicional à informação $I_t$.
    * Se a função perda é **quadrática**, a previsão ótima é a **esperança condicional**: $Y^*_{t+h|t} = E[Y_{t+h} | I_t]$.
    * Se a função perda é a **assimétrica "check function"** (associada à regressão quantílica), a previsão ótima é o **quantil condicional**: $Y^*_{t+h|t} = Q_{\tau}(Y_{t+h} | I_t)$.
    * Portanto, ao se perguntar "Qual a previsão ótima?", deve-se primeiro perguntar "Qual a sua função perda?". As seguradoras, por exemplo, não usam função perda quadrática, pois estão interessadas em eventos extremos, não na média.
* **Conceito de Quantil:**
    * Dada uma variável aleatória $\epsilon$ com Função de Distribuição Acumulada (CDF) $F(\cdot)$.
    * A CDF $F(c)$ fornece a probabilidade $P(\epsilon \le c)$.
    * O $\tau$-ésimo quantil de $\epsilon$, denotado por $c_{\tau}$ (ou $q_{\tau}$), é o valor tal que $F(c_{\tau}) = \tau$, ou $c_{\tau} = F^{-1}(\tau)$.
    * Se $F(c) = 0.5$, $c$ é a mediana (quantil 0.5).
    * Se $F(c) = 0.75$, $c$ é o quantil 0.75 (75º percentil), significando que $P(\epsilon \le c) = 0.75$.
* **Regressão Quantílica:**
    * Considerando o modelo de regressão linear: $Y_i = \beta_0 + \beta_1 X_i + \epsilon_i$, onde $\epsilon_i$ é IID com média zero e variância $\sigma^2_{\epsilon}$.
    * **Caso Homoscedástico:** A variância do erro (e de Y dado X) é constante: $\text{Var}(Y_i|X_i) = \sigma^2_{\epsilon}$.
        * A média condicional é $E[Y_i|X_i] = \beta_0 + \beta_1 X_i$.
        * O $\tau$-ésimo quantil condicional de $Y_i$ dado $X_i$ é $Q_{\tau}(Y_i|X_i) = (\beta_0 + F^{-1}_{\epsilon}(\tau)) + \beta_1 X_i$.
        * Note que o coeficiente angular $\beta_1$ é o mesmo da média condicional. Apenas o intercepto $(\beta_0 + F^{-1}_{\epsilon}(\tau))$ varia com $\tau$.
        * Graficamente, as retas dos diferentes quantis condicionais são paralelas à reta da média condicional.
        * Em modelos homoscedásticos, onde a dispersão dos dados é constante, a regressão quantílica oferece pouca informação adicional em relação à regressão pela média (OLS), pois a média já representa bem a distribuição condicional.
    * **Caso Heteroscedástico:** A variância de $Y_i$ dado $X_i$ não é constante (ex: a dispersão dos dados aumenta com $X_i$).
        * Neste caso, a média condicional $E[Y_i|X_i]$ não representa adequadamente todos os indivíduos ou a distribuição em diferentes níveis de $X_i$.
        * A regressão quantílica torna-se mais útil: $Q_{\tau}(Y_i|X_i) = \beta_0(\tau) + \beta_1(\tau) X_i$. Agora, tanto o intercepto $\beta_0(\tau)$ quanto o coeficiente angular $\beta_1(\tau)$ podem variar com o quantil $\tau$.
        * A regressão quantílica permite modelar como diferentes quantis da distribuição condicional de Y mudam com X, capturando a heterogeneidade dos dados.