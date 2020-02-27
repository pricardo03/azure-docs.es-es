---
title: Recomendaciones del contenedor de Azure Security Center | Microsoft Docs
description: En este documento se explican las recomendaciones de Azure Security Center para saber cómo proteger los contenedores.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 2e76c7f7-a3dd-4d9f-add9-7e0e10e9324d
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/20/2018
ms.author: memildin
ms.openlocfilehash: b8b8b05f703a3eb05936ca95e2047a13650914cf
ms.sourcegitcommit: 0cc25b792ad6ec7a056ac3470f377edad804997a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/25/2020
ms.locfileid: "77604264"
---
# <a name="understand-azure-security-center-container-recommendations"></a>Descripción de las recomendaciones sobre contenedores de Azure Security Center

Al migrar las aplicaciones monolíticas para que ejecuten en producción aplicaciones críticas, nativas de la nube en contenedores, puede aprovechar las ventajas de las características de contenedores, incluida la implementación y actualización sencillas y rápidas. A medida que el número de contenedores implementados continúa aumentando, deben implementarse soluciones de seguridad para proporcionarle visibilidad sobre el estado de seguridad de los contenedores y ayudarle a protegerlos contra amenazas.

Azure Security Center proporciona las siguientes funcionalidades para ayudarle a proteger los contenedores:

- **Visibilidad de los contenedores hospedados en máquinas IaaS Linux**<br>En Azure Security Center, la pestaña Contenedores muestra todas las máquinas virtuales implementadas con Docker. Al explorar los problemas de seguridad en una máquina virtual, Security Center proporciona información adicional relacionada con los contenedores en la máquina, como la versión de Docker y el número de imágenes que se ejecutan en el host.

    ![pestaña contenedor](./media/security-center-container-recommendations/docker-recommendation.png)


- **Recomendaciones de seguridad según la referencia de CIS para Docker**<br>Security Center examina las configuraciones de Docker y le ofrece visibilidad sobre los errores de configuración al proporcionarle una lista de todas las reglas con errores que se han evaluado. Security Center proporciona directrices para ayudarle a resolver estos problemas rápidamente y ahorrar tiempo. Security Center evalúa continuamente las configuraciones de Docker y le proporciona su estado más reciente.

    ![pestaña contenedor](./media/security-center-container-recommendations/container-cis-benchmark.png)

- **Detección de amenazas de contenedor en tiempo real**<br> Security Center ofrece detección de amenazas en tiempo real para los contenedores en máquinas Linux con el componente AuditD. Las alertas identifican varias actividades sospechosas de Docker, como la creación de un contenedor con privilegios en el host, una indicación de que un servidor Secure Shell (SSH) se ejecuta dentro de un contenedor de Docker o el uso de mineros de criptografía. Puede usar esta información para corregir problemas de seguridad y mejorar la seguridad de los contenedores rápidamente.

    ![pestaña contenedor](./media/security-center-container-recommendations/docker-threat-detection.png)

## <a name="recommendations"></a>Recomendaciones
Utilice las tablas siguientes como referencia para comprender la disponibilidad de los contenedores hospedados en máquinas IaaS Linux y la evaluación de seguridad de sus configuraciones de Docker.

| Recomendación | Descripción | Corrección |
| --- | --- | --- |
|Corrección de vulnerabilidades en las configuraciones de seguridad de contenedores |Corrección de vulnerabilidades en las configuraciones de seguridad de contenedores en función de las prácticas recomendadas de configuración.| Para corregir vulnerabilidades en las configuraciones de seguridad de contenedores:<br>1. Revise la lista de reglas erróneas.<br>2. Corrija cada una de las reglas según las instrucciones especificadas.|


## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre las recomendaciones que se aplican a otros tipos de recursos de Azure, consulte los siguientes artículos:

* [Supervisión de identidad y acceso en Azure Security Center](security-center-identity-access.md)
* [Protección de las redes en Azure Security Center](security-center-network-recommendations.md)
* [Protección del servicio SQL de Azure en Azure Security Center](security-center-sql-service-recommendations.md)

Para más información sobre el Centro de seguridad, consulte los siguientes recursos:

* [Protección de las aplicaciones y las máquinas en Azure Security Center](security-center-virtual-machine-protection.md)
* [Establecimiento de directivas de seguridad en Azure Security Center](tutorial-security-policy.md): aprenda a configurar directivas de seguridad para las suscripciones y los grupos de recursos de Azure.
* [Administración y respuesta a las alertas de seguridad en Azure Security Center](security-center-managing-and-responding-alerts.md) : obtenga información sobre cómo administrar y responder a alertas de seguridad.