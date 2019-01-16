---
title: Acceso seguro a Azure Logic Apps | Microsoft Docs
description: Incorporación de seguridad para Azure Logic Apps, incluidos desencadenadores, entradas y salidas, parámetros y otros servicios
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: kevinlam1
ms.author: klam
ms.reviewer: estfan, LADocs
ms.assetid: 9fab1050-cfbc-4a8b-b1b3-5531bee92856
ms.topic: article
ms.date: 01/08/2019
ms.openlocfilehash: a7d34b76eb6184e546c8217aa6b3723819be70be
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/10/2019
ms.locfileid: "54189537"
---
# <a name="secure-access-in-azure-logic-apps"></a>Protección del acceso en Azure Logic Apps

Estos son los elementos de la aplicación lógica donde puede proteger el acceso:

* [Solicitud o desencadenadores de Webhook](#secure-triggers)
* [Operaciones como la administración, edición o visualización](#secure-operations) de una aplicación lógica
* [Entradas y salidas](#secure-run-history) del historial de ejecución de la aplicación lógica
* [Parámetros de acción y entradas](#secure-action-parameters)
* [Servicios que obtienen solicitudes](#secure-requests) desde la aplicación lógica

<a name="secure-triggers"></a>

## <a name="secure-access-to-request-triggers"></a>Acceso seguro a desencadenadores de solicitud

Cuando la aplicación lógica usa un desencadenador basado en solicitud HTTP, como el desencadenador [Solicitud](../connectors/connectors-native-reqres.md) o [Webhook](../connectors/connectors-native-webhook.md), puede restringir el acceso para que solo los clientes autorizados puedan iniciar la aplicación lógica. Todas las solicitudes que recibe una aplicación lógica están cifrados y protegidos con el protocolo Capa de sockets seguros (SSL). Existen distintas formas de proteger el acceso a este tipo de desencadenador:

* [Generación de firmas de acceso compartido](#sas)
* [Restricción de las direcciones IP entrantes](#restrict-incoming-IP)
* [Incorporación de Azure Active Directory, OAuth u otra medida de seguridad](#add-authentication)

<a name="sas"></a>

### <a name="generate-shared-access-signatures"></a>Generación de firmas de acceso compartido

Cada de punto de conexión de la solicitud de una aplicación lógica incluye una [Firma de acceso compartido (SAS)](../storage/common/storage-dotnet-shared-access-signature-part-1.md) en la dirección URL del punto de conexión. Cada dirección URL contiene un parámetro de consulta `sp`, `sv` y `sig`:

* `sp` especifica los permisos, que se asignan a los métodos HTTP permitidos para su uso.
* `sv` especifica la versión usada para generar la firma.
* `sig` se usa para autenticar el acceso al desencadenador.

La firma se genera mediante el algoritmo SHA256 con una clave de acceso secreta en todas las rutas de acceso de direcciones URL y propiedades. La clave secreta nunca se expone ni se publica, y se mantiene cifrada y almacenada con la aplicación lógica. La aplicación lógica solo autoriza los desencadenadores que contienen una firma válida creada con la clave secreta. 

Más información sobre la protección del acceso con Firma de acceso compartido:

* [Regenerar las claves de acceso](#access-keys)
* [Crear direcciones URL de devolución de llamada de expiración próxima](#expiring-URLs)
* [Crear direcciones URL con clave principal o secundaria](#primary-secondary-key)

<a name="access-keys"></a>

#### <a name="regenerate-access-keys"></a>Regenerar las claves de acceso

Para regenerar una clave de acceso nueva en cualquier momento, use API REST de Azure o Azure Portal. Se invalidan todas las direcciones URL generadas previamente que usan la clave anterior y ya no tienen autorización para desencadenar la aplicación lógica. Las direcciones URL que se recuperan tras la regeneración se firman con la nueva clave de acceso.

1. En Azure Portal, abra la aplicación lógica donde está la clave que quiere regenerar.

1. En el menú de la aplicación lógica, en **Configuración**, seleccione **Claves de acceso**.

1. Seleccione la clave que quiere regenerar y complete el proceso.

<a name="expiring-urls"></a>

#### <a name="create-callback-urls-with-expiration-dates"></a>Creación de direcciones URL de devolución de llamada con fechas de expiración

Si comparte una dirección URL del punto de conexión del desencadenador basado en una solicitud con otras partes, es posible generar direcciones URL de devolución de llamada con claves y fechas de expiración específicas, según sea necesario. De esta forma puede acumular claves sin problema o restringir el acceso para desencadenar la aplicación lógica a un determinado intervalo de tiempo. Puede especificar una fecha de expiración para una dirección URL mediante la [API REST de Logic Apps](https://docs.microsoft.com/rest/api/logic/workflowtriggers), por ejemplo:

``` http
POST 
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Logic/workflows/{workflowName}/triggers/{triggerName}/listCallbackUrl?api-version=2016-06-01
```

En el cuerpo, incluya la propiedad `NotAfter` mediante una cadena de fecha de JSON. Esta propiedad devuelve una dirección URL de devolución de llamada que solo es válida hasta la fecha y hora `NotAfter`.

<a name="primary-secondary-key"></a>

#### <a name="create-urls-with-primary-or-secondary-secret-key"></a>Creación de direcciones URL con clave secreta principal o secundaria

Al generar o enumerar direcciones URL de devolución de llamada para desencadenadores basados en solicitud, también puede especificar qué clave se va a usar para firmar la dirección URL. Puede generar una dirección URL firmada por una clave específica mediante la [API REST de Logic Apps](https://docs.microsoft.com/rest/api/logic/workflowtriggers), por ejemplo:

``` http
POST 
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Logic/workflows/{workflowName}/triggers/{triggerName}/listCallbackUrl?api-version=2016-06-01
```

En el cuerpo, incluya la propiedad `KeyType` como `Primary` o `Secondary`. Esta propiedad devuelve una dirección URL firmada por una clave segura especificada.

<a name="restrict-incoming-ip"></a>

### <a name="restrict-incoming-ip-addresses"></a>Restricción de las direcciones IP entrantes

Junto con la Firma de acceso compartido, es posible que quiera limitar los clientes específicos que pueden llamar a su aplicación lógica.  
Por ejemplo, si administra el punto de conexión de solicitud con Azure API Management, puede restringir la aplicación lógica para que solo acepte solicitudes provenientes de la dirección IP de la instancia de API Management. 

#### <a name="set-ip-ranges---azure-portal"></a>Establecimiento de intervalos IP: Azure Portal

Para configurar esta restricción en Azure Portal, vaya a la configuración de la aplicación lógica: 

1. En Azure Portal, abra la aplicación lógica en Diseñador de aplicación lógica. 

1. En el menú de la aplicación lógica, en **Configuración**, seleccione **Configuración de flujo de trabajo**.

1. En **Configuración de control de acceso** > 
**Direcciones IP entrantes permitidas**, seleccione **Intervalos IP específicos**.

1. En **Intervalos IP para desencadenadores**, especifique los intervalos de direcciones IP que acepta el desencadenador. Un intervalo IP válido usa estos formatos: *x.x.x.x/x* o *x.x.x.x-x.x.x.x* 

Si quiere que la aplicación lógica solo active una aplicación lógica anidada, en la lista **Direcciones IP entrantes permitidas**, seleccione **Cualquier otra aplicación lógica**. Con esta opción se escribe una matriz vacía en el recurso de la aplicación lógica, por lo que solo las llamadas desde el servicio Logic Apps (aplicaciones lógicas principales) pueden desencadenar la aplicación lógica anidada.

> [!NOTE]
> Independientemente de la dirección IP, de todos modos puede ejecutar una aplicación lógica que tiene un desencadenador basado en solicitud mediante el uso de `/triggers/{triggerName}/run` a través de la API REST de Azure o de API Management. Sin embargo, este escenario sigue requiriendo la autenticación con la API REST de Azure y todos los eventos aparecen en el registro de auditoría de Azure. Asegúrese de establecer las directivas de control de acceso como corresponda.

#### <a name="set-ip-ranges---logic-app-deployment-template"></a>Establecimiento de intervalos IP: plantilla de implementación de aplicación lógica

Si automatiza las implementaciones de aplicaciones lógicas mediante una [plantilla de implementación de Azure Resource Manager](logic-apps-create-deploy-template.md), puede establecer los intervalos IP en esa plantilla, por ejemplo:

``` json
{
   "properties": {
      "definition": {},
      "parameters": {},
      "accessControl": {
         "triggers": {
            "allowedCallerIpAddresses": [
               {
               "addressRange": "192.168.12.0/23"
               },
               {
                  "addressRange": "2001:0db8::/64"
               }
            ]
         }
      }
   },
   "type": "Microsoft.Logic/workflows",
}
```

<a name="add-authentication"></a>

### <a name="add-azure-active-directory-oauth-or-other-security"></a>Incorporación de Azure Active Directory, OAuth u otra medida de seguridad

Para agregar más protocolos de autorización a la aplicación lógica, considere la posibilidad de usar [Azure API Management](https://azure.microsoft.com/services/api-management/). Este servicio ofrece supervisión enriquecida, seguridad, directiva y documentación para cualquier punto de conexión y le ofrece la funcionalidad de exponer la aplicación lógica como una API. API Management puede exponer un punto de conexión público o privado para la aplicación lógica, que luego puede usar Azure Active Directory, OAuth, certificado u otros estándares de seguridad. Cuando API Management recibe una solicitud, el servicio la envía a la aplicación lógica y también hace cualquier transformación o restricción necesaria en el proceso. Para permitir que solo API Management desencadene la aplicación lógica, puede usar la configuración del intervalo IP entrante de la aplicación lógica. 

<a name="secure-operations"></a>

## <a name="secure-access-to-logic-app-operations"></a>Protección del acceso a las operaciones de aplicación lógica

Para permitir que solo usuarios o grupos específicos ejecuten las operaciones en una aplicación lógica, se puede restringir el acceso en tareas como administración, edición y visualización. Logic Apps admite [Control de acceso basado en rol (RBAC) de Azure](../role-based-access-control/role-assignments-portal.md), que se puede personalizar o asigna roles integrados a los miembros de la suscripción, por ejemplo:

* **Colaborador de aplicación lógica**: los usuarios pueden ver, editar y actualizar la aplicación lógica. Este rol no puede eliminar la aplicación lógica ni ejecutar las operaciones de administrador.
* **Operador de aplicación lógica**: los usuarios pueden ver la aplicación lógica y el historial de ejecución, y habilitar o deshabilitar la aplicación lógica. Este rol no puede editar ni actualizar la aplicación lógica.

Para evitar que otros usuarios cambien o elimine la aplicación lógica, puede usar el [bloqueo de recursos de Azure](../azure-resource-manager/resource-group-lock-resources.md). Esta funcionalidad le permite evitar que otros usuarios cambien o eliminen los recursos de producción.

<a name="secure-run-history"></a>

## <a name="secure-access-to-logic-app-run-history"></a>Protección del acceso al historial de ejecución de la aplicación lógica

Para proteger el contenido que se transmite como entradas o salidas desde ejecuciones anteriores de la aplicación lógica, puede restringir el acceso a intervalos de direcciones IP específicos. Esta funcionalidad le ofrece más control de acceso. Todos los datos de ejecución de una aplicación lógica están cifrados cuando están en tránsito y en reposo. Cuando se solicita el historial de ejecución de una aplicación lógica, Logic Apps autentica esa solicitud y proporciona vínculos a las entradas y salidas de las solicitudes y respuestas del flujo de trabajo de la aplicación lógica. Puede proteger estos vínculos para que solo las solicitudes provenientes de una dirección IP específica devuelvan ese contenido. Por ejemplo, incluso puede especificar una dirección IP como `0.0.0.0-0.0.0.0` para que nadie pueda acceder a las entradas y salidas. Solo una persona con permisos de administrador puede quitar esta restricción, proporcionando la posibilidad para acceso "Just-In-Time" al contenido de la aplicación lógica.

### <a name="set-ip-ranges---azure-portal"></a>Establecimiento de intervalos IP: Azure Portal

Para configurar esta restricción en Azure Portal, vaya a la configuración de la aplicación lógica:

1. En Azure Portal, abra la aplicación lógica en Diseñador de aplicación lógica. 

1. En el menú de la aplicación lógica, en **Configuración**, seleccione **Configuración de flujo de trabajo**.

1. En **Configuración de control de acceso** > 
**Direcciones IP entrantes permitidas**, seleccione **Intervalos IP específicos**.

1. En **Intervalos IP para contenido**, especifique los intervalos de direcciones IP que pueden acceder a contenido desde las entradas y salidas. Un intervalo IP válido usa estos formatos: *x.x.x.x/x* o *x.x.x.x-x.x.x.x* 

### <a name="set-ip-ranges---logic-app-deployment-template"></a>Establecimiento de intervalos IP: plantilla de implementación de aplicación lógica

Si automatiza las implementaciones de aplicaciones lógicas mediante una [plantilla de implementación de Azure Resource Manager](logic-apps-create-deploy-template.md), puede establecer los intervalos IP en esa plantilla, por ejemplo:

``` json
{
   "properties": {
      "definition": {},
      "parameters": {},
      "accessControl": {
         "contents": {
            "allowedCallerIpAddresses": [
               {
               "addressRange": "192.168.12.0/23"
               },
               {
                  "addressRange": "2001:0db8::/64"
               }
            ]
         }
      }
   },
   "type": "Microsoft.Logic/workflows",
}
```

<a name="secure-action-parameters"></a>

## <a name="secure-action-parameters-and-inputs"></a>Protección de parámetros de acción y entradas

Al realizar la implementación en distintos entornos, puede que quiera parametrizar aspectos específicos de la definición de flujo de trabajo de la aplicación lógica. Por ejemplo, puede especificar parámetros en la [plantilla de implementación de Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md#parameters). Para acceder al valor de parámetro de un recurso durante el tiempo de ejecución, puede usar la expresión `@parameters('parameterName')`, proporcionada por el [lenguaje de definición de flujo de trabajo](https://aka.ms/logicappsdocs). 

También puede proteger los parámetros específicos que no quiere que se muestre mientras edita el flujo de trabajo de la aplicación lógica al usar el tipo de parámetro `securestring`. Por ejemplo, puede proteger parámetros como el id. de cliente y el secreto de cliente usados para autenticar una acción HTTP con [Azure Active Directory](../connectors/connectors-native-http.md#authentication).
Al especificar un tipo de parámetro como `securestring`, el parámetro no se devuelve con la definición del recurso y no es accesible mediante la visualización del recurso después de la implementación. 

> [!NOTE]
> Cuando se usa un parámetro en el encabezado o el cuerpo de una solicitud, ese parámetro se puede ver cuando se accede al historial de ejecución de la aplicación lógica y a la solicitud HTTP saliente. Asegúrese de configurar las directivas de acceso al contenido según corresponda.
> Los encabezados de autorización nunca son visibles a través de entradas o salidas. Por lo tanto, si aquí se usa un secreto, no se podrá recuperar.

En este ejemplo se muestra una plantilla de implementación de Azure Resource Manager que usa más de un parámetro en tiempo de ejecución con el tipo `securestring`: 

* `armTemplatePasswordParam`, que es la entrada del parámetro `logicAppWfParam` de la definición de la aplicación lógica

* `logicAppWfParam`, que es la entrada de la acción HTTP mediante autenticación básica

En un archivo de parámetros independiente, puede especificar el valor de entorno del parámetro `armTemplatePasswordParam`, o bien puede recuperar los secretos en tiempo de implementación mediante el uso de [KeyVault de Azure Resource Manager](../azure-resource-manager/resource-manager-keyvault-parameter.md).
La sección `parameters` interna pertenece a la definición del flujo de trabajo de la aplicación lógica, mientras que la sección `parameters` externa pertenece a la plantilla de implementación.

```json
{
   "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
   "contentVersion": "1.0.0.0",
   "parameters": {
      "logicAppName": {       
         "type": "string",
         "minLength": 1,
         "maxLength": 80,
         "metadata": {         
            "description": "Name of the Logic App."       
         }     
      },
      "armTemplatePasswordParam": {
         "type": "securestring"     
      },     
      "logicAppLocation": {       
         "type": "string",
         "defaultValue": "[resourceGroup().location]",
         "allowedValues": [         
            "[resourceGroup().location]",
            "eastasia",
            "southeastasia",
            "centralus",
            "eastus",
            "eastus2",
            "westus",
            "northcentralus",
            "southcentralus",
            "northeurope",
            "westeurope",
            "japanwest",
            "japaneast",
            "brazilsouth",
            "australiaeast",
            "australiasoutheast",
            "southindia",
            "centralindia",
            "westindia",
            "canadacentral",
            "canadaeast",
            "uksouth",
            "ukwest",
            "westcentralus",
            "westus2"
         ],
         "metadata": {
            "description": "Location of the Logic App."
         }
      }
   },
   "variables": {},
   "resources": [
      {       
         "name": "[parameters('logicAppName')]",
         "type": "Microsoft.Logic/workflows",
         "location": "[parameters('logicAppLocation')]",
         "tags": {
            "displayName": "LogicApp"
         },
         "apiVersion": "2016-06-01",
         "properties": {
            "definition": {
               "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-0601/workflowdefinition.json#",
               "actions": {
                  "HTTP": {
                     "type": "Http",
                     "inputs": {
                        "method": "GET",
                        "uri": "http://www.microsoft.com",
                        "authentication": {
                           "type": "Basic",
                           "username": "username",
                              "password": "@parameters('logicAppWfParam')"
                        }
                     },
                  "runAfter": {}
                  }
               },
               "parameters": { 
                  "logicAppWfParam": {
                     "type": "securestring"
                  }
               },
               "triggers": {
                  "manual": {
                     "type": "Request",
                     "kind": "Http",
                     "inputs": {
                        "schema": {}
                     }
                  }
               },
               "contentVersion": "1.0.0.0",
               "outputs": {}
            },
            "parameters": {
               "logicAppWfParam": {
                  "value": "[parameters('armTemplatePasswordParam')]"
               }
            }
         }
      }
   ],
   "outputs": {} 
}   
```

<a name="secure-requests"></a>

## <a name="secure-access-to-services-receiving-requests"></a>Protección del acceso a los servicios que reciben solicitudes

Estas son algunas de las maneras en que puede proteger cualquier punto de acceso donde la aplicación lógica necesita acceso y envía solicitudes.

### <a name="add-authentication-on-outbound-requests"></a>Incorporación de la autenticación en las solicitudes salientes

Cuando se trabaja con una acción HTTP, HTTP + Swagger (Open API) o Webhook, puede agregar autenticación a la solicitud enviada por la aplicación lógica. Por ejemplo, puede usar la autenticación básica, la autenticación de certificado o la autenticación de Azure Active Directory. Para más información, consulte la [autenticación de desencadenadores o acciones](logic-apps-workflow-actions-triggers.md#connector-authentication) y [autenticación para acciones HTTP](../connectors/connectors-native-http.md#authentication).

### <a name="restrict-access-to-logic-app-ip-addresses"></a>Restricción del acceso a direcciones IP de aplicación lógica

Todas las llamadas desde aplicaciones lógicas provienen de direcciones IP designadas específicas según la región. Puede agregar un filtrado que solo acepta solicitudes provenientes de esas direcciones IP. Para esas direcciones IP, consulte [Límites y configuración de Azure Logic Apps](logic-apps-limits-and-config.md#configuration).

### <a name="secure-on-premises-connectivity"></a>Protección de la conectividad local

Azure Logic Apps ofrece integración con estos servicios para una comunicación local segura y confiable.

#### <a name="on-premises-data-gateway"></a>Puerta de enlace de datos local

Muchos de los conectores administrados de Azure Logic Apps proporcionan conexiones seguras a los sistemas locales, como el sistema de archivos, SQL, SharePoint, DB2, etc. La puerta de enlace envía datos desde orígenes locales en canales cifrados hasta Azure Service Bus. Todo el tráfico se origina como tráfico de salida seguro desde el agente de puerta de enlace. Obtenga información sobre [el funcionamiento de la puerta de enlace de datos local](logic-apps-gateway-install.md#gateway-cloud-service).

#### <a name="azure-api-management"></a>Azure API Management

[Azure API Management](https://azure.microsoft.com/services/api-management/) ofrece opciones de conexión local, como la integración de ExpressRoute y una red privada virtual sitio a sitio para un proxy seguro y comunicación con sistemas locales. En el Diseñador de aplicación lógica, puede seleccionar una API expuesta por API Management desde el flujo de trabajo de la aplicación lógica, proporcionando acceso rápido a los sistemas locales.

## <a name="next-steps"></a>Pasos siguientes

* [Creación de una plantilla de implementación](logic-apps-create-deploy-template.md)  
* [Control de excepciones](logic-apps-exception-handling.md)  
* [Supervisar las aplicaciones lógicas](logic-apps-monitor-your-logic-apps.md)  
* [Diagnóstico de errores y problemas de las aplicaciones lógicas](logic-apps-diagnosing-failures.md)  
