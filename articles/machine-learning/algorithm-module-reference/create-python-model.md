---
title: 'Create Python Model: referencia para los módulos'
titleSuffix: Azure Machine Learning
description: Obtenga información sobre cómo usar el modelo Creación de modelo Python en Azure Machine Learning para crear el módulo de modelado personalizado o de procesamiento de datos.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 11/19/2019
ms.openlocfilehash: e26ae7ded47a235ecf1829946c1cc79af9a0fb8c
ms.sourcegitcommit: 812bc3c318f513cefc5b767de8754a6da888befc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2020
ms.locfileid: "77152404"
---
# <a name="create-python-model"></a>Creación de modelo Python

En este artículo se describe un módulo del diseñador de Azure Machine Learning (versión preliminar).

Aprenda a usar el módulo **Creación de modelo Python** para crear un modelo no entrenado a partir de un script de Python. Puede basar el modelo en cualquier aprendiz incluido en un paquete de Python en el entorno del diseñador de Azure Machine Learning. 

Después de crear el modelo, puede usar [Train model](train-model.md) (Entrenar modelo) para entrenar el modelo en un conjunto de datos, al igual que cualquier otro aprendiz en Azure Machine Learning. El modelo entrenado puede pasarse a [Score Model](score-model.md) (Puntuación del modelo) para usar el modelo para hacer predicciones. Luego, se puede guardar el modelo entrenado y se puede publicar el flujo de trabajo de puntuación como servicio web.

> [!WARNING]
> Actualmente no es posible pasar los resultados puntuados de un modelo de Python a [Evaluate Model](evaluate-model.md). Si tiene que evaluar un modelo, puede escribir script de Python personalizado y ejecutarlo mediante el módulo [Execute Python Script](execute-python-script.md) (Ejecutar script de Python).  


## <a name="how-to-configure-create-python-model"></a>Cómo configurar Create Python Model

El uso de este módulo requiere conocimientos intermedios o expertos de Python. El módulo admite el uso de cualquier aprendiz que se incluya en los paquetes de Python ya instalados en Azure Machine Learning. Consulte la lista de paquetes de Python instalado previamente en [Execute Python Script](execute-python-script.md).
  

En este artículo se mostrará cómo usar **Create Python Model** (Crear modelo de Python) con una canalización sencilla. A continuación se muestra el gráfico de la canalización.

![crear-python-model](./media/module/aml-create-python-model.png)

1.  Haga clic en **Create Python Model** (Creación de modelo de Python), edite el script para implementar el proceso de modelado o de administración de datos. Puede basar el modelo en cualquier aprendiz que se incluya en un paquete de Python en el entorno de Azure Machine Learning.


    A continuación encontrará un código de ejemplo de un clasificador bayesiano ingenuo de dos clases con el conocido paquete *sklearn*.

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


2. Conecte el módulo **Create Python Model** que acaba de crear a **Train Model** y **Score Model**.

3. Si tiene que evaluar el modelo, agregue un módulo [Execute Python Script](execute-python-script.md) y edite el script de Python para implementar la evaluación.

A continuación encontrará un código de evaluación.

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
