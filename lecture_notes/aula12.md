## Aula 12: 03/07/2025

### **1. Regressão Espúria e a Definição de Cointegração**

Esta seção formaliza o conceito de cointegração como a solução para o problema da regressão espúria quando se trabalha com múltiplas séries temporais não estacionárias.

* **Problema e Solução Padrão:** O problema da **regressão espúria** ocorre quando se roda uma regressão em nível com variáveis não estacionárias ($I(1)$), o que pode gerar resultados estatisticamente significativos falsos. A solução mais simples é rodar um Vetor Autorregressivo (VAR) com as variáveis em **primeira diferença** ($\Delta Y_t$).
* **A Limitação do VAR em Diferenças:** Se as séries não estacionárias possuem uma relação de equilíbrio de longo prazo, um VAR em primeiras diferenças é um modelo mal especificado, pois omite uma variável preditora importante. O modelo correto nesses casos é o **Modelo de Correção de Erros (VECM)**.
* **Definição Formal de Cointegração:** Um vetor de $N$ séries temporais $Y_t$, onde cada série é não estacionária ($I(1)$), é dito **cointegrado** se existe um vetor de cointegração $\beta$ tal que a combinação linear $\beta'Y_t$ é estacionária ($I(0)$).
    * **Normalização:** O vetor $\beta$ não é único. Para garantir a unicidade, aplica-se uma normalização, usualmente fixando o primeiro coeficiente como 1. Isso permite escrever a relação de cointegração como uma equação de regressão de longo prazo:
        $$y_{1t} = \alpha + \beta_2 y_{2t} + \dots + \beta_n y_{nt} + u_t$$
        Onde $u_t = \beta'Y_t$ é o termo de erro estacionário que representa os desvios do equilíbrio de longo prazo. Uma regressão em nível só faz sentido se as variáveis forem cointegradas.

### **2. A Intuição da Cointegração: Tendências Estocásticas Comuns**

A cointegração é possível porque as séries não estacionárias compartilham a mesma fonte de não estacionariedade.

* **Representação de um Passeio Aleatório:** Um processo com raiz unitária (passeio aleatório), $y_t = y_{t-1} + \epsilon_t$, pode ser reescrito como uma acumulação de choques passados:
    $$y_t = \sum_{s=1}^{t} \epsilon_s$$
    Este termo de somatório, $\sum \epsilon_s$, é a **tendência estocástica** que torna a série não estacionária.
* **Tendência Estocástica Comum:** Séries temporais são cointegradas quando são geradas pela mesma tendência estocástica comum. O vetor de cointegração funciona ao criar uma combinação linear que cancela essa tendência comum, resultando em uma série estacionária.
    * **Exemplo:** Se temos duas séries $y_1$ e $y_2$ geradas por uma mesma tendência estocástica $TS_t = \sum v_s$:
        * $y_{2t} = TS_t + \text{ruído estacionário}_2$
        * $y_{1t} = \beta_2 \cdot TS_t + \text{ruído estacionário}_1$
        A combinação linear $y_{1t} - \beta_2 y_{2t}$ irá eliminar o termo $TS_t$, resultando em uma série puramente estacionária.
* **Número de Vetores e Tendências:** Para um sistema com $N$ variáveis, existe uma relação fundamental: se há **R vetores de cointegração**, então deve haver **N-R tendências estocásticas comuns**.
    * Se $N=2$ e $R=1 \implies$ há $2-1=1$ tendência estocástica comum.
    * Se $N=3$ e $R=1 \implies$ há $3-1=2$ tendências estocásticas comuns.
    * Se $N=3$ e $R=2 \implies$ há $3-2=1$ tendência estocástica comum.
    * O número de vetores de cointegração $R$ não pode ser igual a $N$, pois isso implicaria $N-R=0$ tendências estocásticas, significando que as séries já eram estacionárias para começar.

### **3. Testes de Cointegração**

Existem duas abordagens principais para testar a existência e o número de relações de cointegração.

#### **3.1 O Teste de Engle-Granger (Baseado nos Resíduos)**

Este teste é mais simples e aplicável quando se suspeita que há no máximo **um** vetor de cointegração ($R=1$).

* **Procedimento em 2 Passos:**
    1.  **Estimar a Regressão de Longo Prazo:** Roda-se uma regressão em nível de uma variável sobre as outras por MQO e salvam-se os resíduos, $\hat{u}_t$.
    2.  **Testar os Resíduos:** Aplica-se um teste de raiz unitária (ADF) sobre os resíduos $\hat{u}_t$.
* **Hipóteses:**
    * $H_0$: O resíduo possui raiz unitária (não é estacionário).
    * $H_A$: O resíduo é estacionário.
* **Conclusão:** A hipótese nula de **não cointegração** é rejeitada se a hipótese nula do teste de raiz unitária for rejeitada (ou seja, se os resíduos forem estacionários).
* **Valores Críticos:** Os valores críticos para o teste de raiz unitária sobre resíduos são ligeiramente diferentes dos valores críticos do ADF padrão, mas os pacotes estatísticos fazem este ajuste automaticamente.

#### **3.2 O Teste de Johansen (Baseado no Sistema)**

Este é o teste mais popular e geral, pois permite identificar múltiplos vetores de cointegração ($R \ge 1$).

* **Procedimento Sequencial:** O teste determina o número de vetores de cointegração ($R$) através de uma sequência de testes de hipóteses.
    1.  **Etapa 1:** Testa-se $H_0: R=0$ (não há cointegração) contra $H_A: R > 0$. Se $H_0$ não for rejeitada, o teste para e conclui-se que não há cointegração. Se for rejeitada, passa-se para a próxima etapa.
    2.  **Etapa 2:** Testa-se $H_0: R=1$ contra $H_A: R > 1$. Se $H_0$ não for rejeitada, o teste para e conclui-se que existe um vetor de cointegração. Se for rejeitada, continua-se.
    3.  **Continuação:** O processo continua, testando $H_0: R=k$ contra $H_A: R > k$, até que uma hipótese nula não seja rejeitada.
* **Importância dos Componentes Determinísticos:** A escolha correta do caso para os componentes determinísticos (constante, tendência) é crucial, pois afeta os valores críticos do teste. A inspeção visual do gráfico das séries é o primeiro passo para determinar qual dos cinco casos possíveis deve ser utilizado no teste de Johansen.

### **4. O Modelo de Correção de Erros (VECM) e a Previsão**

* **Estrutura:** O VECM é um VAR em primeiras diferenças que incorpora a relação de equilíbrio de longo prazo. Para um sistema com um vetor de cointegração, sua forma é:
    $$\Delta Y_t = \alpha (\beta' Y_{t-1}) + \text{lags de } \Delta Y_t + \text{...}$$
    Onde $\beta' Y_{t-1}$ é o **termo de correção de erro**, que representa o desequilíbrio do período anterior.
* **Mecanismo (Termostato):** Se no período $t-1$ as variáveis se desviam do equilíbrio, o termo $(\beta' Y_{t-1})$ se torna diferente de zero. O coeficiente $\alpha$ mede a **velocidade de ajuste**, ou seja, quanto de $\Delta Y_t$ mudará no período $t$ para "corrigir" o erro e levar o sistema de volta ao equilíbrio.
* **Poder de Previsão:** O VECM é um modelo de previsão poderoso porque combina a dinâmica de curto prazo (os lags de $\Delta Y_t$) com a teoria econômica de longo prazo (a relação de cointegração). Isso o torna superior a um simples VAR em diferenças quando a cointegração está presente.