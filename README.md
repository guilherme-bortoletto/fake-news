
-----

# Detecção de Fake News com Processamento em Larga Escala Utilizando MongoDB Atlas e Python

## Conceito do projeto

O projeto tem como foco o desenvolvimento de um pipeline completo para a detecção automática de fake news, integrando tecnologias de armazenamento e processamento de dados. Utilizando o **MongoDB Atlas** como banco de dados NoSQL e a biblioteca **Pymongo** em um ambiente **Databricks** para a análise e processamento, o sistema é capaz de armazenar, transformar e analisar milhares de notícias jornalísticas.

Nosso objetivo é demonstrar a aplicação de tecnologias NoSQL e frameworks de processamento para:

  * Armazenar de forma flexível e escalável um volume significativo de notícias.
  * Processar dados textuais em larga escala utilizando técnicas de NLP (*Natural Language Processing*).
  * Treinar e avaliar um modelo de *machine learning* para classificar notícias.
  * Avaliar a viabilidade da arquitetura proposta no contexto de aplicações robustas.
  * Identificar padrões linguísticos, como as palavras mais frequentes em textos de *fake news*.

## Pré-requisitos e recursos utilizados

**Linguagens e Bibliotecas:**

  * **Python 3.x**
  * **Pymongo:** Para interação direta com o MongoDB Atlas.
  * **Pandas:** Para manipulação e análise de dados.
  * **Scikit-learn:** Para pré-processamento, vetorização (TF-IDF) e treinamento do modelos.
  * **NLTK:** Para processamento de linguagem natural (stopwords).
  * **Matplotlib / Seaborn:** Para visualização de dados e geração de gráficos.

**Infraestrutura e Plataformas:**

  * **MongoDB Atlas:** Banco de dados NoSQL na nuvem para armazenamento dos artigos.
  * **Databricks:** Plataforma utilizada para a execução dos notebooks de análise em Python.

**Fonte de Dados:**

  * **Fake and Real News Dataset – Kaggle:** O conjunto de dados original, contendo aproximadamente 44.000 artigos, foi utilizado como base.
      * Disponível em: [Fake and Real News Dataset](https://www.kaggle.com/datasets/clmentbisaillon/fake-and-real-news-dataset)

## Passo a passo

O projeto foi estruturado seguindo um pipeline claro de processamento de dados, desde a coleta até a análise e avaliação do modelo.

1.  **Obtenção dos Dados**: Download dos arquivos `Fake.csv` e `True.csv` a partir da fonte de dados no Kaggle.
2.  **Ingestão no Banco de Dados**: Cada notícia foi convertida para o formato JSON e inserida em uma *collection* específica (`Fake` ou `Real`) no cluster do MongoDB Atlas.
3.  **Conexão e Leitura**: Utilizando um notebook no ambiente Databricks, estabelecemos a conexão com o MongoDB Atlas via `Pymongo` e carregamos os dados para análise.
4.  **Processamento e Vetorização**: Os textos foram processados com técnicas de NLP (limpeza, tokenização, remoção de stopwords) e transformados em vetores numéricos com a técnica **TF-IDF**.
5.  **Treinamento do Modelo**: Um modelo de classificação de **Regressão Logística** foi treinado com os dados vetorizados para distinguir entre notícias falsas e verdadeiras.
6.  **Análise e Avaliação**: Foram realizadas análises exploratórias para identificar padrões linguísticos (palavras mais frequentes, n-gramas, análise de sentimento) e o modelo foi avaliado quanto à sua acurácia e outras métricas de desempenho.

### Fluxograma do Pipeline

```mermaid
graph TD
    subgraph "Origem dos Dados"
        A["Kaggle
        (Arquivos CSV)"];
    end

    subgraph "Banco de Dados (Nuvem)"
        B["MongoDB Atlas"];
        subgraph "Collections"
            B1["Collection: Fake"];
            B2["Collection: Real"];
        end
        B -.-> B1;
        B -.-> B2;
    end
    
    subgraph "Ambiente de Análise (Databricks)"
        C["Notebook Python"];
        D["Processamento e Vetorização
        (scikit-learn e TF-IDF)"];
        E["Treinamento do Modelo
        (Regressão Logística)"];
        F["Análise Textual Exploratória
        (N-gramas e Sentimento)"];
        G{{Modelo Classificador}};
        H{{Resultados e Visualizações}};
    end

    A -- "Passo 1: Obtenção e Inserção" --> B;
    B -- "Passo 2: Leitura com Pymongo" --> C;
    C -- "Passo 3: Processamento" --> D;
    D -- "Passo 4a: Treinamento" --> E;
    D -- "Passo 4b: Análise" --> F;
    E -- "Passo 5: Gera Resultado" --> G;
    F -- "Passo 6: Gera Resultado" --> H;
```

## Instalação

Para replicar o ambiente do projeto, siga os passos abaixo:

1.  **Clone o repositório:**
    ```bash
    git clone https://github.com/guilherme-bortoletto/fake-news.git
    cd fake-news
    ```
2.  **Crie e ative um ambiente virtual:**
    ```bash
    python -m venv venv
    source venv/bin/activate  # No Windows: venv\Scripts\activate
    ```
3.  **Instale as dependências:**
    ```bash
    pip install pymongo pandas scikit-learn nltk matplotlib seaborn jupyter
    ```
4.  **Configure o MongoDB Atlas:**
      * Crie um cluster gratuito no [MongoDB Atlas](https://www.mongodb.com/cloud/atlas).
      * Crie um banco de dados (ex: `Fake_and_Real`) e duas collections (ex: `Fake` e `Real`).
      * Obtenha a **URI de conexão** e substitua no script de análise.
5.  **Carregue os dados:** Execute um script para ler os arquivos `Fake.csv` e `True.csv` e inseri-los nas *collections* correspondentes no MongoDB Atlas.

## Execução

1.  Abra o notebook de análise (arquivo `.ipynb`) no ambiente de sua preferência (Jupyter, VS Code ou Databricks).
2.  Assegure-se de que a URI de conexão com o MongoDB Atlas está corretamente configurada no código:
    ```python
    from pymongo import MongoClient

    # Substitua pela sua URI de conexão
    conn_uri = "mongodb+srv://<user>:<password>@cluster.mongodb.net/?retryWrites=true&w=majority"
    client = MongoClient(conn_uri)
    db = client["Fake_and_Real"]
    ```
3.  Execute as células do notebook sequencialmente para realizar a leitura dos dados, o pré-processamento, o treinamento do modelo e a visualização dos resultados.

## Bugs/problemas conhecidos

  * **Mudança de Estratégia de Processamento:** O plano inicial era utilizar **PySpark** no Databricks para o processamento distribuído. Contudo, devido a limitações da versão **Databricks Community Edition**, que removeu o acesso a clusters computacionais em seu plano gratuito, o conector `spark-mongodb` tornou-se inviável. Como solução, o projeto foi adaptado para usar a biblioteca **Pymongo** para interação direta com o MongoDB, mantendo a integridade do pipeline, embora sem o processamento distribuído do Spark.

## Autores

  * **Vinicius Silva Castro** – 802138
  * **Guilherme Campos Bortoletto** – 801477


--
## Análise Detalhada e Resultados

Nesta fase, o desempenho do modelo de classificação foi rigorosamente avaliado e uma análise quantitativa aprofundada foi realizada para extrair padrões linguísticos e estatísticos dos textos.

### Avaliação de Desempenho do Modelo (Regressão Logística)

Para este projeto, foram testados diferentes modelos de classificação para a tarefa de detecção de fake news. Dentre as abordagens avaliadas, o modelo de Regressão Logística foi o que apresentou a melhor performance, demonstrando uma capacidade excepcional de distinguir entre os textos com uma acurácia de 99% no conjunto de teste, além de métricas de precisão e recall quase perfeitas para ambas as classes.

#### Relatório de Classificação:

```
              precision    recall  f1-score   support

           0       0.99      0.99      0.99      4733
           1       0.99      0.99      0.99      4247

    accuracy                           0.99      8980
   macro avg       0.99      0.99      0.99      8980
weighted avg       0.99      0.99      0.99      8980
```

#### Matriz de Confusão:

A matriz de confusão abaixo detalha os acertos e erros do modelo, onde a classe `0` representa notícias falsas e a `1`, notícias verdadeiras.

```
[[4684   49]
 [  51 4196]]
```

|                         | **Previsto: Falso (0)** | **Previsto: Real (1)** |
|:------------------------|:-----------------------:|:------------------------:|
| **Verdadeiro: Falso (0)** | 4684 ✅ (Verdadeiro Negativo - TN) | 49 ❌ (Falso Positivo - FP) |
| **Verdadeiro: Real (1)** | 51 ❌ (Falso Negativo - FN)       | 4196 ✅ (Verdadeiro Positivo - TP) |

##### 🧠 Interpretação:

  - O modelo classificou corretamente **4684** notícias falsas e **4196** notícias verdadeiras.
  - Os erros foram mínimos: apenas **49** notícias verdadeiras foram classificadas como falsas (falsos positivos) e **51** notícias falsas foram consideradas verdadeiras (falsos negativos).
  - Essa alta taxa de acerto confirma que o modelo de Regressão Logística é extremamente eficaz e robusto para este problema.

### **Análise Quantitativa e Linguística**

A análise exploratória dos dados revelou diferenças sistemáticas e marcantes entre os padrões linguísticos de notícias falsas e verdadeiras. Estes padrões, quando examinados em conjunto, formam uma "impressão digital" que permite a um modelo de machine learning classificar os textos com alta precisão.

#### 1. Palavras Mais Frequentes por Classe

A análise das palavras mais frequentes expõe uma diferença fundamental na estratégia de comunicação de cada tipo de conteúdo. As notícias falsas são dominadas por termos que evocam forte viés político e personalismo, como "hillary", "clinton", "obama" e "trump". A alta frequência desses nomes próprios sugere uma tática focada em polarização e resposta emocional, explorando figuras públicas como âncoras para a narrativa. Palavras como "us", "president" e "people" reforçam um foco em temas nacionais, muitas vezes no contexto de teorias da conspiração ou desinformação estratégica.

Em contrapartida, as notícias reais apresentam um perfil linguístico mais neutro, factual e institucional. Termos como "washington", "united", "states" e "government" refletem um foco em processos, instituições e locais, em vez de personalidades. Expressões como "told", "could" e "house" indicam um discurso mais descritivo e cauteloso, característico do jornalismo profissional que visa relatar eventos em vez de provocar reações. Essa clara distinção lexical é uma das features mais poderosas para o modelo de classificação.
![Imagem](https://github.com/guilherme-bortoletto/fake-news/edit/main/img/most_frequence_words.png)

#### 2. Nuvem de Palavras (Word Cloud)

As nuvens de palavras reforçam e complementam visualmente os padrões identificados na análise de frequência. Na nuvem das **fake news**, o tamanho proeminente de nomes como "Trump", "Hillary" e "Clinton" confirma o foco avassalador em figuras políticas. A presença marcante desses nomes sugere que a notoriedade dessas personalidades é usada como um atalho para gerar engajamento e credibilidade fabricada.

Em nítido contraste, a nuvem de palavras das **notícias reais** exibe um vocabulário mais distribuído e diversificado. Termos como "Washington", "United States", "government" e "House" são predominantes, refletindo uma abordagem mais sóbria e centrada em fatos institucionais. A visualização mostra que, enquanto as fake news giram em torno de poucos indivíduos, as notícias reais cobrem um espectro mais amplo de tópicos, o que é um indicador de práticas jornalísticas mais equilibradas.
![Imagem](https://github.com/guilherme-bortoletto/fake-news/edit/main/img/word_cloud.png)

#### 3. Distribuição de Tamanho dos Textos

A análise da distribuição do comprimento dos textos (em número de caracteres) revela que a maioria dos artigos, em ambas as classes, se concentra na faixa de 0 a 5.000 caracteres. Existe um pico notável em torno de 2.000 caracteres, onde a curva de notícias falsas (label 0) é ligeiramente mais alta, sugerindo uma tendência para textos um pouco mais curtos nessa categoria. Uma hipótese para esse padrão é que a desinformação é frequentemente otimizada para consumo rápido em plataformas de mídia social. Apesar dessa sutil diferença, as distribuições gerais são bastante semelhantes, indicando que o tamanho do texto, por si só, é uma feature de baixo poder preditivo em comparação com as características lexicais.
![Imagem](https://github.com/guilherme-bortoletto/fake-news/edit/main/img/text_length.png)

#### 4. Frequência de Bigramas (Pares de Palavras)

A análise de bigramas revela diferenças marcantes na construção frasal. Os conteúdos falsos são dominados por combinações que nomeiam figuras políticas, como "donald trump" e "hillary clinton". Esse padrão sugere uma estratégia deliberada de vincular a desinformação a personalidades com forte carga política, explorando vieses cognitivos preexistentes no público.

Em contraste, as notícias reais apresentam uma estrutura mais convencional e profissional. Bigramas como "trump said" e "said statement" refletem a prática jornalística de atribuição clara de fontes. Da mesma forma, pares como "prime minister" e "north korea" demonstram uma cobertura mais ampla de temas internacionais. Essa diferença é crucial: as fake news focam em *quem* é o assunto, enquanto as notícias reais focam em *quem disse o quê*, um pilar da credibilidade jornalística.
![Imagem](https://github.com/guilherme-bortoletto/fake-news/edit/main/img/bigrama.png)

#### 5. Frequência de Trigramas (Trios de Palavras)

Os trigramas revelam padrões ainda mais sofisticados de credibilidade e atribuição. Nas notícias falsas, destacam-se estruturas que tentam fabricar autoridade, como "century wire says" (uma fonte de baixa reputação) ou "president donald trump" usado de forma isolada para dar peso a uma alegação.

Por outro lado, as notícias reais contêm trigramas que refletem práticas editoriais rigorosas, como "white house said" e "secretary state rex", que demonstram uma preocupação com a atribuição precisa de declarações a fontes reconhecidas. A variedade de combinações, incluindo nomes de oficiais e instituições, mostra uma cobertura mais contextualizada. Enquanto as fake news se apropriam de nomes para dar uma falsa impressão de legitimidade, as notícias reais os utilizam para construir uma narrativa factual e verificável.
![Imagem](https://github.com/guilherme-bortoletto/fake-news/edit/main/img/trigrama.png)

#### 6. Análise de Sentimento

O gráfico de boxplot da polaridade de sentimento (onde -1.0 é negativo e +1.0 é positivo) mostra que as medianas de ambas as classes estão próximas de 0.0, indicando um tom majoritariamente neutro. Contudo, a análise da dispersão revela um insight importante: a classe **Fake** (vermelha) exibe uma variabilidade significativamente maior, com mais outliers nos extremos negativo e positivo. Isso sugere que as notícias falsas recorrem com mais frequência a uma linguagem carregada de emoção (tanto positiva quanto negativa) para gerar engajamento, indignação ou euforia. As notícias reais, embora possam cobrir eventos com carga emocional, tendem a manter uma distribuição de sentimento mais contida e próxima da neutralidade, refletindo um compromisso com a objetividade.
![Imagem](https://github.com/guilherme-bortoletto/fake-news/edit/main/img/sentimento.png)

#### 7. Análise de Features de Comprimento

Os dados revelam padrões sutis, porém consistentes, na estrutura textual. Em média, os textos classificados como fake news são ligeiramente mais longos, tanto em caracteres (1740.86 vs. 1719.16) quanto em palavras (230.74 vs. 226.66). Esse achado, embora com uma diferença pequena, pode contradizer a percepção de que a desinformação é sempre superficial. Uma hipótese é que alguns conteúdos falsos utilizam uma linguagem mais prolixa e com excesso de detalhes para criar uma falsa aura de profundidade e autoridade, tentando sobrecarregar o leitor com informações para parecerem mais críveis.

| Classe | Média de Caracteres | Média de Palavras |
| :--- | :---: | :---: |
| **Fake News (0)** | 1740.86 | 230.74 |
| **Notícias Reais (1)** | 1719.16 | 226.66 |

## Conclusão

Este trabalho realizou uma análise detalhada de fake news utilizando uma infraestrutura tecnológica eficiente composta por MongoDB Atlas, PyMongo e Databricks. Essa combinação mostrou-se ideal para todo o fluxo de trabalho, desde o armazenamento flexível dos dados em formato JSON até a execução otimizada das análises textuais e estatísticas.

A arquitetura provou-se escalável e flexível, permitindo adaptar a estrutura dos dados conforme novas necessidades surgiam. A análise linguística forneceu evidências robustas sobre as assinaturas da desinformação, como o uso excessivo de nomes de figuras públicas, tom emocional e ausência de fontes atribuídas. O modelo preditivo, baseado nessas características, alcançou um desempenho excelente, validando a eficácia da abordagem.

Os resultados sugerem que a combinação dessas features pode servir como base para sistemas de classificação automática mais sofisticados. Para o público geral, estes achados oferecem indicadores práticos para a identificação de conteúdo suspeito. Como próximos passos, sugere-se a integração dessas descobertas em modelos mais avançados e a criação de materiais educativos que traduzam esses padrões em orientações acessíveis.

## Demais anotações e referências

1.  **MongoDB Atlas e Integração com Python (PyMongo)**
    MongoDB, Inc. (2024). *A Guide to Connect Databricks and MongoDB Atlas using Python API*. CloudThat.
    Disponível em: [https://www.cloudthat.com/resources/blog/a-guide-to-connect-databricks-and-mongodb-atlas-using-python-api](https://www.cloudthat.com/resources/blog/a-guide-to-connect-databricks-and-mongodb-atlas-using-python-api)

2.  **Databricks e Processamento de Dados com MongoDB Atlas**
    Raisinghani, A. (2024). *Utilizing PySpark to Connect MongoDB Atlas with Azure Databricks*. MongoDB Developer Center.
    Disponível em: [https://www.mongodb.com/developer/languages/python/atlas-databricks-pyspark-demo](https://www.mongodb.com/developer/languages/python/atlas-databricks-pyspark-demo)

3.  **Documentação Oficial do PyMongo**
    MongoDB, Inc. (2025). *PyMongo Documentation*.
    Disponível em: [https://pymongo.readthedocs.io](https://pymongo.readthedocs.io)

4.  **Documentação Oficial do Databricks**
    Databricks, Inc. (2025). *Databricks Documentation*.
    Disponível em: [https://docs.databricks.com](https://docs.databricks.com)
