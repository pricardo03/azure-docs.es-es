---
author: orspod
ms.service: data-explorer
ms.topic: include
ms.date: 01/08/2020
ms.author: orspodek
ms.openlocfilehash: f9788e4623ce60ad55d79558d1d77a17eb2a9f26
ms.sourcegitcommit: 5b073caafebaf80dc1774b66483136ac342f7808
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/09/2020
ms.locfileid: "75779964"
---
### <a name="event-system-properties-mapping"></a>Asignación de propiedades del sistema de eventos

Si seleccionó **Propiedades del sistema de eventos** en la sección **Origen de datos** de la tabla anterior, vaya a la [interfaz de usuario web](https://dataexplorer.azure.com/) para ejecutar el comando de KQL correspondiente para crear la asignación correcta.

   **Para la asignación de CSV:**

    ```kusto
    .create table MyTable ingestion csv mapping "CsvMapping1"
    '['
    '   { "column" : "messageid", "DataType":"string", "Properties":{"Ordinal":"0"}},'
    '   { "column" : "userid", "DataType":"string", "Properties":{"Ordinal":"1"}},'
    '   { "column" : "other", "DataType":"int", "Properties":{"Ordinal":"2"}}'
    ']'
    ```
 
   **Para la asignación de JSON:**

    ```kusto
    .create table MyTable ingestion json mapping "JsonMapping1"
    '['
    '    { "column" : "messageid", "datatype" : "string", "Properties":{"Path":"$.message-id"}},'
    '    { "column" : "userid", "Properties":{"Path":"$.user-id"}},'
    '    { "column" : "other", "Properties":{"Path":"$.other"}}'
    ']'
    ```

   > [!TIP]
   > * Debe incluir todas las propiedades seleccionadas en la asignación. 
   > * El orden de las propiedades es importante en la asignación de CSV. Las propiedades del sistema deben aparecer antes de todas las demás propiedades y en el mismo orden en el que aparecen en la lista desplegable **Propiedades del sistema de eventos**.