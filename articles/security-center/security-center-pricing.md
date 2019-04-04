---
title: Actualización al nivel Estándar de Security Center para mejorar la seguridad | Microsoft Docs
description: En este artículo se ofrece información sobre los precios de Azure Security Center.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: 4d1364cd-7847-425a-bb3a-722cb0779f78
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/17/2019
ms.author: monhaber
ms.openlocfilehash: 27acda2496adea39321e498868aebcc2f824df3c
ms.sourcegitcommit: 9f4eb5a3758f8a1a6a58c33c2806fa2986f702cb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2019
ms.locfileid: "58905297"
---
# <a name="upgrade-to-security-centers-standard-tier-for-enhanced-security"></a>Actualización al nivel Estándar de Security Center para mejorar la seguridad
Azure Security Center ofrece una administración de seguridad unificada y una protección contra amenazas avanzada para las cargas de trabajo que se ejecutan en Azure, de forma local y en otras nubes. Ofrece visibilidad y control sobre las cargas de trabajo de nube híbrida, defensas activas que reducen la exposición a las amenazas y una detección inteligente, lo que le permitirá mantenerse al día de los ciberataques, que se expanden rápidamente.

## <a name="pricing-tiers"></a>Planes de tarifa
Security Center se ofrece en dos niveles:

- El nivel **Gratis** se habilita automáticamente en todas las suscripciones de Azure y proporciona recomendaciones de directivas de seguridad, de evaluación continua de la seguridad y de seguridad que requieren acción para que pueda proteger sus recursos de Azure.
- El nivel **Estándar** amplía las capacidades del nivel Gratis a las cargas de trabajo que se ejecutan en nubes privadas y otras nubes públicas, lo que ofrece una administración de seguridad unificada y protección contra amenazas en todas sus cargas de trabajo de nube híbrida. El nivel Estándar también incorpora capacidades de detección de amenazas avanzada, donde se emplea el análisis de comportamiento integrado y el aprendizaje automático para identificar ataques y vulnerabilidades de día cero, controles de acceso y de aplicación para reducir la exposición a los ataques a la red y al malware, etc. Puede probar el nivel estándar de forma gratuita. Estándar de Security Center es compatible con Azure escalado de máquinas virtuales de los recursos incluidas las máquinas virtuales, conjuntos, App Service, servidores SQL Server y las cuentas de almacenamiento. Si tiene el nivel estándar de Azure Security Center, puede optar por no recibir soporte técnico en función del tipo de recurso. 


Para más información, vea la [página de precios](https://azure.microsoft.com/pricing/details/security-center/) de Security Center.

## <a name="try-standard-free-for-30-days"></a>Pruebe el nivel Estándar de forma gratuita durante 30 días
El nivel Estándar se ofrece gratis durante los primeros 30 días. Después de 30 días, si decide seguir usando el servicio, se empezará a cobrar automáticamente por su uso.

Puede actualizar una suscripción a Azure entera al nivel Estándar, que heredarán todos los recursos dentro de la suscripción.

Para obtener el nivel Estándar:

1. Seleccione **Directiva de seguridad** en el menú principal de **Security Center**.
2. Seleccione la suscripción que quiera actualizar al nivel Estándar.
3. En la hoja **Directiva de seguridad**, seleccione **Plan de tarifa**.
4. Seleccione **Estándar** para efectuar la actualización.
5. Haga clic en **Save**(Guardar).

(Los precios en la imagen por ejemplo son solo con fines). ![Precios de Security Center](./media/security-center-pricing/get-standard.png)

> [!NOTE]
> Para habilitar todas las características de Security Center, debe aplicar el plan para la suscripción que contiene las máquinas virtuales aplicables de tarifa estándar. La configuración de los precios para un área de trabajo no habilita el acceso a VM Just-In-Time, los controles de aplicación adaptables ni las detecciones de red para los recursos de Azure.
>
>

## <a name="why-upgrade-to-standard"></a>¿Por qué actualizar al nivel Estándar?
Security Center ofrece una seguridad mejorada y una protección contra amenazas para las cargas de trabajo de nube híbrida, como, por ejemplo:

- **Seguridad híbrida**: Obtenga una vista unificada de la seguridad de todas sus cargas de trabajo locales y en la nube. Aplique directivas de seguridad y evalúe constantemente la seguridad de las cargas de trabajo de nube híbrida para garantizar el cumplimiento normativo con los estándares de seguridad. Recopile, busque y analice datos de seguridad de una gran variedad de orígenes, incluidos firewalls y otras soluciones de asociados.
- **Detección de amenazas avanzada**: use análisis avanzado y el gráfico de seguridad inteligente de Microsoft para adelantarse a los ciberataques en constante cambio.  Aproveche la funcionalidad integrada de análisis del comportamiento y aprendizaje automático para identificar ataques y vulnerabilidades de seguridad de día cero. Supervise las redes, las máquinas y los servicios en la nube para detectar ataques entrantes y actividad posterior a una infracción de seguridad. Optimice la investigación con herramientas interactivas e inteligencia de amenazas contextual.
- **Controles de acceso y de aplicación**: bloquee el malware y otras aplicaciones no deseadas aplicando recomendaciones de inclusión en lista blanca adaptadas a sus cargas de trabajo específicas y basadas en el aprendizaje automático. Reduzca la superficie expuesta a ataques de red con el acceso controlado Just-In-Time a los puertos de administración de las máquinas virtuales de Azure, lo que reducirá drásticamente la exposición a ataques por fuerza bruta y otros tipos de ataques de red.


## <a name="next-steps"></a>Pasos siguientes
En este artículo se han presentado los precios de Security Center. Para obtener más información sobre la seguridad mejorada y la protección contra amenazas avanzada del nivel Estándar, vea:

- [Detección de amenazas avanzada](security-center-threat-report.md)
- [Just-in-control de acceso de máquina virtual de tiempo](security-center-just-in-time.md)

<!--Image references-->
[1]: ./media/security-center-pricing/get-standard.png
