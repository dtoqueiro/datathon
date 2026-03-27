# 🏦 Análise de Sentimento e Dores em Reclamações Financeiras (NLP)
**Tech Challenge - Fase 5 | Pós-Tech Data Science**

![Python](https://img.shields.io/badge/python-3.10%2B-blue.svg)
![Polars](https://img.shields.io/badge/polars-Fast%20DataFrames-yellow)
![DuckDB](https://img.shields.io/badge/duckdb-SQL%20Analytics-orange)
![HuggingFace](https://img.shields.io/badge/%F0%9F%A4%97%20Hugging%20Face-Transformers-yellow)
![PyTorch](https://img.shields.io/badge/PyTorch-%23EE4C2C.svg?style=flat&logo=PyTorch&logoColor=white)

## 📌 Visão Geral do Projeto
Este projeto desenvolve um pipeline escalável de Processamento de Linguagem Natural (NLP) para ingerir, classificar e analisar um volume massivo de reclamações de consumidores financeiros (Consumer Complaints). 

O principal desafio resolvido foi a **ausência de rótulos prévios de sentimento** em um dataset de gigabytes. Para contornar isso com eficiência de hardware (RAM/VRAM), a solução implementa uma arquitetura de dados *Medallion* combinada com a técnica de **Knowledge Distillation (Teacher-Student)**.

---

## 🏗️ Arquitetura da Solução

O pipeline foi desenhado para maximizar a performance analítica e o custo computacional de inferência:

1. **Engenharia de Dados (Out-of-Core):** Ingestão em *streaming* via **DuckDB** e manipulação *Lazy* via **Polars**, permitindo o processamento de milhões de linhas sem gargalos de memória RAM, salvando em formato colunar Parquet (Snappy).
2. **Modelo Teacher (RoBERTa):** Utilização de um modelo pesado para inferência em batch via GPU, gerando *pseudo-labels* de alta precisão.
3. **Gold Rule & Curadoria:** Aplicação de filtro (>75% de confiança do Teacher e descarte de neutros) para criar o *Gold Dataset*.
4. **Modelo Student (DistilBERT):** *Fine-tuning* de um modelo mais leve usando texto processado (spaCy), resultando em uma inferência **~2x mais rápida**, ideal para ambientes de produção.
5. **Business Intelligence:** Aplicação de **BERTopic** estritamente na base de sentimentos negativos para descoberta não-supervisionada das causas-raiz (Dores do Cliente).

---

## 📊 Principais Insights de Negócio

> **Verificação Visual:** Acesse a pasta `reports/business_insights/` para os gráficos completos (Top Produtos, Top Problemas, Top Empresas).

---

## Intstruções de Instalação
Notebook principal é o arquivo 0_notebook_principal.ipynb , antes de rodar certifique-se de instalar as dependências e baixar o arqiuivo [complaints.csv.zip](https://www.consumerfinance.gov/data-research/consumer-complaints/#get-the-data) e colocálo no caminho: data/raw/
