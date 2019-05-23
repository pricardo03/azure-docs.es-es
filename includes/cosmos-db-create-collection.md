---
title: archivo de inclusión
description: archivo de inclusión
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 04/13/2018
ms.author: sngun
ms.custom: include file
ms.openlocfilehash: 28ecac39d991754cfadeb87479c336a6c6086fd7
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66151370"
---
Ahora puede usar la herramienta Explorador de datos en Azure Portal para crear una base de datos y una colección. 

1. Haga clic en **Explorador de datos** > **Nueva colección**. 
    
    El área **Agregar colección** se muestra en el extremo derecho, pero es posible que haya que desplazarse hacia la derecha para verlo.

    ![Explorador de datos de Azure Portal, panel Agregar colección](./media/cosmos-db-create-collection/azure-cosmosdb-data-explorer.png)

2. En la página **Agregar colección**, especifique la configuración de la nueva colección.

    Configuración|Valor sugerido|DESCRIPCIÓN
    ---|---|---
    Id. de base de datos|Tareas|Escriba *Tareas* como nombre de la nueva base de datos. Los nombres de base de datos tienen que tener entre 1 y 255 caracteres y no pueden contener `/, \\, #, ?` o espacios finales.
    Id. de colección|Elementos|Escriba *Elementos* como nombre de la nueva colección. Los identificadores de las colecciones tienen los mismos requisitos de caracteres que los nombres de las bases de datos.
    Clave de partición| `<Your partition key>`| Escriba una clave de partición como */userid*.
    Throughput|400 RU|Cambie el rendimiento a 400 unidades de solicitud por segundo (RU/s). Si quiere reducir la latencia, puede escalar verticalmente el rendimiento más adelante. 
    
    Además de la configuración anterior, puede agregar opcionalmente **claves únicas** para la colección. En este ejemplo vamos a dejar el campo en blanco. Las claves únicas proporcionan a los desarrolladores la capacidad de agregar una capa de integridad de datos a la base de datos. Mediante la creación de una directiva de clave única al crear una colección, se garantiza la unicidad de uno o varios valores por clave de partición. Para más información, consulte el artículo [Claves únicas en Azure Cosmos DB](../articles/cosmos-db/unique-keys.md).
    
    Haga clic en **OK**.

    El Explorador de datos muestra la nueva base de datos y la colección.

    ![El Explorador de datos de Azure Portal mostrando la nueva base de datos y la colección](./media/cosmos-db-create-collection/azure-cosmos-db-new-collection.png)