---
title: Actualización de la versión del sistema operativo en Azure Security Center | Microsoft Docs
description: En este artículo, mostramos cómo implementar la recomendación **Actualizar versión del sistema operativo** de Azure Security Center.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: aa372492-ecdb-4368-8fdd-d8ed31e216ee
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: a17bc3cf8c8f1ffbb2a91e17944bfc1c203f439d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60905855"
---
# <a name="update-os-version-in-azure-security-center"></a>Actualización de la versión del sistema operativo en Azure Security Center
Para las máquinas virtuales de los servicios en la nube, Azure Security Center recomienda que se actualice el sistema operativo si hay disponible una versión más reciente.  Se supervisan los roles de web y de trabajo de servicios en la nube que se ejecutan en espacios de producción.

> [!NOTE]
> En este documento se presenta el servicio mediante una implementación de ejemplo.  No se trata de una guía paso a paso.
> 
> 

## <a name="implement-the-recommendation"></a>Implementación de la recomendación
1. En la hoja **Recomendaciones**, seleccione **Actualizar la versión del SO**.
   ![Actualizar versión del sistema operativo][1]
2. Se abrirá la hoja **Actualizar versión del sistema operativo**. Siga los pasos de esta hoja para actualizar la versión del sistema operativo.

## <a name="see-also"></a>Vea también
En este artículo, mostramos cómo implementar la recomendación "Actualizar versión del sistema operativo" de Security Center. Para más información acerca de los servicios en la nube y la actualización de la versión de sistema operativo para un servicio en la nube, consulte:

* [Información general de Cloud Services](../cloud-services/cloud-services-choose-me.md)
* [Actualización de un servicio en la nube](../cloud-services/cloud-services-update-azure-service.md)
* [Configuración de Cloud Services](../cloud-services/cloud-services-how-to-configure-portal.md)

Para más información sobre el Centro de seguridad, consulte los siguientes recursos:

* [Establecimiento de directivas de seguridad en Azure Security Center](tutorial-security-policy.md): aprenda a configurar directivas de seguridad para las suscripciones y los grupos de recursos de Azure.
* [Administración de recomendaciones de seguridad en Azure Security Center](security-center-recommendations.md): recomendaciones que le ayudan a proteger los recursos de Azure.
* [Supervisión del estado de seguridad en Azure Security Center](security-center-monitoring.md): obtenga información sobre cómo supervisar el mantenimiento de los recursos de Azure.
* [Administración y respuesta a las alertas de seguridad en Azure Security Center](security-center-managing-and-responding-alerts.md): obtenga información sobre cómo administrar y responder a alertas de seguridad.
* [Supervisión de las soluciones de asociados con Azure Security Center](security-center-partner-solutions.md): aprenda a supervisar el estado de mantenimiento de las soluciones de asociados.
* [Preguntas más frecuentes sobre Azure Security Center](security-center-faq.md): busque las preguntas más frecuentes sobre cómo usar el servicio.
* [Blog de seguridad de Azure](https://blogs.msdn.com/b/azuresecurity/): obtenga las últimas noticias e información sobre la seguridad en Azure.

<!--Image references-->
[1]: ./media/security-center-update-os-version/update-os-version.png
