# Detecção de Fake News com Processamento em Larga Escala

**Autores:**

* Vinicius Silva Castro – 802138

* Guilherme Campos Bortoletto – 801477

**Ano:** 2025

---

## 📄 Resumo

Este projeto desenvolve um pipeline completo para a detecção automática de *fake news*, integrando tecnologias de armazenamento NoSQL e processamento de dados em larga escala. Utilizando o **MongoDB Atlas** como banco de dados e o **Apache Spark (PySpark)** como plataforma de processamento distribuído, o sistema é capaz de armazenar, transformar e analisar um grande volume de notícias jornalísticas.

A solução utiliza um dataset do Kaggle com aproximadamente 44.000 notícias, que são pré-processadas e inseridas no MongoDB. Em seguida, o pipeline com Spark executa tarefas de Processamento de Linguagem Natural (NLP), como tokenização e vetorização com TF-IDF, para treinar modelos de *machine learning* capazes de classificar textos como verdadeiros ou falsos. Além da classificação, o projeto inclui uma análise exploratória para identificar padrões linguísticos, como as palavras mais frequentes em conteúdos falsos, oferecendo insights sobre o vocabulário da desinformação.

## 🎯 Objetivo

O objetivo principal é construir um pipeline de ponta a ponta para a detecção de notícias falsas, demonstrando a aplicação de tecnologias modernas para:

* **Armazenar** de forma flexível e escalável um grande volume de notícias reais e falsas.

* **Processar** os dados textuais em larga escala com técnicas de NLP.

* **Treinar e avaliar** um modelo de *machine learning* para classificação automática de notícias.

* **Avaliar a viabilidade** da arquitetura proposta (MongoDB + Spark) em um contexto de aplicação real que demanda escalabilidade e robustez.

* **Identificar padrões linguísticos** em textos de *fake news* através de análises exploratórias.

## 🛠️ Tecnologias Escolhidas

### MongoDB Atlas

A escolha de um banco de dados NoSQL como o **MongoDB** foi fundamental devido à natureza semi-estruturada das notícias.

* **Modelo de Documentos Flexível:** Utiliza documentos JSON, que se adaptam perfeitamente a campos como título, conteúdo e metadados variáveis.

* **Escalabilidade na Nuvem:** Por ser uma solução gerenciada, o Atlas permite fácil escalabilidade horizontal para lidar com grandes volumes de dados.

* **Recursos Nativos:** Oferece indexação e busca textual otimizadas, além de integração direta com Apache Spark, o que é essencial para o processamento distribuído.

### Apache Spark (PySpark)

Para processar um grande volume de texto de forma eficiente, o **Apache Spark** foi a escolha ideal.

* **Processamento Distribuído:** Sua arquitetura permite escalar o processamento horizontalmente, garantindo performance.

* **Ecossistema de Machine Learning:** Conta com bibliotecas integradas como **Spark MLlib** e **Spark NLP**, que facilitam o treinamento de modelos e o tratamento linguístico.

* **Execução Otimizada:** O uso de DataFrames e a execução preguiçosa (*lazy execution*) contribuem para uma manipulação de dados mais rápida e eficiente.

### Integração MongoDB + Spark

A combinação das duas tecnologias é o grande diferencial da arquitetura. O conector oficial permite que o Spark leia e escreva dados diretamente no MongoDB, possibilitando o processamento *in-place* e reduzindo a latência ao evitar a movimentação desnecessária de dados.

## 📊 Fonte de Dados

* **Dataset:** Fake and Real News Dataset – Kaggle

* **Link:** [`https://www.kaggle.com/datasets/clmentbisaillon/fake-and-real-news-dataset`](https://www.kaggle.com/datasets/clmentbisaillon/fake-and-real-news-dataset)

* **Conteúdo:** Aproximadamente 44.000 artigos, divididos em `Fake.csv` e `True.csv`.

* **Atributos:** `title`, `text`, `subject`, `date`.

## ⚙️ Arquitetura e Fluxo do Projeto

O pipeline do projeto segue as seguintes etapas:

1. **Obtenção dos Dados:** Download dos arquivos `.csv` do Kaggle.

2. **Pré-processamento e Ingestão:** Um script em Python realiza uma limpeza inicial e insere as notícias como documentos JSON no MongoDB Atlas, em *collections* separadas (`Fake` e `Real`).

3. **Leitura com Spark:** O Apache Spark se conecta ao MongoDB Atlas via conector e carrega os dados em um DataFrame distribuído.

4. **Processamento com PySpark:** São aplicadas transformações de NLP:

   * Tokenização.

   * Remoção de *stopwords*.

   * Vetorização com **TF-IDF**.

5. **Treinamento do Modelo:** Algoritmos como **Naive Bayes** e **Regressão Logística** são treinados com os dados vetorizados.

6. **Análise Exploratória:** Em paralelo, são geradas estatísticas para identificar padrões, como a frequência de palavras e n-gramas.

## 📈 Resultados Esperados

* **Pipeline Funcional:** Um fluxo de trabalho de ponta a ponta, demonstrando a integração bem-sucedida entre MongoDB Atlas e Apache Spark.

* **Modelo de Classificação Avaliado:** Um modelo de *machine learning* treinado e avaliado com métricas de desempenho como **acurácia, precisão e recall**.

* **Análise Linguística:** Geração de visualizações e estatísticas, como nuvens de palavras e rankings de termos mais frequentes, que revelem os padrões de vocabulário usados em notícias falsas.
