---
title: Directivas de seguridad de Azure Security Center configuradas individualmente o como parte de Azure Policy | Microsoft Docs
description: Con este documento aprenderá a configurar las directivas en Azure Security Center o en Azure Policy.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: mbaldwin
editor: ''
ms.assetid: cd906856-f4f9-4ddc-9249-c998386f4085
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/5/2018
ms.author: terrylan
ms.openlocfilehash: 3c198ea44953c0b2e72a544cd0e83b6592d9a81f
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "47032091"
---
# <a name="setting-security-policies-in-security-center-or-in-azure-policy"></a>Configurar directivas de seguridad en Security Center o en Azure Policy

En este artículo se explica cómo configurar directivas de seguridad de Azure Security Center. Las directivas de Azure Security Center se integran con las directivas de Azure Policy, de modo que puede configurarlas en Security Center en una suscripción específica, o en [Azure Policy](../azure-policy/azure-policy-introduction.md), lo que permite establecer directivas en varios grupos de administración y varias suscripciones.

## <a name="what-are-security-policies"></a>¿Qué son las directivas de seguridad?
Una directiva de seguridad define la configuración deseada de las cargas de trabajo. Además, ayuda a garantizar el cumplimiento de los requisitos de seguridad normativos o de la empresa. En Azure Security Center, puede definir directivas para las suscripciones de Azure y adaptarlas al tipo de carga de trabajo o a la confidencialidad de los datos. Por ejemplo, es posible que las aplicaciones que usan datos regulados, como la información de identificación personal, requieran un mayor nivel de seguridad que otras cargas de trabajo. Para establecer una directiva en varias suscripciones o grupos de administración, debe configurarla en [Azure Policy](../azure-policy/azure-policy-introduction.md).

> [!NOTE]
> Si ya ha configurado directivas de seguridad en una suscripción que forma parte de un grupo de administración o tiene varias asignaciones de directiva, esas directivas aparecen atenuadas en Security Center para que pueda administrar la directiva en el nivel de grupo de administración a través de la página Azure Policy. 

## <a name="how-security-policies-work"></a>¿Cómo funcionan las directivas de seguridad?
Security Center crea automáticamente una directiva de seguridad predeterminada para cada una de las suscripciones de Azure. Puede modificar las directivas de Security Center o usar Azure Policy para realizar lo siguiente:
- Crear nuevas definiciones de directiva.
- Asignar directivas entre grupos de administración y suscripciones, que pueden representar una organización entera o una unidad de negocio dentro de la organización.
- Supervisar el cumplimiento de las directivas.

Para más información sobre Azure Policy, consulte [Creación y administración de directivas para aplicar el cumplimiento](../azure-policy/create-manage-policy.md).

Una directiva de Azure consta de los siguientes componentes:

- Una **directiva** es una regla.
- Una **iniciativa** es una colección de directivas.
- Una **asignación** es una aplicación de una iniciativa o una directiva para un ámbito concreto (grupo de administración, suscripción o grupo de recursos).

Un recurso se evalúa con las directivas que están asignadas a él y recibe una proporción de cumplimiento en función del número de directivas con las que guarda conformidad el recurso.

## <a name="who-can-edit-security-policies"></a>¿Quién puede editar directivas de seguridad?
Security Center usa el control de acceso basado en rol (RBAC), que proporciona roles integrados que se pueden asignar a usuarios, grupos y servicios en Azure. Cuando un usuario abre Security Center, solo ve la información relacionada con los recursos a los que tiene acceso. Esto significa que a los usuarios se les asigna el rol de propietario, colaborador o lector para la suscripción o el grupo de recursos a los que pertenece un recurso. Además de estos roles, hay dos roles específicos de Security Center:

- Lector de seguridad: el usuario tiene derecho a visualizar el contenido de Security Center (recomendaciones, alertas, directivas y estados) pero no puede realizar cambios.
- Administrador de seguridad: tiene los mismos derechos que el lector de seguridad, pero también puede actualizar la directiva de seguridad o descartar recomendaciones y alertas.

## <a name="edit-security-policies"></a>Edición de directivas de seguridad
Puede editar la directiva de seguridad predeterminada de cada una de las suscripciones y grupos de administración de Azure en Security Center. Para modificar una directiva de seguridad, debe ser propietario, colaborador o administrador de seguridad de la suscripción o del grupo de administración que la contiene. Para ver las directivas de seguridad de Security Center:

> [!NOTE]
> Las directivas establecidas en una suscripción que forma parte de un grupo de administración o tiene varias asignaciones de directiva, aparecerán atenuadas en Security Center. Puede editar estas directivas en [Azure Policy](../azure-policy/azure-policy-introduction.md). 

1. En el panel **Security Center**, en **POLICY & COMPLIANCE** (DIRECTIVA Y CUMPLIMIENTO), seleccione **Security policy** (Directiva de seguridad). Se abre el panel **Policy Management** (Administración de directivas).

    ![El panel Administración de directivas](./media/security-center-azure-policy/security-center-policies-fig10.png)

  En este panel se muestra el número de grupos de administración, suscripciones y áreas de trabajo, así como la estructura del grupo de administración.

  > [!NOTE]
  > El panel de Security Center podría mostrar un número de suscripciones en **Subscription coverage** (Cobertura de suscripción) superior al que se muestra en **Policy Management** (Administración de directivas). En la cobertura de suscripción se muestra el número de suscripciones Estándar, Gratis y "no cubiertas". Las suscripciones "no cubiertas" no tiene habilitado Security Center y no se muestran en **Policy Management** (Administración de directivas).
  >
  >

  En las columnas de la tabla se muestra:

 - Policy Initiative Assignment (Asignación de iniciativa de directiva): directivas e iniciativas integradas de Security Center que se asignan a una suscripción o un grupo de administración.
 - Compliance (Cumplimiento): puntuación general de cumplimiento para un grupo de administración, una suscripción o un área de trabajo. La puntuación es la media ponderada de las asignaciones. La media ponderada tiene en cuenta el número de directivas en una única asignación y el número de recursos al que se aplica la asignación.

 Por ejemplo, si la suscripción tiene dos máquinas virtuales y tiene asignada una iniciativa con cinco directivas, entonces usted tiene 10 valoraciones en la suscripción. Si una de las máquinas virtuales no cumple las dos directivas, la puntuación general de cumplimiento de la asignación de la suscripción es del 80 %.

 - Coverage (Cobertura): identifica el plan de tarifa, Gratis o Estándar, en el que se ejecuta el grupo de administración, la suscripción o el área de trabajo.  Para obtener más información sobre los planes de tarifa de Security Center, vea [Precios](security-center-pricing.md).
 - Settings (Configuración): las suscripciones tienen el vínculo **Edit settings** (Editar configuración). Al seleccionar **Edit settings** (Editar configuración), puede actualizar la configuración de suscripción, como la colección de datos, el plan de tarifa y las notificaciones por correo electrónico.

2. Seleccione la suscripción o el grupo de administración para los que quiere habilitar una directiva de seguridad. Se abre **Directiva de seguridad**.

3.  En **Security policy** (Directiva de seguridad), seleccione los controles que quiere que supervise Security Center y sobre los que desea que proporcione recomendaciones mediante la selección de **On** (Activado).  Seleccione **Off** (Desactivado) si no desea que Security Center supervise ese control.

    ![Componentes de la directiva](./media/security-center-azure-policy/security-policy.png)

4. Seleccione **Guardar**.

## <a name="available-security-policy-definitions"></a>Definiciones de directivas de seguridad disponibles

Para comprender las definiciones de directiva que están disponibles en la directiva de seguridad predeterminada, consulte la tabla siguiente:

| Directiva | Lo que hace la directiva habilitada |
| --- | --- |
| Actualizaciones del sistema |Recupera una lista diaria de las actualizaciones críticas y de seguridad disponibles en Windows Update o Windows Server Update Services. La lista recuperada depende del servicio que está configurado para su máquina virtual y recomienda aplicar las actualizaciones que faltan. En los sistemas Linux, la directiva utiliza el sistema de administración de paquetes proporcionado por la distribución para determinar los paquetes que tienen actualizaciones disponibles. También comprueba las actualizaciones de seguridad y críticas de las máquinas virtuales de [Azure Cloud Services](../cloud-services/cloud-services-how-to-configure-portal.md). |
| Configuraciones de seguridad |Analiza las configuraciones del sistema operativo diariamente para determinar los problemas que podrían hacer que la máquina virtual sea vulnerable a ataques. La directiva también recomienda cambios en la configuración para afrontar estas vulnerabilidades. Consulte la [lista de líneas de base recomendadas](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335) para más información sobre las configuraciones específicas que se están supervisando. (En este momento, Windows Server 2016 no es totalmente compatible). |
| Endpoint Protection |Recomienda configurar Endpoint Protection para todas las máquinas virtuales (VM) Windows para facilitar la identificación y eliminación de virus, spyware y otro software malintencionado. |
| Cifrado de discos |Se recomienda habilitar el cifrado de disco en todas las máquinas virtuales para mejorar la protección de datos en reposo. |
| Grupos de seguridad de red |Recomienda configurar los [grupos de seguridad de red](../virtual-network/security-overview.md) para controlar el tráfico entrante y saliente de máquinas virtuales que tienen puntos de conexión públicos. Todas las interfaces de red de máquina virtual heredan los grupos de seguridad de red configurados para una subred, a menos que se especifique lo contrario. Además de comprobar que se ha configurado un grupo de seguridad de red, esta directiva evalúa las reglas de seguridad de entrada para identificar aquellas que permiten el tráfico entrante. |
| Firewall de aplicaciones web |Recomienda configurar un firewall de aplicaciones web en máquinas virtuales cuando se cumple una de las siguientes condiciones: <ul><li>Se usa una [dirección IP pública a nivel de instancia](../virtual-network/virtual-networks-instance-level-public-ip.md) y las reglas de seguridad de entrada del grupo de seguridad de red asociado se configuran para permitir el acceso al puerto 80/443.</li><li>Se usa una dirección IP de carga equilibrada y el equilibrio de carga asociado y las reglas de traducción de direcciones de red (NAT) entrantes están configuradas para permitir el acceso al puerto 80 o 443. Para más información, consulte [Compatibilidad de Azure Resource Manager con el equilibrador de carga](../load-balancer/load-balancer-arm.md).</li> |
| Firewall de próxima generación |Amplía las medidas de protección de la red más allá de los grupos de seguridad de red, que están integrados en Azure. Security Center detecta las implementaciones para las que se recomienda un firewall de próxima generación, y a continuación puede configurar una aplicación virtual. |
| Auditoría y detección de amenazas de SQL |Recomienda que se habilite la auditoría del acceso a Azure Database, con fines de cumplimiento, así como la detección avanzada de amenazas, con fines de investigación. |
| Cifrado de SQL |Recomienda que se habilite el cifrado en reposo para Azure SQL Database, las copias de seguridad asociadas y los archivos de registro de transacciones. Aunque haya infracción de datos, no se podrán leer. |
| Evaluación de vulnerabilidades |Se recomienda instalar una solución de evaluación de vulnerabilidades en la máquina virtual. |
| Cifrado de almacenamiento |Actualmente, esta característica está disponible para Azure Blob Storage y Azure Files. Después de habilitar el Cifrado del servicio Storage, solo se cifrarán los datos nuevos, mientras que los archivos existentes en esta cuenta de almacenamiento permanecerán sin cifrar. |
| Acceso de red JIT |Cuando el acceso a la red Just-In-Time está habilitado, Security Center bloquea el tráfico entrante a las máquinas virtuales de Azure mediante la creación de una regla de grupo de seguridad de red. Seleccione los puertos de la máquina virtual a los que se debe bloquear el tráfico entrante. Para más información, consulte [Administrar el acceso a máquina virtual mediante Just-In-Time](https://docs.microsoft.com/azure/security-center/security-center-just-in-time). |

## <a name="management-groups"></a>Grupos de administración
Si su organización tiene varias suscripciones, puede que necesite una manera de administrar el acceso, las directivas y el cumplimiento de esas suscripciones de forma eficaz. Los grupos de administración de Azure proporcionan un nivel de ámbito por encima de las suscripciones. Las suscripciones se organizan en contenedores llamados "grupos de administración" y aplican sus condiciones de gobierno a los grupos de administración. Todas las suscripciones dentro de un grupo de administración heredan automáticamente las directivas que se aplican al grupo de administración. Cada directorio tiene un grupo de administración de nivel superior único denominado "raíz". Este grupo de administración raíz está integrado en la jerarquía de manera que contiene todos los grupos de administración y suscripciones. Este grupo de administración raíz permite que las directivas globales y las asignaciones de control de acceso basado en rol (RBAC) se apliquen en el nivel de directorio. Para configurar grupos de administración para usarlos con Azure Security Center, siga las instrucciones que se describen en el artículo [Visibilidad de todos los inquilinos en Azure Security Center](security-center-management-groups.md). 

> [!NOTE]
> Es importante que comprenda la jerarquía de grupos de administración y suscripciones. Para más información sobre los grupos de administración, la administración raíz y el acceso a grupos de administración, consulte [Organización de recursos con grupos de administración de Azure](../governance/management-groups/index.md#root-management-group-for-each-directory).
>
>


## <a name="next-steps"></a>Pasos siguientes
En este artículo, ha aprendido a configurar directivas de seguridad en Security Center. Para más información sobre Security Center, consulte los siguientes artículos:

* [Guía de planeamiento y operaciones de Azure Security Center](security-center-planning-and-operations-guide.md): aprenda a planear y comprender las consideraciones de diseño sobre Azure Security Center.
* [Supervisión del estado de seguridad en Azure Security Center](security-center-monitoring.md): obtenga información sobre cómo supervisar el mantenimiento de los recursos de Azure.
* [Administración y respuesta a las alertas de seguridad en Azure Security Center](security-center-managing-and-responding-alerts.md): aprenda a administrar y responder a alertas de seguridad.
* [Supervisión de las soluciones de asociados con Azure Security Center](security-center-partner-solutions.md): aprenda a supervisar el estado de mantenimiento de las soluciones de asociados.
* [Visibilidad de todos los inquilinos en Azure Security Center](security-center-management-groups.md): aprenda a configurar grupos de administración para Azure Security Center. 
* [Preguntas frecuentes acerca de Azure Security Center](security-center-faq.md): obtenga respuestas a las preguntas más frecuentes sobre cómo usar el servicio.
* [Blog de seguridad de Azure](http://blogs.msdn.com/b/azuresecurity/): encuentre entradas de blog sobre el cumplimiento y la seguridad de Azure.

Para más información acerca de Azure Policy, consulte [¿Qué es Azure Policy?](../azure-policy/azure-policy-introduction.md)
