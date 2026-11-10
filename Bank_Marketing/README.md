# Bank Marketing: Previsão de Adesão a Depósitos a Prazo

## Sobre o projeto

Este projeto utiliza técnicas de Machine Learning para prever quais clientes de uma instituição bancária têm maior probabilidade de aderir a uma oferta de depósito a prazo (*term deposit*), com base em informações demográficas, financeiras e relacionadas às campanhas de marketing.

O objetivo é desenvolver modelos de classificação capazes de auxiliar na identificação de potenciais clientes, contribuindo para a priorização de contatos em campanhas de marketing direto.

O projeto utiliza o conjunto de dados **Bank Marketing**, disponibilizado pela UCI Machine Learning Repository, no qual a variável-alvo indica se o cliente aderiu (`yes`) ou não (`no`) ao produto oferecido.

## Desenvolvimento

O trabalho envolveu as principais etapas de um processo de modelagem preditiva, incluindo:

- **Análise exploratória:** investigação das características do conjunto de dados, distribuição das variáveis, valores ausentes e desbalanceamento da variável-alvo.
- **Pré-processamento:** tratamento das variáveis numéricas e categóricas, codificação de atributos e preparação dos dados para os algoritmos.
- **Divisão dos dados:** separação estratificada em conjuntos de treinamento (70%), validação (10%) e teste (20%), preservando o conjunto de teste para a avaliação final.
- **Comparação de modelos:** avaliação de Regressão Logística, Árvore de Decisão, Random Forest e XGBoost.
- **Seleção do modelo:** aplicação de uma estratégia de ranking híbrido baseada em *Reciprocal Rank Fusion* (RRF), combinando F1-score, PR-AUC e ROC-AUC.
- **Otimização de hiperparâmetros:** utilização de Grid Search com validação cruzada no conjunto de treinamento.
- **Otimização do threshold:** avaliação de estratégias baseadas na maximização do F1-score e do índice de Youden, comparando seus resultados com o threshold padrão de 0,5.
- **Avaliação final:** análise das métricas de classificação e da matriz de confusão no conjunto de teste, sem utilizá-lo nas etapas de seleção e otimização.

A otimização do threshold considerou não apenas o ganho de F1-score, mas também as variações de FPR e Recall em relação ao threshold padrão. Dessa forma, a escolha buscou equilibrar a identificação de potenciais clientes e o controle de classificações positivas incorretas.

## Experimentos: com e sem `duration`

Foram desenvolvidos dois notebooks com a mesma metodologia, diferenciando-se principalmente pela utilização da variável `duration`, que representa a duração da ligação telefônica.

Essa separação permite investigar como a disponibilidade dessa informação influencia o desempenho dos modelos e as possibilidades de aplicação dos resultados.

### Experimento 1: Sem `duration`

Neste cenário, a variável `duration` foi excluída porque representa uma informação que somente se torna conhecida durante ou após a ligação. O objetivo é simular uma situação de prospecção na qual o banco precisa identificar potenciais clientes antes de realizar o contato.

Um modelo de XGBoost apresentou o melhor desempenho geral entre os algoritmos avaliados. Na avaliação final, os resultados foram:

| Métrica | Resultado |
|---|---:|
| ROC-AUC | 0,7972 |
| PR-AUC | 0,4599 |
| F1-score | 0,4829 |
| Acurácia | 86,97% |
| Precisão | 45,08% |
| Recall | 51,98% |
| FPR | 8,39% |


Dos 1.058 clientes que aderiram ao depósito no conjunto de teste, o modelo identificou corretamente 550. Além disso, entre os clientes classificados como potenciais interessados, 45,08% efetivamente aderiram ao produto, aproximadamente 3,85 vezes a prevalência original de adesões.

### Experimento 2: Com `duration`

Neste segundo cenário, a variável `duration` foi incorporada aos atributos utilizados pelo modelo, permitindo avaliar seu impacto na capacidade preditiva.

O XGBoost novamente apresentou o melhor desempenho geral, alcançando os seguintes resultados na avaliação final:

| Métrica | Resultado |
|---|---:|
| ROC-AUC | 0,9334 |
| PR-AUC | 0,6310 |
| F1-score | 0,6137 |
| Acurácia | 89,67% |
| Precisão | 54,56% |
| Recall | 70,13% |
| FPR | 7,74% |


Nesse cenário, o modelo identificou corretamente 742 dos 1.058 clientes que aderiram ao depósito. A precisão de 54,56% também representa uma concentração de adesões aproximadamente 4,66 vezes maior que a prevalência original.

### Comparação dos resultados

| Métrica | Sem `duration` | Com `duration` |
|---|---:|---:|
| Adesões identificadas | 550 | 742 |
| ROC-AUC | 0,7972 | 0,9334 |
| PR-AUC | 0,4599 | 0,6310 |
| F1-score | 0,4829 | 0,6137 |
| Acurácia | 86,97% | 89,67% |
| Precisão | 45,08% | 54,56% |
| Recall | 51,98% | 70,13% |
| FPR | 8,39% | 7,74% |
| Falsos positivos | 670 | 618 |

A inclusão de `duration` proporcionou uma melhora consistente em todas as métricas apresentadas, permitindo identificar **192 adesões adicionais** e reduzir em **52 o número de falsos positivos** no conjunto de teste.

É importante considerar, entretanto, que os dois experimentos representam cenários de aplicação diferentes. Enquanto o modelo sem `duration` pode auxiliar na priorização de clientes antes das ligações, o modelo com essa variável depende de uma informação que só está disponível durante ou após o contato.

Assim, o primeiro cenário representa a aplicação mais adequada à prospecção, enquanto o segundo demonstra o potencial preditivo adicional proporcionado pela disponibilidade da duração da chamada.

## Tecnologias e bibliotecas

- Python
- Pandas e NumPy
- Matplotlib e Seaborn
- Scikit-learn
- XGBoost
- Jupyter Notebook

## Estrutura do projeto

- `bank_marketing_sem_duration.ipynb`: análise e modelagem sem a variável `duration`.
- `bank_marketing_com_duration.ipynb`: análise e modelagem incorporando a variável `duration`.

## Considerações finais

O projeto permitiu explorar diferentes algoritmos de classificação, estratégias de seleção de modelos, otimização de hiperparâmetros e calibração de thresholds em um problema de classificação desbalanceado.

Além da comparação entre algoritmos, os experimentos demonstram a importância de considerar o contexto de aplicação e a disponibilidade das variáveis no momento da previsão. A comparação entre os cenários com e sem `duration` evidencia como uma informação adicional pode alterar significativamente o desempenho preditivo e os resultados obtidos sob uma perspectiva de negócio.