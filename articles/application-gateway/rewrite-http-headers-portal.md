---
title: Vuelva a escribir los encabezados de solicitud y respuesta HTTP con Azure Application Gateway mediante Azure portal | Microsoft Docs
description: Obtenga información sobre cómo usar el portal de Azure para configurar una puerta de enlace de aplicaciones de Azure para volver a escribir los encabezados HTTP en las solicitudes y respuestas que se pasa a través de la puerta de enlace
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 04/10/2019
ms.author: absha
ms.custom: mvc
ms.openlocfilehash: 9d52114f5d01beca53ed48ee2114dc98cc20f3a2
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59682431"
---
# <a name="rewrite-http-request-and-response-headers-with-azure-application-gateway---azure-portal"></a>Vuelva a escribir los encabezados de solicitud y respuesta HTTP con Azure Application Gateway mediante Azure portal

Este artículo muestra cómo usar el portal de Azure para configurar un [SKU de puerta de enlace de aplicaciones v2](<https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant>) volver a escribir los encabezados HTTP en las solicitudes y respuestas.

> [!IMPORTANT]
> La SKU de escalabilidad automática y de puerta de enlace de aplicaciones con redundancia de zona actualmente está en versión preliminar pública. Esta versión preliminar se ofrece sin contrato de nivel de servicio y no es aconsejable usarla para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="before-you-begin"></a>Antes de empezar

Debe tener un v2 Application Gateway SKU, puesto que el encabezado de la capacidad de reescritura no se admite para la SKU v1. Si no tiene la SKU de v2, cree un [SKU de puerta de enlace de aplicaciones v2](<https://docs.microsoft.com/azure/application-gateway/tutorial-autoscale-ps>) antes de comenzar.

## <a name="what-is-required-to-rewrite-a-header"></a>¿Qué es necesario volver a escribir un encabezado

Para configurar la reescritura del encabezado HTTP, deberá:

1. Cree los objetos necesarios para reescribir los encabezados HTTP:

   - **Vuelva a escribir la acción**: se utiliza para especificar la solicitud y los campos de encabezado de solicitud que va a escribir y el nuevo valor que deben volver a escribir en los encabezados del original. Puede asociar uno o más de reescritura de condición con una acción de reescritura.

   - **Vuelva a escribir la condición**: Es una configuración opcional. Si se agrega una condición de reescritura, se evaluará el contenido de las respuestas y solicitudes HTTP (S). Si la solicitud HTTP (S) o la respuesta coincide con la condición de reescritura, se basará la decisión de ejecutar la acción de reescritura asociada con la condición de reescritura. 

     Si más de una de las condiciones asociadas con una acción y, a continuación, la acción se ejecutará sólo cuando se cumplen todas las condiciones, es decir, se realizará una operación AND lógica.

   - **La regla de reescritura**: regla de reescritura contiene varias acciones de reescritura - vuelva a escribir las combinaciones de condición.

   - **Secuencia de regla**: ayuda a determinar el orden en que las distintas reglas de reescritura se ejecutan. Esto resulta útil cuando hay varias reglas de reescritura en un conjunto de reescritura. La regla de reescritura con el menor valor de secuencia de la regla se ejecuta en primer lugar. Si proporciona la misma secuencia de regla para dos reglas de reescritura, a continuación, el orden de ejecución será no determinista.

   - **Vuelva a escribir conjunto**: contiene varias reglas de reescritura que se van a asociadas a una regla de enrutamiento de solicitud.

2. Se le pedirá para adjuntar la reescritura establecido con una regla de enrutamiento. Esto es porque la configuración de reescritura se ha asociado al agente de escucha de origen a través de la regla de enrutamiento. Al usar una regla de enrutamiento básica, la configuración de reescritura de encabezados se asocia a un agente de escucha de origen y es una reescritura de encabezados global. Al usar una regla de enrutamiento basada en rutas, la configuración de reescritura de encabezado se define en la asignación de la ruta de URL. Por lo tanto, solo se aplica al área específica de la ruta de acceso de un sitio.

Puede crear varios conjuntos de reescritura de encabezado de http y cada conjunto de reescritura se puede aplicar a varios agentes de escucha. Sin embargo, puede aplicar sólo una reescritura establecido en un agente de escucha concreto.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en [Azure Portal](https://portal.azure.com/) con su cuenta de Azure.

## <a name="configure-header-rewrite"></a>Configurar la reescritura de encabezado

En este ejemplo, se modificará la dirección URL de redirección al volver a escribir el encabezado de ubicación en la respuesta http enviada por la aplicación de back-end. 

1. Seleccione **todos los recursos**y, a continuación, seleccione la puerta de enlace de la aplicación.

2. Seleccione **reescribe** en el menú izquierdo.

3. Haga clic en **+ reescribir conjunto**. 

   ![Agregar conjunto de reescritura](media/rewrite-http-headers-portal/add-rewrite-set.png)

4. Proporcione el nombre para el conjunto de reescritura y asociarla a una regla de enrutamiento:

   - Escriba el nombre de la reescritura establecido el **nombre** cuadro de texto.
   - Seleccione una o varias reglas que se muestran en el **asociados a las reglas de enrutamiento** lista. Solo puede seleccionar las reglas que no se han asociado con otros conjuntos de reescritura. Las reglas que ya se han asociado con otros conjuntos de reescritura se atenuarán.
   - Haga clic en Siguiente.
   
     ![Agregar el nombre y la asociación](media/rewrite-http-headers-portal/name-and-association.png)

5. Cree una regla de reescritura:

   - Haga clic en **+ Agregar regla de reescritura**.![ Agregar regla de reescritura](media/rewrite-http-headers-portal/add-rewrite-rule.png)
   - Proporcione un nombre para la regla de reescritura en el cuadro de texto de nombre de regla de reescritura y proporcionar una secuencia de la regla.![Agregar nombre de la regla](media/rewrite-http-headers-portal/rule-name.png)

6. En este ejemplo, se reescribirá el encabezado de ubicación solo si contiene una referencia a "azurewebsites.net". Para ello, agregue una condición para evaluar si el encabezado de ubicación en la respuesta contiene azurewebsites.net:

   - Haga clic en **+ Agregar condición** y, a continuación, haga clic en la sección con la **si** instrucciones para expandir lo![ Agregar nombre de la regla](media/rewrite-http-headers-portal/add-condition.png)

   - Seleccione **encabezado HTTP** desde el **tipo de variable para comprobar** lista desplegable. 

   - Seleccione **tipo de encabezado** como **respuesta**.

   - Puesto que en este ejemplo, estamos evaluando el encabezado location que resulta ser un encabezado común, seleccione **encabezado común** botón de radio como el **nombre de encabezado**.

   - Seleccione **ubicación** desde el **encabezado común** lista desplegable.

   - Seleccione **No** como el **distingue mayúsculas de minúsculas** configuración.

   - Seleccione **igual (=)** desde el **operador** lista desplegable.

   - Especifique el patrón de expresión regular. En este ejemplo, usaremos el patrón `(https?):\/\/.*azurewebsites\.net(.*)$` .

   - Haga clic en **OK**.

     ![Modificar encabezado de ubicación](media/rewrite-http-headers-portal/condition.png)

7. Agregar una acción para volver a escribir el encabezado de ubicación:

   - Seleccione **establecer** como el **tipo de acción**.

   - Seleccione **respuesta** como el **tipo de encabezado**.

   - Seleccione **encabezado común** como el **nombre de encabezado**.

   - Seleccione **ubicación** desde el **encabezado común** lista desplegable.

   - Escriba el valor del encabezado. En este ejemplo, usaremos `{http_resp_Location_1}://contoso.com{http_resp_Location_2}` como el valor del encabezado. Esta acción reemplazará *azurewebsites.net* con *contoso.com* en el encabezado location.

   - Haga clic en **OK**.

     ![Modificar encabezado de ubicación](media/rewrite-http-headers-portal/action.png)

8. Haga clic en **crear** para crear la reescritura de conjunto.

   ![Modificar encabezado de ubicación](media/rewrite-http-headers-portal/create.png)

9. Se abrirá en la vista de conjunto de reescritura. Compruebe que el conjunto de reescritura que creó anteriormente está presente en la lista de conjuntos de reescritura.

   ![Modificar encabezado de ubicación](media/rewrite-http-headers-portal/rewrite-set-list.png)

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca de la configuración necesaria para realizar algunas comunes casos de uso, vea [encabezado común escribir escenarios](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers).

   
