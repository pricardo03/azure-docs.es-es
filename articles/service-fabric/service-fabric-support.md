---
title: Información acerca las opciones de soporte técnico de Azure Service Fabric | Microsoft Docs
description: Versiones de clúster de Azure Service Fabric compatibles y vínculos para presentar incidencias de soporte técnico
services: service-fabric
documentationcenter: .net
author: pkcsf
manager: jpconnock
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: troubleshooting
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/24/2018
ms.author: pkc
ms.openlocfilehash: a931de8be07d41cf4daab63aa7691973ee158452
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60545068"
---
# <a name="azure-service-fabric-support-options"></a>Opciones de soporte técnico de Azure Service Fabric

Hay varias opciones configuradas para usted con el fin de brindar el soporte técnico adecuado para los clústeres de Service Fabric en los que ejecuta las cargas de trabajo de la aplicación. Tiene la posibilidad de elegir las opciones correctas según el nivel del soporte técnico que se necesita y la gravedad del problema. 

## <a name="report-production-issues-or-request-paid-support-for-azure"></a>Notifique problemas en el entorno de producción o bien solicite soporte técnico de pago para Azure

Si desea informar sobre problemas en el clúster de Service Fabric que implementó en Azure, abra una incidencia de soporte técnico [en Azure Portal](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) o en el [Portal de soporte técnico de Microsoft](https://support.microsoft.com/oas/default.aspx?prid=16146).

Más información sobre:
 
- [Soporte técnico de Microsoft para Azure](https://azure.microsoft.com/support/plans/?b=16.44).
- [Soporte técnico Premier de Microsoft](https://support.microsoft.com/en-us/premier).

> [!Note]
> Clústeres que se ejecutan en un nivel de confiabilidad bronze le permiten ejecutar cargas de trabajo de prueba solo. Si experimenta problemas con un clúster con una confiabilidad bronze, el equipo de soporte técnico de Microsoft le ayudarán a mitigar el problema, pero no llevará a cabo un análisis de causa raíz. Consulte [las características de confiabilidad del clúster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-reliability-characteristics-of-the-cluster) para obtener más detalles.
>
> Para obtener más información sobre lo que es necesario para un clúster listo para producción, consulte el [lista de comprobación de preparación para la producción](https://docs.microsoft.com/azure/service-fabric/service-fabric-production-readiness-checklist).

<a id="getlivesitesupportonprem"></a>

## <a name="report-production-issues-or-request-paid-support-for-standalone-service-fabric-clusters"></a>Notifique problemas en el entorno de producción o bien solicite soporte técnico de pago para clústeres independientes de Service Fabric

Si desea informar sobre problemas en el clúster de Service Fabric que implementó de forma local o en otras nubes, abra una incidencia de soporte técnico profesional en el [Portal de soporte técnico de Microsoft](https://support.microsoft.com/oas/default.aspx?prid=16146).

Más información sobre:

- [Soporte técnico profesional para Microsoft local](https://support.microsoft.com/en-us/gp/offerprophone?wa=wsignin1.0).
- [Soporte técnico Premier de Microsoft](https://support.microsoft.com/en-us/premier).

## <a name="report-azure-service-fabric-issues"></a>Información de problemas con Azure Service Fabric

Configuramos un repositorio de GitHub para informar problemas con Service Fabric.  También supervisamos activamente los foros siguientes.

### <a name="github-repo"></a>Repositorio de GitHub 

Informe problemas con Azure Service Fabric en el [repositorio de Git Service-Fabric-issues](https://github.com/Azure/service-fabric-issues). Este repositorio está diseñado para informar y rastrear problemas con Azure Service Fabric y para generar solicitudes de funciones pequeñas. **No lo use para informar problemas con el sitio activo**.

### <a name="stackoverflow-and-msdn-forums"></a>StackOverflow y foros de MSDN

La [etiqueta Service Fabric en StackOverflow][stackoverflow] y el [foro de Service Fabric en MSDN][msdn-forum] son ideales para hacer preguntas sobre cómo funciona la plataforma y cómo podría realizar ciertas tareas con ella.

### <a name="azure-feedback-forum"></a>Foro de comentarios de Azure

El [foro de comentarios de Azure para Service Fabric][uservoice-forum] es el lugar ideal para enviar ideas sobre características importantes que tenga para el producto, porque revisamos las solicitudes más populares que forman parte de nuestro planeamiento de mediano a largo plazo. Se recomienda buscar apoyo para sus sugerencias dentro de la comunidad.

## <a name="service-fabric-preview-versions---unsupported-for-production-use"></a>Versiones de vista previa de Service Fabric: no admitidas para su uso en producción

De vez en cuando, publicamos versiones que tienen características importantes sobre las que deseamos saber su opinión, que se publican como versiones preliminares. Estas versiones preliminares solo se deben usar con fines de prueba. El clúster de producción debe estar ejecutando siempre una versión de Service Fabric compatible y estable. Una versión preliminar siempre comienza con un número de versión principal y secundaria de 255. Por ejemplo, si ve una versión 255.255.5703.949 de Service Fabric, esa versión es solo para su uso en clústeres de prueba y se encuentra en versión preliminar. Estas versiones preliminares también se anuncian en el [blog del equipo de Service Fabric](https://blogs.msdn.microsoft.com/azureservicefabric) y se detallan en las características incluidas.
No hay ninguna opción de soporte técnico de pago para estas versiones preliminares. Use una de las opciones enumeradas en [Notificación de problemas Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-support#report-azure-service-fabric-issues) para formular preguntas o proporcionar comentarios.

## <a name="next-steps"></a>Pasos siguientes

[Versiones admitidas de Service Fabric](service-fabric-versions.md)

<!--references-->
[msdn-forum]: https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureServiceFabric
[stackoverflow]: https://stackoverflow.com/questions/tagged/azure-service-fabric
[uservoice-forum]: https://feedback.azure.com/forums/293901-service-fabric
[acom-docs]: https://aka.ms/servicefabricdocs
[sample-repos]: https://aka.ms/servicefabricsamples