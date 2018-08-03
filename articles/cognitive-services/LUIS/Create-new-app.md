---
title: Creación de una aplicación con LUIS | Microsoft Docs
description: Cree y administre las aplicaciones en la página web de Language Understanding (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 04/17/2018
ms.author: v-geberr
ms.openlocfilehash: 998a85720f5707fbf6ed4c5cfa3ed0dab5d1cc0e
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/06/2018
ms.locfileid: "37865052"
---
# <a name="create-an-app"></a>Crear una aplicación
Una aplicación se crea de varias maneras: 

* [Comience](#create-new-app) con una aplicación vacía y cree intenciones, expresiones y entidades.
* [Comience](#create-new-app) con una aplicación vacía y agregue un [dominio creado previamente](luis-how-to-use-prebuilt-domains.md).
* [Importe una aplicación de LUIS](#import-new-app) desde un archivo JSON que ya contenga las intenciones, expresiones y entidades.

## <a name="what-is-an-app"></a>¿Qué es una aplicación?
La aplicación contiene [versiones](luis-how-to-manage-versions.md) del modelo, así como cualquier [colaborador](luis-how-to-collaborate.md) para la aplicación. Cuando se crea la aplicación, se selecciona la referencia cultural ([idioma](luis-supported-languages.md)) que **no se puede cambiar después**. 

La versión predeterminada de una aplicación nueva es "0.1". 

Las aplicaciones se pueden crear y administrar en la página **My Apps** (Mis aplicaciones). Siempre se puede acceder a esta página haciendo clic en **My Apps** (Mis aplicaciones) en la barra de navegación superior del sitio web de [LUIS](luis-reference-regions.md). 

[![](media/luis-create-new-app/apps-list.png "Captura de pantalla de la lista de aplicaciones")](media/luis-create-new-app/apps-list.png#lightbox)

## <a name="create-new-app"></a>Crear una aplicación

1. En la página **My Apps** (Mis aplicaciones), haga clic en **Create new app** (Crear aplicación).
2. En el cuadro de diálogo asigne el nombre "TravelAgent" a la aplicación.

    ![Cuadro de diálogo para crear una aplicación](./media/luis-create-new-app/create-app.png)

3. Elija la referencia cultural de la aplicación (para la aplicación TravelAgent, seleccione Inglés) y, después, haga clic en **Done** (Listo). 

    >[!NOTE]
    >La referencia cultural no se puede cambiar una vez creada la aplicación. 

## <a name="import-new-app"></a>Importar la aplicación nueva
En el archivo JSON puede establecer el nombre (máximo 50 caracteres), la versión (máximo 10 caracteres) y la descripción de una aplicación. En [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/tree/master/documentation-samples/Examples-BookFlight) hay ejemplos de archivos JSON de la aplicación.

1. En la página **My Apps** (Mis aplicaciones), haga clic en **Import new app** (Importar aplicación nueva).
2. En el cuadro de diálogo **Import new app** (Importar aplicación nueva), seleccione el archivo JSON en el que se define la aplicación de LUIS.

    ![Cuadro de diálogo para importar la aplicación nueva](./media/luis-create-new-app/import-app.png)

## <a name="export-app"></a>Exportar la aplicación
1. En la página **Mis aplicaciones**, seleccione los puntos suspensivos (***...***) ubicados al final de la fila de aplicación.

    [![](media/luis-create-new-app/apps-list.png "Captura de pantalla del cuadro de diálogo emergente de las acciones de la aplicación")](media/luis-create-new-app/three-dots.png#lightbox)

2. Seleccione **Export app** (Exportar aplicación) en el menú. 

## <a name="rename-app"></a>Cambiar el nombre de la aplicación

1. En la página **Mis aplicaciones**, seleccione los puntos suspensivos (***...***) ubicados al final de la fila de aplicación. 
2. Seleccione **Rename** (Cambiar nombre) en el menú.
3. Escriba el nombre nuevo de la aplicación y haga clic en **Done** (Listo).

## <a name="delete-app"></a>Eliminar la aplicación

> [!CAUTION]
> La aplicación se elimina para todos los colaboradores y el propietario. [Exporte](#export-app) la aplicación antes de eliminarla. 

1. En la página **Mis aplicaciones**, seleccione los puntos suspensivos (***...***) ubicados al final de la fila de aplicación. 
2. Seleccione **Delete** (Eliminar) en el menú.
3. En la ventana de confirmación, haga clic en **Ok** (Aceptar).

## <a name="export-endpoint-logs"></a>Exportar los registros de punto de conexión
Los registros contienen la consulta, la hora UTC y la respuesta JSON de LUIS.

1. En la página **Mis aplicaciones**, seleccione los puntos suspensivos (***...***) ubicados al final de la fila de aplicación. 
2. Seleccione **Export endpoint logs** (Exportar registros de punto de conexión) en el menú.

```
Query,UTC DateTime,Response
text i'm driving and will be 30 minutes late to the meeting,02/13/2018 15:18:43,"{""query"":""text I'm driving and will be 30 minutes late to the meeting"",""intents"":[{""intent"":""None"",""score"":0.111048922},{""intent"":""SendMessage"",""score"":0.987501}],""entities"":[{""entity"":""i ' m driving and will be 30 minutes late to the meeting"",""type"":""Message"",""startIndex"":5,""endIndex"":58,""score"":0.162995353}]}"
```

## <a name="next-steps"></a>Pasos siguientes

La primera tarea en la aplicación es [agregar intenciones](luis-how-to-add-intents.md).