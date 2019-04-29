---
title: Identidades administradas para recursos de Azure con Azure Service Bus en versión preliminar | Microsoft Docs
description: Uso de identidades administradas para recursos de Azure con Azure Service Bus
services: service-bus-messaging
documentationcenter: na
author: axisc
manager: timlt
editor: spelluru
ms.assetid: ''
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/01/2018
ms.author: aschhab
ms.openlocfilehash: 5edeebd9698384785082e5a441c24e136ed22481
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61317082"
---
# <a name="managed-identities-for-azure-resources-with-service-bus"></a>Identidades administradas para recursos de Azure con Service Bus 

[Identidades administradas para recursos de Azure](../active-directory/managed-identities-azure-resources/overview.md) es una característica de Azure que permite crear una identidad segura asociada a la implementación en la que se ejecuta el código de la aplicación. A continuación, puede asociar esa identidad con los roles de control de acceso que conceden permisos personalizados para acceder a recursos específicos de Azure que la aplicación necesita.

Con las identidades administradas, la plataforma Azure administra esta identidad en tiempo de ejecución. No es necesario almacenar y proteger las claves de acceso en la configuración o el código de la aplicación, ya sea para la propia identidad o para los recursos a los que necesita acceder. Una aplicación cliente de Service Bus que se ejecuta dentro de una aplicación de Azure App Service o en una máquina virtual con entidades administradas habilitadas para admitir recursos de Azure no necesita controlar las reglas y las claves SAS ni cualquier otro token de acceso. La aplicación cliente solo necesita la dirección del punto de conexión del espacio de nombres de mensajería de Service Bus. Cuando se conecta la aplicación, Service Bus enlaza el contexto de la entidad administrada con el cliente en una operación que se muestra en un ejemplo más adelante en este artículo. Una vez creada la asociación con una identidad administrada, el cliente de Service Bus puede realizar todas las operaciones autorizadas. La autorización se concede mediante la asociación de una entidad administrada con roles de Service Bus. 

## <a name="service-bus-roles-and-permissions"></a>Roles y permisos de Service Bus

Solo puede agregar una identidad administrada a los roles "Propietario" o "Colaborador" de un espacio de nombres de Service Bus. De este modo se concede el control total de identidades sobre todas las entidades del espacio de nombres. Pero las operaciones de administración que cambian la topología del espacio de nombres solo se admiten en un principio mediante Azure Resource Manager, y no mediante la interfaz de administración nativa de REST para Service Bus. Esta compatibilidad también significa que no puede usar el objeto [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) del cliente .NET Framework dentro de una identidad administrada.

## <a name="use-service-bus-with-managed-identities-for-azure-resources"></a>Usar Services Bus con identidades administradas para recursos de Azure

En esta sección se describen los pasos necesarios para crear e implementar una aplicación de ejemplo que se ejecuta en una identidad administrada, cómo conceder a dicha identidad acceso a un espacio de nombres de mensajería de Service Bus y cómo la aplicación interactúa con identidades de Service Bus mediante dicha identidad.

En esta introducción se describe una aplicación web hospedada en [Azure App Service](https://azure.microsoft.com/services/app-service/). Los pasos necesarios para una aplicación hospedada en máquinas virtuales son similares.

### <a name="create-an-app-service-web-application"></a>Creación de una aplicación web de App Service

El primer paso es crear una aplicación ASP.NET de App Service. Si no está familiarizado con cómo hacerlo en Azure, siga [esta guía de procedimientos](../app-service/app-service-web-get-started-dotnet-framework.md). Sin embargo, en lugar de crear una aplicación de MVC, tal como se muestra en el tutorial, cree una aplicación de formularios Web Forms.

### <a name="set-up-the-managed-identity"></a>Configuración de la identidad administrada

Una vez creada la aplicación, vaya a la aplicación web recién creada en Azure Portal (también se muestra en la guía de procedimientos); a continuación, vaya a la página **Identidad de servicio administrada** y habilite la característica: 

![](./media/service-bus-managed-service-identity/msi1.png)

Una vez habilitada la característica, se crea una identidad de servicio en Azure Active Directory y se configura en el host de App Service.

### <a name="create-a-new-service-bus-messaging-namespace"></a>Creación de un espacio de nombres de mensajería de Service Bus

A continuación, [cree un espacio de nombres de mensajería de Service Bus](service-bus-create-namespace-portal.md) en una de las regiones de Azure compatibles con la versión preliminar de RBAC: **Este de EE. UU.**, **Este de EE. UU. 2** o **Europa Occidental**. 

Vaya a la página **Control de acceso (IAM)** del espacio de nombres en el portal y, después, haga clic en **Agregar asignación de roles** para agregar la identidad administrada al rol **Propietario**. Para ello, busque el nombre de la aplicación web en el campo **Seleccionar** del panel **Agregar permisos** y, a continuación, haga clic en la entrada. A continuación, haga clic en **Guardar**.

La identidad administrada de la aplicación web ya tiene acceso al espacio de nombres de Service Bus y a la cola que ha creado antes. 

### <a name="run-the-app"></a>Ejecución de la aplicación

Ahora, modifique la página predeterminada de la aplicación de ASP.NET que ha creado. Puede utilizar el código de la aplicación web de [este repositorio de GitHub](https://github.com/Azure-Samples/app-service-msi-servicebus-dotnet).  

La página Default.aspx es su página de destino. El código puede encontrarse en el archivo Default.aspx.cs. El resultado es una aplicación web básica con unos cuantos campos de entrada y con los botones **enviar** y **recibir** que se conectan a Service Bus para enviar o recibir mensajes.

Tenga en cuenta cómo se inicializa el objeto [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory). En lugar de usar el proveedor de tokens de Token de acceso compartido (SAS), el código crea un proveedor de tokens para la identidad administrada con la llamada a `TokenProvider.CreateManagedServiceIdentityTokenProvider(ServiceAudience.ServiceBusAudience)`. Por lo tanto, no hay ningún secreto para conservar y usar. El proveedor de tokens controla automáticamente el flujo del contexto de la identidad administrada para Service Bus y el protocolo de enlace de autorización. Es un modelo más sencillo que si se usa SAS.

Una vez realizados estos cambios, publique y ejecute la aplicación. Puede obtener fácilmente los datos de publicación correctos mediante la descarga y posterior importación de un perfil de publicación en Visual Studio:

![](./media/service-bus-managed-service-identity/msi3.png)
 
Para enviar o recibir mensajes, escriba el nombre del espacio de nombres y el nombre de la entidad que ha creado. Después, haga clic en **Enviar** o **Recibir**.


> [!NOTE]
> - La identidad administrada solo funciona en el entorno de Azure, en servicios de aplicaciones, en máquinas virtuales de Azure y en conjuntos de escalado. En el caso de las aplicaciones de .NET, la biblioteca Microsoft.Azure.Services.AppAuthentication, que es la que usa el paquete NuGet de Service Bus, proporciona una abstracción sobre este protocolo y admite una experiencia de desarrollo local. Esta biblioteca también permite probar el código localmente en una máquina de desarrollo, con su cuenta de usuario de Visual Studio, la CLI de Azure 2.0 o la autenticación integrada de Active Directory. Para más información acerca de las opciones de desarrollo local con esta biblioteca, consulte [Autenticación entre servicios en Azure Key Vault mediante .NET](../key-vault/service-to-service-authentication.md).  
> 
> - Actualmente, las identidades administradas no funcionan con las ranuras de implementación de App Service.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la mensajería de Service Bus, consulte los siguientes temas:

* [Colas, temas y suscripciones de Service Bus](service-bus-queues-topics-subscriptions.md)
* [Introducción a las colas de Service Bus](service-bus-dotnet-get-started-with-queues.md)
* [Uso de temas y suscripciones de Service Bus](service-bus-dotnet-how-to-use-topics-subscriptions.md)