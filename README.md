# Detecção de Entregas em Atraso - DataCo Global
<img src=https://i.ibb.co/Hq1pc1f/navio.png>  
<sub>Fonte: <a href="https://br.pinterest.com/pin/634726759305697560/">Pinterest</a></sub>

## 📌 Descrição do Projeto
Este repositório é o coração do projeto de analytics da DataCo Global, onde mergulhamos profundamente nos dados para desvendar as operações da empresa e otimizar seu desempenho. Nosso foco principal foi na detecção de entregas em atraso, podendo impactar diretamente a satisfação do cliente, gerando implicações financeiras negativas à longo prazo, como perda de reputação e perda de mercado.

📄 [Veja a apresentação do projeto](https://github.com/ejunior029/supplyChain_late_deliveries/blob/main/reports/Apresenta%C3%A7%C3%A3o%20-%20%20DataCo%20Global.pdf)  
📄 [Veja o notebook do case de entregas atrasadas](https://github.com/ejunior029/supplyChain_late_deliveries/blob/main/notebooks/Late_Deliveries_Supply_Chain.ipynb)

## 💼 Entendimento do Negócio

Em um mundo onde transações financeiras se tornaram a grande moeda do comércio global, a segurança e integridade desses processos são de suma importância. Empresas de supply chain que operam internacionalmente estão **cada vez mais vulneráveis a esquemas fraudulentos**, que não só comprometem suas finanças mas também sua credibilidade e operações. 

Ao adentrar a era do big data e aprendizado de máquina, temos a oportunidade de **criar barreiras mais sofisticadas e adaptativas contra a fraude**.

Este projeto desenvolve um **modelo avançado de machine learning** com a capacidade de **detectar e prever atividades fraudulentas** em uma escala global. 
  
  
### **Principais KPIs (Indicadores Chave de Desempenho) de Fraude:**

- **Estratégias de Aceitação:** Melhoria da precisão na aceitação de transações legítimas, reduzindo o impacto dos falsos positivos.
- **Lucro Bruto:** Operações financeiras que eram fraude e que foram corretamente negadas
- **Perda Financeira**: Operações financeiras que eram fraude e que não foram identificadas
- **Lucro Líquido**: Lucro remanescente das fraudes identificadas corretamente e das não identificadas

## 🛠 Pré-Processamento Dos Dados

### _Considerações_
1. Foram consideradas as vendas realizadas somente até setembro/2017 devido a um possível problema operacional na base, que fez com que após esta data apenas vendas a novos clientes fossem consideradas, desconsiderando os clientes antigos. Além disso, os clientes novos compravam apenas uma vez. Sendo assim, para não trabalhar com um conjunto de dados com uma possível interferência, foi realizado este filtro.
2. As colunas a seguir:  
    * *Order Item Product Price*
    * *Sales*
    * *Order Item Total*
    * *Product Price*
    * *Order Profit Per Order*

Foram consideradas como **unidades financeiras** no modelo (ex: Dólar)

### _Foram removidas todas as variáveis não entendidas como importantes para o modelo_

1. Variáveis do tipo *date*
   * *Days for shipping (real)*
   * *Days for shipment (scheduled)*
   * *shipping date (DateOrders)*
   * *order date (DateOrders)*
     
2. Variáveis contendo ID
   * *Customer Id*
   * *Order Customer Id*
   * *Order Id*
   * *Order Item Cardprod Id*
   * *Order Item Id*
   * *Product Card Id*

3. Informações pessoais de clientes
   * *Category Name*
   * *Customer Email*
   * *Customer Fname*
   * *Customer Lname*
   * *Customer Password*

4. Dados de Localização
   * *Customer Street*
   * *Customer Zipcode*
   * *Latitude*
   * *Longitude*

5. Dados de Produtos
   * *Product Description*
   * *Product Image*
   * *Product Status*

6. Dados de Pedidos
   * *Order Profit Per Order*
   * *Delivery Status
   * *Order Status*

7. Demais colunas removidas
   * *Category Name*
   * *Department Name*
  
### _Foi criado um Pipeline para fazer o pré-processamento automaticamente no conjunto de testes, de tal forma que considerasse_:

**1. Tratamento de Variáveis Categóricas**
  * O tratamento de variáveis categóricas foi realizado com o CatBoostEncoder

**2. Tratamento de *Missing Values***
  * O preenchimento de valores faltantes para variáveis categóricas com a moda
  * O preenchimento de valores faltantes para variáveis numéricas com a mediana
 
## 🤖 Modelagem

Os dados foram divididos em 70% para treino e 30% para teste, sendo que são extremamente desbalanceados, conforme a Figura abaixo:

<img src="https://i.ibb.co/7zjpbkF/fraude.png">

Possuindo apenas 2,3% de operações fraudulentas. 

Além disso, foi construída uma função de validação cruzada estratificada (pois a base é desbalanceada), dividindo os dados em 5 folds.

<img src=https://i.ibb.co/RPMwXQ5/vc.png>

Com isso, foram obtidas 5 valores para cada métrica, sendo realizada a média para chegar à um único valor representativo.

## Métricas Obtidas Pelo Modelo
Neste projeto foram utilizados os seguintes modelos:

- LightGBM
- XGBoost
- CatBoost
- Balanced Random Forest

### **Feature Selection**
Como o XGBoost obteve o melhor desempenho geral, foi o modelo escolhido para passar por um processo de feature selection que envolve a seleção das variáveis (atributos) mais importantes para serem usadas na construção do modelo preditivo. O objetivo é **melhorar a performance** do modelo, **reduzir a complexidade computacional** e **evitar o overfitting**. Foi utilizada a técnica chamada ***Recursive Feature Elimination*** (RFE), que é uma abordagem que seleciona atributos por meio de um processo iterativo de ajuste do modelo e remoção das variáveis menos importantes.

### **Tunagem de Hiperparâmetros**
Em seguida o XGBoost passou por um processo de tunagem de hiperparâmetros, através de um algoritmo de otimização bayesiana que teve o objetivo de encontrar os parâmetros que maximizassem a métrica ROC AUC. Ao final do processo, o XGBoost apresentou os seguintes resultados:

- Média da ROC_AUC: 0.9042
- Média da Revocação: 0.9587
- Média da Medida-F1: 0.2344
- Média da Precision-Recall AUC: 0.5428
- Média da Acurácia: 0.8597

Com isso, o XGBoost foi escolhido para identificar o quanto a DataCo Global poderia deixar de perder se possuísse um modelo antifraude para fazer a segurança de operações financeiras de pagamento dos clientes.

## 💡Desempenho Financeiro e Métricas

No processo de extrair o máximo do modelo para evitar perdas financeiras oriundas de operações fraudulentas, foram obtidos os seguintes resultados para métricas e indicadores financeiros:

- **Threshold Ótimo:** 96
- **Economia Significativa:** Implementação do modelo de detecção de fraude resultaria em uma economia estimada de **$1.09 milhão**.
- **Desafios Identificados:** A análise revelou uma perda potencial de **$24 mil** devido a operações fraudulentas não detectadas.
- **Impacto Líquido:** Com as melhorias propostas, a economia total projetada é de aproximadamente **$1.06 milhão**.
- **Razão Lucro/Receita:** 97.7% 

## 🚧 Próximos Passos
A fase subsequente do projeto é o seu deployment, envolvendo a implantação do modelo em um ambiente operacional para avaliação da sua eficiência em condições reais.


