### **Aula 11: 02/07/2025**

### **1. Identificação de VAR com Restrições de Longo Prazo: A Abordagem de Blanchard-Quah**

Esta seção continua a discussão sobre a identificação de VARs estruturais, focando na abordagem que utiliza restrições de longo prazo baseadas na teoria econômica.

* **A Matriz de Covariância de Longo Prazo ($\Lambda$):** Um resultado fundamental da teoria de séries temporais é que a matriz de covariância de longo prazo de um vetor de variáveis $Y_t$ pode ser expressa em função da matriz de impacto de longo prazo ($\Theta(1)$) e da matriz de covariância dos choques estruturais ($\Sigma_\epsilon$). A relação é:
    $$\Lambda = \Theta(1)\Sigma_\epsilon\Theta(1)'$$

* **A Estratégia de Identificação:** A identificação é alcançada combinando resultados da teoria econômica e da álgebra linear.
    1.  **Restrição 1 (Normalização):** Assume-se, sem perda de generalidade, que os choques estruturais têm variância unitária, de modo que sua matriz de covariância é a matriz identidade: $\Sigma_\epsilon = I$. Com isso, a equação se simplifica para $\Lambda = \Theta(1)\Theta(1)'$.
    2.  **Restrição 2 (Teoria Econômica):** Conforme discutido na aula anterior, a teoria econômica (modelo AD-AS) sugere que choques de demanda não têm efeito de longo prazo sobre o produto. Isso impõe uma restrição de zero em um dos elementos da matriz de impacto de longo prazo (ex: $[\Theta(1)]_{1,2} = 0$), o que torna a matriz $\Theta(1)$ **triangular inferior** (*lower triangular*).
    3.  **Solução via Decomposição de Matriz:** O problema se resume a encontrar uma matriz triangular inferior $\Theta(1)$ cujo produto com sua transposta seja igual à matriz $\Lambda$. Por definição, esta matriz $\Theta(1)$ é a **decomposição de Cholesky** (ou a "raiz quadrada") da matriz $\Lambda$.

* **O Processo Final:**
    1.  Estima-se o VAR na forma reduzida e calcula-se uma estimativa da matriz de covariância de longo prazo, $\hat{\Lambda}$.
    2.  Aplica-se a decomposição de Cholesky em $\hat{\Lambda}$ para obter $\hat{\Theta}(1)$.
    3.  Usa-se a relação $\hat{\Theta}(1)=(I-\hat{A}_1)^{-1}\hat{B}^{-1}$ para encontrar a matriz de efeitos contemporâneos $\hat{B}$.
    4.  Com $\hat{B}$ e os parâmetros da forma reduzida, calculam-se as Funções de Impulso-Resposta ($\hat{\Theta}_S = \hat{A}_1^S \hat{B}^{-1}$).

### **2. Testes de Raiz Unitária: Dickey-Fuller e ADF**

Esta seção aborda como testar formalmente se uma série temporal é não estacionária (possui uma raiz unitária).

* **O Problema com o Teste-t Padrão:** Ao testar a hipótese nula de raiz unitária ($H_0: \phi=1$) em um processo AR(1) como $y_t = \phi y_{t-1} + \epsilon_t$, a estatística-t usual não segue a distribuição normal padrão.
* **A Distribuição Dickey-Fuller (DF):** Sob a hipótese nula de raiz unitária, a estatística-t converge para uma distribuição não padrão, conhecida como distribuição Dickey-Fuller, que é derivada de funcionais do movimento Browniano.
    * **Movimento Browniano:** É um conceito de um passeio aleatório em tempo contínuo, geralmente definido no intervalo [0, 1].
    * **Valores Críticos:** A consequência mais importante é que os valores críticos da distribuição DF são diferentes (mais negativos) que os da normal. Por exemplo, para um teste de 5% à esquerda, o valor crítico da normal é -1.64, enquanto o da DF é aproximadamente -1.95. Usar o valor crítico errado pode levar a rejeitar incorretamente a hipótese nula.
* **Componentes Determinísticos:** A especificação da regressão usada para o teste afeta a distribuição e os valores críticos. A inspeção visual do gráfico da série ajuda a decidir qual caso usar:
    * **Sem intercepto nem tendência:** O valor crítico é ~ -1.95.
    * **Com intercepto:** A distribuição muda para uma versão "de-meaned" (média removida), e o valor crítico se torna mais negativo (~ -2.9).
    * **Com intercepto e tendência:** A distribuição muda para uma versão "de-trended" (tendência removida), e o valor crítico é ainda mais negativo (~ -3.4).
* **Teste Augmented Dickey-Fuller (ADF):** O teste DF padrão assume que os erros são ruído branco. Se houver correlação serial nos erros, o teste ADF deve ser usado. Ele "aumenta" a regressão adicionando defasagens da variável em primeira diferença ($\Delta y_{t-p}$) para capturar essa correlação serial e garantir que o resíduo final seja ruído branco. Os valores críticos do teste ADF são os mesmos do teste DF correspondente.

### **3. Cointegração e Modelos de Correção de Erros (VECM)**

* **Regressão Espúria:** Regredir uma série não estacionária em outra série não estacionária independente geralmente resulta em um coeficiente estatisticamente significante e um R² alto, mesmo que não haja relação verdadeira entre elas. Isso é conhecido como **regressão espúria**, um artefato da correlação artificial gerada pela presença de raiz unitária. Uma solução simples é rodar a regressão com as variáveis em primeira diferença.
* **Conceito de Cointegração:** Um conjunto de séries não estacionárias (I(1)) é dito **cointegrado** se existe uma combinação linear entre elas que é estacionária (I(0)).
    * **Intuição Econômica:** A cointegração representa uma **relação de equilíbrio de longo prazo** estável entre as variáveis. Embora as séries possam se desviar aleatoriamente no curto prazo, elas estão "amarradas" por essa relação de longo prazo.
    * **Exemplos:** Renda e consumo (hipótese da renda permanente); taxas de juros de curto e longo prazo (arbitragem); preços de ações e dividendos; taxa de câmbio à vista e a termo.
* **Modelo de Correção de Erros (VECM):** Se as séries são cointegradas, usar um VAR apenas em primeiras diferenças descarta a valiosa informação sobre a relação de longo prazo. O VECM é um VAR que incorpora essa informação.
    * **Estrutura:** É um VAR com as variáveis em primeira diferença ($\Delta Y_t$), mas que inclui um termo adicional: o **termo de correção de erro**, que é o desvio do equilíbrio de longo prazo no período anterior (ex: $c_{t-1} - y_{t-1}$).
    * **Mecanismo (Termostato):** Se no período $t-1$ as variáveis se desviam do equilíbrio, o termo de correção de erro se torna diferente de zero. No período $t$, o modelo prevê uma mudança em $\Delta Y_t$ que "corrige" parte desse desequilíbrio, empurrando o sistema de volta para a relação de longo prazo. O coeficiente $\alpha$ mede a velocidade desse ajuste.