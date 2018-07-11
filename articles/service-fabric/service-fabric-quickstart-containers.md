---
title: Creación de una aplicación contenedor de Windows de Service Fabric en Azure | Microsoft Azure
description: En esta guía de inicio rápido, creará su primera aplicación contenedora Windows en Azure Service Fabric.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: vturecek
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: quickstart
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/30/2018
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 95877f8b81641dd70434fc167003cfcce07d9e84
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2018
ms.locfileid: "37110817"
---
# <a name="quickstart-deploy-windows-containers-to-service-fabric"></a>Inicio rápido: Implementación de contenedores de Windows en Service Fabric

Azure Service Fabric es una plataforma de sistemas distribuidos para implementar y administrar microservicios y contenedores escalables y confiables.

Para ejecutar una aplicación que existe en un contenedor de Windows en un clúster de Service Fabric no hay que hacer ningún cambio en la aplicación. Esta guía de inicio rápido muestra cómo implementar una imagen de contenedor Docker creada previamente en una aplicación de Service Fabric. Cuando haya terminado, tendrá un contenedor de Windows Server 2016 Nano e IIS en ejecución. Esta guía de inicio rápido describe la implementación de un contenedor Windows; lea [esta otra guía](service-fabric-quickstart-containers-linux.md) para implementar un contenedor Linux.

![Página web predeterminada de IIS][iis-default]

En esta guía de inicio rápido, aprenderá a hacer lo siguiente:

* Empaquetado de un contenedor de imagen de Docker
* Configuración de la comunicación
* Creación y empaquetamiento de la aplicación de Service Fabric
* Implementación de la aplicación contenedora en Azure

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure (puede crear una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)).
* Un equipo de desarrollo en el que se ejecute:
  * Visual Studio 2015 o Visual Studio 2017.
  * [SDK y herramientas de Service Fabric](service-fabric-get-started.md).

## <a name="package-a-docker-image-container-with-visual-studio"></a>Empaquetado de un contenedor de imagen de Docker con Visual Studio

Las herramientas y el SDK de Service Fabric proporcionan una plantilla de servicio que le ayuda a implementar un contenedor en un clúster de Service Fabric.

Inicie Visual Studio como administrador.  Seleccione **Archivo** > **Nuevo** > **Proyecto**.

Seleccione **Aplicación de Service Fabric**, asígnele el nombre "MyFirstContainer" y haga clic en **Aceptar**.

Seleccione **Contenedor** en las plantillas **Aplicaciones y contenedores hospedados**.

En **Nombre de imagen**, escriba "microsoft/iis:nanoserver", la [imagen de base de Windows Server Nano Server e IIS](https://hub.docker.com/r/microsoft/iis/).

Configure la asignación de los puertos del host al contenedor para que las solicitudes que lleguen al puerto 80 para el servicio se asignen al puerto 80 del contenedor.  Establezca **Puerto del contenedor** en "80" y establezca **Puerto del host** en "80".  

Asigne el nombre "MyContainerService" al servicio y haga clic en **Aceptar**.

<<<<<<< Ascendente actualizado ![Cuadro de diálogo de servicio nuevo][new-service]
=======
## <a name="configure-communication-and-container-port-to-host-port-mapping"></a>Configuración de la comunicación y asignación del puerto "puerto a host" del contenedor

El servicio necesita un punto de conexión para la comunicación.  Para esta guía de inicio rápido, el servicio en contenedor escucha en el puerto 80.  En el Explorador de soluciones, abra *MyFirstContainer/ApplicationPackageRoot/MyContainerServicePkg/ServiceManifest.xml*.  Actualice el valor de `Endpoint` en el archivo ServiceManifest.xml y agregue el protocolo, el puerto y el esquema de URI:

```xml
<Resources>
    <Endpoints>
        <Endpoint Name="MyContainerServiceTypeEndpoint" UriScheme="http" Port="80" Protocol="http"/>
   </Endpoints>
</Resources>
```

Si se especifica `UriScheme`, se registra automáticamente el punto de conexión del contenedor con el servicio de nombres de Service Fabric para facilitar la detección. Al final de este artículo se proporciona un archivo ServiceManifest.xml de ejemplo.

Configure la asignación de los puertos del host al contenedor para que las solicitudes que lleguen al puerto 80 para el servicio se asignen al puerto 80 del contenedor.  En el Explorador de soluciones, abra *MyFirstContainer/ApplicationPackageRoot/ApplicationManifest.xml* y especifique una directiva `PortBinding` en `ContainerHostPolicies`.  Para esta guía de inicio rápido, `ContainerPort` es 80 y `EndpointRef` es "MyContainerServiceTypeEndpoint" (el punto de conexión definido en el manifiesto del servicio).

```xml
<ServiceManifestImport>
...
  <ConfigOverrides />
  <Policies>
    <ContainerHostPolicies CodePackageRef="Code">
      <PortBinding ContainerPort="80" EndpointRef="MyContainerServiceTypeEndpoint"/>
    </ContainerHostPolicies>
  </Policies>
</ServiceManifestImport>
```

Al final de este artículo se proporciona un archivo ApplicationManifest.xml de ejemplo.
>>>>>>> Cambios guardados provisionalmente

## <a name="specify-the-os-build-for-your-container-image"></a>Especificación de la compilación del sistema operativo para la imagen de contenedor
Los contenedores creados con una versión específica de Windows Server podrían no ejecutarse en un host que ejecute una versión distinta de Windows Server. Por ejemplo, los contenedores creados con Windows Server versión 1709 no funcionan en hosts que ejecutan Windows Server 2016. Para más información, consulte [Sistema operativo del contenedor Windows Server y compatibilidad con el sistema operativo del host](service-fabric-get-started-containers.md#windows-server-container-os-and-host-os-compatibility). 

Con la versión 6.1 del runtime de Service Fabric y versiones más recientes, puede especificar varias imágenes de sistema operativo por contenedor y etiquetar cada una con la versión de compilación del sistema operativo en el que deben implementarse. Esto ayuda a asegurarse de que la aplicación se ejecutará en hosts que ejecutan versiones diferentes del sistema operativo Windows. Para más información, consulte [Especificación de las imágenes de contenedores de compilación específica del sistema operativo](service-fabric-get-started-containers.md#specify-os-build-specific-container-images). 

Microsoft publica imágenes distintas para las versiones de IIS que se compilaron con versiones distintas de Windows Server. Para asegurarse de que Service Fabric implementa un contenedor compatible con la versión de Windows Server que se ejecuta en los nodos del clúster donde se implementa la aplicación, agregue las líneas siguientes al archivo *ApplicationManifest.xml*. La versión de compilación de Windows Server 2016 es la 14393 y la versión de compilación de la versión 1709 de Windows Server es la 16299. 

```xml
    <ContainerHostPolicies CodePackageRef="Code"> 
      <ImageOverrides> 
        ...
          <Image Name="microsoft/iis:nanoserverDefault" /> 
          <Image Name= "microsoft/iis:nanoserver" Os="14393" /> 
          <Image Name="microsoft/iis:windowsservercore-1709" Os="16299" /> 
      </ImageOverrides> 
    </ContainerHostPolicies> 
```

El manifiesto de servicio continúa especificando solo una imagen para el servidor nanoserver, `microsoft/iis:nanoserver`. 

## <a name="create-a-cluster"></a>Creación de un clúster

Para implementar la aplicación en un clúster de Azure, puede unirse a un clúster de entidad. Los clústeres de entidad son clústeres de Service Fabric gratuitos y de duración limitada, hospedados en Azure y ejecutados por el equipo de Service Fabric, donde cualquier usuario puede implementar aplicaciones y obtener información sobre la plataforma.  El clúster usa un único certificado autofirmado para la seguridad de nodo a nodo así como para la de cliente a nodo. Los clústeres de la entidad admiten contenedores. Si decide configurar y usar su propio clúster, el clúster debe ejecutarse en una SKU que admita contenedores (por ejemplo, Windows Server 2016 Datacenter con Containers).

Inicie sesión y [únase a un clúster de Windows](http://aka.ms/tryservicefabric). Descargue los certificados PFX en el equipo. Para ello, haga clic en el vínculo **PFX**. Haga clic en el vínculo **How to connect to a secure Party cluster?** (Cómo conectarse a un clúster de entidad segura) y copie la contraseña del certificado. El certificado, la contraseña del certificado y el valor de **Punto de conexión** se usan en los pasos siguientes.

![PFX y punto de conexión](./media/service-fabric-quickstart-containers/party-cluster-cert.png)

> [!Note]
> Hay un número limitado de clústeres de entidad por hora. Si se produce un error al intentar iniciar sesión en un clúster de entidad, puede esperar un tiempo y volver a intentarlo, o puede seguir estos pasos del tutorial [Implementación de una aplicación .NET](https://docs.microsoft.com/azure/service-fabric/service-fabric-tutorial-deploy-app-to-party-cluster#deploy-the-sample-application) para crear un clúster de Service Fabric en su suscripción de Azure e implementar la aplicación en él. El clúster que creó mediante Visual Studio admite Containers. Después de haber implementado y comprobado la aplicación en el clúster, puede pasar directamente a [Manifiestos de servicio y de aplicación de Service Fabric de ejemplo completos](#complete-example-service-fabric-application-and-service-manifests) en esta guía de inicio rápido.
>

En un equipo Windows, instale el archivo PFX en el almacén de certificados *CurrentUser\My*.

```powershell
PS C:\mycertificates> Import-PfxCertificate -FilePath .\party-cluster-873689604-client-cert.pfx -CertStoreLocation Cert:\CurrentUser\My -Password (ConvertTo-SecureString 873689604 -AsPlainText -Force)


  PSParentPath: Microsoft.PowerShell.Security\Certificate::CurrentUser\My

Thumbprint                                Subject
----------                                -------
3B138D84C077C292579BA35E4410634E164075CD  CN=zwin7fh14scd.westus.cloudapp.azure.com
<<<<<<< Updated upstream
``` 
=======
```

Remember the thumbprint for the following step.
>>>>>>> Stashed changes

## Deploy the application to Azure using Visual Studio

Now that the application is ready, you can deploy it to a cluster directly from Visual Studio.

Right-click **MyFirstContainer** in the Solution Explorer and choose **Publish**. The Publish dialog appears.

<<<<<<< Updated upstream
Copy the **Connection Endpoint** from the Party cluster page into the **Connection Endpoint** field. For example, `zwin7fh14scd.westus.cloudapp.azure.com:19000`. 
=======
Copy the **Connection Endpoint** from the Party cluster page into the **Connection Endpoint** field. For example, `zwin7fh14scd.westus.cloudapp.azure.com:19000`. Click **Advanced Connection Parameters** and verify the connection parameter information.  *FindValue* and *ServerCertThumbprint* values must match the thumbprint of the certificate installed in the previous step.

![Publish Dialog](./media/service-fabric-quickstart-containers/publish-app.png)
>>>>>>> Stashed changes

Click **Publish**.

Each application in the cluster must have a unique name.  Party clusters are a public, shared environment however and there may be a conflict with an existing application.  If there is a name conflict, rename the Visual Studio project and deploy again.

Open a browser and navigate to the **Connection endpoint** specified in the Party cluster page. You can optionally prepend the scheme identifier, `http://`, and append the port, `:80`, to the URL. For example, http://zwin7fh14scd.westus.cloudapp.azure.com:80. You should see the IIS default web page:
![IIS default web page][iis-default]

<<<<<<< Updated upstream
=======
## Complete example Service Fabric application and service manifests

Here are the complete service and application manifests used in this quickstart.

### ServiceManifest.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="MyContainerServicePkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType.
         The UseImplicitHost attribute indicates this is a guest service. -->
    <StatelessServiceType ServiceTypeName="MyContainerServiceType" UseImplicitHost="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <!-- Follow this link for more information about deploying Windows containers to Service Fabric: https://aka.ms/sfguestcontainers -->
      <ContainerHost>
        <ImageName>microsoft/iis:nanoserver</ImageName>
      </ContainerHost>
    </EntryPoint>
    <!-- Pass environment variables to your container: -->
    <!--
    <EnvironmentVariables>
      <EnvironmentVariable Name="VariableName" Value="VariableValue"/>
    </EnvironmentVariables>
    -->
  </CodePackage>

  <!-- Config package is the contents of the Config directoy under PackageRoot that contains an 
       independently-updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="MyContainerServiceTypeEndpoint" UriScheme="http" Port="80" Protocol="http"/>
    </Endpoints>
  </Resources>
</ServiceManifest>
```

### <a name="applicationmanifestxml"></a>ApplicationManifest.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest ApplicationTypeName="MyFirstContainerType"
                     ApplicationTypeVersion="1.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric"
                     xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Parameters>
    <Parameter Name="MyContainerService_InstanceCount" DefaultValue="-1" />
  </Parameters>
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion 
       should match the Name and Version attributes of the ServiceManifest element defined in the 
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="MyContainerServicePkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <ContainerHostPolicies CodePackageRef="Code">
        <PortBinding ContainerPort="80" EndpointRef="MyContainerServiceTypeEndpoint"/>
      </ContainerHostPolicies>
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <!-- The section below creates instances of service types, when an instance of this 
         application type is created. You can also create one or more instances of service type using the 
         ServiceFabric PowerShell module.
         
         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
    <Service Name="MyContainerService" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="MyContainerServiceType" InstanceCount="[MyContainerService_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```

>>>>>>> Cambios guardados provisionalmente
## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a hacer lo siguiente:

* Empaquetado de un contenedor de imagen de Docker
* Configuración de la comunicación
* Creación y empaquetamiento de la aplicación de Service Fabric
* Implementación de la aplicación contenedora en Azure

Para más información sobre cómo trabajar con contenedores de Windows en Service Fabric, siga el tutorial para aplicaciones de contenedor de Windows.

> [!div class="nextstepaction"]
> [Creación de una aplicación de contenedor de Windows](./service-fabric-host-app-in-a-container.md)

[iis-default]: ./media/service-fabric-quickstart-containers/iis-default.png
[publish-dialog]: ./media/service-fabric-quickstart-containers/publish-dialog.png
[new-service]: ./media/service-fabric-quickstart-containers/NewService.png
