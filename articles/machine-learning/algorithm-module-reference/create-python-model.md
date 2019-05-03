---
title: 'Crear el modelo de Python: Referencia de módulo'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo utilizar el modelo de crear el modelo de Python en el servicio Azure Machine Learning para crear el módulo personalizado de modelado o procesamiento de datos.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/06/2019
ROBOTS: NOINDEX
ms.openlocfilehash: ea9b50cede3e2d264ca0476b6a987ca6896c3c79
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029751"
---
# <a name="create-python-model"></a>Crear el modelo de Python

En este artículo se describe cómo usar el **crear modelo de Python** módulo para crear un modelo no entrenado desde un script de Python. 

Puede basar el modelo en cualquier aprendiz que se incluye en un paquete de Python en el entorno de Azure Machine Learning. 

Después de crear el modelo, puede usar [entrenar modelo](train-model.md) para entrenar el modelo en un conjunto de datos, al igual que cualquier otro aprendiz en Azure Machine Learning. El modelo entrenado puede pasarse a [Score Model](score-model.md) para utilizar el modelo para realizar predicciones. A continuación, se puede guardar el modelo entrenado y se puede publicar el flujo de trabajo de puntuación como un servicio web.

> [!WARNING]
> Actualmente no es posible pasar los resultados puntuados de un modelo de Python para [Evaluate Model](evaluate-model.md). Si necesita evaluar un modelo, puede escribir script de Python personalizado y ejecutarlo mediante el [Execute Python Script](execute-python-script.md) módulo.  


## <a name="how-to-configure-create-python-model"></a>Cómo configurar crear modelo de Python

Uso de este módulo requiere conocimientos intermedios o experto de Python. El módulo admite el uso de cualquier aprendiz que se incluye en los paquetes de Python instalados en Azure Machine Learning. Consulte la lista de paquetes de Python instalado previamente en [Execute Python Script](execute-python-script.md).
  

En este artículo se mostrará cómo utilizar el **crear modelo de Python** con un experimento sencillo. A continuación es el gráfico del experimento.

![create-python-model](./media/module/aml-create-python-model.png)

1.  Haga clic en **crear modelo de Python**, edite el script para implementar el modelado o procesar de administración de datos. Puede basar el modelo en cualquier aprendiz que se incluye en un paquete de Python en el entorno de Azure Machine Learning.


    A continuación es un código de ejemplo de clasificador Bayes Naive de dos clases mediante el popular *sklearn* paquete.

```Python

# The script MUST define a class named AzureMLModel.
# This class MUST at least define the following three methods:
    # __init__: in which self.model must be assigned,
    # train: which trains self.model, the two input arguments must be pandas DataFrame,
    # predict: which generates prediction result, the input argument and the prediction result MUST be pandas DataFrame.
# The signatures (method names and argument names) of all these methods MUST be exactly the same as the following example.


import pandas as pd
from sklearn.naive_bayes import GaussianNB


class AzureMLModel:
    def __init__(self):
        self.model = GaussianNB()
        self.feature_column_names = list()

    def train(self, df_train, df_label):
        self.feature_column_names = df_train.columns.tolist()
        self.model.fit(df_train, df_label)

    def predict(self, df):
        return pd.DataFrame(
            {'Scored Labels': self.model.predict(df[self.feature_column_names]), 
             'probabilities': self.model.predict_proba(df[self.feature_column_names])[:, 1]}
        )


```


2. Conectar el **crear modelo de Python** módulo que acaba de crear a un **Train Model** y **puntuar modelo**

3. Si tiene que evaluar el modelo, agregue un [Execute Python Script](execute-python-script.md) y edite el script de Python para implementar la evaluación.

A continuación son ejemplos de código de evaluación.

```Python


# The script MUST contain a function named azureml_main
# which is the entry point for this module.

# imports up here can be used to 
import pandas as pd

# The entry point function can contain up to two input arguments:
#   Param<dataframe1>: a pandas.DataFrame
#   Param<dataframe2>: a pandas.DataFrame
def azureml_main(dataframe1 = None, dataframe2 = None):
    
    from sklearn.metrics import accuracy_score, precision_score, recall_score, roc_auc_score, roc_curve
    import pandas as pd
    import numpy as np
    
    scores = dataframe1.ix[:, ("income", "Scored Labels", "probabilities")]
    ytrue = np.array([0 if val == '<=50K' else 1 for val in scores["income"]])
    ypred = np.array([0 if val == '<=50K' else 1 for val in scores["Scored Labels"]])    
    probabilities = scores["probabilities"]
    
    accuracy, precision, recall, auc = \
    accuracy_score(ytrue, ypred),\
    precision_score(ytrue, ypred),\
    recall_score(ytrue, ypred),\
    roc_auc_score(ytrue, probabilities)
    
    metrics = pd.DataFrame();
    metrics["Metric"] = ["Accuracy", "Precision", "Recall", "AUC"];
    metrics["Value"] = [accuracy, precision, recall, auc]
    
    return metrics,

```
