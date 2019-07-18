---
title: Versión preliminar del control de acceso basado en rol (RBAC) de Azure Service Bus | Microsoft Docs
description: Control de acceso basado en rol de Azure Service Bus
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
ms.date: 09/19/2018
ms.author: aschhab
ms.openlocfilehash: 6f5390162ce56a0e77ef41740d7e88f3546c5530
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67444730"
---
# <a name="active-directory-role-based-access-control-preview"></a>Control de acceso basado en rol de Active Directory (versión preliminar)

Microsoft Azure proporciona una administración integrada del control de acceso para recursos y aplicaciones basados en Azure Active Directory (Azure AD). Con Azure AD, puede administrar las cuentas y aplicaciones del usuario específicamente para sus aplicaciones basadas en Azure, o puede federar la infraestructura existente de Active Directory con Azure AD para un inicio de sesión único para toda la empresa que también abarque los recursos de Azure y las aplicaciones hospedadas en Azure. Posteriormente, puede asignar esas identidades de usuario y de aplicación a roles globales y específicos del servicio para otorgar acceso a los recursos de Azure.

Para Azure Service Bus, la administración de los espacios de nombres y de todos los recursos relacionados mediante Azure Portal y la API de administración de recursos de Azure, ya se ha protegido mediante el modelo de *control de acceso basado en rol* (RBAC). El control de acceso basado en rol de las operaciones en tiempo de ejecución es una característica que está ahora en versión preliminar pública.

Una aplicación que use el control de acceso basado en rol de Azure AD no necesita controlar las reglas y claves de SAS ni ningún otro token de acceso. La aplicación cliente permite interactuar con Azure AD para establecer un contexto de autenticación y permite adquirir un token de acceso para Service Bus. Con respecto a las cuentas de usuario de dominio que requieren un inicio de sesión interactivo, la aplicación no controla nunca ninguna credencial directamente.

## <a name="service-bus-roles-and-permissions"></a>Roles y permisos de Service Bus

Azure proporciona los siguientes roles RBAC integrados para autorizar el acceso a un espacio de nombres de Service Bus:

* [Propietario de datos de Azure Service Bus (versión preliminar)](../role-based-access-control/built-in-roles.md#azure-service-bus-data-owner-preview): permite el acceso a datos en el espacio de nombres de Service Bus y sus entidades (colas, temas, suscripciones y filtros).

>[!IMPORTANT]
> Antes, se podía agregar una identidad administrada a los roles **"Propietario"** o **"Colaborador"** ,
>
> pero los privilegios de acceso a datos de los roles **"Propietario"** y **"Colaborador"** ya no tienen vigor. Si usaba los roles **"Propietario"** o **"Colaborador"** , deberá adaptarlos para usar el rol **"Propietario de datos de Azure Service Bus (versión preliminar)"** .

## <a name="use-service-bus-with-an-azure-ad-domain-user-account"></a>Uso de Service Bus con una cuenta de usuario de dominio de Azure AD

En la siguiente sección se describen los pasos necesarios para crear y ejecutar una aplicación de ejemplo que solicita un usuario interactivo de Azure AD para iniciar sesión, cómo conceder acceso a Service Bus a esa cuenta de usuario y cómo utilizar esa identidad para acceder a Event Hubs.

En esta introducción se describe una aplicación de consola simple, y el [código para esta aplicación se encuentra en GitHub](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/RoleBasedAccessControl).

### <a name="create-an-active-directory-user-account"></a>Creación de una cuenta de usuario de Active Directory

El primer paso es opcional. Cada suscripción de Azure se empareja automáticamente con un inquilino de Azure Active Directory y, si tiene acceso a una suscripción de Azure, su cuenta de usuario ya está registrada. Esto significa que solamente puede usar su cuenta.

Si desea crear una cuenta específica para este escenario, [siga estos pasos](../automation/automation-create-aduser-account.md). Debe tener permiso para crear cuentas en el inquilino de Azure Active Directory, lo cual puede que no sea el caso para escenarios empresariales más grandes.

### <a name="create-a-service-bus-namespace"></a>Creación de un espacio de nombres de Service Bus

Después, [cree un espacio de nombres de mensajería de Service Bus](service-bus-create-namespace-portal.md).

Una vez creado el espacio de nombres, vaya a su página de **Control de acceso (IAM)** en Azure Portal y, a continuación, haga clic en **Agregar asignación de roles** para agregar la cuenta de usuario de Azure AD al rol de propietario. Si usa su propia cuenta de usuario y ha creado el espacio de nombres, ya está en el rol de propietario. Para agregar una cuenta diferente al rol, busque el nombre de la aplicación web en el campo **Seleccionar** del panel **Agregar permisos** y, a continuación, haga clic en la entrada. A continuación, haga clic en **Guardar**.

Ahora, la cuenta de usuario ya tiene acceso al espacio de nombres de Service Bus y a la cola que creó anteriormente.

### <a name="register-the-application"></a>Registro de la aplicación

Antes de poder ejecutar la aplicación de ejemplo, regístrela en Azure AD y apruebe la petición de consentimiento que permite que la aplicación pueda acceder a Azure Service Bus en su nombre.

Dado que la aplicación de ejemplo es una aplicación de consola, debe registrar una aplicación nativa y agregar permisos de API para **Microsoft.ServiceBus** en el conjunto de "permisos necesarios". Las aplicaciones nativas también necesitan un **URI de redireccionamiento** en Azure AD que actúe como identificador. No es necesario que este URI sea un destino de red. Use `https://servicebus.microsoft.com` para este ejemplo, dado que el ejemplo de código ya utiliza ese URI.

Se proporciona información detallada sobre los pasos de registro en [este tutorial](../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md). Siga los pasos para registrar una aplicación **nativa** y, a continuación, siga las instrucciones de actualización para agregar **Microsoft.ServiceBus** API a los permisos necesarios. A medida que vaya realizando los pasos, tome nota del valor de **TenantId** y **ApplicationId**, ya que los necesitará para ejecutar la aplicación.

### <a name="run-the-app"></a>Ejecución de la aplicación

Antes de poder ejecutar el ejemplo, edite el archivo App.config y, según el escenario, establezca los siguientes valores:

- `tenantId`: establézcalo en el valor de **TenantId**.
- `clientId`: establézcalo en el valor de **ApplicationId**.
- `clientSecret`: si desea iniciar sesión mediante el secreto de cliente, créelo en Azure AD. Además, utilice una aplicación web o API en lugar de una aplicación nativa. Además, agregue la aplicación en **Control de acceso (IAM)** en el espacio de nombres que creó anteriormente.
- `serviceBusNamespaceFQDN`: establézcalo en el nombre DNS completo del espacio de nombres de Service Bus recién creado. Por ejemplo, `example.servicebus.windows.net`.
- `queueName`: establézcalo en el nombre de la cola que creó.
- El URI de redireccionamiento que especificó en la aplicación en los pasos anteriores.

Al ejecutar la aplicación de consola, deberá seleccionar un escenario. Haga clic en **Interactive User Login** (Inicio de sesión de usuario interactivo). Para ello, escriba su número y pulse ENTRAR. La aplicación muestra una ventana de inicio de sesión, solicita su consentimiento para acceder a Service Bus y, a continuación, utiliza el servicio para ejecutarse en el escenario de envío o recepción mediante la identidad de inicio de sesión.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre la mensajería de Service Bus, consulte los siguientes temas:

* [Colas, temas y suscripciones de Service Bus](service-bus-queues-topics-subscriptions.md)
* [Introducción a las colas de Service Bus](service-bus-dotnet-get-started-with-queues.md)
* [Uso de temas y suscripciones de Service Bus](service-bus-dotnet-how-to-use-topics-subscriptions.md)
