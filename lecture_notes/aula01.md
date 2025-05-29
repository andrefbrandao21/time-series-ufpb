## Aula 01: 28/05/2025


### **Curso de Séries Temporais e Ferramentas:**
* O curso será sobre séries temporais, começando do básico e progredindo para tópicos mais avançados, incluindo machine learning.
* Será explorada a possibilidade de aprender econometria com o ChatGPT, destacando que a parte tecnológica do ChatGPT é muito boa para gerar códigos. A limitação apontada é que a melhor comunicação com o ChatGPT é em inglês, pois a qualidade cai com o português.
* O curso terá uma parte teórica que cobrirá vários tópicos de forma aprofundada, com ênfase no domínio da notação teórica para uma comunicação eficaz.
* Haverá uma parte prática com códigos em R, utilizando muitos "loops" para incluir dinâmica na estimação dos modelos.
* Os livros de referência para a parte teórica serão:
Walter Anders (Applied Econometric Time Series) e
James Hamilton (Time Series Analysis).

### **Conceitos Fundamentais de Séries Temporais:**

* **Diferença entre *cross-section* e séries temporais:**
    Em dados *cross-section*, as observações $Y_i$ para $i=1, \dots, N$ são I.I.D. (independentes e identicamente distribuídas). Isso significa que $Y_i$ e $Y_j$ são independentes para $i \neq j$, o que implica $\text{Cov}(Y_i, Y_j) = 0$. Adicionalmente, todas as observações $Y_i$ vêm da mesma distribuição populacional, ou seja, possuem a mesma média $E[Y_i] = \mu$ e variância $\text{Var}(Y_i) = \sigma^2$ para todo $i$.
* A **hipótese de I.I.D.** é crucial para provar propriedades desejáveis de estimadores, como o estimador de média amostral $\hat{\mu} = \frac{1}{N}\sum_{i=1}^N Y_i$. Esta hipótese garante que $\hat{\mu}$ é não-enviesado ($E[\hat{\mu}] = \mu$) e consistente ($\hat{\mu} \xrightarrow{p} \mu$ quando $N \to \infty$).
* **Séries temporais e memória:** Ao contrário dos dados I.I.D., as séries temporais $\{Y_t\}_{t=1}^T$ não possuem a propriedade de "mostra aleatória", tornando a análise mais complexa, mas também criando oportunidades.
* A principal característica das séries temporais é a "memória temporal", o que significa que observações passadas $Y_{t-k}$ influenciam as futuras $Y_t$. Essa memória é "importantíssimo para previsão", pois permite extrapolar para o futuro.
* **Processo de Ruído Branco ($\epsilon_t$):** É definido como um processo I.I.D. e "sem memória". Representa o "building block" (parte básica) de qualquer modelo de série temporal. Retornos de ações em baixa frequência são citados como um exemplo de processo de ruído branco.
* **Operador de Defasagem ($L$):** Introduzido como um operador que defasa a série temporal por um período. Para uma série $Y_t$, o operador de defasagem $L$ age como $L Y_t = Y_{t-1}$. É uma ferramenta útil para entender a relação entre $Y_t$ e $Y_{t-1}$ em modelos autorregressivos.

### **Previsão, Robótica e Inteligência Artificial:**
* A memória temporal é a base para a família de modelos ARIMA, que utiliza essa memória para identificar o modelo que melhor representa os dados.
* A capacidade de memória é fundamental para o funcionamento de robôs e modelos de inteligência artificial como o ChatGPT; sem memória, eles não conseguiriam aprender ou fazer previsões.
* Um robô é descrito como um modelo de série temporal que faz previsões e toma decisões baseadas em teoria econômica A qualidade da previsão é crucial para o sucesso de um robô.
* A vida humana também depende da capacidade de prever o futuro para tomar decisões.
* Robôs "odeiam errar" e erram quando expostos a situações "inusitadas" ou "imprevisíveis".
* A solução para a imprevisibilidade em carros autônomos seria "tirar os humanos da equação", pois eles são a principal fonte de "ruído" e situações inusitadas

### **Modelos de Previsão:**
* A família de modelos ARIMA é um *benchmark* em análise de previsão; qualquer novo modelo proposto deve ter um desempenho superior a eles em termos de precisão para ser considerado melhor.
* A técnica Box-Jenkins é usada para determinar o modelo ARIMA mais adequado para os dados.



### Séries de Tempo com Comportamento Cíclico/Sazonal:
* Algumas séries de tempo apresentam um comportamento cíclico ou sazonal, como vendas que sobem no Natal ou em determinadas estações.
* Outras séries podem ter um comportamento mais próximo de um ruído branco.

### **Modelos para Séries de Tempo**:
* **Modelos Autorregressivos (AR)**: Usados para capturar componentes cíclicos ou séries com "memória". A abreviação é AR.
* **Modelos de Média Móvel (MA - Moving Average)**: Usados para processos que são "puro ruído" mas que ainda possuem memória. A abreviação é MA.
    * Modelos MA são funções de ruído branco "passável".
    * A combinação de ruídos brancos pode ser previsível. Um exemplo é a média dos últimos três dias de uma ação, que tem memória e é previsível.

### **Identificação de Modelos (AR vs. MA)**:
* É um desafio identificar se os dados vieram de um processo AR ou MA, pois a natureza fornece os dados sem dizer como foram gerados.
* A **Metodologia de Box-Jenkins (Bob Jentz)** é usada para responder a essas perguntas.

### **Conceitos Fundamentais para a Metodologia Box-Jenkins**:
* **Autocorrelação (ACF)**: Mede a correlação de uma série temporal com suas versões passadas (lags).
    * A definição de correlação é a força e direção da associação entre variáveis.
    * No contexto de séries temporais, a autocorrelação ($\rho_j$) de um processo AR(1) é igual a $\alpha^j$, onde $\alpha$ é o coeficiente autorregressivo.
    * Para processos AR, a autocorrelação **decai lentamente**.
* **Autocorrelação Parcial (PACF)**: Mede a correlação entre Yt e Yt-j após remover o efeito das variáveis intermediárias ($Y_{t-1}$, ..., $Y_{t-j+1}$).
    * Para um processo AR(1), a autocorrelação parcial terá um "spike" (pico significativo) apenas no primeiro lag e será zero para os lags seguintes. Isso ajuda a determinar o número de lags (P) em um modelo AR.

### **Estacionariedade**:
* Um conceito crucial para que as fórmulas de autocorrelação façam sentido.
* Uma série é considerada **fracamente estacionária** se:
    1.  Sua esperança (média) é constante ao longo do tempo ($\mu$) 
    2.  Sua variância é constante ao longo do tempo ($\sigma^2$).
    3.  A covariância entre $Y_t$ e $Y_{t-j}$ depende apenas do intervalo $j$ (lag), e não do tempo $t$.
* A metodologia de Box-Jenkins só funciona para séries estacionárias.
* A condição de estacionariedade implica que o valor absoluto de $\alpha$ (coeficiente autorregressivo) deve ser menor que 1 ($|\alpha| < 1$).

### **Comparação entre Processos AR e MA**:
* **Processo AR (ex: AR(1))**: Possui **memória longa**, ou seja, a covariância decai lentamente e existe para j infinito. Útil para modelar variáveis cíclicas.
* **Processo MA (ex: MA(2))**: Possui **memória curta**, onde a autocorrelação se torna zero após um certo número de lags.

### **Importância da Previsão**:
* Tudo que tem memória é bom para previsão.
* Identificar e modelar a memória de um processo é fundamental para desenvolver ferramentas e tomar decisões corretas, pois uma previsão errada leva a decisões erradas.