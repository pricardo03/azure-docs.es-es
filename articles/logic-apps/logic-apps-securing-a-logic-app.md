---
title: 'Proteger el acceso y los datos: Azure Logic Apps'
description: Proteja las entradas de parámetros, los desencadenadores de solicitudes HTTP, el historial de ejecución, las operaciones de aplicaciones lógicas y las conexiones con otros servicios en Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 06/28/2019
ms.openlocfilehash: be10d144fadb21a695c5573c82681a26136e71d4
ms.sourcegitcommit: e97a0b4ffcb529691942fc75e7de919bc02b06ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/15/2019
ms.locfileid: "71004099"
---
# <a name="secure-access-and-data-in-azure-logic-apps"></a>Proteger el acceso y los datos en Azure Logic Apps

Para controlar el acceso y proteger los datos en Azure Logic Apps, puede configurar la seguridad en estas áreas:

* [Acceso a los desencadenadores de solicitudes HTTP](#secure-triggers)
* [Acceso a las operaciones de las aplicaciones lógicas](#secure-operations)
* [Acceso a las entradas y salidas del historial de ejecución](#secure-run-history)
* [Acceso a las entradas de parámetros](#secure-action-parameters)
* [Acceso a los servicios y sistemas invocados desde aplicaciones lógicas](#secure-requests)

<a name="secure-triggers"></a>

## <a name="access-to-http-request-triggers"></a>Acceso a desencadenadores de solicitudes HTTP

Cuando la aplicación lógica usa un desencadenador basado en solicitudes HTTP, como el desencadenador [Solicitud](../connectors/connectors-native-reqres.md) o [Webhook](../connectors/connectors-native-webhook.md), puede limitar el acceso para que solo los clientes autorizados puedan iniciar la aplicación lógica. Todas las solicitudes que recibe una aplicación lógica están cifrados y protegidos con el protocolo Capa de sockets seguros (SSL). 

Estas son las distintas formas de proteger el acceso a este tipo de desencadenador:

* [Generación de firmas de acceso compartido](#sas)
* [Restricción de las direcciones IP entrantes](#restrict-incoming-ip-addresses)
* [Incorporación de Azure Active Directory, OAuth u otra medida de seguridad](#add-authentication)

<a name="sas"></a>

### <a name="generate-shared-access-signatures-sas"></a>Generación de firmas de acceso compartido (SAS)

Cada de punto de conexión de la solicitud de una aplicación lógica tiene una [Firma de acceso compartido (SAS)](https://docs.microsoft.com/rest/api/storageservices/constructing-a-service-sas) en la dirección URL del punto de conexión con el formato siguiente:

`https://<request-endpoint-URI>sp=<permissions>sv=<SAS-version>sig=<signature>`

Cada dirección URL contiene un parámetro de consulta `sp`, `sv` y `sig`, tal y como se describe en esta tabla:

| Parámetro de consulta | DESCRIPCIÓN |
|-----------------|-------------|
| `sp` | Especifica los permisos para los métodos HTTP permitidos que se usarán. |
| `sv` | Especifica la versión de SAS que se usará para generar la firma. |
| `sig` | Especifica la firma que se usará para autenticar el acceso al desencadenador. Esta firma se genera mediante el algoritmo SHA256 con una clave de acceso secreta en todas las rutas de acceso de direcciones URL y propiedades. Esta clave nunca se expone ni se publica, y se mantiene cifrada y almacenada con la aplicación lógica. La aplicación lógica solo autoriza los desencadenadores que contienen una firma válida creada con la clave secreta. |
|||

Para obtener más información sobre la protección del acceso con la Firma de acceso compartido, consulte estas secciones del tema:

* [Regenerar las claves de acceso](#access-keys)
* [Crear direcciones URL de devolución de llamada de expiración próxima](#expiring-urls)
* [Crear direcciones URL con clave principal o secundaria](#primary-secondary-key)

<a name="access-keys"></a>

#### <a name="regenerate-access-keys"></a>Regenerar las claves de acceso

Para generar una nueva clave de acceso protegida en cualquier momento, use la API REST de Azure o Azure Portal. Se invalidan todas las direcciones URL generadas previamente que usan la clave anterior y ya no tienen autorización para desencadenar la aplicación lógica. Las direcciones URL que se recuperan tras la regeneración se firman con la nueva clave de acceso.

1. En Azure Portal, abra la aplicación lógica donde está la clave que quiere regenerar.

1. En el menú de la aplicación lógica, en **Configuración**, seleccione **Claves de acceso**.

1. Seleccione la clave que quiere regenerar y finalice el proceso.

<a name="expiring-urls"></a>

#### <a name="create-expiring-callback-urls"></a>Crear direcciones URL de devolución de llamada de expiración próxima

Si comparte la dirección URL del punto de conexión de un desencadenador basado en una solicitud HTTP con otras partes, puede generar direcciones URL de devolución de llamada con claves específicas y fechas de expiración. De esta forma puede acumular claves fácilmente o restringir el acceso para desencadenar la aplicación lógica en función de un determinado intervalo de tiempo. Para especificar una fecha de expiración para una dirección URL, use la [API REST de Logic Apps](https://docs.microsoft.com/rest/api/logic/workflowtriggers); por ejemplo:

``` http
POST /subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.Logic/workflows/<workflow-name>/triggers/<trigger-name>/listCallbackUrl?api-version=2016-06-01
```

En el cuerpo, incluya la propiedad `NotAfter` mediante una cadena de fecha de JSON. Esta propiedad devuelve una dirección URL de devolución de llamada que solo es válida hasta la fecha y hora `NotAfter`.

<a name="primary-secondary-key"></a>

#### <a name="create-urls-with-primary-or-secondary-secret-key"></a>Creación de direcciones URL con clave secreta principal o secundaria

Al generar o enumerar direcciones URL de devolución de llamada para desencadenadores basados en solicitudes HTTP, puede especificar qué clave se va a usar para firmar la dirección URL. Para generar una dirección URL firmada por una clave específica, use la [API REST de Logic Apps](https://docs.microsoft.com/rest/api/logic/workflowtriggers); por ejemplo:

``` http
POST /subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.Logic/workflows/<workflow-name>/triggers/<trigger-name>/listCallbackUrl?api-version=2016-06-01
```

En el cuerpo, incluya la propiedad `KeyType` como `Primary` o `Secondary`. Esta propiedad devuelve una dirección URL firmada por una clave segura especificada.

<a name="restrict-incoming-ip"></a>

### <a name="restrict-incoming-ip-addresses"></a>Restricción de las direcciones IP entrantes

Junto con la Firma de acceso compartido, es posible que quiera limitar los clientes específicos que pueden llamar a su aplicación lógica. Por ejemplo, si administra el punto de conexión de solicitud mediante Azure API Management, puede restringir la aplicación lógica para que solo acepte solicitudes procedentes de la dirección IP de la instancia de API Management.

#### <a name="restrict-incoming-ip-ranges-in-azure-portal"></a>Restricción de los intervalos IP entrantes en Azure Portal

1. En Azure Portal, abra la aplicación lógica en Diseñador de aplicación lógica.

1. En el menú de la aplicación lógica, en **Configuración**, seleccione **Configuración de flujo de trabajo**.

1. En **Configuración de control de acceso** > **Direcciones IP entrantes permitidas**, seleccione **Intervalos IP específicos**.

1. En **Intervalos IP para desencadenadores**, especifique los intervalos de direcciones IP que acepta el desencadenador.

   Un intervalo IP válido usa estos formatos: *x.x.x.x/x* o *x.x.x.x-x.x.x.x*

Si quiere que la aplicación lógica solo se desencadene como aplicación lógica anidada, en la lista **Direcciones IP entrantes permitidas**, seleccione **Cualquier otra aplicación lógica**. Esta opción escribe una matriz vacía en el recurso de la aplicación lógica. De este modo, solo las llamadas del servicio de Logic Apps (las aplicaciones lógicas principales) pueden desencadenar la aplicación lógica anidada.

> [!NOTE]
> Independientemente de la dirección IP, de todos modos puede ejecutar una aplicación lógica que tiene un desencadenador basado en solicitudes HTTP mediante el uso de `/triggers/<trigger-name>/run` a través de la API REST de Azure o de API Management. Sin embargo, este escenario sigue requiriendo la autenticación con la API REST de Azure. Todos los eventos aparecen en el registro de auditoría de Azure. Asegúrese de establecer las directivas de control de acceso como corresponda.

#### <a name="restrict-incoming-ip-ranges-in-azure-resource-manager-template"></a>Restricción de los intervalos IP entrantes en la plantilla de Azure Resource Manager

Si automatiza las implementaciones de aplicaciones lógicas mediante una [plantilla de Azure Resource Manager](../logic-apps/logic-apps-create-deploy-template.md), puede especificar los intervalos IP mediante la sección `accessControl` con la sección `triggers` en la definición de recursos de la aplicación lógica; por ejemplo:

``` json
{
   "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
   "contentVersion": "1.0.0.0",
   "parameters": {},
   "variables": {},
   "resources": [
      {
         "name": "[parameters('LogicAppName')]",
         "type": "Microsoft.Logic/workflows",
         "location": "[parameters('LogicAppLocation')]",
         "tags": {
            "displayName": "LogicApp"
         },
         "apiVersion": "2016-06-01",
         "properties": {
            "definition": {<workflow-definition>},
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
         }
      }
   ],
   "outputs": {}
}
```

<a name="add-authentication"></a>

### <a name="add-azure-active-directory-oauth-or-other-security"></a>Incorporación de Azure Active Directory, OAuth u otra medida de seguridad

Para agregar más protocolos de autorización a la aplicación lógica, considere la posibilidad de usar [Azure API Management](../api-management/api-management-key-concepts.md). Este servicio le ofrece la funcionalidad necesaria para exponer su aplicación lógica como API y le ofrece una supervisión enriquecida, seguridad, directiva y documentación para cualquier punto de conexión. API Management puede exponer un punto de conexión público o privado para la aplicación lógica, que luego puede usar Azure Active Directory, OAuth, certificado u otros estándares de seguridad. Cuando API Management recibe una solicitud, el servicio la envía a la aplicación lógica y también hace cualquier transformación o restricción necesaria en el proceso. Para permitir que solo API Management desencadene la aplicación lógica, puede usar la configuración del intervalo IP entrante de la aplicación lógica.

<a name="secure-operations"></a>

## <a name="access-to-logic-app-operations"></a>Acceso a las operaciones de las aplicaciones lógicas

Puede permitir que solo determinados usuarios o grupos ejecuten operaciones específicas, como administrar, editar y ver aplicaciones lógicas. Para controlar sus permisos, use el [Control de acceso basado en rol (RBAC) de Azure](../role-based-access-control/role-assignments-portal.md) para asignar roles personalizados o integrados a los miembros de la suscripción a Azure:

* [Colaborador de aplicación lógica](../role-based-access-control/built-in-roles.md#logic-app-contributor): Le permite administrar aplicaciones lógicas, pero no puede cambiar el acceso a ellas.

* [Operador de aplicación lógica](../role-based-access-control/built-in-roles.md#logic-app-operator): Le permite leer, habilitar y deshabilitar aplicaciones lógicas, pero no puede editarlas ni actualizarlas.

Para evitar que otros usuarios cambien o eliminen la aplicación lógica, puede usar el [bloqueo de recursos de Azure](../azure-resource-manager/resource-group-lock-resources.md), que impide que otros usuarios cambien o eliminen los recursos de producción.

<a name="secure-run-history"></a>

## <a name="access-to-run-history-data"></a>Acceso a los datos del historial de ejecución

Durante la ejecución de una aplicación lógica, todos los datos se cifran en tránsito mediante [Seguridad de la capa de transporte (TLS)](https://azure.microsoft.com/updates/app-service-and-functions-hosted-apps-can-now-update-tls-versions/) y en [reposo](../security/fundamentals/encryption-atrest.md). Cuando finaliza la ejecución de la aplicación lógica, puede ver el historial de esa ejecución, incluidos los pasos que se ejecutaron junto con el estado, la duración, las entradas y las salidas de cada acción. Este completo detalle proporciona información sobre cómo se ejecuta la aplicación lógica y dónde puede empezar a solucionar los problemas que surjan.

Cuando accede al historial de ejecución de la aplicación lógica, Logic Apps autentica su acceso y proporciona vínculos a las entradas y salidas de las solicitudes y respuestas de la ejecución de la aplicación lógica. Sin embargo, para las acciones que controlan contraseñas, secretos, claves u otra información confidencial, es recomendable evitar que otros usuarios vean los datos y accedan a ellos. Por ejemplo, si la aplicación lógica obtiene un secreto de [Azure Key Vault](../key-vault/key-vault-overview.md) para usarlo al autenticar una acción HTTP, puede ocultar ese secreto de la vista.

Para controlar el acceso a las entradas y salidas del historial de ejecución de la aplicación lógica, tiene estas opciones:

* [Restricción del acceso por intervalo de direcciones IP](#restrict-ip).

  Esta opción le permite proteger el acceso al historial de ejecución en función de las solicitudes de un intervalo de direcciones IP específico.

* [Oculte los datos del historial de ejecución mediante ofuscación](#obfuscate).

  En muchos desencadenadores y acciones, puede ocultar las entradas, salidas o ambas del historial de ejecución de una aplicación lógica.

<a name="restrict-ip"></a>

### <a name="restrict-access-by-ip-address-range"></a>Restringir el acceso por intervalo de direcciones IP

Puede limitar el acceso a las entradas y salidas del historial de ejecución de la aplicación lógica para que solo las solicitudes de intervalos de direcciones IP específicas puedan ver los datos. Por ejemplo, para impedir que alguien tenga acceso a las entradas y salidas, especifique un intervalo de direcciones IP como, por ejemplo, `0.0.0.0-0.0.0.0`. Solo una persona con permisos de administrador puede quitar esta restricción, lo que proporciona la posibilidad de usar el acceso "Just-In-Time" para los datos de la aplicación lógica. Puede especificar los intervalos IP que se van a restringir desde Azure Portal o en una plantilla de Azure Resource Manager que se usa para la implementación de la aplicación lógica.

#### <a name="restrict-ip-ranges-in-azure-portal"></a>Restricción de los intervalos IP en Azure Portal

1. En Azure Portal, abra la aplicación lógica en Diseñador de aplicación lógica.

1. En el menú de la aplicación lógica, en **Configuración**, seleccione **Configuración de flujo de trabajo**.

1. En **Configuración de control de acceso** > **Direcciones IP entrantes permitidas**, seleccione **Intervalos IP específicos**.

1. En **Intervalos IP para contenido**, especifique los intervalos de direcciones IP que pueden acceder a contenido desde las entradas y salidas. 

   Un intervalo IP válido usa estos formatos: *x.x.x.x/x* o *x.x.x.x-x.x.x.x*

#### <a name="restrict-ip-ranges-in-azure-resource-manager-template"></a>Restricción de los intervalos IP entrantes en la plantilla de Azure Resource Manager

Si automatiza las implementaciones de aplicaciones lógicas mediante una [plantilla de Azure Resource Manager](../logic-apps/logic-apps-create-deploy-template.md), puede especificar los intervalos IP mediante la sección `accessControl` con la sección `contents` en la definición de recursos de la aplicación lógica; por ejemplo:

``` json
{
   "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
   "contentVersion": "1.0.0.0",
   "parameters": {},
   "variables": {},
   "resources": [
      {
         "name": "[parameters('LogicAppName')]",
         "type": "Microsoft.Logic/workflows",
         "location": "[parameters('LogicAppLocation')]",
         "tags": {
            "displayName": "LogicApp"
         },
         "apiVersion": "2016-06-01",
         "properties": {
            "definition": {<workflow-definition>},
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
         }
      }
   ],
   "outputs": {}
}
```

<a name="obfuscate"></a>

### <a name="hide-data-from-run-history-by-using-obfuscation"></a>Oculte los datos del historial de ejecución mediante ofuscación.

En muchos desencadenadores y acciones, esta configuración oculta las entradas, salidas o ambas del historial de ejecución de una aplicación lógica. Estas son algunas [consideraciones que se deben tener en cuenta](#obfuscation-considerations) al usar esta configuración para proteger los datos.

#### <a name="secure-inputs-and-outputs-in-the-designer"></a>Protección de entradas y salidas en el diseñador

1. Si la aplicación lógica aún no está abierta en [Azure Portal](https://portal.azure.com), abra la aplicación lógica en el diseñador de aplicaciones lógicas.

   ![Abrir una aplicación lógica de ejemplo](media/logic-apps-securing-a-logic-app/sample-logic-app.png)

1. En el desencadenador o la acción donde quiera proteger los datos, seleccione los puntos suspensivos ( **...** ) y, a continuación, seleccione **Configuración**.

   ![Abrir "Configuración"](media/logic-apps-securing-a-logic-app/open-settings.png)

1. Active **Entradas seguras**, **Salidas seguras** o ambas opciones. Cuando haya finalizado, seleccione **Listo**.

   ![Activar las entradas o salidas seguras](media/logic-apps-securing-a-logic-app/turn-on-secure-inputs-outputs.png)

   Ahora, la acción o el desencadenador muestra un icono de candado en la barra de título.

   ![Icono de candado en la barra de título](media/logic-apps-securing-a-logic-app/title-bar-lock-icon.png)

   Los tokens que representan las salidas protegidas de acciones anteriores también muestran los iconos de candado. Por ejemplo, al seleccionar un resultado de este tipo en la lista de contenido dinámico para usarlo en una acción, el token muestra un icono de candado.

   ![Seleccionar la salida](media/logic-apps-securing-a-logic-app/select-secured-token.png)

1. Una vez ejecutada la aplicación lógica, puede ver el historial de esa ejecución.

   1. En el panel **Información general** de la aplicación lógica, seleccione la ejecución que quiera ver.

   1. En el panel **Ejecución de aplicación lógica**, expanda las acciones que quiera revisar.

      Si decide proteger las entradas y las salidas, esos valores aparecerán ocultos.

      ![Datos ocultos en el historial de ejecución](media/logic-apps-securing-a-logic-app/hidden-data-run-history.png)

<a name="secure-data-code-view"></a>

#### <a name="secure-inputs-and-outputs-in-code-view"></a>Protección de entradas y salidas en la vista Código

En la definición de desencadenador o acción subyacente, agregue o actualice la matriz `runtimeConfiguration.secureData.properties` con uno de estos valores o con ambos:

* `"inputs"`: Protege las entradas en el historial de ejecución.
* `"outputs"`: Protege las salidas en el historial de ejecución.

Estas son algunas [consideraciones que se deben tener en cuenta](#obfuscation-considerations) al usar esta configuración para proteger los datos.

```json
"<trigger-or-action-name>": {
   "type": "<trigger-or-action-type>",
   "inputs": {
      <trigger-or-action-inputs>
   },
   "runtimeConfiguration": {
      "secureData": {
         "properties": [
            "inputs",
            "outputs"
         ]
      }
   },
   <other-attributes>
}
```

<a name="obfuscation-considerations"></a>

#### <a name="considerations-when-hiding-inputs-and-outputs"></a>Consideraciones a la hora de ocultar las entradas y salidas

* Cuando se protegen las entradas o salidas de un desencadenador o una acción, Logic Apps no envía los datos protegidos a Azure Log Analytics. Además, no se pueden agregar [propiedades con seguimiento](logic-apps-monitor-your-logic-apps.md#azure-diagnostics-event-settings-and-details) al desencadenador o acción para su supervisión.

* La [API de Logic Apps para controlar el historial del flujo de trabajo](https://docs.microsoft.com/rest/api/logic/) no devuelve salidas protegidas.

* En una acción en la que se protegen las entradas o en que se usan explícitamente salidas seguras, y esta acción devuelve una respuesta con salidas que incluyen estos datos protegidos, tiene que activar manualmente las **Salidas seguras** en esta acción para proteger las salidas.

* Asegúrese de activar **Entradas seguras** o **Salidas seguras** en las acciones de nivel inferior en las que espera que el historial de ejecución proteja los datos.

  **Configuración de las salidas seguras**

  Al activar manualmente las **Salidas seguras** en un desencadenador o una acción, Logic Apps protege estas salidas en el historial de ejecución. Si una acción de nivel inferior usa explícitamente estas salidas protegidas como entradas, Logic Apps oculta las entradas de esta acción en el historial de ejecución, pero *no habilita* la opción de **Entradas seguras** de la acción.

  ![Salidas seguras como entradas y repercusión descendente en la mayoría de las acciones](media/logic-apps-securing-a-logic-app/secure-outputs-as-inputs-flow.png)

  Las acciones de redacción, análisis JSON y respuesta solo tienen la opción **Entradas seguras**. Cuando está activada, esta opción también oculta las salidas de estas acciones. Si estas acciones usan explícitamente las salidas protegidas de nivel superior como entradas, Logic Apps ocultará las entradas y salidas de estas acciones, pero *no habilitará* la opción **Entradas seguras** de estas acciones. Si una acción de nivel inferior usa explícitamente las salidas ocultas de las acciones de redacción, análisis JSON o respuesta como entradas, Logic Apps *no oculta las entradas o salidas de la acción de nivel inferior*.

  ![Salidas seguras como entradas y repercusión descendente en acciones especificas](media/logic-apps-securing-a-logic-app/secure-outputs-as-inputs-flow-special.png)

  **Opción Entradas seguras**

  Al activar manualmente las **Entradas seguras** en un desencadenador o una acción, Logic Apps protege estas entradas en el historial de ejecución. Si una acción de nivel inferior usa explícitamente las salidas visibles de ese desencadenador o acción como entradas, Logic Apps oculta las entradas de esta acción de nivel inferior en el historial de ejecución, pero *no habilita* las **Entradas seguras** en esta acción y no oculta las salidas de la acción.

  ![Entradas seguras y repercusión descendente en la mayoría de las acciones](media/logic-apps-securing-a-logic-app/secure-inputs-flow.png)

  Si las acciones de redacción, análisis JSON y respuesta usan explícitamente las salidas visibles del desencadenador o la acción que tiene las entradas protegidas, Logic Apps oculta las entradas y salidas de estas acciones, pero *no habilita* la opción **Entradas seguras** de la acción. Si una acción de nivel inferior usa explícitamente las salidas ocultas de las acciones de redacción, análisis JSON o respuesta como entradas, Logic Apps *no oculta las entradas o salidas de la acción de nivel inferior*.

  ![Entradas seguras y repercusión descendente en determinadas acciones](media/logic-apps-securing-a-logic-app/secure-inputs-flow-special.png)

<a name="secure-action-parameters"></a>

## <a name="access-to-parameter-inputs"></a>Acceso a entradas de parámetros

Si implementa en entornos diferentes, considere la posibilidad de parametrizar los valores de la definición de flujo de trabajo que varían en función de esos entornos. De este modo, puede usar una [plantilla de Azure Resource Manager ](../azure-resource-manager/resource-group-authoring-templates.md#parameters) para implementar la aplicación lógica, proteger la información confidencial definiendo parámetros seguros y proporcionar las entradas de parámetros por separado mediante los parámetros de la plantilla con un [archivo de parámetros](../azure-resource-manager/resource-group-template-deploy.md#pass-parameter-values).

Por ejemplo, si autentica acciones HTTP con [Azure Active Directory](../logic-apps/logic-apps-workflow-actions-triggers.md#connector-authentication), puede definir y proteger los parámetros que aceptan el id. de cliente y el secreto de cliente que se usan para la autenticación. Para definir estos parámetros para la aplicación lógica, use la sección `parameters` dentro de la definición de flujo de trabajo de la aplicación lógica. Para proteger los valores de parámetros que no quiera mostrar al editar la aplicación lógica o visualizar el historial de ejecución, defina los parámetros con el tipo `securestring` o `secureobject` y use la codificación necesaria. Los parámetros de este tipo no se devuelven con la definición del recurso y no son accesibles al visualizar el recurso después de la implementación. Para tener acceso a estos valores de parámetro durante el tiempo de ejecución, use la expresión `@parameters('<parameter-name>')` dentro de la definición del flujo de trabajo. Esta expresión solo se evalúa en tiempo de ejecución y se describe con el [lenguaje de definición de flujo de trabajo](../logic-apps/logic-apps-workflow-definition-language.md).

> [!NOTE]
> Si usa un parámetro en el encabezado o el cuerpo de una solicitud HTTP, ese parámetro se puede ver al visualizar el historial de ejecución de la aplicación lógica y la solicitud HTTP saliente. Asegúrese de definir también las directivas de acceso al contenido según corresponda. Los encabezados de autorización nunca son visibles a través de entradas o salidas. Por lo tanto, si aquí se usa un secreto, no se podrá recuperar.

Para obtener más información, consulte [Parámetros protegidos en las definiciones de flujo de trabajo](#secure-parameters-workflow) más adelante en este tema.

Al automatizar las implementaciones con [plantillas de Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md#parameters), puede definir parámetros de plantilla protegidos, que se evalúan en la implementación, mediante los tipos `securestring` y `secureobject`. Para definir parámetros de plantilla, use la sección `parameters` de nivel superior de la plantilla, que es independiente y diferente de la sección `parameters` de la definición de flujo de trabajo. Para proporcionar valores para los parámetros de plantilla, use un [archivo de parámetros](../azure-resource-manager/resource-group-template-deploy.md#pass-parameter-values) independiente.

Por ejemplo, si usa secretos, puede definir y usar parámetros de plantilla protegidos que recuperen dichos secretos de [Azure Key Vault](../key-vault/key-vault-overview.md) en la implementación. A continuación, puede hacer referencia a Key Vault y al secreto en el archivo de parámetros. Para más información, consulte los temas siguientes:

* [Uso de Azure Key Vault para pasar valores de parámetro seguros durante la implementación](../azure-resource-manager/resource-manager-keyvault-parameter.md)
* [Parámetros seguros en plantillas de Azure Resource Manager](#secure-parameters-deployment-template) más adelante en este tema

<a name="secure-parameters-workflow"></a>

### <a name="secure-parameters-in-workflow-definitions"></a>Parámetros protegidos en las definiciones de flujo de trabajo

Para proteger la información confidencial en la definición del flujo de trabajo de aplicación lógica, use parámetros seguros, de forma que esta información no esté visible después de guardar la aplicación lógica. Por ejemplo, supongamos que tiene una acción HTTP que requiere autenticación básica que usa un nombre de usuario y una contraseña. En la definición del flujo, la sección `parameters` define los parámetros `basicAuthPasswordParam` y `basicAuthUsernameParam` mediante el tipo `securestring`. A continuación, la definición de la acción hace referencia a estos parámetros en la sección `authentication`.

```json
"definition": {
   "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
   "actions": {
      "HTTP": {
         "type": "Http",
         "inputs": {
            "method": "GET",
            "uri": "https://www.microsoft.com",
            "authentication": {
               "type": "Basic",
               "username": "@parameters('basicAuthUsernameParam')",
               "password": "@parameters('basicAuthPasswordParam')"
            }
         },
         "runAfter": {}
      }
   },
   "parameters": {
      "basicAuthPasswordParam": {
         "type": "securestring"
      },
      "basicAuthUsernameParam": {
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
}
```

<a name="secure-parameters-deployment-template"></a>

### <a name="secure-parameters-in-azure-resource-manager-templates"></a>Parámetros seguros en plantillas de Azure Resource Manager

Una plantilla de Resource Manager para una aplicación lógica tiene varias secciones `parameters`. Para proteger las contraseñas, las claves, los secretos y otros datos confidenciales, defina parámetros seguros en el nivel de plantilla y de definición de flujo de trabajo mediante el tipo `securestring` o `secureobject`. Después, puede almacenar estos valores en [Azure Key Vault](../key-vault/key-vault-overview.md) y usar el [archivo de parámetros](../azure-resource-manager/resource-group-template-deploy.md#pass-parameter-values) para hacer referencia al almacén de claves y al secreto. A continuación, la plantilla recupera esa información en la implementación. Para más información, consulte [Uso de Azure Key Vault para pasar el valor de parámetro seguro durante la implementación](../azure-resource-manager/resource-manager-keyvault-parameter.md).

Aquí tiene más información sobre estas secciones `parameters`:

* En el nivel superior de la plantilla, una sección `parameters` define los parámetros de los valores que utiliza la plantilla durante la *implementación*. Por ejemplo, estos valores pueden incluir cadenas de conexión para un entorno de implementación concreto. Después, puede almacenar estos valores en un [archivo de parámetros](../azure-resource-manager/resource-group-template-deploy.md#pass-parameter-values), lo que facilita la modificación de estos valores.

* Dentro de la definición de recursos de la aplicación lógica, pero fuera de la definición del flujo de trabajo, una sección `parameters` especifica los valores de los parámetros de la definición del flujo de trabajo. En esta sección, puede asignar estos valores mediante expresiones de plantilla que hagan referencia a los parámetros de la plantilla. Estas expresiones se evalúan en la implementación.

* Dentro de la definición del flujo de trabajo, una sección `parameters` define los parámetros que usa la aplicación lógica en tiempo de ejecución. Después, puede hacer referencia a estos parámetros en el flujo de trabajo de la aplicación lógica mediante expresiones de definición de flujo de trabajo, que se evalúan en tiempo de ejecución.

Esta plantilla de ejemplo tiene varias definiciones de parámetros seguros que usan el tipo `securestring`:

| Nombre de parámetro | DESCRIPCIÓN |
|----------------|-------------|
| `TemplatePasswordParam` | Parámetro de plantilla que acepta una contraseña que se pasa, a continuación, al parámetro `basicAuthPasswordParam` de la definición del flujo de trabajo. |
| `TemplateUsernameParam` | Parámetro de plantilla que acepta un nombre de usuario que se pasa a continuación al parámetro `basicAuthUserNameParam` de la definición del flujo de trabajo |
| `basicAuthPasswordParam` | Parámetro de definición de flujo de trabajo que acepta la contraseña para la autenticación básica en una acción HTTP |
| `basicAuthUserNameParam` | Parámetro de definición de flujo de trabajo que acepta el nombre de usuario para la autenticación básica en una acción HTTP |
|||

```json
{
   "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
   "contentVersion": "1.0.0.0",
   "parameters": {
      "LogicAppName": {
         "type": "string",
         "minLength": 1,
         "maxLength": 80,
         "metadata": {
            "description": "Name of the Logic App."
         }
      },
      "TemplatePasswordParam": {
         "type": "securestring"
      },
      "TemplateUsernameParam": {
         "type": "securestring"
      },
      "LogicAppLocation": {
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
         "name": "[parameters('LogicAppName')]",
         "type": "Microsoft.Logic/workflows",
         "location": "[parameters('LogicAppLocation')]",
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
                        "uri": "https://www.microsoft.com",
                        "authentication": {
                           "type": "Basic",
                           "username": "@parameters('basicAuthUsernameParam')",
                           "password": "@parameters('basicAuthPasswordParam')"
                        }
                     },
                  "runAfter": {}
                  }
               },
               "parameters": {
                  "basicAuthPasswordParam": {
                     "type": "securestring"
                  },
                  "basicAuthUsernameParam": {
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
               "basicAuthPasswordParam": {
                  "value": "[parameters('TemplatePasswordParam')]"
               },
               "basicAuthUsernameParam": {
                  "value": "[parameters('TemplateUsernameParam')]"
               }
            }
         }
      }
   ],
   "outputs": {}
}
```

<a name="secure-requests"></a>

## <a name="access-to-services-and-systems-called-from-logic-apps"></a>Acceso a servicios y sistemas invocados desde aplicaciones lógicas

Estas son algunas de las maneras de proteger los puntos de conexión en que la aplicación lógica necesita acceso para enviar solicitudes.

* Incorporación de la autenticación en las solicitudes salientes.

  Cuando trabaja con una acción HTTP, HTTP + Swagger (Open API) o Webhook, puede agregar autenticación a la solicitud enviada por la aplicación lógica. Por ejemplo, puede usar la autenticación básica, la autenticación de certificado o la autenticación de Azure Active Directory. Para obtener más información, consulte la [autenticación de desencadenadores o acciones](../logic-apps/logic-apps-workflow-actions-triggers.md#connector-authentication).

* Restricción del acceso desde direcciones IP de aplicación lógica.

  Todas las llamadas a los puntos de conexión desde aplicaciones lógicas proceden de direcciones IP designadas específicas que se basan en las regiones de las aplicaciones lógicas. Puede agregar un filtrado que solo acepta solicitudes provenientes de esas direcciones IP. Para obtener esas direcciones IP, consulte [Límites y configuración de Azure Logic Apps](logic-apps-limits-and-config.md#configuration).

* Autenticación y acceso a los recursos como identidades administradas.

  Para obtener acceso a los recursos en otros inquilinos de Azure Active Directory (Azure AD), la aplicación lógica puede autenticar su identidad sin iniciar sesión mediante una identidad administrada (anteriormente conocida como Managed Service Identity o MSI), en lugar de credenciales o secretos. Azure administra esta identidad y le ayuda a proteger las credenciales porque, de esta forma, no tiene que proporcionar secretos o cambiarlos. Para obtener más información sobre cómo configurar y usar una identidad administrada asignada por el sistema para la aplicación lógica, consulte [Autenticación y acceso a los recursos con identidades administradas en Azure Logic Apps](../logic-apps/create-managed-service-identity.md).

* Proteja las conexiones a sistemas locales.

  Azure Logic Apps ofrece integración con estos servicios para una comunicación local segura y confiable.

  * Puerta de enlace de datos local

    Muchos conectores administrados de Azure Logic Apps proporcionan conexiones seguras a sistemas locales, como el sistema de archivos, SQL, SharePoint y DB2. La puerta de enlace envía datos desde orígenes locales en canales cifrados hasta Azure Service Bus. Todo el tráfico se origina como tráfico de salida seguro desde el agente de puerta de enlace. Obtenga información sobre [el funcionamiento de la puerta de enlace de datos local](logic-apps-gateway-install.md#gateway-cloud-service).

  * Conexión mediante Azure API Management

    [Azure API Management](../api-management/api-management-key-concepts.md) ofrece opciones de conexión local, como la integración de ExpressRoute y una red privada virtual sitio a sitio para un proxy seguro y comunicación con sistemas locales. Desde el flujo de trabajo de la aplicación lógica del Diseñador de aplicación lógica, puede seleccionar una API expuesta por API Management, lo que proporciona un acceso rápido a los sistemas locales.

## <a name="next-steps"></a>Pasos siguientes

* [Crear plantillas de implementación](logic-apps-create-deploy-template.md)  
* [Supervisar las aplicaciones lógicas](logic-apps-monitor-your-logic-apps.md)  
* [Diagnóstico de errores y problemas de las aplicaciones lógicas](logic-apps-diagnosing-failures.md)  
* [Automatización de la implementación de aplicaciones lógicas](logic-apps-azure-resource-manager-templates-overview.md)
