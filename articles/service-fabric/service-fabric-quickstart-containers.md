---
title: Creación de una aplicación contenedor de Windows de Service Fabric en Azure | Microsoft Azure
description: En esta guía de inicio rápido, creará su primera aplicación contenedora Windows en Azure Service Fabric.
services: service-fabric
documentationcenter: .net
author: TylerMSFT
manager: timlt
editor: vturecek
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: quickstart
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/30/2018
ms.author: twhitney
ms.custom: mvc
ms.openlocfilehash: 2855d28a3d5414413ca1657a7bef9c060f6d4424
ms.sourcegitcommit: d372d75558fc7be78b1a4b42b4245f40f213018c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2018
ms.locfileid: "51300343"
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

![Cuadro de diálogo Nuevo servicio][new-service]

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

Inicie sesión y [únase a un clúster de Windows](https://aka.ms/tryservicefabric). Descargue los certificados PFX en el equipo. Para ello, haga clic en el vínculo **PFX**. Haga clic en el vínculo **How to connect to a secure Party cluster?** (Cómo conectarse a un clúster de entidad segura) y copie la contraseña del certificado. El certificado, la contraseña del certificado y el valor de **Punto de conexión** se usan en los pasos siguientes.

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
```

Recuerde la huella digital para el paso siguiente.

## <a name="deploy-the-application-to-azure-using-visual-studio"></a>Implementación de la aplicación en Azure con Visual Studio

Ahora que la aplicación está lista, puede implementarla en un clúster directamente desde Visual Studio.

Haga clic con el botón derecho en **MyFirstContainer** en el Explorador de soluciones y seleccione **Publicar**. Aparece el cuadro de diálogo de publicación.

Copie el valor de **Punto de conexión** de la página Clúster de entidad en el campo **Punto de conexión**. Por ejemplo, `zwin7fh14scd.westus.cloudapp.azure.com:19000`. Haga clic en **Parámetros de conexión avanzada** y compruebe la información de los parámetros de conexión.  Los valores de *FindValue* y *ServerCertThumbprint* deben coincidir con la huella digital del certificado instalado en el paso anterior.

![Cuadro de diálogo de publicación](./media/service-fabric-quickstart-containers/publish-app.png)

Haga clic en **Publicar**.

Todas las aplicaciones del clúster deben tener un nombre único.  Sin embargo, los clústeres de entidad son un entorno compartido y público, por lo que es posible que se produzca un conflicto con una aplicación existente.  Si se produce un conflicto de nombres, cambie el nombre del proyecto de Visual Studio y vuelva a realizar la implementación.

Abra un explorador y vaya al **punto de conexión** especificado en la página del clúster de la entidad. Si lo desea, puede anteponer el identificador de esquema `http://` y agregar el puerto `:80` a la dirección URL. Por ejemplo, http://zwin7fh14scd.westus.cloudapp.azure.com:80. Debería ver la página web predeterminada de IIS: ![Página web predeterminada de IIS][iis-default]

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
