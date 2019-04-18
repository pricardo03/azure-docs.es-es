---
title: Visualizar datos desde el Explorador de datos de Azure con Grafana
description: En este procedimiento, aprenderá a configurar Azure Data Explorer como origen de datos de Grafana y a visualizarlos después desde un clúster de ejemplo.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 12/05/2018
ms.openlocfilehash: 188cb310cfc13fe2fc41ba3e01deb01068c0184d
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59048323"
---
# <a name="visualize-data-from-azure-data-explorer-in-grafana"></a>Visualización de datos desde Azure Data Explorer en Grafana

Grafana es una plataforma de análisis que permite consultar y visualizar datos y crear y compartir paneles basados en las visualizaciones. Grafana proporciona un *complemento* para Azure Data Explorer, que permite conectarse a datos desde Azure Data Explorer y visualizarlos. En este artículo, aprenderá a configurar Azure Data Explorer como origen de datos de Grafana y a visualizarlos después desde un clúster de ejemplo.

## <a name="prerequisites"></a>Requisitos previos

Necesita lo siguiente para completar este procedimiento:

* [Grafana versión 5.3.0 o una versión posterior](https://docs.grafana.org/installation/) para su sistema operativo

* El [complemento de Azure Data Explorer](https://grafana.com/plugins/grafana-azure-data-explorer-datasource/installation) para Grafana

* Un clúster que incluya los datos de ejemplo de StormEvents. Para más información, consulte [Guía de inicio rápido: Creación de un clúster y de la base de datos de Azure Data Explorer](create-cluster-database-portal.md) e [Ingesta de datos de ejemplo en Azure Data Explorer](ingest-sample-data.md).

    [!INCLUDE [data-explorer-storm-events](../../includes/data-explorer-storm-events.md)]

## <a name="configure-the-data-source"></a>Configuración del origen de datos

Realice los pasos siguientes para configurar Azure Data Explorer como origen de datos de Grafana. Trataremos estos pasos con más detalle en esta sección:

1. Cree una entidad de servicio de Azure Active Directory (Azure AD). Grafana usa la entidad de servicio para acceder al servicio de Azure Data Explorer.

1. Agregue la entidad de servicio de Azure AD al rol *visores* en la base de datos de Azure Data Explorer.

1. Especifique las propiedades de conexión de Grafana en función de la información de la entidad de servicio de Azure AD y, a continuación, pruebe la conexión.

### <a name="create-a-service-principal"></a>Creación de una entidad de servicio

Puede crear la entidad de servicio en [Azure Portal](#azure-portal) o mediante la experiencia de línea de comandos de la [CLI de Azure](#azure-cli). Independientemente del método que utilice, después de crear la entidad, obtendrá los valores de cuatro propiedades de conexión que usará en pasos posteriores.

#### <a name="azure-portal"></a>Azure Portal

1. Para crear la entidad de servicio, siga las instrucciones de la [documentación de Azure Portal](/azure/active-directory/develop/howto-create-service-principal-portal).

    1. En la sección [Asignación de la aplicación a un rol](/azure/active-directory/develop/howto-create-service-principal-portal#assign-the-application-to-a-role), asigne un tipo de rol de **lector** al clúster de Azure Data Explorer.

    1. En la sección [Obtención de valores para iniciar sesión](/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in), copie los tres valores de propiedad indicados en los pasos: **Id. de directorio** (Id. de inquilino), **Id. de aplicación** y **Contraseña**.

1. En Azure Portal, seleccione **Suscripciones** y copie el identificador de la suscripción en la que creó la entidad de servicio.

    ![Id. de suscripción (Azure Portal)](media/grafana/subscription-id-portal.png)

#### <a name="azure-cli"></a>Azure CLI

1. Crear una entidad de servicio. Establezca un ámbito adecuado y un tipo de rol de `reader`.

    ```azurecli
    az ad sp create-for-rbac --name "https://{UrlToYourGrafana}:{PortNumber}" --role "reader" \
                             --scopes /subscriptions/{SubID}/resourceGroups/{ResourceGroupName}
    ```

    Para más información, vea [Creación de una entidad de servicio de Azure con la CLI de Azure](/cli/azure/create-an-azure-service-principal-azure-cli).

1. El comando devuelve un conjunto de resultados similar al siguiente. Copie los tres valores de propiedad: **id_aplicación**, **contraseña** e **inquilino**.

    ```json
    {
      "appId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
      "displayName": "{UrlToYourGrafana}:{PortNumber}",
      "name": "https://{UrlToYourGrafana}:{PortNumber}",
      "password": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
      "tenant": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"
    }
    ```

1. Obtenga una lista de las suscripciones.

    ```azurecli
    az account list --output table
    ```

    Copie el identificador de suscripción apropiado.

    ![Id. de suscripción (CLI)](media/grafana/subscription-id-cli.png)

### <a name="add-the-service-principal-to-the-viewers-role"></a>Adición de la entidad de servicio al rol visores

Ahora que ya tiene una entidad de servicio, agréguela al rol *visores* en la base de datos de Azure Data Explorer. Puede realizar esta tarea en **Permisos** en Azure Portal o en **Consulta** mediante un comando de administración.

#### <a name="azure-portal---permissions"></a>Azure Portal (Permisos)

1. En Azure Portal, vaya al clúster de Azure Data Explorer.

1. En la sección **Introducción**, seleccione la base de datos con los datos de ejemplo de StormEvents.

    ![Seleccionar base de datos](media/grafana/select-database.png)

1. Seleccione **Permisos** y luego **Agregar**.

    ![Permisos de base de datos](media/grafana/database-permissions.png)

1. En **Add database permissions** (Agregar permisos de base de datos), seleccione el rol **Visor** y después **Select principals** (Seleccionar entidades).

    ![Incorporación de permisos de base de datos](media/grafana/add-permission.png)

1. Busque la entidad de servicio que creó (en el ejemplo se muestra la entidad **mb-grafana**). Seleccione la entidad y después **Seleccionar**.

    ![Administración de permisos en Azure Portal](media/grafana/new-principals.png)

1. Seleccione **Guardar**.

    ![Administración de permisos en Azure Portal](media/grafana/save-permission.png)

#### <a name="management-command---query"></a>Comando de administración (Consulta)

1. En Azure Portal, vaya al clúster de Azure Data Explorer y seleccione **Consulta**.

    ![Consultar](media/grafana/query.png)

1. Ejecute el comando siguiente en la ventana de consulta. Use el identificador de la aplicación y el identificador de inquilino de Azure Portal o la CLI.

    ```kusto
    .add database {TestDatabase} viewers ('aadapp={ApplicationID};{TenantID}')
    ```

    El comando devuelve un conjunto de resultados similar al siguiente. En este ejemplo, la primera fila es para un usuario existente en la base de datos y la segunda fila es para la entidad de servicio que se acaba de agregar.

    ![Conjunto de resultados](media/grafana/result-set.png)

### <a name="specify-properties-and-test-the-connection"></a>Especificar propiedades y probar la conexión

Con la entidad de servicio asignada al rol *visores*, ahora puede especificar propiedades en la instancia de Grafana y probar la conexión a Azure Data Explorer.

1. En Grafana, en el menú izquierdo, seleccione el icono de engranaje y, a continuación, **Data Sources** (Orígenes de datos).

    ![Orígenes de datos](media/grafana/data-sources.png)

1. Seleccione **Add data source**(Agregar origen de datos).

1. En la página **Data Sources / New** (Orígenes de datos / Nuevo), escriba un nombre para el origen de datos y, a continuación, seleccione el tipo **Azure Data Explorer Datasource** (Origen de datos de Azure Data Explorer).

    ![Nombre y tipo de conexión](media/grafana/connection-name-type.png)

1. Escriba el nombre del clúster en el formulario https://{ClusterName}.{Region}.kusto.windows.net. Especifique los demás valores desde Azure Portal o la CLI. Consulte la tabla debajo de la imagen siguiente para una asignación.

    ![Propiedades de la conexión](media/grafana/connection-properties.png)

    | UI de Grafana | Azure Portal | Azure CLI |
    | --- | --- | --- |
    | Id. de suscripción | ID. DE SUSCRIPCIÓN | SubscriptionId |
    | Identificador de inquilino | Identificador de directorio | tenant |
    | Id. de cliente | Identificador de aplicación | appId |
    | Secreto del cliente | Contraseña | contraseña |
    | | | |

1. Seleccione **Guardar y probar**.

    Si la prueba se realiza correctamente, vaya a la sección siguiente. Si tiene algún problema, compruebe los valores especificados en Grafana y revise los pasos anteriores.

## <a name="visualize-data"></a>Visualización de datos

Ahora que ya ha terminado de configurar Azure Data Explorer como origen de datos para Grafana, es el momento de visualizar los datos. Le mostraremos un ejemplo básico aquí, pero puede hacer mucho más. Se recomienda revisar [Escribir consultas para Azure Data Explorer](write-queries.md) para obtener ejemplos de otras consultas ejecutadas en el conjunto de datos de ejemplo.

1. En Grafana, en el menú izquierdo, seleccione el icono del signo más y después **Dashboard** (Panel).

    ![Crear panel](media/grafana/create-dashboard.png)

1. En la pestaña **Add** (Agregar), seleccione **Graph** (Gráfico).

    ![Agregar gráfico](media/grafana/add-graph.png)

1. En el panel de gráfico, seleccione **Panel Title** (Título del panel) y después **Edit** (Editar).

    ![Editar panel](media/grafana/edit-panel.png)

1. En la parte inferior del panel, seleccione **Data Source** (Origen de datos) y después seleccione el origen de datos que ha configurado.

    ![Selección de origen de datos](media/grafana/select-data-source.png)

1. En el panel de consulta, copie la siguiente consulta y después seleccione **Run** (Ejecutar). La consulta desglosa el recuento de eventos por día para el conjunto de datos de ejemplo.

    ```kusto
    StormEvents
    | summarize event_count=count() by bin(StartTime, 1d)
    ```

    ![Ejecutar consulta](media/grafana/run-query.png)

1. El gráfico no muestra ningún resultado porque su ámbito predeterminado son los datos de las seis últimas horas. En el menú superior, seleccione **Last 6 hours** (Últimas 6 horas).

    ![Últimas seis horas](media/grafana/last-six-hours.png)

1. Especifique un intervalo personalizado que abarque 2007, el año incluido en el conjunto de datos de ejemplo StormEvents. Seleccione **Aplicar**.

    ![Personalizar intervalo de fecha](media/grafana/custom-date-range.png)

    Ahora el gráfico muestra los datos de 2007, desglosados por días.

    ![Gráfico finalizado](media/grafana/finished-graph.png)

1. En el menú superior, seleccione el icono para guardar: ![Icono Save (Guardar)](media/grafana/save-icon.png).

## <a name="next-steps"></a>Pasos siguientes

[Write queries for Azure Data Explorer](write-queries.md) (Escritura de consultas del Explorador de datos de Azure)

[Tutorial: Visualización de datos desde Azure Data Explorer en Power BI](visualize-power-bi.md)