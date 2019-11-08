---
title: archivo de inclusión
description: archivo de inclusión
services: machine-learning
ms.service: machine-learning
ms.custom: include file
ms.topic: include
author: xiaoharper
ms.author: zhanxia
ms.date: 10/18/2019
ms.openlocfilehash: 08833f869a7838fe893b8e941072078f89033c2b
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73511829"
---
1. Inicie sesión en [Azure Machine Learning Studio](https://ml.azure.com).

1. Actualice el área de trabajo a Enterprise Edition.

    Después de la actualización, todos los experimentos de la interfaz visual se convertirán en borradores de canalización en el diseñador.
    
    > [!NOTE]
    > No es necesario actualizar la Enterprise Edition para convertir los servicios Web de interfaz visual en puntos de conexión en tiempo real.
    
1. Vaya a la sección diseñador del área de trabajo para ver la lista de borradores de la canalización. 
    
    Los servicios web convertidos se pueden encontrar en **Puntos de conexión** > **Extremos en tiempo real**.

1. Seleccione un borrador de canalización para abrirlo.

    Si se produjo un error durante el proceso de conversión, aparecerá un mensaje de error con instrucciones para resolver el problema. 

### <a name="known-issues"></a>Problemas conocidos

 A continuación se indican problemas conocidos de migración que deben solucionarse manualmente:

- Módulos de **Importar datos** o **Exportar datos**
        
    Si tiene un módulo de **Importar datos** o **Exportar datos** en el experimento, debe actualizar el origen de datos para que use un almacén de datos. Para más información sobre cómo crear un almacén de datos, consulte [Cómo acceder a los datos en Azure Storage Services](../articles/machine-learning/service/how-to-access-data.md). La información de la cuenta de almacenamiento en la nube se ha agregado a los comentarios del módulo **Importar datos** o **Exportar datos** para su comodidad. 
      