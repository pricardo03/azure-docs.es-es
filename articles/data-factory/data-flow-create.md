---
title: Creación de un flujo de datos de asignación con Azure Data Factory Mapping Data Flow
description: Cómo crear un Azure Data Factory asignación de flujo de datos
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: 366ed60534544ebbf889e2f72fe703f9b821f871
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2019
ms.locfileid: "65235657"
---
# <a name="create-azure-data-factory-data-flow"></a>Creación de un flujo de datos con Azure Data Factory

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Los flujos de datos de asignación de ADF proporcionan una manera de transformar los datos a escala sin necesidad de programar. Puede diseñar un trabajo de transformación de datos en el diseñador de flujos de datos realizando una serie de transformaciones. Comience realice todas las transformaciones que desee en el origen y, después, continúe con los pasos de transformación de datos. A continuación, complete el flujo de datos con los receptores para enviar los resultados a un destino.

Comience creando primero una nueva factoría de datos de V2 en Azure portal. Después de crear la nueva factoría, haga clic en el icono "Crear y supervisar" para abrir la interfaz de usuario de Data Factory.

![Opciones de flujos de datos](media/data-flow/v2portal.png "data flow create")

Cuando esté en la interfaz de usuario de Data Factory, puede usar flujos de datos de ejemplo. Los ejemplos están disponibles en la galería de plantillas de ADF. En ADF, seleccione "Create Pipeline from Template" (Crear canalización con plantilla) y seleccione la categoría Data Flow (Flujo de datos) en la galería de plantillas.

![Opciones de flujos de datos](media/data-flow/template.png "data flow create")

Se le pedirá que escriba la información de la cuenta de Azure Blob Storage.

![Opciones de flujos de datos](media/data-flow/template2.png "data flow create 2")

[Aquí encontrará los datos usados en estos ejemplos](https://github.com/kromerm/adfdataflowdocs/tree/master/sampledata). Descargue los datos de ejemplo y guarde los archivos en las cuentas de Azure Blob Storage para poder ejecutar los ejemplos.

## <a name="create-new-data-flow"></a>Crear nuevo flujo de datos

Crear recurso "signo" situado en la UI de ADF para crear flujos de datos de uso.

![Opciones de flujo de datos](media/data-flow/newresource.png "New Resource")

## <a name="next-steps"></a>Pasos siguientes

Comience a crear la transformación de datos con un [transformación del origen](data-flow-source.md).
