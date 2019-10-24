---
title: Creación de un flujo de datos de asignación de Azure Data Factory
description: Cómo crear un flujo de datos de asignación de Azure Data Factory
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: 31528c0ac7796aafbde74a3814d971138ec4ba2e
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2019
ms.locfileid: "72387257"
---
# <a name="create-azure-data-factory-data-flow"></a>Creación de un flujo de datos con Azure Data Factory



Los flujos de datos de asignación de ADF proporcionan una manera de transformar los datos a escala sin necesidad de programar. Puede diseñar un trabajo de transformación de datos en el diseñador de flujos de datos realizando una serie de transformaciones. Comience realice todas las transformaciones que desee en el origen y, después, continúe con los pasos de transformación de datos. A continuación, complete el flujo de datos con los receptores para enviar los resultados a un destino.

Primero, cree una factoría de datos V2 en Azure Portal. Después de crear la nueva factoría, haga clic en el icono "Crear y supervisar" para abrir la interfaz de usuario de Data Factory.

![Opciones de Data Flow](media/data-flow/v2portal.png "creación de un flujo de datos")

Cuando esté en la interfaz de usuario de Data Factory, puede usar flujos de datos de ejemplo. Los ejemplos están disponibles en la galería de plantillas de ADF. En ADF, seleccione "Create Pipeline from Template" (Crear canalización con plantilla) y seleccione la categoría Data Flow (Flujo de datos) en la galería de plantillas.

![Opciones de Data Flow](media/data-flow/template.png "creación de un flujo de datos")

Se le pedirá que escriba la información de la cuenta de Azure Blob Storage.

![Opciones de Data Flow](media/data-flow/template2.png "creación de un flujo de datos 2")

[Aquí encontrará los datos usados en estos ejemplos](https://github.com/kromerm/adfdataflowdocs/tree/master/sampledata). Descargue los datos de ejemplo y guarde los archivos en las cuentas de Azure Blob Storage para poder ejecutar los ejemplos.

## <a name="create-new-data-flow"></a>Creación de un flujo de datos

Use el botón más situado en la interfaz de usuario de ADF para crear flujos de datos.

![Opciones de Data Flow](media/data-flow/newresource.png "Nuevo recurso")

## <a name="next-steps"></a>Pasos siguientes

Empiece a crear la transformación de datos con una [transformación de origen](data-flow-source.md).
