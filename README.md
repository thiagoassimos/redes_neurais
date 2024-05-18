## Descrição do problema

Os dois conjuntos de dados estão relacionados com variantes tintas e brancas do vinho português “Vinho Verde”. A referência [Cortez et al., 2009]. Devido a questões de privacidade e logística, apenas estão disponíveis variáveis físico-químicas (entradas) e sensoriais (saída) (por exemplo, não existem dados sobre tipos de uva, marca do vinho, preço de venda do vinho, etc.).
As classes são ordenadas e não equilibradas (ex. há muito mais vinhos normais do que excelentes ou ruins). Algoritmos de detecção de valores discrepantes poderiam ser usados para detectar alguns vinhos excelentes ou ruins. Além disso, não temos certeza se todas as variáveis de entrada são relevantes. 

Vamos usar redes neurais para classificar esses vinhos como bons ou ruins.

Neste modelo de rede neural serão usadas **```2 camadas```**: **```1 de entrada```** e **```1 de saída```**, com **```11 neurônios na camada de entrada```** e **```1 na camada de saída```**. Como só temos 1 classe binária então vamos usar 1 neurônio por classe, já que o valor produzido por esse neurônio será interpretado como a probabilidade do vinho pertencer à classe positiva. Isso justifica o fato de não utilizarmos neurônios nas camadas intermediárias.

Algoritmo implementado para treinamento é o **```backpropagation```**.

## ```Funções de ativação```

- camada de entrada: **```relu```**;
- camada oculta: não será implementada, apesar da função generalizar para camadas ocultas, caso queira adicionar posteriormente;
- camada de saída: **```sigmóide```**.

A escolha pela **```relu```** na camada de entrada é porque como os neurônios somente são ativados com inputs positivos, então isso impede que todos os neurônios sejam ativados ao mesmo tempo. Um outro ponto importante é que ela torna o treinamento computacionalmente mais eficiente, permitindo um treinamento mais focado, isto é, com neurônios mais especializados.

Na camada de saída a escolha pela **```sigmóide```** se deve ao fato dela transformar a saída da última camada em um valor no intervalo entre 0 e 1, representando uma probabilidade. Como este é um problema de classificação binária, esse valor pode ser interpretado como a probabilidade de pertencer à classe positiva, como dito anteriormente. Além disso a sigmóide é uma função de classe $C^\infty$, ou seja, todas as suas derivadas são contínuas, o que facilita o cálculo dos gradientes durante o treinamento quando se usa, por exemplo, o **```backpropagation```**. Ela também ajuda a evitar o problema de "explodir" gradientes, que pode ocorrer durante o treinamento de redes neurais mais profundas, pois limita a saída entre 0 e 1, evitando esses valores extremamente grandes.

## Função de otimização

Embora o algoritmo **```Adam```** possua convergência mais rápida, a escolha é pelo **```SGD```** devido ao fato dele generalizar melhor que o primeiro e assim apresentar melhores resultados.

### ```Métrica utilizada```: ```F-Beta-Score```

Estamos lidando com um problema de classificação binária, do tipo: 
- 1 para vinhos bons; 
- 0 para vinhos ruins. 
  
Então, deve ser dada uma ênfase maior para os erros por falso positivo (vinho ruim classificado como bom) e falso negativo (vinho bom classificado como ruim), respectivamente essas métricas são a **```precision```** e a **```recall```**. Sendo assim, seria mais interessante usar a média harmônica entre elas que é definida pela **```f-beta score```**, ou seja, um valor alto de **```f-beta score```** só ocorrerá se a **```recall```** e **```precision```** forem altas. Além disso, a **```f-beta score```** também é uma boa métrica para trabalhar com classes desbalanceadas, que é o caso deste problema, onde temos aproximadamente **```66,51%```** de vinhos classificados como bons e **```33,49%```** classificados como ruins.

# Resultados

![Alt text](image.png)

```python

  Modelo escolhido: 11 neurônios na camada de entrada e 1 na camada de saída

      TP: 53,7%
      TN: 22,7%
      FP: 10,8%
      FN: 12,8%  
```

```python

  Métrica para a matriz de confusão: F-Beta Score

      0: 0.64
      1: 0.83
  Lembrando que 0 só diz respeito à taxa de TN e 1 à taxa de TP.
```

![Alt text](image-1.png)


## Considerações finais

Um ponto que deve ser relatado para a apresentação dos resultados é que há um desbalanceamento de classes em relação à quantidade de vinhos bons e ruins no dataset (temos mais vinhos bons do que vinhos ruins).

A partir da análise da matriz de confusão podemos inferir que o modelo apresenta bons resultados, com uma taxa total de acerto de **```76,4% (TP+TN)```** e de erro **```23,6% (FP+FN)```**. 

Uma outra observação é que aumentar a quantidade de neurônios não vai produzir melhora nos resultados, pois com **```22```** e **```33 neurônios```** os resultados mantiveram-se foram ligeiramente inferiores a **```11 neurônios```**. 

 No caso dos histogramas, podemos dizer que os dados de treino e teste possuem amplitudes semelhantes, indicando que o modelo não está se adaptando demais aos dados de treino. Isso é um bom sinal quando se leva em conta a capacidade de generalização do modelo para novos conjuntos de dados.