Com base nas transcrições da aula do dia 25 de junho de 2025, aqui estão as notas de aula, elaboradas no formato solicitado.

---

## Aula 09: 25/06/2025

### **Parte 1: Critérios de Seleção de Modelos e Introdução à Causalidade em Macroeconomia**

Esta seção aborda os critérios de informação para seleção de modelos aninhados e estabelece a ponte entre a análise de causalidade em microeconomia e os desafios da macroeconomia.

#### **1. Seleção de Modelos Aninhados: AIC vs. BIC**
Para comparar modelos que são "aninhados" (ou seja, um modelo mais simples M1 é um caso especial de um modelo mais complexo M2), critérios de informação como o AIC e o BIC podem ser utilizados. Isso é comum na família de modelos ARIMA.

* **Akaike Information Criterion (AIC):**

    * **Fórmula:** $AIC_M = \log(\hat{\sigma}^2_M) + \frac{2K_M}{N}$, onde $\hat{\sigma}^2_M$ é a variância residual do modelo M, $K_M$ é o número de parâmetros estimados e N é o tamanho da amostra. É crucial que o tamanho da amostra N seja o mesmo para todos os modelos comparados.
    * **Interpretação:** O critério representa um trade-off. O primeiro termo ($\log(\hat{\sigma}^2_M)$) melhora com a adição de regressores (a variância diminui), enquanto o segundo termo ($\frac{2K_M}{N}$) atua como uma penalidade por aumentar a complexidade do modelo.
    * **Regra de Decisão:** Escolhe-se o modelo que apresentar o menor valor de AIC.
    * **Propriedade de Consistência:** O AIC não é consistente. Isso significa que, mesmo com uma amostra muito grande, existe uma probabilidade positiva de que o critério escolha o modelo incorreto.

* **Bayesian Information Criterion (BIC) ou Schwarz Criterion (SC):**

    * **Fórmula:** $BIC_M = \log(\hat{\sigma}^2_M) + \frac{\log(N) \cdot K_M}{N}$.
    * **Diferença Chave:** A penalidade do BIC, $\log(N)$, é maior que a penalidade do AIC (2) para amostras com $N \ge 8$. Isso faz com que o BIC penalize modelos maiores de forma mais rigorosa.
    * **Implicação:** O BIC tende a escolher modelos mais parcimoniosos (com menos parâmetros) em comparação ao AIC.
    * **Propriedade de Consistência:** O BIC é consistente. A probabilidade de ele selecionar o modelo verdadeiro se aproxima de 1 à medida que o tamanho da amostra N tende ao infinito.

#### **2. A Limitação dos Critérios de Informação e a Motivação para Machine Learning**

* **Explosão Combinatória:** Os critérios AIC e BIC são computacionalmente viáveis para comparar modelos aninhados e ordenados. No entanto, para modelos não aninhados, o número de combinações possíveis de regressores cresce exponencialmente ($2^K$). Com $K=10$ regressores, já existem 1024 modelos possíveis para testar, tornando a abordagem impraticável.
* **Solução com Machine Learning:** Essa limitação computacional motivou o desenvolvimento de métodos de *supervised machine learning* como o Lasso e o Elastic Net, que conseguem selecionar a especificação correta do modelo de forma eficiente mesmo em cenários com um número imenso de regressores.

#### **3. Causalidade: Da Microeconomia (RCT) à Macroeconomia (Choques)**

* **Causalidade em Micro vs. Macro:** Em microeconometria, a causalidade é frequentemente estudada pelo efeito de um programa ou política sobre indivíduos. Em macroeconomia, o análogo a um programa é um "choque": um evento novo e inesperado que afeta a economia.
* **Exemplo de Choque Macroeconômico:** Um aumento na taxa de juros pelo Banco Central que é maior do que o mercado esperava.
* **O Padrão-Ouro: Randomized Control Trials (RCTs):**
    * O RCT é a forma mais robusta de identificar efeitos causais. Nela, indivíduos são sorteados aleatoriamente para um grupo de tratamento e um grupo de controle.
    * Como a alocação é aleatória, os dois grupos são idênticos em média, exceto pela exposição ao tratamento.
    * O Efeito Médio do Tratamento (ATE) é simplesmente a diferença entre a média do resultado no grupo de tratamento e a média no grupo de controle.
    * Este efeito pode ser estimado através de uma simples regressão linear: $Y = \beta_0 + \beta_1 X + \epsilon$, onde X é uma dummy de tratamento e o coeficiente $\beta_1$ representa o ATE.
* **Análise de Impulso-Resposta:** O objetivo em macroeconomia é conduzir uma Análise de Impulso-Resposta, que é o equivalente macroeconômico do ATE. Ela mede como as variáveis macroeconômicas (inflação, desemprego) respondem ao longo do tempo a um choque inicial.

#### **4. O Desafio em Macroeconomia: Estimação de Choques**
* **Diferença Fundamental do RCT:** Em um RCT, o "tratamento" é observado e conhecido. Em macroeconomia, o choque não é diretamente observado e precisa ser estimado a partir dos dados.
* **Abordagens Principais:** Duas abordagens dominam a literatura para estimar choques e calcular as funções de impulso-resposta:
    1.  Vetores Autorregressivos Estruturais (SVAR).
    2.  Projeções Locais (Local Projections).
O restante da aula foca na abordagem SVAR.

---

### **Parte 2: O Modelo de Vetores Autorregressivos (VAR)**
Esta seção introduz formalmente o modelo VAR, diferenciando sua forma estrutural de sua forma reduzida, e apresenta o problema central de identificação.


### **1. O VAR Estrutural (SVAR): Contemporaneidade e Endogeneidade**

A análise começa com a definição do modelo que representa a teoria econômica, o VAR Estrutural.

* **Estrutura Matemática:** Para um sistema com duas variáveis ($y_1$ e $y_2$) e uma defasagem, o SVAR é representado por um sistema de duas equações lineares:
    $$y_{1,t} = \gamma_{10} - \beta_{12}y_{2,t} + \gamma_{11}y_{1,t-1} + \gamma_{12}y_{2,t-1} + \epsilon_{1,t}$$ $$y_{2,t} = \gamma_{20} - \beta_{21}y_{1,t} + \gamma_{21}y_{1,t-1} + \gamma_{22}y_{2,t-1} + \epsilon_{2,t}$$

* **Efeitos Contemporâneos:** A característica que define o modelo como "estrutural" é a presença dos termos $y_{1,t}$ e $y_{2,t}$ no lado direito das equações. Os coeficientes $\beta_{12}$ e $\beta_{21}$ medem o impacto instantâneo (contemporâneo) de uma variável sobre a outra.

* **Endogeneidade:** A presença desses efeitos contemporâneos cria um problema de simultaneidade ou endogeneidade. Como $y_{1,t}$ afeta $y_{2,t}$ e vice-versa, os regressores contemporâneos ($y_{1,t}$ e $y_{2,t}$) são correlacionados com os termos de erro ($\epsilon_{1,t}$ e $\epsilon_{2,t}$), violando uma premissa fundamental do método de Mínimos Quadrados Ordinários (MQO). Portanto, a estimação direta desses coeficientes por MQO resultaria em estimadores viesados e inconsistentes.

* **Choques Estruturais ($\epsilon_t$):** Em forma matricial, o sistema acima é escrito como:
    $$\begin{pmatrix} 1 & \beta_{12} \\ \beta_{21} & 1 \end{pmatrix} \begin{pmatrix} y_{1t} \\ y_{2t} \end{pmatrix} = \begin{pmatrix} \gamma_{10} \\ \gamma_{20} \end{pmatrix} + \begin{pmatrix} \gamma_{11} & \gamma_{12} \\ \gamma_{21} & \gamma_{22} \end{pmatrix} \begin{pmatrix} y_{1,t-1} \\ y_{2,t-1} \end{pmatrix} + \begin{pmatrix} \epsilon_{1t} \\ \epsilon_{2t} \end{pmatrix}$$
    Ou, de forma compacta:
    $$B Y_t = \Gamma_0 + \Gamma_1 Y_{t-1} + \epsilon_t$$
    Os choques estruturais no vetor $\epsilon_t$ são, por definição teórica, não correlacionados entre si. Isso significa que sua matriz de covariância, $\Sigma_\epsilon$, é diagonal:
    $$\Sigma_\epsilon = E[\epsilon_t \epsilon_t'] = \begin{pmatrix} \sigma^2_{\epsilon_1} & 0 \\ 0 & \sigma^2_{\epsilon_2} \end{pmatrix}$$

### **2. O VAR na Forma Reduzida: A Solução para Estimação**

Para contornar o problema da endogeneidade, o SVAR é algebricamente manipulado para sua forma reduzida.

* **Derivação Matemática:** Assumindo que a matriz $B$ é invertível, pré-multiplicamos a equação do SVAR por $B^{-1}$:
    $$B^{-1}B Y_t = B^{-1}\Gamma_0 + B^{-1}\Gamma_1 Y_{t-1} + B^{-1}\epsilon_t$$ $$Y_t = A_0 + A_1 Y_{t-1} + e_t$$
    Onde os novos coeficientes e o novo termo de erro são definidos como:
    * $A_0 = B^{-1}\Gamma_0$ (vetor de interceptos da forma reduzida)
    * $A_1 = B^{-1}\Gamma_1$ (matriz de coeficientes da forma reduzida)
    * $e_t = B^{-1}\epsilon_t$ (vetor de erros da forma reduzida)

* **Resultado e Estimação:** Na forma reduzida, o lado direito de cada equação contém apenas variáveis pré-determinadas (constantes e defasagens de $Y_t$). A endogeneidade simultânea foi eliminada, e o sistema pode ser estimado consistentemente por MQO, equação por equação.

* **Choques da Forma Reduzida ($e_t$):** Os erros da forma reduzida, $e_t$, são combinações lineares dos choques estruturais $\epsilon_t$. Sua matriz de covariância, $\Omega$, é derivada da seguinte forma:
    $$\Omega = E[e_t e_t'] = E[(B^{-1}\epsilon_t)(B^{-1}\epsilon_t)'] = B^{-1} E[\epsilon_t \epsilon_t'] (B^{-1})' = B^{-1} \Sigma_\epsilon (B^{-1})'$$
    Como $\Sigma_\epsilon$ é diagonal, mas $B^{-1}$ geralmente não é, a matriz resultante $\Omega$ não será diagonal. Isso significa que os erros da forma reduzida, $e_{1t}$ e $e_{2t}$, são correlacionados, e um choque em $e_{1t}$ não pode ser interpretado como um choque "puro" na primeira variável.

### **3. O Problema de Identificação: Contando Parâmetros**

A capacidade de estimar a forma reduzida não garante que possamos conhecer os parâmetros estruturais de interesse.

* **O Dilema:** O objetivo é estimar os parâmetros do SVAR ($B, \Gamma_0, \Gamma_1, \Sigma_\epsilon$), mas só podemos estimar diretamente os parâmetros da forma reduzida ($A_0, A_1, \Omega$). A questão é se podemos usar os parâmetros estimados para recuperar os estruturais.

* **Contagem de Parâmetros:**
    * **Parâmetros Estruturais (10 incógnitas):**
        * Matriz $B$ (Efeitos Contemporâneos): 2 parâmetros ($\beta_{12}, \beta_{21}$).
        * Matriz $\Gamma_0$ (Interceptos Estruturais): 2 parâmetros ($\gamma_{10}, \gamma_{20}$).
        * Matriz $\Gamma_1$ (Coeficientes Defasados): 4 parâmetros ($\gamma_{11}, \gamma_{12}, \gamma_{21}, \gamma_{22}$).
        * Matriz $\Sigma_\epsilon$ (Variâncias dos Choques): 2 parâmetros ($\sigma^2_{\epsilon_1}, \sigma^2_{\epsilon_2}$).
        * **Total = 10 parâmetros estruturais**.
    * **Parâmetros da Forma Reduzida (9 Estimáveis):**
        * Vetor $A_0$ (Interceptos): 2 parâmetros.
        * Matriz $A_1$ (Coeficientes Defasados): 4 parâmetros.
        * Matriz $\Omega$ (Covariância dos Erros): 3 parâmetros únicos ($\omega_{11}, \omega_{22}, \omega_{12}=\omega_{21}$, pois a matriz é simétrica).
        * **Total = 9 parâmetros estimáveis**.

* **Conclusão:** Temos **10 incógnitas** (parâmetros estruturais), mas apenas **9 equações** (parâmetros conhecidos da forma reduzida) para resolvê-las. O sistema é **sub-identificado**. É matematicamente impossível encontrar uma solução única.

* **Solução:** Para resolver o sistema, precisamos de pelo menos uma equação adicional. Essa equação vem de uma **restrição de identificação** — uma hipótese baseada na teoria econômica (ou em uma convenção estatística) que fixa o valor de um dos parâmetros estruturais (ex: $\beta_{12}=0$), permitindo a solução para os demais.

### **4. As Quatro Representações do VAR e o Caminho para a Função de Impulso-Resposta**

O caminho da estimação para a análise de política passa por quatro representações matemáticas do mesmo sistema.

1.  **SVAR (Estrutural):** $B Y_t = \Gamma_0 + \Gamma_1 Y_{t-1} + \epsilon_t$. O modelo teórico.
2.  **VAR (Forma Reduzida):** $Y_t = A_0 + A_1 Y_{t-1} + e_t$. O modelo estimável.
3.  **VMA (Vetorial de Médias Móveis):** Qualquer VAR estacionário pode ser invertido para uma representação VMA infinita. Usando o operador de defasagem $L$:
    $$(I - A_1 L)Y_t = A_0 + e_t \implies Y_t = (I - A_1 L)^{-1}A_0 + (I - A_1 L)^{-1}e_t$$
    Isso pode ser escrito como:
    $$Y_t = \mu + \sum_{k=0}^{\infty} A_1^k e_{t-k} = \mu + \Psi(L)e_t$$
    Esta forma mostra como $Y_t$ é uma função de todos os choques passados da forma reduzida. Os coeficientes $\Psi_k = A_1^k$ são as matrizes do VMA.
4.  **SVMA (VMA Estrutural):** Esta é a representação final, obtida substituindo $e_{t-k} = B^{-1}\epsilon_{t-k}$ na equação do VMA:
    $$Y_t = \mu + \sum_{k=0}^{\infty} (A_1^k B^{-1}) \epsilon_{t-k}$$
    Definindo a matriz de impulso-resposta no horizonte $k$ como $\Theta_k = A_1^k B^{-1}$, temos:
    $$Y_t = \mu + \sum_{k=0}^{\infty} \Theta_k \epsilon_{t-k}$$
    Esta equação é a **Função de Impulso-Resposta**. Ela mostra matematicamente como as variáveis em $Y_t$ respondem dinamicamente aos choques estruturais puros, $\epsilon_{t-k}$. O elemento $(i,j)$ da matriz $\Theta_k$ representa o efeito de um choque na variável $j$ sobre a variável $i$ após $k$ períodos.