---
title: Cuadernos de Jupyter de ejemplo con un conjunto de datos de NOAA abierto
titleSuffix: Azure Open Datasets
description: Use los cuadernos de Jupyter de ejemplo para Azure Open Datasets para obtener información sobre cómo cargar los conjuntos de datos abiertos y usarlos para enriquecer los datos de demostración. Las técnicas incluyen el uso de Spark y Pandas para procesar los datos.
ms.service: open-datasets
ms.topic: sample
author: cjgronlund
ms.author: cgronlun
ms.date: 05/02/2019
ms.openlocfilehash: c1b86199f13454f4785a6737b25e489d45dd53f8
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65027551"
---
# <a name="example-jupyter-notebooks-show-how-to-enrich-data-with-open-datasets"></a>Los cuadernos de Jupyter de ejemplo muestran cómo enriquecer los datos con Open Datasets 
Los cuadernos de Jupyter de ejemplo para Azure Open Datasets le muestran cómo cargar conjuntos de datos abiertos y usarlos para enriquecer los datos de demostración. Las técnicas incluyen el uso de Apache Spark y Pandas para procesar los datos.

>[!IMPORTANT]
>Al trabajar en un entorno distinto de Spark, Open Datasets permite descargar un solo mes de datos a la vez, con determinadas clases con el fin de evitar MemoryError con grandes conjuntos de datos.

## <a name="load-noaa-integrated-surface-database-isd-data"></a>Cargar datos de Integrated Surface Database (ISD) de NOAA 
|Bloc de notas        | DESCRIPCIÓN                                    |
|----------------|------------------------------------------------|
|[Cargar un mes reciente de datos meteorológicos en un dataframe de Pandas](https://github.com/Azure/OpenDatasetsNotebooks/blob/master/tutorials/data-access/02-weather-to-pandas-dataframe.ipynb) | Obtenga información sobre cómo cargar los datos meteorológicos históricos en su dataframe de Pandas favorito. |
|[Cargar un mes reciente de datos meteorológicos en un dataframe de Spark](https://github.com/Azure/OpenDatasetsNotebooks/blob/master/tutorials/data-access/01-weather-to-spark-dataframe.ipynb) | Obtenga información sobre cómo cargar los datos meteorológicos históricos en su dataframe de Spark favorito.  |

## <a name="join-demo-data-with-noaa-isd-data"></a>Unir datos de demostración con datos de ISD de NOAA 
|Bloc de notas        | DESCRIPCIÓN                                    |
|----------------|------------------------------------------------|
|[Unir datos de demostración con datos meteorológicos: Pandas ](https://github.com/Azure/OpenDatasetsNotebooks/blob/master/tutorials/data-join/02-weather-join-in-pandas.ipynb) | Una un conjunto de datos de demostración de un mes de ubicaciones de sensores con lecturas meteorológicas en un dataframe de Pandas.  |
|[Unir datos de demostración con datos meteorológicos: Spark](https://github.com/Azure/OpenDatasetsNotebooks/blob/master/tutorials/data-join/01-weather-join-in-spark.ipynb) | Una un conjunto de datos de demostración de ubicaciones de sensores con lecturas meteorológicas en un dataframe de Spark. |

## <a name="join-nyc-taxi-data-with-noaa-isd-data"></a>Unir datos de taxis de Nueva York con datos de ISD de NOAA 
|Bloc de notas        | DESCRIPCIÓN                                    |
|----------------|------------------------------------------------|
|[Datos de viajes de taxis, enriquecidos con datos meteorológicos: Pandas](https://github.com/Azure/OpenDatasetsNotebooks/blob/master/tutorials/data-join/04-nyc-taxi-join-weather-in-pandas.ipynb) | Cargue datos de taxis verdes de la ciudad de Nueva York (más de un mes) y enriquézcalos con datos meteorológicos en un dataframe de Pandas. Este ejemplo reemplaza el método `get_pandas_limit` y equilibra el rendimiento de carga de datos con la cantidad de datos.|
|[Datos de viajes de taxis enriquecidos con datos meteorológicos: Spark](https://github.com/Azure/OpenDatasetsNotebooks/blob/master/tutorials/data-join/03-nyc-taxi-join-weather-in-spark.ipynb) | Cargue datos de taxis verdes de la ciudad de Nueva York y enriquézcalos con datos meteorológicos en un dataframe de Spark.  |

## <a name="next-steps"></a>Pasos siguientes

* [Tutorial: Regression modeling with automated machine learning and an open dataset](tutorial-opendatasets-automl.md) (Tutorial: Modelado de regresión con aprendizaje automático automatizado y un conjunto de datos abierto)
* [SDK de Python para Open Datasets](https://aka.ms/open-datasets-api)
* [Catálogo de Azure Open Datasets](https://azure.microsoft.com/services/open-datasets/catalog/)
