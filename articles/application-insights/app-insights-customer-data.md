---
title: Guía sobre datos personales almacenados en Azure Application Insights | Microsoft Docs
description: En este artículo se describe cómo administrar los datos personales almacenados en Azure Application Insights, así como los métodos para identificarlos y quitarlos.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 05/18/2018
ms.reviewer: Evgeny.Ternovsky
ms.author: mbullwin
ms.openlocfilehash: 95e421278b46015e761764792e11dec0351b9785
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/11/2018
ms.locfileid: "35294428"
---
# <a name="guidance-for-personal-data-stored-in-application-insights"></a>Guía sobre datos personales almacenados en Application Insights

Application Insights es un almacén de datos donde es probable que haya datos personales. En este artículo se describe en qué parte de Application Insights se encuentran estos datos normalmente, así como las funcionalidades disponibles para tratarlos.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="strategy-for-personal-data-handling"></a>Estrategia de tratamiento de datos personales

Mientras que usted y su empresa sean los responsables en última instancia de determinar la estrategia de tratamiento de los datos privados (si los hubiera), pueden adoptarse los siguientes enfoques. Aparecen en orden de preferencia (de más a menos) desde un punto de vista técnico:
* Siempre que sea posible, detener la recopilación de los datos, ofuscarlos o anonimizarlos, o bien ajustar la recopilación de los datos para excluirlos por ser de carácter _privado_. Este método es el preferido, ya que evita la necesidad de crear una estrategia de tratamiento de datos rápida y costosa.
* Si no es posible, trate de normalizar los datos para reducir el impacto en el rendimiento y la plataforma de datos. Por ejemplo, en lugar de registrar un identificador de usuario explícito, cree una búsqueda para los datos que se correlacionarán el nombre de usuario y sus detalles con un identificador interno que luego puede registrarse en otra parte. De este modo, si uno de los usuarios le pide eliminar su información personal, se puede hacer suprimiendo únicamente la fila en la tabla de búsqueda correspondientes al usuario. 
* Por último, si se deben recopilar datos privados, cree un proceso en la ruta de acceso de la API de depuración y en la de la API de consulta existente para cumplir con las obligaciones relativas a exportar y eliminar cualquier dato privado asociado con un usuario.

## <a name="where-to-look-for-private-data-in-application-insights"></a>¿Dónde se pueden buscar datos privados en Application Insights?

Application Insights es un almacén completamente flexible que, además de recomendar un esquema a los datos, permite reemplazar todos los campos con valores personalizados. Por lo tanto, no podemos saber exactamente dónde se encuentran datos privados en su aplicación concreta. Las siguientes ubicaciones, sin embargo, son buenas puntos iniciales en su inventario:

* *Direcciones IP*: aunque Application Insights ofuscará de forma predeterminada todos los campos de direcciones IP a "0.0.0.0", se trata de un patrón bastante común para reemplazar este valor con la dirección IP de usuario real con el fin de mantener la información de la sesión. La siguiente consulta de Analytics puede utilizarse para buscar cualquier tabla que contenga valores en la columna de dirección IP que no sea "0.0.0.0" en las últimas 24 horas:
    ```
    search client_IP != "0.0.0.0"
    | where timestamp > ago(1d)
    | summarize numNonObfuscatedIPs_24h = count() by $table
    ```
* *Identificadores de usuario*: de forma predeterminada, Application Insights usará identificadores generados de forma aleatoria para el usuario y el seguimiento de sesión. Sin embargo, es habitual ver estos campos reemplazos para almacenar un identificador más relevante para la aplicación. Por ejemplo: nombres de usuario, GUID de AAD, etc. Estos identificadores suelen considerarse como datos personales y, por lo tanto, deben tratarse de la forma adecuada. Siempre recomendamos tratar de ofuscar o anonimizar estos identificadores. Algunos campos donde suelen encontrarse estos valores son session_Id, user_Id, user_AuthenticatedId, user_AccountId y customDimensions.
* *Datos personalizados*: Application Insights permite anexar un conjunto de dimensiones personalizadas a cualquier tipo de datos. Estas dimensiones pueden ser *cualquier* dato. Utilice la siguiente consulta para identificar las dimensiones personalizadas recopiladas durante las últimas 24 horas:
    ```
    search * 
    | where isnotempty(customDimensions)
    | where timestamp > ago(1d)
    | project $table, timestamp, name, customDimensions 
    ```
* *Datos en tránsito y en memoria*: Application Insights hará un seguimiento de las excepciones, las solicitudes, las llamadas de dependencia y los seguimientos. A menudo se pueden recopilar los datos privados en el código y en las llamadas HTTP. Revise las excepciones, las solicitudes, las dependencias y las tablas de seguimiento para identificar estos datos. Use [inicializadores de telemetría](https://docs.microsoft.com/azure/application-insights/app-insights-api-filtering-sampling) siempre que sea posible para ofuscar estos datos.
* *Capturas de Snapshot Debugger*: la característica [Snapshot Debugger](https://docs.microsoft.com/azure/application-insights/app-insights-snapshot-debugger) de Application Insights permite recopilar las instantáneas de depuración cuando se detecta una excepción en la instancia de producción de la aplicación. Las instantáneas expondrán el seguimiento de la pila completo que provoca las excepciones, así como los valores para las variables locales en cada etapa de la pila. Por desgracia, esta característica no permite eliminar de forma selectiva los puntos de ajuste ni acceder mediante programación a los datos dentro de la instantánea. Por lo tanto, si la velocidad de retención de instantáneas predeterminada no satisface sus requisitos de cumplimiento, recomendamos desactivar la característica.

## <a name="how-to-export-and-delete-private-data"></a>Cómo exportar y eliminar datos privados

Se __recomienda encarecidamente__, cuando sea posible, reestructurar la directiva de recopilación de datos para deshabilitar la recopilación, ofuscación y anonimización de datos, o bien modificarlos para excluirlos por considerarse de carácter "privado". Si los datos se tratan después de haberse recopilado, tanto usted como su equipo tendrán que asumir costos relacionados con la definición y automatización de una estrategia, la creación de una interfaz para que los clientes interactúen con sus datos y el mantenimiento continuo. Además, esto requiere que Application Insights funcione de forma intensiva, y debido al gran volumen de llamadas de API de purga o consulta, es posible que afecte al resto de las interacciones con Application Insights. Dicho esto, hay realmente algunos escenarios válidos en los que se deben recopilar datos privados. En estos casos, los datos deben tratarse como se describe en esta sección.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

### <a name="view-and-export"></a>Visualización y exportación

Para ver y exportar solicitudes de datos, se debe usar la [API de consulta](https://dev.applicationinsights.io/quickstart). Tendrá que decidir qué lógica implementar para convertir la forma de los datos a uno adecuado para los usuarios. [Azure Functions](https://azure.microsoft.com/services/functions/) es un buen lugar para hospedar esta lógica.

### <a name="delete"></a>Eliminar

> [!WARNING]
> Las eliminaciones en Application Insights son destructivas y no reversibles. Por tanto, extreme las precauciones cuando las ejecute.

Hemos publicado como parte de un caso de tratamiento de privacidad una ruta de acceso a una API de "purga". Esta ruta de acceso debe usarse con moderación debido al riesgo asociado inherente, al posible impacto en el rendimiento y a la posibilidad de sesgar completamente agregaciones, medidas y otros aspectos de los datos de Application Insights. Consulte la sección [Estrategia de tratamiento de datos personales](#strategy-for-personal-data-handling) anterior para conocer otras maneras de controlar los datos privados.

La purga es una operación con muchos privilegios que ninguna aplicación ni usuario de Azure (incluido incluso el propietario del recurso) tiene permisos para ejecutarla sin que se le haya concedido expresamente un rol en Azure Resource Manager. Este rol es _Purgador de datos_ y se debe delegar con cuidado debido a la posibilidad de pérdida de datos.

Una vez que se ha asignado el rol de Azure Resource Manager, habrá disponibles dos nuevas rutas de acceso de API, la documentación para desarrolladores completa y la forma de la llamada vinculada:

* [Purga POST](https://docs.microsoft.com/rest/api/application-insights/components/purge): toma un objeto que especifica los parámetros de datos que se van a eliminar y devuelve un GUID de referencia.
* Estado de purga GET: la llamada de purga POST devolverá un encabezado "x-ms-status-location" que incluye una dirección URL a la que se puede llamar para determinar el estado de la API de purga. Por ejemplo: 
   ```
   x-ms-status-location: https://management.azure.com/subscriptions/[SubscriptionId]/resourceGroups/[ResourceGroupName]/providers/microsoft.insights/components/[ComponentName]/operations/purge-[PurgeOperationId]?api-version=2015-05-01
   ```

Aunque la mayoría de las operaciones de purga pueden completarse mucho más rápido que lo establecido en el Acuerdo de Nivel de Servicio (SLA), debido a su gran impacto en la plataforma de datos que utiliza Application Insights, el SLA formal para la realización de operaciones de purga es de 30 días.

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre cómo se recopilan, procesan y protegen los datos, vea [Seguridad de datos de Application Insights](app-insights-data-retention-privacy.md).