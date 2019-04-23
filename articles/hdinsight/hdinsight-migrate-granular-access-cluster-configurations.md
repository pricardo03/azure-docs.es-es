---
title: Migrar a acceso granular basada en roles para configuraciones de clúster - Azure HDInsight
description: Obtenga información sobre los cambios necesarios para migrar a acceso granular basada en roles para configuraciones de clúster.
author: tylerfox
ms.author: tyfox
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/19/2019
ms.openlocfilehash: 0422d848ccdf9ba82e68813de64eec863ee4ad29
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2019
ms.locfileid: "60006240"
---
# <a name="migrate-to-granular-role-based-access-for-cluster-configurations"></a>Migrar a acceso granular basada en roles para configuraciones de clúster

Presentamos algunos cambios importantes para admitir el acceso más específico basado en roles para obtener información confidencial. Como parte de ellos cambia, algunos **acción puede ser necesaria** si está usando uno de los [afectados y escenarios de entidades](#am-i-affected-by-these-changes).

## <a name="what-is-changing"></a>¿Qué está cambiando?

Anteriormente, se podrían obtener secretos a través de la API de HDInsight a los usuarios de clúster que poseen el propietario, Colaborador o lector [roles RBAC](https://docs.microsoft.com/azure/role-based-access-control/rbac-and-directory-admin-roles).
A partir de ahora, estos secretos dejará de estar accesibles para los usuarios con el rol de lector. También se puede introducir un nuevo rol 'Operador de clúster de HDInisght' que se pueda recuperar los secretos sin que se va a conceder los permisos administrativos de colaborador o propietario. Resumiendo:

| Rol                                  | Anteriormente                                                                                       | Now       |
|---------------------------------------|--------------------------------------------------------------------------------------------------|-----------|
| Lector                                | -Acceso de lectura, como los secretos                                                                   | -Acceso de lectura, **excepto** secretos |           |   |   |
| Operador de clústeres de HDInsight<br>(Nueva función) | N/D                                                                                              | : Acceso lectura/escritura, incluidos los secretos         |   |   |
| Colaborador                           | : Acceso lectura/escritura, incluidos los secretos<br>-Crear y administrar todos los tipos de recursos de Azure.     | Sin cambios |
| Propietario                                 | : Acceso lectura/escritura como los secretos<br>-Acceso completo a todos los recursos<br>-Delegar el acceso a otros usuarios | Sin cambios |

Para obtener información sobre cómo agregar la asignación de roles de operador de clúster de HDInsight a un usuario para concederles acceso de lectura/escritura a los secretos de clúster, vea la siguiente sección, [agregar la asignación de roles de operador de clúster de HDInsight a un usuario](#add-the-hdinsight-cluster-operator-role-assignment-to-a-user).

## <a name="am-i-affected-by-these-changes"></a>¿Estoy vieron estos cambios?

Afecta a las entidades y los escenarios siguientes:

- [API](#api): Los usuarios que utilizan el `/configurations` o `/configurations/{configurationName}` puntos de conexión.
- [Las herramientas de Azure HDInsight para Visual Studio Code](#azure-hdinsight-tools-for-visual-studio-code) versión ___ e inferiores.
- [Kit de herramientas de Azure para IntelliJ](#azure-toolkit-for-intellij) versión __ y a continuación.
- [Kit de herramientas de Azure para Eclipse](#azure-toolkit-for-eclipse) versión __ y a continuación.
- [SDK para .NET](#sdk-for-net)
    - [las versiones 1.x o 2.x](#versions-1x-and-2x): Los usuarios que utilizan el `GetClusterConfigurations`, `GetConnectivitySettings`, `ConfigureHttpSettings`, `EnableHttp` o `DisableHttp` métodos de la clase ConfigurationsOperationsExtensions.
    - [las versiones 3.x y hasta](#versions-3x-and-up): Los usuarios que utilizan el `EnableHttp`, `DisableHttp`, `Update`, o `Get` métodos desde el `ConfigurationsOperationsExtensions` clase.
- [SDK para Python](#sdk-for-python): Los usuarios que utilizan el `get` o `update` métodos de la clase ConfigurationsOperations.
- [SDK de Java](#sdk-for-java): Los usuarios que utilizan el `update` o `get` métodos de la clase ConfigurationsInner.
- [SDK para Go](#sdk-for-go): Los usuarios que utilizan el `Get` o `Update` métodos de la estructura ConfigurationsClient.

Consulte las secciones siguientes (o use los vínculos anteriores) para ver la migración, los pasos para su escenario.

### <a name="api"></a>API

Las siguientes API se puede cambiar o en desuso:

- [**GET /configurations/ {configurationName}** ](https://docs.microsoft.com/rest/api/hdinsight/hdinsight-cluster#get-configuration) (se quita la información confidencial) https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.HDInsight/clusters/{clusterName}/configurations/{configurationName}?api-version={api-version}
    - Anteriormente se utiliza para obtener los tipos de configuración individuales (incluidos los secretos).
    - Esta llamada API devolverá ahora tipos de configuración individuales con secretos que se omite. Para obtener todas las configuraciones, como los secretos, use la nueva [/configurations POST]() llamar. Para obtener sólo la configuración de puerta de enlace, use el nuevo [/getGatewaySettings POST]() llamar.
- [**GET /configurations** ](https://docs.microsoft.com/rest/api/hdinsight/hdinsight-cluster#get-configurations) (en desuso) https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.HDInsight/clusters/{clusterName}/configurations?api-version={api-version}
    - Anteriormente se utiliza para obtener todas las configuraciones (incluidos los secretos)
    - Ya no se admitirá esta llamada API. Para obtener todas las configuraciones en el futuro, use la nueva [/configurations POST]() llamar. Para obtener las configuraciones con los parámetros confidenciales se omite, use el [/configurations/ GET {configurationName}]() llamar.
- [**POST /configurations/ {configurationName}** ](https://docs.microsoft.com/rest/api/hdinsight/hdinsight-cluster#change-connectivity-settings) (en desuso) https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.HDInsight/clusters/{clusterName}/configurations/{configurationName}?api-version={api-version}
    - Anteriormente se usa para actualizar las credenciales de puerta de enlace.
    - Esta llamada de API se en desuso y ya no se admite. Use la nueva [/updateGatewaySettings POST]() en su lugar.

El reemplazo siguiente que se han agregado API:</span>

- **POST /configurations** https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.HDInsight/clusters/{clusterName}/configurations?api-version={api-version}
    - Utilice esta API para obtener todas las configuraciones, incluidos los secretos.
- **POST /getGatewaySettings** https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.HDInsight/clusters/{clusterName}/getGatewaySettings?api-version={api-version}
    - Utilice esta API para obtener la configuración de puerta de enlace.
- **POST /updateGatewaySettings** https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.HDInsight/clusters/{clusterName}/updateGatewaySettings?api-version={api-version}
    - Utilice esta API para actualizar la configuración de puerta de enlace (nombre de usuario o contraseña).

### <a name="azure-hdinsight-tools-for-visual-studio-code"></a>Herramientas de Azure HDInsight para Visual Studio Code

Si usa la versión 1.1.1 o una versión anterior, actualice a la [versión más reciente de Azure HDInsight Tools para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=mshdinsight.azure-hdinsight&ssr=false) para evitar interrupciones.

### <a name="azure-toolkit-for-intellij"></a>Kit de herramientas de Azure para IntelliJ

Si usa la versión 3.21.0 o una versión anterior, actualice a la [versión más reciente del Kit de herramientas de Azure para IntelliJ complemento](https://plugins.jetbrains.com/plugin/8053-azure-toolkit-for-intellij) para evitar interrupciones.

### <a name="azure-toolkit-for-eclipse"></a>Kit de herramientas de Azure para Eclipse

Si está usando la versión de 2019-03-29 o una versión anterior, actualice a la versión más reciente del Kit de herramientas de Azure para Eclipse con el fin de evitar interrupciones.

### <a name="sdk-for-net"></a>SDK para .NET

#### <a name="versions-1x-and-2x"></a>Las versiones 1.x y 2.x

Actualice a [versión 2.1.0](https://www.nuget.org/packages/Microsoft.Azure.Management.HDInsight/2.1.0) del SDK para .NET de HDInsight. Si usa un método que se ven afectado por estos cambios, pueden ser necesarias modificaciones de código mínimo:

- `ClusterOperationsExtensions.GetClusterConfigurations` le **ya no se devuelven los parámetros confidenciales** como claves de almacenamiento (sitio principal) o las credenciales de HTTP (puerta de enlace).
    - Para recuperar todas las configuraciones, incluidos los parámetros confidenciales, utilice `ClusterOperationsExtensions.ListConfigurations` a partir de ahora.  Tenga en cuenta que los usuarios con el rol "Lector" no podrá usar este método. Esto permite un control granular sobre el que los usuarios pueden acceder a información confidencial para un clúster.
    - Para recuperar solo las credenciales de puerta de enlace HTTP, utilice `ClusterOperationsExtensions.GetGatewaySettings`.

- `ClusterOperationsExtensions.GetConnectivitySettings` Ahora está en desuso y se ha reemplazado por `ClusterOperationsExtensions.GetGatewaySettings`.

- `ClusterOperationsExtensions.ConfigureHttpSettings` Ahora está en desuso y se ha reemplazado por `ClusterOperationsExtensions.UpdateGatewaySettings`.

- `ConfigurationsOperationsExtensions.EnableHttp` y `DisableHttp` están en desuso. HTTP ahora siempre está habilitado, por lo que estos métodos ya no son necesarios.

#### <a name="versions-3x-and-up"></a>Las versiones 3.x y superiores

Actualice a [versión 5.0.0](https://www.nuget.org/packages/Microsoft.Azure.Management.HDInsight/5.0.0) del SDK para .NET de HDInsight. Si usa un método que se ven afectado por estos cambios, pueden ser necesarias modificaciones de código mínimo:

- [`ConfigurationOperationsExtensions.Get`](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.hdinsight.configurationsoperationsextensions.get?view=azure-dotnet) le **ya no se devuelven los parámetros confidenciales** como claves de almacenamiento (sitio principal) o las credenciales de HTTP (puerta de enlace).
    - Para recuperar todas las configuraciones, incluidos los parámetros confidenciales, utilice `ConfigurationOperationsExtensions.List` a partir de ahora.  Tenga en cuenta que los usuarios con el rol "Lector" no podrá usar este método. Esto permite un control granular sobre el que los usuarios pueden acceder a información confidencial para un clúster. 
    - Para recuperar solo las credenciales de puerta de enlace HTTP, utilice `ClusterOperationsExtensions.GetGatewaySettings`. 
- [`ConfigurationsOperationsExtensions.Update`](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.hdinsight.configurationsoperationsextensions.update?view=azure-dotnet) Ahora está en desuso y se ha reemplazado por `ClusterOperationsExtensions.UpdateGatewaySettings`. 
- [`ConfigurationsOperationsExtensions.EnableHttp`](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.hdinsight.configurationsoperationsextensions.enablehttp?view=azure-dotnet) y [ `DisableHttp` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.hdinsight.configurationsoperationsextensions.disablehttp?view=azure-dotnet) están en desuso. HTTP ahora siempre está habilitado, por lo que estos métodos ya no son necesarios.

### <a name="sdk-for-python"></a>SDK para Python

Actualice a [versión 1.0.0](https://pypi.org/project/azure-mgmt-hdinsight/1.0.0/) del SDK de HDInsight para Python. Si usa un método que se ven afectado por estos cambios, pueden ser necesarias modificaciones de código mínimo:

- [`ConfigurationsOperations.get`](https://docs.microsoft.com/python/api/azure-mgmt-hdinsight/azure.mgmt.hdinsight.operations.configurations_operations.configurationsoperations?view=azure-python#get-resource-group-name--cluster-name--configuration-name--custom-headers-none--raw-false----operation-config-) le **ya no se devuelven los parámetros confidenciales** como claves de almacenamiento (sitio principal) o las credenciales de HTTP (puerta de enlace).
    - Para recuperar todas las configuraciones, incluidos los parámetros confidenciales, utilice [ `ConfigurationsOperations.list` ](https://docs.microsoft.com/python/api/azure-mgmt-hdinsight/azure.mgmt.hdinsight.operations.configurations_operations.configurationsoperations?view=azure-python#list-resource-group-name--cluster-name--custom-headers-none--raw-false----operation-config-) a partir de ahora.  Tenga en cuenta que los usuarios con el rol "Lector" no podrá usar este método. Esto permite un control granular sobre el que los usuarios pueden acceder a información confidencial para un clúster. 
    - Para recuperar solo las credenciales de puerta de enlace HTTP, utilice [ `ConfigurationsOperations.get_gateway_settings` ](https://docs.microsoft.com/python/api/azure-mgmt-hdinsight/azure.mgmt.hdinsight.operations.clusters_operations.clustersoperations?view=azure-python#get-gateway-settings-resource-group-name--cluster-name--custom-headers-none--raw-false----operation-config-).
- [`ConfigurationsOperations.update`](https://docs.microsoft.com/python/api/azure-mgmt-hdinsight/azure.mgmt.hdinsight.operations.clusters_operations.clustersoperations?view=azure-python#update-resource-group-name--cluster-name--tags-none--custom-headers-none--raw-false----operation-config-) Ahora está en desuso y se ha reemplazado por [ `ClusterOperationsExtensions.update_gateway_settings` ](https://docs.microsoft.com/python/api/azure-mgmt-hdinsight/azure.mgmt.hdinsight.operations.clusters_operations.clustersoperations?view=azure-python#update-gateway-settings-resource-group-name--cluster-name--parameters--custom-headers-none--raw-false--polling-true----operation-config-).

### <a name="sdk-for-java"></a>SDK de Java

Actualice a [versión 1.0.0](https://search.maven.org/artifact/com.microsoft.azure.hdinsight.v2018_06_01_preview/azure-mgmt-hdinsight/) de HDInsight SDK para Java. Si usa un método que se ven afectado por estos cambios, pueden ser necesarias modificaciones de código mínimo:

- [`ConfigurationsInner.get`](https://docs.microsoft.com/java/api/com.microsoft.azure.management.hdinsight.v2018__06__01__preview.implementation._configurations_inner.get) le **ya no se devuelven los parámetros confidenciales** como claves de almacenamiento (sitio principal) o las credenciales de HTTP (puerta de enlace).
    - Para recuperar todas las configuraciones, incluidos los parámetros confidenciales, utilice `ConfigurationsInner.list` a partir de ahora.  Tenga en cuenta que los usuarios con el rol "Lector" no podrá usar este método. Esto permite un control granular sobre el que los usuarios pueden acceder a información confidencial para un clúster. 
    - Para recuperar solo las credenciales de puerta de enlace HTTP, utilice `ConfigurationsOperations.get_gateway_settings`.
- [`ConfigurationsInner.update`](https://docs.microsoft.com/java/api/com.microsoft.azure.management.hdinsight.v2018__06__01__preview.implementation._configurations_inner.update) Ahora está en desuso y se ha reemplazado por `ClusterOperationsExtensions.update_gateway_settings`.

### <a name="sdk-for-go"></a>SDK para Go

Actualice a [versión 27.1.0](https://github.com/Azure/azure-sdk-for-go/tree/master/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight) de HDInsight SDK para Go. Si usa un método que se ven afectado por estos cambios, pueden ser necesarias modificaciones de código mínimo:

- [`ConfigurationsClient.get`](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight#ConfigurationsClient.Get) le **ya no se devuelven los parámetros confidenciales** como claves de almacenamiento (sitio principal) o las credenciales de HTTP (puerta de enlace).
    - Para recuperar todas las configuraciones, incluidos los parámetros confidenciales, utilice [ `ConfigurationsClient.list` ](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight#ConfigurationsClient.List) a partir de ahora.  Tenga en cuenta que los usuarios con el rol "Lector" no podrá usar este método. Esto permite un control granular sobre el que los usuarios pueden acceder a información confidencial para un clúster. 
    - Para recuperar solo las credenciales de puerta de enlace HTTP, utilice [ `ClustersClient.get_gateway_settings` ](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight#ClustersClient.GetGatewaySettings).
- [`ConfigurationsClient.update`](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight#ConfigurationsClient.Update) Ahora está en desuso y se ha reemplazado por [ `ClustersClient.update_gateway_settings` ](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight#ClustersClient.UpdateGatewaySettings).

## <a name="add-the-hdinsight-cluster-operator-role-assignment-to-a-user"></a>Agregar la asignación de roles de operador de clúster de HDInsight a un usuario

Un usuario con el [colaborador](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#contributor) o [propietario](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#owner) rol puede conceder el rol operador del clúster de HDInsight a los usuarios que desea tener acceso de lectura/escritura a los secretos de clúster de HDInsight, como las credenciales de puerta de enlace del clúster y las claves de cuenta de almacenamiento.

### <a name="using-the-azure-cli"></a>Uso de la CLI de Azure

La manera más sencilla de agregar esta asignación de roles es mediante el siguiente comando de CLI de Azure:

```azurecli-interactive
az role assignment create --role "HDInsight Cluster Operator" --assignee user@domain.com
```

> [!NOTE]
> Este comando debe ejecutarse por un usuario con los roles de colaborador o propietario, como solo se puede conceder estos permisos. El `--assignee` es la dirección de correo electrónico del usuario a quien desea asignar el rol operador del clúster de HDInsight.

El comando anterior, concede este rol en el nivel de suscripción. Para en su lugar, simplemente conceder este rol en el nivel de grupo de recursos, puede modificar el comando siguiente manera:

```azurecli-interactive
az role assignment create --role "HDInsight Cluster Operator" --assignee user@domain.com -g <ResourceGroupName>
```

### <a name="using-the-azure-portal"></a>Uso de Azure Portal

Como alternativa, puede usar el portal de Azure para agregar la asignación de roles de operador de clúster de HDInsight a un usuario. Consulte la documentación, [administrar el acceso a recursos de Azure mediante RBAC y Azure portal: agregar una asignación de roles](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal#add-a-role-assignment).
