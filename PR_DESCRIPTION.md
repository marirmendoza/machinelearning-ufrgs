# Pull Request: Expansão com Bloco 5 — Mineração de Padrões Frequentes & Regras de Associação (Aula 22 P2)

## 📌 Contexto & Objetivo

Este PR expande o módulo de **Aprendizado Não-Supervisionado** da disciplina de Aprendizado de Máquina (**INF01017 - UFRGS**), ministrada pela **Profa. Mariana Recamonde-Mendoza** com desenvolvimento por **Luciano Beylouni**, integrando o conteúdo completo da **Aula 22 - Parte 2: Mineração de Padrões Frequentes e Extração de Regras de Associação**.

Enquanto os Blocos 1 a 4 focaram no agrupamento de instâncias e geometria no espaço euclidiano (K-Means, distâncias e DBSCAN), o **Bloco 5** aprofunda a mineração de dependências entre atributos em dados transacionais (Market Basket Data), cobrindo desde os fundamentos métricos até a execução detalhada do algoritmo Apriori, a armadilha estatística da confiança e o paradigma do FP-Growth.

---

## 🚀 O que foi Implementado no Bloco 5

Foram adicionados **4 novos slides altamente interativos**, simuladores e conexões completas de navegação:

### 1. Slide 5.1 (`agrupamento/b5-s01.html`) — Padrões Frequentes, Itens & Suporte
* **Conceitos Formais da Aula:** Base transacional $T$, conjunto de itens $I$, $k$-itemsets e representação matricial binária (0/1).
* **Métricas Fundamentais:** Suporte absoluto ($s_T(I) = |K_T(I)|$), suporte relativo ($\sigma_T(I) = \frac{s_T(I)}{n}$) e limiar $\sigma_{min}$ ($minsup$).
* **Regras de Associação & Confiança:** Definição da regra $A \Rightarrow C$ ($A \cap C = \emptyset$) e sua confiança condicional $conf_T(A \Rightarrow C) = P(C \mid A)$.
* **Laboratório Interativo:** Tabela de transações oficial dos slides 7 a 11 ($n=8$), com seletor de itemsets e regras, destacando em tempo real as linhas correspondentes e calculando as métricas com KaTeX.

### 2. Slide 5.2 (`agrupamento/b5-s02.html`) — Espaço de Busca & Princípio Apriori
* **Explosão Combinatória:** Análise do crescimento exponencial do reticulado ($2^d - 1$ combinações possíveis).
* **Propriedade da Monotonicidade do Suporte (Downward-Closure):**
  $$K_T(X \cup Y) \subseteq K_T(X) \implies s_T(X \cup Y) \le s_T(X)$$
* **O Teorema Apriori & Regra de Poda:** Se um itemset é infrequente, todos os seus superconjuntos são garantidamente infrequentes e podados do espaço de busca.
* **Laboratório do Reticulado (Lattice Simulator):** Visualização interativa do reticulado de itens $\{a, b, c, d\}$ em 4 níveis hierárquicos interligados. Permite ao estudante clicar em qualquer nó (ex: $\{b,c\}$ ou $\{d\}$) para ver a poda em cascata em ação, com cálculo imediato do percentual de nós economizados sem leitura em disco.

### 3. Slide 5.3 (`agrupamento/b5-s03.html`) — Algoritmo Apriori Passo a Passo
* **Simulador Oficial do Exemplo da Aula (Slides 48-67):** Base de 7 transações ($t_1$ a $t_7$) com $\sigma_{min} = 40\%$ (mínimo de 3 transações) e $c_{min} = 80\%$.
* **Etapa 1 (Itemsets Frequentes):**
  * Geração e contagem de $C_1$, eliminação de $\{d\}$ ($2/7 = 28\% < 40\%$) e obtenção de $F_1 = \{\{a\}, \{b\}, \{c\}\}$.
  * Autocombinação para gerar $C_2 = \{\{a,b\}, \{a,c\}, \{b,c\}\}$, descarte de $\{b,c\}$ e obtenção de $F_2 = \{\{a,b\}, \{a,c\}\}$.
  * Tentativa de gerar $C_3 = \{\{a,b,c\}\}$, demonstrando a poda Apriori porque $\{b,c\} \notin F_2$ e encerramento com $F_3 = \emptyset$.
* **Etapa 2 (Extração de Regras Fortes):**
  * Avaliação das 4 regras possíveis ($a \Rightarrow b$, $b \Rightarrow a$, $a \Rightarrow c$, $c \Rightarrow a$).
  * Seleção das regras fortes que superam $c_{min} = 80\%$: $C = \{b \Rightarrow a \text{ (100\%)}, \; c \Rightarrow a \text{ (80\%)}\}$.
* **Controles Interativos:** Botões *Anterior*, *Próximo Passo*, *Executar Tudo* e *Resetar*, sincronizados com realce de linhas na tabela de dados.

### 4. Slide 5.4 (`agrupamento/b5-s04.html`) — A Armadilha da Confiança, Lift & FP-Growth
* **O Paradoxo do Chá & Café (Slides 70-73):** Demonstração do perigo de avaliar regras apenas pela confiança. No exemplo das 1000 pessoas, a regra $\{Chá\} \Rightarrow \{Café\}$ tem 75% de confiança, mas a taxa basal de café na população é de 80%, caracterizando associação negativa/substituição.
* **Métrica de Lift (Coeficiente de Interesse):**
  $$Lift(A \Rightarrow C) = \frac{s_T(A \cup C)}{s_T(A) \cdot s_T(C)} = \frac{conf(A \Rightarrow C)}{s_T(C)}$$
  * $Lift = 1$: Independência estatística.
  * $Lift > 1$: Associação positiva (atração mútua).
  * $Lift < 1$: Associação negativa (produtos substitutos / repulsão).
* **Simulador de Lift & Tabela de Contingência:** Campos numéricos interativos e medidor visual com agulha colorida indicando atração, independência ou repulsão.
* **Apriori vs. FP-Growth (Slides 74-76):** Análise dos gargalos do Apriori ($k$ varreduras completas no disco, geração de candidatos) versus as vantagens do **FP-Growth** (apenas 2 varreduras, compressão em **FP-Tree** e mineração recursiva sem candidatos).

---

## 🔗 Integração & Navegação

* **`agrupamento/index.html`:** Adicionada a seção do **Bloco 5** com cards interativos para cada um dos 4 novos slides, e subtítulo atualizado para refletir tanto Agrupamento quanto Associação.
* **`agrupamento/b4-s03.html`:** Botão final de navegação atualizado para apontar suavemente para o início do Bloco 5 (`b5-s01.html`).
* **`index.html` (portal principal):** Atualização do card para **Agrupamento & Associação**.

---

## 🧪 Como Validar Localmente

1. Execute o servidor web estático:
   ```bash
   python3 -m http.server 8000
   ```
2. Acesse:
   - `http://localhost:8000/agrupamento/index.html` para visualizar o Bloco 5 completo.
   - `http://localhost:8000/agrupamento/b5-s01.html` para testar os filtros de suporte.
   - `http://localhost:8000/agrupamento/b5-s02.html` para interagir com o reticulado e a poda.
   - `http://localhost:8000/agrupamento/b5-s03.html` para acompanhar o passo a passo do Apriori.
   - `http://localhost:8000/agrupamento/b5-s04.html` para experimentar a tabela Chá/Café e o medidor de Lift.
