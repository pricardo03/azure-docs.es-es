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
ms.openlocfilehash: e144214a58f9fe383cf4edd878554792d9d6a6f9
ms.sourcegitcommit: ed66a704d8e2990df8aa160921b9b69d65c1d887
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64947162"
---
# <a name="rewrite-http-request-and-response-headers-with-azure-application-gateway---azure-portal"></a>Vuelva a escribir los encabezados de solicitud y respuesta HTTP con Azure Application Gateway mediante Azure portal

En este artículo se describe cómo usar el portal de Azure para configurar un [SKU de puerta de enlace de aplicaciones v2](<https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant>) instancia volver a escribir los encabezados HTTP en las solicitudes y respuestas.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="before-you-begin"></a>Antes de empezar

Debe tener una instancia SKU de puerta de enlace de aplicación v2 para completar los pasos descritos en este artículo. No se admite la reescritura de encabezados en la SKU v1. Si no tiene la SKU de v2, cree un [SKU de puerta de enlace de aplicaciones v2](https://docs.microsoft.com/azure/application-gateway/tutorial-autoscale-ps) instancia antes de comenzar.

## <a name="create-required-objects"></a>Crear los objetos necesarios

Para configurar la reescritura del encabezado HTTP, es preciso completar estos pasos.

1. Crear los objetos que son necesarios para la reescritura del encabezado HTTP:

   - **Vuelva a escribir la acción**: Se usa para especificar la solicitud y el nuevo valor para los encabezados y los campos de encabezado de solicitud que va a escribir. Puede asociar uno o más condiciones con una acción de reescritura de reescritura.

   - **Vuelva a escribir la condición**: Una configuración opcional. Condiciones de reescritura evalúan el contenido de las solicitudes HTTP (S) y las respuestas. Si la solicitud HTTP (S) o la respuesta coincide con la condición de reescritura, se producirá la acción de reescritura.

     Si más de una condición se asocia con una acción, se produce la acción solo cuando se cumplen todas las condiciones. En otras palabras, la operación es una operación AND lógica.

   - **La regla de reescritura**: Contiene varias acciones de reescritura o vuelva a escribir las combinaciones de condición.

   - **Secuencia de regla**: Ayuda a determina el orden en el que se ejecutan las reglas de reescritura. Esta configuración es útil cuando haya varias reglas de reescritura en un conjunto de reescritura. Una regla de reescritura que tiene un valor de secuencia más bajo de regla se ejecuta primera. Si asigna el mismo valor de secuencia de la regla a dos reglas de reescritura, el orden de ejecución es no determinista.

   - **Vuelva a escribir conjunto**: Contiene varias reglas de reescritura que se asociarán con una regla de enrutamiento de solicitud.

2. Adjunte la reescritura establecida en una regla de enrutamiento. La configuración de reescritura se adjunta al agente de escucha de origen a través de la regla de enrutamiento. Cuando se usa una regla de enrutamiento básica, la configuración de reescritura de encabezado está asociada con un agente de escucha de origen y es una reescritura encabezado global. Cuando se usa una regla de enrutamiento basada en ruta de acceso, la configuración de reescritura de encabezado se define en el mapa de ruta de acceso de dirección URL. En ese caso, se aplica solo al área de ruta de acceso específica de un sitio.

Puede crear varios conjuntos de reescritura de encabezado HTTP y aplicar cada reescritura establecido en varios agentes de escucha. Pero puede aplicar sólo una reescritura establecido en un agente de escucha concreto.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en [Azure Portal](https://portal.azure.com/) con su cuenta de Azure.

## <a name="configure-header-rewrite"></a>Configurar la reescritura de encabezado

En este ejemplo, se modificará una dirección URL de redirección al volver a escribir el encabezado de ubicación en la respuesta HTTP enviada por una aplicación de back-end.

1. Seleccione **todos los recursos**y, a continuación, seleccione la puerta de enlace de la aplicación.

2. Seleccione **reescribe** en el panel izquierdo.

3. Seleccione **reescribir conjunto**:

   ![Agregar conjunto de reescritura](media/rewrite-http-headers-portal/add-rewrite-set.png)

4. Proporcione un nombre para el conjunto de reescritura y asociarla a una regla de enrutamiento:

   - Escriba el nombre para la reescritura establecido el **nombre** cuadro.
   - Seleccione una o varias de las reglas enumeradas en la **asociados a las reglas de enrutamiento** lista. Puede seleccionar sólo las reglas que aún no han asociado con otros conjuntos de reescritura. Las reglas que ya se han asociado con otros conjuntos de reescritura aparecen atenuadas.
   - Seleccione **Next** (Siguiente).
   
     ![Agregar el nombre y la asociación](media/rewrite-http-headers-portal/name-and-association.png)

5. Cree una regla de reescritura:

   - Seleccione **Agregar regla de reescritura**.

     ![Agregar regla de reescritura](media/rewrite-http-headers-portal/add-rewrite-rule.png)

   - Escriba un nombre para la regla de reescritura en la **nombre de la regla de reescritura** cuadro. Escriba un número en el **regla secuencia** cuadro.

     ![Agregar nombre de la regla de reescritura](media/rewrite-http-headers-portal/rule-name.png)

6. En este ejemplo, se deberá volver a escribir el encabezado de ubicación sólo cuando contiene una referencia a azurewebsites.net. Para ello, agregue una condición para evaluar si el encabezado de ubicación en la respuesta contiene azurewebsites.net:

   - Seleccione **Agregar condición** y, a continuación, seleccione el cuadro que contiene el **si** instrucciones para expandirlo.

     ![Agregar una condición](media/rewrite-http-headers-portal/add-condition.png)

   - En el **tipo de variable para comprobar** lista, seleccione **encabezado HTTP**.

   - En el **tipo de encabezado** lista, seleccione **respuesta**.

   - Dado que en este ejemplo se evalúan el encabezado location, que es un encabezado común, seleccione **encabezado común** en **nombre de encabezado**.

   - En el **encabezado común** lista, seleccione **ubicación**.

   - En **distingue mayúsculas de minúsculas**, seleccione **No**.

   - En el **operador** lista, seleccione **igual (=)**.

   - Especifique un patrón de expresión regular. En este ejemplo, vamos a usar el patrón `(https?):\/\/.*azurewebsites\.net(.*)$`.

   - Seleccione **Aceptar**.

     ![Configurar If condición](media/rewrite-http-headers-portal/condition.png)

7. Agregar una acción para volver a escribir el encabezado de ubicación:

   - En el **tipo de acción** lista, seleccione **establecer**.

   - En el **tipo de encabezado** lista, seleccione **respuesta**.

   - En **nombre de encabezado**, seleccione **encabezado común**.

   - En el **encabezado común** lista, seleccione **ubicación**.

   - Escriba el valor del encabezado. En este ejemplo, vamos a usar `{http_resp_Location_1}://contoso.com{http_resp_Location_2}` como el valor del encabezado. Este valor se reemplazará *azurewebsites.net* con *contoso.com* en el encabezado location.

   - Seleccione **Aceptar**.

     ![Agregar una acción](media/rewrite-http-headers-portal/action.png)

8. Seleccione **crear** para crear la reescritura de conjunto:

   ![Seleccionar Crear](media/rewrite-http-headers-portal/create.png)

9. Se abrirá la vista de conjunto de reescritura. Compruebe que el conjunto de reescritura que creó está en la lista de conjuntos de reescritura:

   ![Vista de conjunto de reescritura](media/rewrite-http-headers-portal/rewrite-set-list.png)

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca de cómo configurar algunos casos de uso comunes, vea [encabezado común escribir escenarios](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers).