---
title: Identidades administradas para recursos de Azure con Azure Event Hubs | Microsoft Docs
description: En este artículo se proporciona información sobre cómo usar identidades administradas para recursos de Azure con Azure Event Hubs.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.date: 05/20/2019
ms.author: shvija
ms.openlocfilehash: dbef1db94d7835bd9326102bd62921c6b3d88d74
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/01/2019
ms.locfileid: "68707067"
---
# <a name="managed-identities-for-azure-resources-with-event-hubs"></a>Identidades administradas para recursos de Azure con Event Hubs

[Identidades administradas para recursos de Azure](../active-directory/managed-identities-azure-resources/overview.md) es una característica de Azure que permite crear una identidad segura asociada a la implementación en la que se ejecuta el código de la aplicación. A continuación, puede asociar esa identidad con los roles de control de acceso que conceden permisos personalizados para acceder a recursos específicos de Azure que la aplicación necesita. 

Con las identidades administradas, la plataforma Azure administra esta identidad en tiempo de ejecución. No es necesario almacenar y proteger las claves de acceso en la configuración o el código de la aplicación, ya sea para la propia identidad o para los recursos a los que necesita acceder. Una aplicación cliente de Event Hubs que se ejecuta dentro de una aplicación de Azure App Service o en una máquina virtual con la compatibilidad habilitada para identidades administradas para recursos de Azure no necesita controlar las reglas y claves SAS ni cualquier otro token de acceso. La aplicación cliente solo necesita la dirección del punto de conexión del espacio de nombres de Event Hubs. Cuando se conecta la aplicación, Event Hubs enlaza el contexto de la identidad administrada con el cliente en una operación que se muestra en un ejemplo más adelante en este artículo.

Una vez creada la asociación con una identidad administrada, un cliente de Event Hubs puede realizar todas las operaciones autorizadas. La autorización se concede mediante la asociación de una identidad administrada con los roles de Event Hubs. 

## <a name="event-hubs-roles-and-permissions"></a>Roles y permisos de Event Hubs
Puede agregar una identidad administrada al rol **Event Hubs Data Owner** (Propietario de los datos de Event Hubs) de un espacio de nombres de Event Hubs. Este rol concede la identidad, el control total (para las operaciones de administración y de datos) en todas las entidades del espacio de nombres.

>[!IMPORTANT]
> Antes se podía agregar una identidad administrada a los roles **Propietario** o **Colaborador**, pero los privilegios de acceso a datos de los roles **Propietario** y **Colaborador** ya no están en vigor. Si está usando el rol **Propietario** o **Colaborador**, cambie al rol **Event Hubs Data Owner** (Propietario de los datos Event Hubs).

Para usar el nuevo rol integrado, siga estos pasos: 

1. Vaya a [Azure Portal](https://portal.azure.com).
2. Navegue hasta el espacio de nombres de Event Hubs.
3. En la página **Espacio de nombres de Event Hubs**, seleccione **Control de acceso (IAM)** en el menú de la izquierda.
4. En la página **Control de acceso (IAM)** , seleccione **Agregar** en la sección **Agregar una asignación de roles**. 

    ![Botón Agregar asignación de roles](./media/event-hubs-managed-service-identity/add-role-assignment-button.png)
5. En la página **Agregar asignación de roles**, siga estos pasos: 
    1. Para **Rol**, seleccione **Azure Event Hubs Data Owner** (Propietario de los datos Event Hubs). 
    2. Seleccione la **identidad** que se va a agregar al rol.
    3. Seleccione **Guardar**. 

        ![Rol Propietario de los datos de Event Hubs](./media/event-hubs-managed-service-identity/add-role-assignment-dialog.png)
6. Cambie a la página **Asignaciones de roles** y confirme que el usuario se agrega al rol **Azure Event Hubs Data Owner** (Propietario de los datos de Event Hubs). 

    ![Confirmación de que el usuario se agrega al rol](./media/event-hubs-managed-service-identity/role-assignments.png)
 
## <a name="use-event-hubs-with-managed-identities-for-azure-resources"></a>Uso de Event Hubs con identidades administradas para recursos de Azure

En la sección siguiente se describen estos pasos:

1. Crear e implementar una aplicación de ejemplo que se ejecute bajo una identidad administrada.
2. Conceder ese acceso de identidad a un espacio de nombres de Event Hubs.
3. Cómo interactúa la aplicación con Events Hubs mediante dicha identidad.

En esta introducción se describe una aplicación web hospedada en [Azure App Service](https://azure.microsoft.com/services/app-service/). Los pasos necesarios para una aplicación hospedada en máquinas virtuales son similares.

### <a name="create-an-app-service-web-application"></a>Creación de una aplicación web de App Service

El primer paso es crear una aplicación ASP.NET de App Service. Si no está familiarizado con cómo hacer esto en Azure, siga [esta guía de procedimientos](../app-service/app-service-web-get-started-dotnet-framework.md). Sin embargo, en lugar de crear una aplicación de MVC, tal como se muestra en el tutorial, cree una aplicación de formularios Web Forms.

### <a name="set-up-the-managed-identity"></a>Configuración de la identidad administrada

Una vez creada la aplicación, vaya a la aplicación web recién creada en Azure Portal (también se muestra en la guía de procedimientos); a continuación, vaya a la página **Identidad de servicio administrada** y habilite la característica: 

![Página Managed Service Identity](./media/event-hubs-managed-service-identity/msi1.png)
 
Una vez habilitada la característica, se crea una identidad de servicio en Azure Active Directory y se configura en el host de App Service.

### <a name="create-a-new-event-hubs-namespace"></a>Creación de un espacio de nombres de Event Hubs

A continuación, [cree un espacio de nombres de Event Hubs](event-hubs-create.md). 

Vaya a la página **Control de acceso (IAM)** del espacio de nombres en el portal y, después, haga clic en **Agregar asignación de roles** para agregar la identidad administrada al rol **Propietario**. Para ello, busque el nombre de la aplicación web en el campo **Seleccionar** del panel **Agregar permisos** y, a continuación, haga clic en la entrada. A continuación, haga clic en **Guardar**. La identidad administrada de la aplicación web ya tiene acceso al espacio de nombres de Event Hubs y al centro de eventos que creó anteriormente. 

### <a name="run-the-app"></a>Ejecución de la aplicación

Ahora, modifique la página predeterminada de la aplicación de ASP.NET que ha creado. También puede utilizar el código de la aplicación web de [este repositorio de GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/Rbac/ManagedIdentityWebApp). 

Una vez que inicie la aplicación, apunte el explorador a EventHubsMSIDemo.aspx. También puede establecerla como la página de inicio. El código puede encontrarse en el archivo EventHubsMSIDemo.aspx.cs. El resultado es una aplicación web mínima con pocos campos de entrada y con botones para **enviar** y **recibir** que se conectan a Event Hubs para enviar o recibir eventos. 

Tenga en cuenta cómo se inicializa el objeto [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory). En lugar de usar el proveedor de tokens de Token de acceso compartido (SAS), el código crea un proveedor de tokens para la identidad administrada con la llamada a `TokenProvider.CreateManagedServiceIdentityTokenProvider(ServiceAudience.EventHubAudience)`. Por lo tanto, no hay ningún secreto para conservar y usar. El proveedor de tokens controla automáticamente el flujo del contexto de la identidad administrada para Event Hubs y el protocolo de enlace de autorización, que es un modelo más sencillo que el uso de SAS.

Una vez realizados estos cambios, publique y ejecute la aplicación. Puede obtener los datos de publicación correctos mediante la descarga y posterior importación de un perfil de publicación en Visual Studio:

![Importar perfil de publicación](./media/event-hubs-managed-service-identity/msi3.png)
 
Para enviar o recibir mensajes, escriba el nombre del espacio de nombres y el nombre de la entidad que creó y, a continuación, haga clic en **enviar** o **recibir**. 
 
La identidad administrada solo funciona dentro del entorno de Azure y únicamente en la implementación de App Service en que la configuró. Las identidades administradas no funcionan con las ranuras de implementación de App Service en este momento.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca de Event Hubs, visite los vínculos siguientes:

* Empiece por un [tutorial de Event Hubs](event-hubs-dotnet-standard-getstarted-send.md)
* [Preguntas más frecuentes sobre Event Hubs](event-hubs-faq.md)
* [Detalles de precios de Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/)
* [Aplicaciones de ejemplo que usan Event Hubs](https://github.com/Azure/azure-event-hubs/tree/master/samples)
