---
title: 'Azure Front Door Service: back-end y grupos de back-end | Microsoft Docs'
description: En este artículo le ayudará a comprender cuáles son los back-end y grupos de back-end para la configuración de Front Door.
services: front-door
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: 228ed5c54a382db7b47d19adacf9e5db398c53ae
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58123698"
---
# <a name="backends-and-backend-pools-in-azure-front-door-service"></a>Back-end y grupos de back-end en Azure Front Door Service
En este artículo se explican los diferentes conceptos relacionados con cómo se puede asignar la implementación de aplicaciones con Front Door. También explicaremos los distintos términos en la parte delantera configuración puerta en torno a la aplicación back-end significado.

## <a name="backend-pool"></a>Grupo back-end
Un grupo de back-end en Front Door hace referencia al conjunto de back-end equivalentes que pueden recibir el mismo tipo de tráfico para su aplicación. En otras palabras, es una agrupación lógica de las instancias de la aplicación en todo el mundo que pueden recibir el mismo tráfico y pueden responder con el comportamiento esperado. Estos back-end se implementan normalmente en diferentes regiones o en la misma región. Además, estos back-end pueden estar en modo de implementación activo/activo o se puede definir con una configuración activo/pasivo.

El grupo back-end también define cómo los distintos back-end deben evaluarse para conocer su estado mediante sondeos de estado y, en consecuencia, cómo debe ocurrir el equilibrio de carga entre los back-end.

### <a name="health-probes"></a>Sondeos de estado
Front Door envía solicitudes de sondeo HTTP/HTTPS periódicas a cada uno de los back-end configurados para determinar la proximidad y el estado de cada back-end para equilibrar la carga de las solicitudes del usuario final. La configuración de sondeo de estado para un grupo de back-end define cómo se sondea el estado de mantenimiento para los back-end de la aplicación. Las siguientes opciones están disponibles para la configuración de equilibrio de carga:

1. **Ruta de acceso**: Ruta de acceso de dirección URL donde se enviarán las solicitudes de sondeo en para todos los back-ends en el grupo de back-end. Por ejemplo, si uno de los back-end es `contoso-westus.azurewebsites.net` y la ruta de acceso se establece en `/probe/test.aspx`, los entornos de Front Door enviarán las solicitudes de sondeo de mantenimiento a http://contoso-westus.azurewebsites.net/probe/test.aspx, suponiendo que el protocolo está establecido en HTTP. 
2. **Protocolo**: Define si las solicitudes de sondeo de estado desde puerta delantera a los back-end se enviarán a través del protocolo HTTP o HTTPS.
3. **Intervalo (segundos)**: Este campo define la frecuencia de los sondeos de estado para los back-end, es decir, los intervalos en la que cada uno de los entornos de la puerta delantera enviará un sondeo. Nota: Si desea obtener conmutaciones por error más rápidas establezca este campo en un valor inferior. En cambio, cuanto menor sea el valor, mayor será el volumen de sondeo de estado que recibirán los back-end. Para hacerse una idea de cuánto volumen de sondeo Front Door generará en sus back-end, veamos un ejemplo. Digamos que el intervalo se establece en 30 segundos y hay unos 90 entornos de Front Door o POP a nivel global. Por lo tanto, cada uno de los back-end recibirá aproximadamente entre 3 y 5 solicitudes de sondeo por segundo.

Lea [sondeos de estado](front-door-health-probes.md) para obtener más información.

### <a name="load-balancing-settings"></a>Configuración de equilibrio de carga
La configuración de equilibrio de carga para el grupo de back-end define cómo se evalúan los sondeos de estado para decidir que el estado del back-end es correcto o incorrecto y cómo se debe equilibrar el tráfico entre los diferentes back-end en el grupo de back-end. Las siguientes opciones están disponibles para la configuración de equilibrio de carga:

1. **El tamaño del ejemplo**: Esta propiedad identifica el número de muestras de los sondeos de estado se debe tener en cuenta para la evaluación de mantenimiento de back-end.
2. **Tamaño de muestra correcta**: Esta propiedad define que del tamaño del ejemplo como se explicó anteriormente, el número de muestras ¿es necesario comprobar el funcionamiento correcto llamar al back-end como en buen estado. 
</br>Por ejemplo, supongamos que para Front Door ha establecido el *intervalo* de sondeo de estado en 30 segundos, el *tamaño del ejemplo* en “5” y el *tamaño de muestra correcta* en “3”. En ese caso, lo que significa esta configuración es que cada vez que se evalúan los sondeos de estado para el back-end, se examinan las cinco últimas muestras, lo que podrían abarcar los últimos 150 segundos (=5*30 s) y, a menos que 3 o más de estos sondeos sean correctos se declarará el estado del back-end como incorrecto. Supongamos que solo había dos sondeos correctos, por lo que se marcará el estado del back-end como incorrecto. La próxima vez que se ejecute la evaluación, si se encuentran 3 sondeos correctos en los últimos cinco, se marcará el estado del back-end como correcto de nuevo.
3. **Sensibilidad a la latencia (latencia adicional)**: El campo de sensibilidad de latencia define si desea que la puerta de entrada para enviar la solicitud al back-ends que están dentro del intervalo de sensibilidad en términos de medición de latencia o reenviar la solicitud para el back-end más cercano. Lea el [método de enrutamiento según menor latencia](front-door-routing-methods.md#latency) para Front Door para obtener más información.

## <a name="backend"></a>Back-end
Un back-end es equivalente a la instancia de implementación de una aplicación en una región. Front Door es compatible con back-end de Azure y que no son de Azure, por lo que aquí la región no está restringida solo a las regiones de Azure, sino que también puede ser su centro de datos local o una instancia de aplicación en alguna otra nube.

Los back-end, en el contexto de Front Door, hacen referencia al nombre de host o a la dirección IP pública de la aplicación, que puede atender solicitudes de cliente. Por lo tanto, los back-end no deben confundirse con el nivel de base de datos, el nivel de capa de almacenamiento, etc., sino que deben verse como el punto de conexión público del back-end de su aplicación.

Al agregar un back-end en un grupo de back-end de su Front Door, deberá rellenar los siguientes detalles:

1. **Tipo de host de back-end**: El tipo de recurso que desea agregar. Front Door es compatible con la detección automática del back-end de la aplicación si proceden del servicio de la aplicación, el servicio en la nube o el almacenamiento. Si desea usar un recurso diferente en Azure o incluso un back-end ajeno a Azure, seleccione “Host personalizado”. Nota: Durante la configuración, las API no comprueban si el back-end es accesible desde los entornos de Front Door, en su lugar, debe asegurarse de que Front Door pueda tener acceso al back-end. 
2. **Nombre de host de suscripción y el back-end**: Si ha seleccionado no escriba 'Host personalizado' para el host de back-end, es necesario definir el ámbito de abajo y seleccione el back-end eligiendo la suscripción adecuada y el nombre de host correspondiente de back-end de la interfaz de usuario.
3. **Encabezado de host de back-end**: El valor del encabezado Host enviado al back-end para cada solicitud. Lea [encabezado de host de back-end](#hostheader) para obtener más información.
4. **Prioridad**: puede asignar prioridades a los diferentes back-end cuando quiera usar un back-end del servicio principal para todo el tráfico y proporcionar copias de seguridad en caso de que el back-end principal o de copia de seguridad no estén disponibles. Obtenga más información sobre [prioridad](front-door-routing-methods.md#priority).
5. **Peso**: puede asignar pesos a sus diferentes back-end cuando desee distribuir el tráfico entre un conjunto de back-end, ya sea de manera uniforme o según los coeficientes de peso. Obtenga más información sobre los [pesos](front-door-routing-methods.md#weighted).


### <a name = "hostheader"></a>Encabezado de host de back-end

Las solicitudes reenviadas por Front Door a un back-end tienen un campo de encabezado de host que el back-end usa para recuperar el recurso de destino. El valor de este campo normalmente procede del URI del back-end y tiene el host y el puerto. Por ejemplo, una solicitud realizada para `www.contoso.com` tendrá el encabezado de host `www.contoso.com`. Si va a configurar su back-end mediante Azure Portal, el valor predeterminado que se rellena para este campo es el nombre de host del back-end. Por ejemplo, si el back-end es `contoso-westus.azurewebsites.net`, el valor que se rellena automáticamente en Azure Portal para el encabezado de host de back-end será `contoso-westus.azurewebsites.net`. 
</br>En cambio, si se usan plantillas de Resource Manager u otro mecanismo y no se ha establecido este campo explícitamente, Front Door envía el nombre de host entrante como el valor de encabezado de host. Por ejemplo, si se realizó la solicitud para `www.contoso.com`, y el back-end es `contoso-westus.azurewebsites.net` con el campo de encabezado de host de back-end vacío, Front Door establecerá el encabezado de host como `www.contoso.com`.

La mayoría de back-end de aplicación (por ejemplo, Web Apps, Blob Storage y Cloud Services) requieren que el encabezado de host coincida con el dominio del back-end. Sin embargo, el host de front-end que enrute el back-end tendrá un nombre de host diferente, como www\.contoso.azurefd.net. Si el back-end que está configurando requiere que el encabezado de host coincida con el nombre de host del back-end, debe asegurarse de que el “Encabezado de host del back-end” también tiene el nombre de host del back-end.

#### <a name="configuring-the-backend-host-header-for-the-backend"></a>Configurar el encabezado de host de back-end para el back-end
El campo “Encabezado de host de back-end” puede configurarse para un back-end en la sección del grupo de back-end.

1. Abra el recurso de Front Door y haga clic en el grupo de back-end con el back-end que se va a configurar.

2. Agregue un back-end si no ha agregado uno, o edite uno existente. El campo “Encabezado de host de back-end” se puede establecer en un valor personalizado o dejar en blanco, lo que significa que el nombre de host para la solicitud entrante se utilizará como el valor del encabezado de host.



## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [crear una instancia de Front Door](quickstart-create-front-door.md).
- Más información acerca de cómo [funciona Front Door](front-door-routing-architecture.md).