---
title: Protección de las aplicaciones y las máquinas
description: En este documento se describen las recomendaciones de Security Center que le ayudan a proteger las máquinas virtuales, equipos, aplicaciones web y los entornos de App Service.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 47fa1f76-683d-4230-b4ed-d123fef9a3e8
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/20/2019
ms.author: memildin
ms.openlocfilehash: 4a6d733b490edd892136f6febcc90c29a5a865e1
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2019
ms.locfileid: "74766810"
---
# <a name="protect-your-machines-and-applications"></a>Protección de las aplicaciones y las máquinas
Cuando Security Center identifica posibles vulnerabilidades de seguridad, crea recomendaciones que le guían por el proceso de configurar los controles necesarios. 

En este artículo se explica la página **Proceso y aplicaciones** de la sección de seguridad de los recursos de Azure Security Center. También se describen algunas de las recomendaciones que verá ahí.

Para ver la lista completa de recomendaciones para los servicios de proceso y aplicaciones, consulte [Recomendaciones de proceso y aplicaciones](recommendations-compute-and-apps.md).

## <a name="view-the-security-of-your-compute-and-apps-resources"></a>Visualización de la seguridad de los recursos de procesos y aplicaciones

![Panel de Security Center](./media/security-center-virtual-machine-recommendations/overview.png)

Para ver el estado de los recursos de proceso y aplicaciones, seleccione **Proceso y aplicaciones** en **Recursos** en la barra lateral de Security Center. Las siguientes pestañas están disponibles:

* **Información general**: muestra las recomendaciones para todos los recursos de proceso y aplicaciones, así como su estado de seguridad actual. 

* [**Máquinas virtuales y equipos**](#vms-and-computers): muestra las recomendaciones para las máquinas virtuales y los equipos, así como el estado de seguridad actual de cada uno.

* [**Conjuntos de escala de las máquinas virtuales**](#vmscale-sets): muestra las recomendaciones para los conjuntos de escalado. 

* [**Cloud Services**](#cloud-services): muestra las recomendaciones para los roles web y de trabajo supervisados por Security Center.

* [**App Services**](#app-services): muestra las recomendaciones para los entornos de App Service y el estado de seguridad actual de cada uno.

* **Contenedores**: muestra las recomendaciones para los contenedores y la evaluación de seguridad de sus configuraciones.

* **Recursos de proceso**: muestra las recomendaciones para los recursos de proceso, como clústeres de Service Fabric y centros de eventos.

### <a name="whats-in-each-tab"></a>¿Qué hay en cada pestaña?

Cada pestaña consta de varias secciones y, en cada una de ellas, puede explorar en profundidad para ver detalles adicionales sobre el elemento mostrado.

En cada pestaña también verá recomendaciones para los recursos de interés en el entorno supervisado. La primera columna muestra la recomendación, la segunda el número total de recursos afectados y la tercera la gravedad del problema.

Cada recomendación tiene un conjunto de acciones que se podrán realizar una vez que la selecciona. Por ejemplo, si selecciona **Faltan las actualizaciones del sistema**, aparece el número de máquinas virtuales y equipos a los que les faltan revisiones y la gravedad de la actualización que falta.

> [!NOTE]
> Las recomendaciones de seguridad son las mismas que las de la página **Recomendaciones**, pero aquí están filtradas por el tipo de recurso específico que ha seleccionado. Para más información sobre cómo resolver las recomendaciones, consulte [Implementación de recomendaciones de seguridad en Azure Security Center](security-center-recommendations.md).
>

### <a name="vms-and-computers"></a>Máquinas virtuales y equipos
La sección de máquinas virtuales y equipos ofrece una visión general de todas las recomendaciones de seguridad para las máquinas virtuales y los equipos. Se incluyen cuatro tipos de máquinas:

![Equipos que no son de Azure](./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon1.png) Equipo que no es de Azure.

![Máquina virtual de Azure Resource Manager](./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon2.png) Máquina virtual de Azure Resource Manager.

![Máquina virtual de Azure clásico](./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon3.png) Máquina virtual de Azure clásico.

![Máquinas virtuales identificadas del área de trabajo](./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon4.png) Máquinas virtuales que se identifican solo desde el área de trabajo que forma parte de la suscripción visualizada. Aquí se incluyen las máquinas virtuales de otras suscripciones que informan al área de trabajo de esta suscripción y las máquinas virtuales que se instalaron con el agente directo de Operations Manager y no tienen un identificador de recurso.

El icono que aparece en cada recomendación le ayuda a identificar rápidamente las máquinas virtuales y los equipos que necesitan atención y el tipo de recomendación. También puede usar filtros para buscar en la lista por **tipo de recurso** y **gravedad**.

Para explorar en profundidad las recomendaciones de seguridad para cada máquina virtual, haga clic en la máquina virtual.
Aquí verá los detalles de seguridad de la máquina virtual o del equipo. En la parte inferior puede ver la acción recomendada y la gravedad de cada problema.
![Cloud Services](./media/security-center-virtual-machine-recommendations/recommendation-list.png)

### <a name="cloud-services"></a>Cloud Services
En el caso de los servicios en la nube, se crea una recomendación cuando la versión del sistema operativo está obsoleta.

![Servicios en la nube](./media/security-center-virtual-machine-recommendations/security-center-monitoring-fig1-new006-2017.png)

En un escenario en el que tenga una recomendación, siga los pasos de esta para actualizar el sistema operativo. Cuando haya alguna actualización disponible, aparecerá una alerta (roja o naranja, según la gravedad del problema). Para obtener una explicación completa de esta recomendación, haga clic en **Actualizar versión del sistema operativo** en la columna **Descripción**.

### <a name="app-services"></a>App Services
Para ver la información de App Service, debe estar en el plan de tarifa estándar de Security Center y habilitar App Service en su suscripción. Para instrucciones sobre cómo habilitar esta característica, consulte [Protección de App Service con Azure Security Center](security-center-app-services.md).


En **App Services**, encontrará una lista de los entornos de App Service y el resumen de su estado según la evaluación que realizó Security Center.

![App Services](./media/security-center-virtual-machine-recommendations/app-services.png)

Se muestran tres tipos de servicios de aplicación:

![Entorno de App Services](./media/security-center-virtual-machine-recommendations/ase.png) Entorno de App Services

![Aplicación web](./media/security-center-virtual-machine-recommendations/web-app.png) Aplicación web

![Aplicación de función](./media/security-center-virtual-machine-recommendations/function-app.png) Aplicación de función

Si selecciona una aplicación web, se abre una vista de resumen con tres pestañas:

   - **Recomendaciones**: en función de las evaluaciones realizadas por Security Center que mostraron errores.
   - **Evaluaciones aprobadas**: lista de las evaluaciones realizadas por Security Center que se aprobaron.
   - **Evaluaciones no disponibles**: lista de las evaluaciones que no se pudieron ejecutar debido a un error o en las que la recomendación no era adecuada para la instancia de App Service concreta.

   En **Recomendaciones** hay una lista de las recomendaciones para la aplicación web seleccionada y la gravedad de cada una.

   ![Recomendaciones de App Services](./media/security-center-virtual-machine-recommendations/app-services-rec.png)

Seleccione una recomendación para ver su descripción, una lista de los recursos con estados correctos e incorrectos y una lista de los recursos sin examinar.

   - En la columna **Evaluaciones aprobadas** hay una lista de las evaluaciones que se aprobaron. La gravedad de estas evaluaciones siempre aparece en verde.

   - Seleccione una evaluación aprobada en la lista para obtener su descripción, una lista de los recursos con estados correctos e incorrectos, y una lista de los recursos sin examinar. Hay una pestaña para los recursos con estado incorrecto, pero está siempre vacía ya que se trata de una evaluación aprobada.

### <a name="vmscale-sets">Conjuntos de escala de las máquinas virtuales</a>
Security Center detecta automáticamente si tiene conjuntos de escalado y recomienda instalar Microsoft Monitoring Agent en ellos.

Para la instalación de Microsoft Monitoring Agent: 

1. Seleccione la recomendación **Instalar el agente de supervisión en conjuntos de escalado de máquinas virtuales**. Obtiene una lista de conjuntos de escalado no supervisados.

1. Seleccione un conjunto de escalado en mal estado. Siga las instrucciones para instalar al agente de supervisión con un área de trabajo completada existente o cree una nueva. Asegúrese de establecer el [plan de tarifa](security-center-pricing.md) del área de trabajo si aún no está establecido.

   ![Instalación de MMA](./media/security-center-virtual-machine-recommendations/install-mms.png)

Para establecer nuevos conjuntos de escalado para instalar automáticamente Microsoft Monitoring Agent:
1. Vaya a Azure Policy y haga clic en **Definiciones**.

1. Busque la directiva **Implementar el agente de Log Analytics para conjuntos de escalado de máquinas virtuales de Windows** y haga clic en ella.

1. Haga clic en **Asignar**.

1. Elija el **ámbito** y el **área de trabajo de Log Analytics** y haga clic en **Asignar**.

Si desea configurar todos los conjuntos de escalado existentes para instalar Microsoft Monitoring Agent, en Azure Policy, vaya a **Corrección** y aplique la directiva existente en conjuntos de escalado existentes.


## <a name="next-steps"></a>Pasos siguientes
Para más información sobre las recomendaciones que se aplican a otros tipos de recursos de Azure, consulte los siguientes artículos:

* [Supervisión de identidad y acceso en Azure Security Center](security-center-identity-access.md)
* [Protección de las redes en Azure Security Center](security-center-network-recommendations.md)
* [Protección del servicio SQL de Azure en Azure Security Center](security-center-sql-service-recommendations.md)
