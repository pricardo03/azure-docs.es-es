---
title: Protección de App Services en Azure Security Center | Microsoft Docs
description: Este artículo le ayuda a comenzar a proteger las instancias de App Services en Azure Security Center.
services: security-center
documentationcenter: na
author: rkarlin
manager: mbaldwin
editor: ''
ms.assetid: e8518710-fcf9-44a8-ae4b-8200dfcded1a
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/14/2018
ms.author: rkarlin
ms.openlocfilehash: f46837751901015fd80d3ecc7a620878cadb3f12
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2018
ms.locfileid: "52317881"
---
# <a name="protect-app-service-with-azure-security-center"></a>Protección de App Service con Azure Security Center
Este artículo le ayuda a usar Azure Security Center para supervisar y proteger las aplicaciones que se ejecutan en App Service.

App Service le permite crear y hospedar aplicaciones web en el lenguaje de programación que prefiera sin tener que administrar la infraestructura. App Service ofrece escalado automático y alta disponibilidad, es compatible con Windows y Linux, así como implementaciones automatizadas desde GitHub, Visual Studio Team Services o cualquier repositorio Git. 

Los atacantes se aprovechan frecuentemente de las vulnerabilidades en las aplicaciones web, ya que tienen una interfaz común y dinámica para casi todas las organizaciones en Internet. Las solicitudes a las aplicaciones que se ejecutan en App Service pasan por varias puertas de enlace implementadas en los centros de datos de Azure de todo el mundo, responsables de enrutar cada solicitud a su aplicación correspondiente. 

Azure Security Center puede ejecutar evaluaciones y recomendaciones sobre las aplicaciones que se ejecutan en App Service en los espacios aislados de la máquina virtual o en instancias a petición. Al aprovechar la visibilidad que Azure tiene como proveedor de la nube, Security Center analiza los registros internos de la instancia de App Service para supervisar ataques comunes de aplicaciones web que a menudo se ejecutan en múltiples destinos.

Security Center aprovecha la escala de la nube para identificar ataques a las aplicaciones de App Service y centrarse en ataques emergentes, mientras que los atacantes se encuentran en la fase de reconocimiento, explorando para identificar vulnerabilidades en varios sitios web hospedados en Azure. Security Center utiliza modelos de análisis y aprendizaje automático para cubrir todas las interfaces que permiten a los clientes interactuar con las aplicaciones, ya sea a través de HTTP o a través de métodos de administración. Además, como servicio de primera línea en Azure, Security Center también se encuentra en una posición única para ofrecer análisis de seguridad basados en host que cubren los nodos de proceso subyacentes para este PaaS, lo que permite a Security Center detectar ataques contra aplicaciones web que ya se habían aprovechado.

## <a name="prerequisites"></a>Requisitos previos

Para supervisar y proteger la instancia de App Service, debe tener un plan de App Service que esté asociado con máquinas dedicadas. Estos planes son: Básico, Estándar, Premium, Aislado o Linux. Azure Security Center no admite los planes Gratis, Compartido o Consumo. Para más información, consulte [Planes de App Service](https://azure.microsoft.com/pricing/details/app-service/plans/).

## <a name="security-center-protection"></a>Protección de Security Center

Azure Security Center protege la instancia de la máquina virtual en la que se está ejecutando la instancia de App Service y la interfaz de administración. También supervisa las solicitudes y respuestas enviadas a las aplicaciones que se ejecutan en App Service, y desde ellas.

Security Center está integrado de forma nativa con App Service, lo que elimina la necesidad de implementación e incorporación; la integración es completamente transparente.



## <a name="enabling-monitoring-and-protection-of-app-service"></a>Habilitación de la supervisión y protección de App Service

1. En Azure, elija Security Center.
2. Vaya a **Directiva de seguridad** y elija una suscripción.
3. Al final de la fila de la suscripción, haga clic en **Editar configuración**.
4. En **Plan de tarifa**, en la fila **App Service**, alterne el plan en **Habilitado**.

![alternancia de app service](./media/security-center-app-services/app-services-toggle.png)

>[!NOTE]
> El número de instancias enumeradas para la cantidad de recursos representa el número de instancias pertinentes de App Service activas en el momento en que ha abierto la hoja del plan de tarifa. Debido a que este número puede cambiar en función de las opciones de escalado que haya seleccionado, el número de instancias por las que se le cobra se modificará en consecuencia.

Para deshabilitar la supervisión y recomendaciones para la instancia de App Service, repita este proceso y alterne el plan de **App Service** en **Deshabilitado**.



## <a name="see-also"></a>Otras referencias
En este artículo, aprendió a usar las funcionalidades de supervisión en Azure Security Center. Para obtener más información sobre el Centro de seguridad de Azure, consulte los siguientes recursos:

* [Establecimiento de directivas de seguridad en Azure Security Center](security-center-azure-policy.md): obtenga información sobre cómo configurar los ajustes de seguridad en Azure Security Center.
* [Administración y respuesta a las alertas de seguridad en Azure Security Center](security-center-managing-and-responding-alerts.md): obtenga información sobre cómo administrar y responder a alertas de seguridad.
* [Supervisión de las soluciones de asociados con Azure Security Center](security-center-partner-solutions.md): aprenda a supervisar el estado de mantenimiento de las soluciones de asociados.
* [Preguntas más frecuentes sobre Azure Security Center](security-center-faq.md): encuentre las preguntas más frecuentes sobre el uso del servicio.
* [Blog de seguridad de Azure](http://blogs.msdn.com/b/azuresecurity/): encuentre entradas de blog sobre el cumplimiento y la seguridad de Azure.
