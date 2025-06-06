## Aula 05: Previsão Quantílica com Dados Textuais (Apresentação de Paper)

### **Motivação e Contexto**

* **Limitações da Previsão Pontual:** Em cenários de alta incerteza e volatilidade, comuns em mercados financeiros, a previsão da média (previsão pontual) é insuficiente. O mercado tem interesse em prever a probabilidade de eventos extremos, como a lucratividade de uma empresa cair abaixo de um certo piso.
* **Previsão Quantílica em Finanças e Economia:** Várias aplicações usam a previsão quantílica para analisar riscos e tomar decisões, como:
    * **Growth at Risk (Crescimento em Risco):** Prever a probabilidade de uma recessão.
    * **Risco Soberano:** Prever o teto da dívida pública, acima do qual ela se torna impagável, o que é de interesse dos bancos que compram títulos do governo.
    * **Gerenciamento de Risco:** Medir a perda potencial de um portfólio (Value at Risk).
    * **Comércio Internacional:** Mostrar que acordos comerciais bilaterais tornam o comércio mais estável ao reduzir sua volatilidade, medida pelo *interquartile range* (diferença entre o terceiro e o primeiro quartil), que pode ser previsto via regressão quantílica.
* **O Papel dos Dados Textuais:**
    * Dados textuais, como notícias, contêm informações não capturadas por indicadores econômicos tradicionais.
    * Vários estudos mostram que dados de notícias são úteis para prever variáveis como PIB, consumo e investimento nos EUA.
    * Muitos textos em jornais financeiros referem-se a eventos (riscos de cauda) em vez de previsões pontuais, justificando o uso de dados textuais para previsão quantílica.
* **Desafios dos Dados Textuais:**
    * **Alta Dimensionalidade:** O número de palavras (variáveis) é muito maior que o número de observações (tempo).
    * **Construção de Índices:** Uma solução comum é criar índices a partir de dicionários de palavras predefinidos (ex: índice de incerteza política). O problema é que esses dicionários são subjetivos e podem não ser estáveis ou adequados para todas as variáveis ou cenários (recessão vs. expansão; downside vs. upside risk).
    * **Esparsidade (Sparsity):** Nem todas as palavras têm poder preditivo em todos os contextos.
* **Pergunta da Pesquisa:** Como construir um dicionário de palavras que seja relevante para prever especificamente os quantis (caudas da distribuição) de uma variável?.

### **Metodologia Proposta (3 Passos)**

A metodologia proposta visa criar um modelo de previsão quantílica usando dados textuais, lidando com os desafios de alta dimensionalidade e esparsidade.

* **Passo 1: Pré-processamento dos Dados Textuais**
    * Todos os artigos de jornais (ex: NYT e WSJ) publicados em um determinado período (ex: mês $t$) são agregados.
    * Cria-se um vetor $\Omega_{t}$ onde cada elemento $\Omega_{it}$ representa a frequência da palavra (ou termo, como bigramas) $i$ nos textos do período $t$.
    * As contagens são normalizadas para criar variáveis $Z_{it}$ com média zero e desvio padrão um.
    * Este passo transforma palavras em dados numéricos sem usar um dicionário predeterminado, resultando em um conjunto de dados de alta dimensão ($N \gg T$) e esparso.

* **Passo 2: Seleção de Palavras com Elastic Net Quantílico (Atenção)**
    * Para cada quantil $\tau$ de interesse, utiliza-se o **Elastic Net para Regressão Quantílica** para selecionar as palavras que são úteis para prever o $\tau$-ésimo quantil da variável de interesse $Y_{t+1}$.
    * A função perda quadrática (de MQO) é substituída pela função perda quantílica ($\rho_{\tau}$), e a minimização é sujeita às penalidades Lasso (L1) e Ridge (L2).
    * Este processo cria dicionários $\Omega_{t}^{\tau}$ específicos para cada quantil, contendo apenas as palavras com poder preditivo para aquele quantil. A técnica de atenção retém o que é útil e joga fora o que não é.
    * O conjunto de dados resultante é denso e adequado para a próxima etapa.
    * O dicionário pode ser atualizado ao longo do tempo (ex: a cada início/fim de recessão, sinalizado por indicadores como o Sahm Indicator) para manter a relevância.

* **Passo 3: Extração de Fatores e Previsão Quantílica**
    * As palavras selecionadas no passo 2 para um dado quantil $\tau$ (o dicionário $\Omega_{t}^{\tau}$) são muitas vezes correlacionadas.
    * Utiliza-se a **Análise de Componentes Principais (PCA)** para extrair um ou mais fatores latentes $F_{t,\tau}$ desse conjunto de palavras selecionadas. O número de fatores pode ser determinado por testes estatísticos.
    * O fator $F_{t,\tau}$ age como um índice carregado com informação textual específica para prever o quantil $\tau$.
    * A equação final de previsão quantílica usa este fator como preditor:
        $$Q_{\tau}(Y_{t+1} | F_{t,\tau}) = \alpha(\tau) + \beta(\tau) F_{t,\tau}$$

### **Análise Empírica: Previsão da Taxa de Câmbio US-Canadá**

* **Objetivo:** Testar se a metodologia proposta consegue prever a variação da taxa de câmbio dólar americano-dólar canadense ($\Delta s_{t+1}$), uma variável notoriamente difícil de prever.
    * A Hipótese de Mercados Eficientes sugere que o melhor modelo para a taxa de câmbio é o **Random Walk (Passeio Aleatório)**, onde as variações são imprevisíveis (ruído branco).
    * Embora a previsão pontual (média) do câmbio seja difícil, o paper argumenta que é possível prever seus quantis usando dados textuais, pois a teoria de mercados eficientes se refere à média, não às caudas da distribuição.
* **Dados:**
    * Dados textuais mensais do New York Times (NYT) e Wall Street Journal (WSJ) de 1980 a 2022.
    * Taxa de câmbio do FRED para o mesmo período.
    * O período cobre diversas crises e eventos econômicos importantes.
* **Resultados:**
    * **Previsão de Densidade:** O modelo proposto, que usa dados textuais selecionados, produz previsões de densidade significativamente melhores que o modelo Random Walk e outros modelos baseados em indicadores econômicos.
    * **Previsão de Intervalo:** O modelo textual gera intervalos de confiança de 95% que se adaptam à volatilidade (alargando em crises). A porcentagem de vezes que o valor real fica fora do intervalo ("hit ratio") é de 6.3%, próximo do nível nominal de 5%. Em contraste, o modelo Random Walk tem um intervalo constante e um "hit ratio" de 11.03%, muito acima do esperado. Testes estatísticos confirmam que apenas o modelo textual produz um número de violações estatisticamente igual a 5%.
    * **Análise de Portfólio (Valor Econômico):**
        * Uma estratégia de investimento foi simulada: comprar dólar americano quando a probabilidade prevista de apreciação é alta (>50%) e vender quando é baixa (<50%).
        * A estratégia baseada no modelo textual gerou uma riqueza acumulada muito superior àquela baseada no Random Walk ou em indicadores econômicos, mesmo com custos de transação.
    * **Dicionários Selecionados:** A análise mostrou que o dicionário de palavras selecionado varia com o tempo e com o risco.
        * Em períodos de **apreciação do dólar (upside risk)**, foram selecionadas palavras ligadas a conflitos geopolíticos, como "military spending" e "soviet union" (guerra como "safe haven" para o dólar).
        * Em períodos de **depreciação do dólar (downside risk)**, foram selecionadas palavras ligadas a recessões e crises econômicas, como "fall oil" e "market collapse".

### **Conclusões e Pesquisa Futura**

* **Palavras importam:** Dados textuais contêm informação valiosa para prever os riscos de cauda (quantis) de variáveis financeiras.
* O método proposto permite que os dicionários de palavras relevantes variem com o tempo e entre diferentes quantis.
* **Pesquisa Futura:** A metodologia pode ser estendida para prever todo o cross-section de retornos de ações, construindo portfólios ativos baseados não apenas no retorno esperado, mas também na volatilidade esperada (prevista com os quantis).