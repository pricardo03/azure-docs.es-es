---
title: Modelo de recurso de aplicación de Azure Service Fabric | Microsoft Docs
description: En este artículo se proporciona información general sobre la administración de una aplicación de Azure Service Fabric con Azure Resource Manager.
services: service-fabric
author: athinanthny
ms.service: service-fabric
ms.topic: conceptual
ms.date: 08/07/2019
ms.author: atsenthi
ms.openlocfilehash: 8e39318dcaa31a111908c6be1ae7a51d73eb7478
ms.sourcegitcommit: 55e0c33b84f2579b7aad48a420a21141854bc9e3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/19/2019
ms.locfileid: "69623824"
---
# <a name="what-is-the-service-fabric-application-resource-model"></a>¿Qué es el modelo de recurso de aplicación de Service Fabric?
Se recomienda que las aplicaciones de Service Fabric se implementen en el clúster de Service Fabric mediante Azure Resource Manager. Este método permite describir las aplicaciones y los servicios en JSON e implementarlos en la misma plantilla de Resource Manager que su clúster. A diferencia de implementar y administrar aplicaciones a través de PowerShell o la CLI de Azure, en este caso no es necesario esperar a que el clúster esté listo. El proceso de registro de la aplicación, su aprovisionamiento e implementación, puede suceder todo en un paso. Este es el procedimiento recomendado para administrar el ciclo de vida de las aplicaciones en el clúster. Para obtener más información, consulte los [procedimientos recomendados](https://docs.microsoft.com/azure/service-fabric/service-fabric-best-practices-infrastructure-as-code#azure-service-fabric-resources).

Cuando sea aplicable, administre las aplicaciones como recursos de Resource Manager para mejorar:
* Registro de auditoría: Resource Manager audita las operaciones y mantiene un *registro de actividad* detallado que puede ayudarle a realizar el seguimiento de los cambios realizados en estas aplicaciones y en el clúster.
* Control de acceso basado en rol: la administración del acceso a los clústeres, así como a las aplicaciones implementadas en el clúster, puede realizarse mediante la misma plantilla de Resource Manager.
* Azure Resource Manager (mediante Azure Portal) se convierte en el lugar central para administrar el clúster y las implementaciones de aplicaciones críticas.

## <a name="service-fabric-application-life-cycle-with-azure-resource-manager"></a>Ciclo de vida de una aplicación de Service Fabric con Azure Resource Manager 
En este documento, aprenderá a:

> [!div class="checklist"]
> * Implementar recursos de aplicación mediante Azure Resource Manager 
> * Actualizar recursos de aplicación mediante Azure Resource Manager
> * Eliminación de recursos de la aplicación

## <a name="deploy-application-resources-using-azure-resource-manager"></a>Implementar recursos de aplicación mediante Azure Resource Manager  
Para implementar una aplicación y sus servicios con el modelo de recurso de aplicación de Azure Resource Manager, debe empaquetar el código de la aplicación, cargar el paquete y, luego, hacer referencia a la ubicación del paquete en una plantilla de Azure Resource Manager como un recurso de aplicación. Para obtener más información, consulte [Empaquetado de una aplicación](https://docs.microsoft.com/azure/service-fabric/service-fabric-package-apps#create-an-sfpkg).
          
Luego, cree una plantilla de Azure Resource Manager, actualice el archivo de parámetros con los detalles de la aplicación e impleméntelo en el clúster de Service Fabric. Puede consultar ejemplos [aquí](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart/tree/master/ARM).

### <a name="create-a-storage-account"></a>Creación de una cuenta de Storage 
La implementación de una aplicación desde una plantilla de Resource Manager requiere una cuenta de almacenamiento para almacenar provisionalmente la imagen de la aplicación. Puede volver a usar una cuenta de almacenamiento existente o crear una nueva para almacenar provisionalmente sus aplicaciones. Si quiere usar una cuenta de almacenamiento existente, puede omitir este paso. 

![Crear una cuenta de almacenamiento][CreateStorageAccount]

### <a name="configure-storage-account"></a>Configuración de la cuenta de almacenamiento 
Una vez creada la cuenta de almacenamiento, debe crear un contenedor de blobs donde se puedan almacenar provisionalmente las aplicaciones. En Azure Portal, navegue hasta la cuenta de almacenamiento que quiera usar para almacenar sus aplicaciones. Seleccione la hoja **Blobs** y haga clic en el botón **Agregar contenedor**. Agregue un nuevo contenedor con el nivel de acceso Público de blob.
   
![Creación de un blob][CreateBlob]

### <a name="stage-application-in-a-storage-account"></a>Almacenamiento provisional de una aplicación en una cuenta de almacenamiento
Antes de que se pueda implementar la aplicación, esta debe almacenarse provisionalmente en un almacén de blobs. En este tutorial, se creará el paquete de aplicación manualmente, pero este paso se puede automatizar.  Para obtener más información, consulte [Empaquetado de una aplicación](https://docs.microsoft.com/azure/service-fabric/service-fabric-package-apps#create-an-sfpkg). En los pasos siguientes se utilizará la [aplicación de ejemplo Voting (Votación)](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart).

1. En Visual Studio, haga clic con el botón derecho en el proyecto Voting (Votación) y seleccione el paquete.   
![Empaquetado de una aplicación][PackageApplication]  
2. Abra el directorio **.\service-fabric-dotnet-quickstart\Voting\pkg\Debug** que acaba de crear y comprima el contenido en un archivo llamado **Voting.zip**, de modo que ApplicationManifest.xml se encuentre en la raíz del archivo .zip.  
![Compresión de la aplicación][ZipApplication]  
3. Cambie el nombre de la extensión del archivo de .zip a **.sfpkg**.
4. En Azure Portal, en el contenedor **apps** de la cuenta de almacenamiento, haga clic en **Cargar** y cargue **Voting.sfpkg**.  
![Carga del paquete de la aplicación][UploadAppPkg]

La aplicación ya está almacenada provisionalmente. Ya estamos a punto para crear la plantilla de Azure Resource Manager e implementar la aplicación.      
   
### <a name="create-the-azure-resource-manager-template"></a>Creación de la plantilla de Azure Resource Manager
La aplicación de ejemplo contiene [plantillas de Azure Resource Manager](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart/tree/master/ARM) que se pueden usar para implementar la aplicación. Los archivos de plantilla se denominan **UserApp.json** y **UserApp.Parameters.json**. 

> [!NOTE] 
> El archivo **UserApp.Parameters.json** debe actualizarse con el nombre del clúster.
>
>

| Parámetro              | DESCRIPCIÓN                                 | Ejemplo                                                      | Comentarios                                                     |
| ---------------------- | ------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| clusterName            | Nombre del clúster en el cual se realiza la implementación | sf-cluster123                                                |                                                              |
| application            | Nombre de la aplicación                 | Voting (Votación)                                                       |
| applicationTypeName    | Nombre del tipo de la aplicación           | VotingType                                                   | Debe coincidir con lo que aparece en ApplicationManifest.xml                 |
| applicationTypeVersion | Versión del tipo de aplicación         | 1.0.0                                                        | Debe coincidir con lo que aparece en ApplicationManifest.xml                 |
| serviceName            | Nombre del servicio         | Voting~VotingWeb                                             | Debe tener el formato ApplicationName~ServiceType            |
| serviceTypeName        | Nombre del tipo del servicio                | VotingWeb                                                    | Debe coincidir con lo que aparece en ServiceManifest.xml                 |
| appPackageUrl          | Dirección URL del almacén de blobs de la aplicación     | https://servicefabricapps.blob.core.windows.net/apps/Voting.sfpkg | Dirección URL del paquete de aplicación en el almacén de blobs (el procedimiento para establecerlo se describe a continuación) |
       
```json
{
    "apiVersion": "2019-03-01",
    "type": "Microsoft.ServiceFabric/clusters/applications",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2019-03-01",
    "type": "Microsoft.ServiceFabric/clusters/applicationTypes",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationTypeName'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2019-03-01",
    "type": "Microsoft.ServiceFabric/clusters/applicationTypes/versions",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationTypeName'), '/', parameters('applicationTypeVersion'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2019-03-01",
    "type": "Microsoft.ServiceFabric/clusters/applications/services",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'), '/', parameters('serviceName'))]",
    "location": "[variables('clusterLocation')]"
}
```

### <a name="deploy-the-application"></a>Implementación de la aplicación 
Para implementar la aplicación y realizar la implementación en el grupo de recursos que contiene el clúster, ejecute New-AzResourceGroupDeployment.
```powershell
New-AzResourceGroupDeployment -ResourceGroupName "sf-cluster-rg" -TemplateParameterFile ".\UserApp.Parameters.json" -TemplateFile ".\UserApp.json" -Verbose
```

## <a name="upgrade-service-fabric-application-using-azure-resource-manager"></a>Actualización de una aplicación de Service Fabric con Azure Resource Manager
Las aplicaciones que ya se hayan implementado en un clúster de Service Fabric se actualizarán por los siguientes motivos:

1. Se agrega un nuevo servicio a la aplicación. Se debe agregar una definición de servicio a los archivos service-manifest.xml y application-manifest.xml. Luego, para reflejar la nueva versión de la aplicación, debe actualizar la versión del tipo de aplicación de la 1.0.0 a la 1.0.1 ([UserApp.parameters.json](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart/blob/master/ARM/UserApp.Parameters.json)).

    ```
    "applicationTypeVersion": {
        "value": "1.0.1"
    },
    "serviceName2": {
        "value": "Voting~VotingData"
    },
    "serviceTypeName2": {
        "value": "VotingDataType"
    }
    ```
2. Se agrega una nueva versión de un servicio existente a la aplicación. Esto implica cambios en el código de la aplicación y actualizaciones de la versión y el nombre del tipo de aplicación.

    ```
     "applicationTypeVersion": {
        "value": "1.0.1"
    },
    ```

## <a name="delete-application-resources"></a>Eliminación de recursos de la aplicación
Las aplicaciones implementadas mediante el modelo de recurso de aplicación en Azure Resource Manager se pueden eliminar del clúster siguiendo estos pasos.

1) Obtención del id. de recurso de la aplicación mediante el comando [Get-AzResource](https://docs.microsoft.com/powershell/module/az.resources/get-azresource?view=azps-2.5.0)  

    #### <a name="get-resource-id-for-application"></a>Obtención del id. de recurso de la aplicación
    ```
    Get-AzResource  -Name <String> | f1
    ```
2) Eliminación de recursos de la aplicación mediante [Remove-AzResource](https://docs.microsoft.com/powershell/module/az.resources/remove-azresource?view=azps-2.5.0)  

    #### <a name="delete-application-resource-using-its-resource-id"></a>Eliminación de un recurso de la aplicación mediante su id. de recurso
    ```
    Remove-AzResource  -ResourceId <String> [-Force] [-ApiVersion <String>]
    ```

## <a name="next-steps"></a>Pasos siguientes
Obtenga información sobre el modelo de recurso de la aplicación:

* [Modelar una aplicación en Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-model)
* [Manifiestos de servicio y de aplicación de Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-and-service-manifests)

<!--Image references-->
[CreateStorageAccount]: ./media/service-fabric-application-model/create-storage-account.png
[CreateBlob]: ./media/service-fabric-application-model/create-blob.png
[PackageApplication]: ./media/service-fabric-application-model/package-application.png
[ZipApplication]: ./media/service-fabric-application-model/zip-application.png
[UploadAppPkg]: ./media/service-fabric-application-model/upload-app-pkg.png