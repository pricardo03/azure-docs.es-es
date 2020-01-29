---
title: 'Exportación de datos: referencia para los módulos'
titleSuffix: Azure Machine Learning
description: Obtenga información sobre cómo usar el módulo Exportación de datos en Azure Machine Learning para guardar los resultados, los datos intermedios y los datos de trabajo de las canalizaciones en los destinos de almacenamiento en la nube fuera de Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 10/22/2019
ms.openlocfilehash: 7f8d3bb6452ee3260e5a89feb37c374418fc6943
ms.sourcegitcommit: a9b1f7d5111cb07e3462973eb607ff1e512bc407
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2020
ms.locfileid: "76312166"
---
# <a name="export-data-module"></a>Módulo Exportación de datos

En este artículo se describe un módulo del diseñador de Azure Machine Learning.

Utilice este módulo para guardar los resultados, los datos intermedios y los datos de trabajo de las canalizaciones en los destinos de almacenamiento en la nube fuera de Azure Machine Learning. 

Este módulo admite la exportación de los datos a los siguientes servicios de datos en la nube:

- Azure Blob Container
- Recurso compartido de archivos de Azure
- Azure Data Lake
- Azure Data Lake Gen2

Antes de exportar los datos, debe registrar un almacén de datos en el área de trabajo de Azure Machine Learning. Para obtener más información, consulte [Cómo acceder a datos](../how-to-access-data.md).

## <a name="how-to-configure-export-data"></a>Procedimiento para configurar la exportación de datos

1. Agregue el módulo **Exportación de datos** a la canalización en el diseñador. Puede encontrar este módulo en la categoría **Entrada y salida**.

1. Conecte **Exportación de datos** al módulo que contiene los datos que desea exportar.

1. Seleccione **Exportación de datos** para abrir el panel **Propiedades**.

1. En **Almacén de datos**, seleccione un almacén de datos existente en la lista desplegable. También puede crear un nuevo almacén de datos. Para ver cómo, visite [how-to-access-data](../how-to-access-data.md)

1. Defina la ruta del almacén de datos en el que se van a escribir los datos. 


1. Para **Formato de archivo**, seleccione el formato en el que se deben almacenar los datos.
 
1. Ejecución de la canalización

## <a name="next-steps"></a>Pasos siguientes

Consulte el [conjunto de módulos disponibles](module-reference.md) para Azure Machine Learning. 