---
title: Avanzada de seguridad de los datos de IaaS en Azure Security Center | Microsoft Docs
description: " Obtenga información sobre cómo habilitar la seguridad de datos avanzados para IaaS en Azure Security Center. "
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: monhaber
ms.assetid: ba46c460-6ba7-48b2-a6a7-ec802dd4eec2
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/08/2019
ms.author: v-mohabe
ms.openlocfilehash: cfe633c5251842257a0bef5237ea6b80aeaf05e9
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65968590"
---
# <a name="advanced-data-security-for-sql-servers-on-iaas"></a>Seguridad avanzada de datos para servidores de SQL Server en IaaS
Seguridad avanzada de datos para servidores de SQL Server en IaaS es un paquete unificado para funcionalidades avanzadas de seguridad SQL. Actualmente incluye funcionalidad para presentar y mitigar posibles vulnerabilidades de la base de datos y para detectar actividades anómalas que podrían indicar una amenaza para la base de datos.

Esta oferta de IaaS SQL Server se basa en la misma tecnología fundamental que se usan en el [paquete de seguridad de datos avanzada de Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-advanced-data-security).


## <a name="overview"></a>Información general

Seguridad de datos avanzados (ADS) proporciona un conjunto de capacidades avanzadas de seguridad SQL, que consta de evaluación de vulnerabilidades y protección contra amenazas avanzada.

* [Evaluación de vulnerabilidades](https://docs.microsoft.com/azure/sql-database/sql-vulnerability-assessment) es un servicio fácil de configurar que puede detectar, realizar un seguimiento y ayudarle a corregir posibles puntos vulnerables de una base de datos. Proporciona visibilidad sobre el estado de seguridad y que incluye los pasos para resolver problemas de seguridad y mejorar sus fortifications de base de datos.
* [Protección contra amenazas avanzada](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-overview) detecta actividades anómalas que indican intentos inusuales y potencialmente dañinos de acceso o ataque a su servidor SQL server. Continuamente supervisa la base de datos para detectar actividades sospechosas y proporciona alertas de seguridad orientados a la acción de patrones de acceso de la base de datos anómalos. Estas alertas proporcionan los detalles de actividad sospechosa y las medidas recomendadas para investigar y mitigar la amenaza.

## <a name="get-started-with-ads-for-iaas"></a>Empezar a trabajar con anuncios para IaaS

Los pasos siguientes ayudarán a comenzar con anuncios de IaaS.

### <a name="set-up-ads-for-iaas"></a>Configuración de anuncios para IaaS

**Antes de comenzar**: Necesita un área de trabajo de Log Analytics para almacenar los registros de seguridad que se está analizados. Si no tiene uno, a continuación, puede crear uno fácilmente, como se explica en [crear un área de trabajo de Log Analytics en Azure portal](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace).

1. Conectar la máquina virtual que hospeda el servidor de SQL para el área de trabajo de Log Analytics. Para obtener instrucciones, consulte [equipos Windows conectarse a Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/agent-windows).

1. En Azure Marketplace, vaya a la [soluciones de seguridad avanzada de datos de SQL](https://ms.portal.azure.com/#create/Microsoft.SQLAdvancedDataSecurity).
(Puede encontrar mediante la opción de búsqueda de marketplace, como ver en la siguiente imagen.) El **seguridad avanzada de datos de SQL** abre la página.

    ![Seguridad de datos avanzados para IaaS](./media/security-center-advanced-iaas-data/sql-advanced-data-security.png)

1. Haga clic en **Create**(Crear). Se muestran los lugares de trabajo.

    ![Creación de seguridad avanzada de datos](./media/security-center-advanced-iaas-data/sql-advanced-data-create.png)

1. Seleccione el área de trabajo y haga clic en **crear**.

   ![Selección del área de trabajo](./media/security-center-advanced-iaas-data/sql-workspace.png)

1. Reinicie el [servidor SQL de la máquina virtual](https://docs.microsoft.com/sql/database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services?view=sql-server-2017).


## <a name="explore-and-investigate-security-alerts"></a>Explore e investigar alertas de seguridad

Puede ver y administrar las alertas de seguridad actual.

1. Haga clic en **Security Center** > **las alertas de seguridad**y haga clic en una alerta.

    ![Buscar la alerta](./media/security-center-advanced-iaas-data/find-alert.png)

1. Desde el **recursos atacados** columna, haga clic en un recurso que ha sido atacado.

1. Para ver detalles de alertas y acciones para investigar la amenaza actual y protección frente a amenazas futuras, desplácese hacia abajo el **información General** página y en el **los pasos de corrección** sección, haga clic en el  **PASOS de investigación** vínculo.

    ![Pasos para la corrección](./media/security-center-advanced-iaas-data/remediation-steps.png)

1. Para ver los registros que están asociados con la activación de la alerta, vaya a **las áreas de trabajo de análisis de registro** y realice los pasos siguientes:

     > [!NOTE]
     > Si **las áreas de trabajo de análisis de registro** no aparece en el menú izquierdo, haga clic en **todos los servicios**y busque **las áreas de trabajo de análisis de registro**.

    1. Asegúrese de que las columnas se muestran los **tarifa** y **WorkspaceID** columnas. (**Las áreas de trabajo de análisis de registro** > **Editar columnas**, agregar **tarifa** y **WorkspaceID**.)

     ![Editar columnas](./media/security-center-advanced-iaas-data/edit-columns.png)

    1. Haga clic en el área de trabajo que tiene los registros de alerta.

    1. En el **General** menú, haga clic en **registros**

    1. Haga clic en el ojo junto a **SQLAdvancedThreatProtection** tabla. Se muestran los registros.

     ![Ver registros](./media/security-center-advanced-iaas-data/view-logs.png)

## <a name="set-up-email-notification-for-atp-alerts"></a>Configurar notificación por correo electrónico para alertas de ATP 

Puede establecer una lista de destinatarios para recibir una notificación por correo electrónico cuando se generan alertas ASC. El correo electrónico contiene un vínculo directo a la alerta en Azure Security Center con todos los detalles pertinentes. 

1. Vaya a **Security Center** > **directiva de seguridad** y en la fila de la suscripción correspondiente, haga clic en **Editar configuración >**.

    ![Configuración de la suscripción](./media/security-center-advanced-iaas-data/subscription-settings.png)

1. Desde el **configuración** menú haga clic en **notificaciones por correo electrónico**. 
1. En el **dirección de correo electrónico** texto, escriba las direcciones de correo electrónico para recibir las notificaciones. Puede especificar más de una dirección de correo electrónico, separe las direcciones de correo electrónico con una coma (,).  Por ejemplo admin1@mycompany.com,admin2@mycompany.com,admin3@mycompany.com

      ![Configuración de correo electrónico](./media/security-center-advanced-iaas-data/email-settings.png)

1. En el **notificación por correo electrónico** configuración, establezca las siguientes opciones:
  
    * **Enviar notificación por correo electrónico para alertas de gravedad alta**: En lugar de enviar mensajes de correo electrónico para todas las alertas, enviar sólo para alertas de gravedad alta.
    * **También enviar notificaciones por correo electrónico a los propietarios de suscripción**:  Enviar notificaciones a los propietarios de suscripciones demasiado.

1. En la parte superior de la **notificaciones por correo electrónico** pantalla, haga clic en **guardar**.

  > [!NOTE]
  > Haga clic en **guardar** antes de cerrar la ventana o el nuevo **notificación por correo electrónico** configuración no se guardará.

## <a name="explore-vulnerability-assessment-reports"></a>Explorar los informes de evaluación de vulnerabilidad

El panel de evaluación de vulnerabilidad proporciona información general de los resultados de evaluación a través de todas las bases de datos. Puede ver la distribución de las bases de datos según la versión de SQL Server, junto con un resumen de error en lugar de pasar las bases de datos y un resumen general de comprobaciones de acuerdo con la distribución de riesgo con errores.

Puede ver los resultados de la evaluación de vulnerabilidad e informes directamente desde Log Analytics.

1. Navegue hasta el área de trabajo de Log Analytics con la solución de ADS.
1. Vaya a **soluciones** y seleccione el **evaluación de vulnerabilidad de SQL** solución.
1. En el **resumen** panel, haga clic en **Ver resumen** y seleccione su **informe de evaluación de vulnerabilidad de SQL**.

    ![Informe de evaluación de SQL](./media/security-center-advanced-iaas-data/ads-sql-server-1.png)

    Carga el panel de informe. Asegúrese de que la ventana de tiempo se establece como mínimo al **últimos 7 días** puesto que el análisis de evaluación de vulnerabilidad se ejecutan en las bases de datos según una programación fija de una vez por cada 7 días.

    ![Establecer los últimos 7 días](./media/security-center-advanced-iaas-data/ads-sql-server-2.png)

1. Para explorar en profundidad para obtener más detalles, haga clic en cualquiera de los elementos de panel. Por ejemplo:

   1. Haga clic en una comprobación de una vulnerabilidad en el **Failed comprueba resumen** sección para ver una tabla de Log Analytics con los resultados de esta comprobación en todas las bases de datos. Las que tienen los resultados se muestran primero.

   1. A continuación, haga clic en para ver los detalles de cada vulnerabilidad, incluidas la descripción de las vulnerabilidades impacto, estado, los riesgos asociados y los resultados reales en esta base de datos. También puede ver la consulta real que se ha ejecutado para llevar a cabo esta comprobación e información de corrección para solucionar esta vulnerabilidad.

    ![Selección del área de trabajo](./media/security-center-advanced-iaas-data/ads-sql-server-3.png)

    ![Selección del área de trabajo](./media/security-center-advanced-iaas-data/ads-sql-server-4.png)

1. Puede ejecutar cualquier consulta de Log Analytics en los datos de resultados de evaluación de vulnerabilidad para segmentar y desglosar los datos según sus necesidades.

## <a name="advanced-threat-protection-for-sql-servers-on-iaas-alerts"></a>Avanzada de protección contra amenazas para servidores SQL Server en IaaS alertas
Las alertas se generan con intentos inusuales y potencialmente dañinos de acceso o ataque a servidores SQL Server. Estos eventos pueden desencadenar las siguientes alertas:

### <a name="anomalous-access-pattern-alerts"></a>Alertas de patrón de acceso anómalos

* **Acceso desde una ubicación inusual:** esta alerta se desencadena cuando se produce un cambio en el patrón de acceso a SQL Server, donde alguien ha iniciado sesión en el servidor SQL Server desde una ubicación geográfica inusual. Causas posibles:
     * Un atacante o ex-República emplean malintencionado accedió a su servidor SQL Server.
     * Un usuario legítimo ha accedido su servidor SQL Server desde una ubicación nueva.
* **Acceso desde una aplicación potencialmente dañina**: su alerta se desencadena cuando una aplicación potencialmente dañina se utiliza para tener acceso a la base de datos. Causas posibles:
     * Un atacante intenta superar su SQL mediante herramientas comunes de ataque.
     * Una legítimo las pruebas de penetración en acción.
* **Acceso desde una entidad de seguridad desconocida**: esta alerta se desencadena cuando se produce un cambio en el patrón de acceso a SQL Server, donde alguien ha iniciado sesión en el servidor SQL Server mediante una entidad de seguridad inusual (usuario de SQL). Causas posibles:
     * Un atacante o ex-República emplean malintencionado accedió a su servidor SQL Server. 
     * Un usuario legítimo ha accedido desde SQL Server con una nueva entidad de.
* **Credenciales de SQL por fuerza bruta**: esta alerta se desencadena cuando hay un número anormalmente elevado de inicios de sesión infructuosos con distintas credenciales. Causas posibles:
     * Un atacante intenta superar su SQL mediante la fuerza bruta.
     * Una legítimo las pruebas de penetración en acción.

### <a name="potential-sql-injection-attacks-coming"></a>Posibles ataques de inyección de código SQL (entrante)

* **Vulnerabilidad a la inyección de código SQL**: esta alerta se desencadena cuando una aplicación genera una instrucción SQL errónea en la base de datos. Esta alerta puede indicar una posible vulnerabilidad a los ataques de inyección de código SQL. Causas posibles:
     * Existe un defecto en el código de la aplicación que crea la instrucción SQL errónea
     * El código de la aplicación o los procedimientos almacenados no corrigen los datos que proporciona el usuario al construir la instrucción SQL errónea, lo que se puede aprovechar para ataques por inyección de código SQL
* **Potencial inyección de código SQL**: esta alerta se desencadena cuando se produce una vulnerabilidad de seguridad activa contra una vulnerabilidad de la aplicación identificada ante inyección de código SQL. Esto significa que el atacante intentan inyectar instrucciones SQL malintencionadas mediante el código de la aplicación vulnerable o procedimientos almacenados.
