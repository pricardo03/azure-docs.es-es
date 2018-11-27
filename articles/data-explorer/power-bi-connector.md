---
title: 'Guía de inicio rápido: Visualización de datos mediante el conector de Azure Data Explorer para Power BI'
description: 'En esta guía de inicio rápido aprenderá a usar una de las tres opciones de visualización de datos de Power BI: el conector de Power BI para Azure Data Explorer.'
services: data-explorer
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: quickstart
ms.date: 11/14/2018
ms.openlocfilehash: 3cb8f52677991997a0176a9f8d408e2fd6d2d8d9
ms.sourcegitcommit: 8314421d78cd83b2e7d86f128bde94857134d8e1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2018
ms.locfileid: "51974278"
---
# <a name="quickstart-visualize-data-using-the-azure-data-explorer-connector-for-power-bi"></a>Guía de inicio rápido: Visualización de datos mediante el conector de Azure Data Explorer para Power BI

El Explorador de datos de Azure es un servicio de exploración de datos altamente escalable y rápido para datos de telemetría y registro. Power BI es una solución de análisis de negocios que le permite visualizar sus datos y compartir los resultados en su organización.

Azure Data Explorer ofrece tres opciones para conectarse a los datos de Power BI: usar el conector integrado, importar una consulta de Azure Data Explorer o usar una consulta SQL. En esta guía de inicio rápido se le enseña a usar el conector integrado para obtener datos y visualizarlos en un informe de Power BI.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita de Azure](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Necesita lo siguiente para completar esta guía de inicio rápido:

* Una cuenta de correo electrónico organizativa que sea miembro de Azure Active Directory, para que pueda conectarse al [clúster de ayuda de Azure Data Explorer](https://dataexplorer.azure.com/clusters/help/databases/samples).

* [Power BI Desktop](https://powerbi.microsoft.com/get-started/) (seleccione **DESCARGAR GRATIS**)

## <a name="get-data-from-azure-data-explorer"></a>Obtención de datos de Azure Data Explorer

En primer lugar, conéctese al clúster de ayuda de Azure Data Explorer, después, traiga un subconjunto de los datos de la tabla *StormEvents*. [!INCLUDE [data-explorer-storm-events](../../includes/data-explorer-storm-events.md)]

1. En la pestaña **Inicio** de Power BI Desktop, seleccione **Obtener datos** y luego **Más**.

    ![Obtener los datos](media/power-bi-connector/get-data-more.png)

1. Busque *Azure Data Explorer* y, a continuación, seleccione **Azure Data Explorer (versión beta)** y **Conectar**.

    ![Buscar y obtener datos](media/power-bi-connector/search-get-data.png)

1. En la pantalla **Conector de vista previa**, seleccione **Continuar**.

1. En la siguiente pantalla, rellene el formulario con esta información.

    ![Opciones de clúster, base de datos y tabla](media/power-bi-connector/cluster-database-table.png)

    **Configuración** | **Valor** | **Descripción del campo**
    |---|---|---|
    | Clúster | *https://help.kusto.windows.net* | La dirección URL para el clúster de ayuda. Para otros clústeres, la dirección URL tiene el formato *https://\<NombreCluster\>.\<Región\>.kusto.windows.net*. |
    | Base de datos | Déjelo en blanco | Una base de datos que se hospeda en el clúster al que se va a conectar. Se seleccionará en un paso posterior. |
    | Nombre de tabla | Déjelo en blanco | Una de las tablas de la base de datos o una consulta como "StormEvents | take 1000". Se seleccionará en un paso posterior. |
    | Opciones avanzadas | Déjelo en blanco | Opciones para las consultas como, por ejemplo, el tamaño del conjunto de resultados. |
    | Modo Conectividad de datos | *DirectQuery* | Determina si Power BI importa los datos o se conecta directamente al origen de datos. Puede usar cualquiera de las opciones con este conector. |
    | | | |

1. Si no dispone de una conexión al clúster de ayuda, inicie sesión. Inicie sesión con una cuenta de organización y, luego, seleccione **Conectar**.

    ![Iniciar sesión](media/power-bi-connector/sign-in.png)

1. En la pantalla **Navegador**, expanda la base de datos **Ejemplos**, seleccione **StormEvents** y **Editar**.

    ![Seleccionar tabla](media/power-bi-connector/select-table.png)

    La tabla se abre en el editor de Power Query, donde puede editar filas y columnas antes de importar los datos.

1. En el Editor de Power Query, seleccione la flecha situada junto a la columna **DamageCrops** y, a continuación, **Orden descendente**.

    ![Ordenar DamageCrops en orden descendente](media/power-bi-connector/sort-descending.png)

1. En la pestaña **Inicio**, seleccione **Conservar filas** y, después, **Conservar filas superiores**. Escriba un valor de *1000* para traer las primeras 1000 filas de la tabla ordenada.

    ![Conservar filas superiores](media/power-bi-connector/keep-top-rows.png)

1. En la pestaña **Inicio**, seleccione **Cerrar y aplicar**.

    ![Cerrar y aplicar](media/power-bi-connector/close-apply.png)

## <a name="visualize-data-in-a-report"></a>Visualización de datos de un informe

[!INCLUDE [data-explorer-power-bi-visualize-basic](../../includes/data-explorer-power-bi-visualize-basic.md)]

## <a name="clean-up-resources"></a>Limpieza de recursos

Si ya no necesita el informe que ha creado para esta guía de inicio rápido, elimine el archivo .pbix de Power BI Desktop.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Guía de inicio rápido: Visualización de datos mediante una consulta importada en Power BI](power-bi-imported-query.md)