---
title: Advanced Threat Protection para Azure Storage
description: Configure Protección contra amenazas avanzada de Azure Storage para detectar anomalías en la actividad de la cuenta y enviarle notificaciones si hay intentos potencialmente dañinos de acceso a su cuenta.
services: storage
author: rmatchoro
ms.service: storage
ms.topic: article
ms.date: 04/03/2019
ms.author: monhaber
ms.manager: shaik
ms.openlocfilehash: c42867ff7aea2210f20a2cd2adb5c067b8f36c80
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64926501"
---
# <a name="advanced-threat-protection-for-azure-storage"></a>Advanced Threat Protection para Azure Storage

Advanced Threat Protection para Azure Storage proporciona un nivel adicional de inteligencia de seguridad que detecta intentos poco habituales y potencialmente peligrosos de acceder o vulnerar las cuentas de almacenamiento. Esta capa de protección le permite afrontar las amenazas sin necesidad de ser un experto en seguridad ni administrar sistemas de supervisión de la seguridad. 

Las alertas de seguridad se desencadenan cuando se producen anomalías en la actividad.  Estas alertas de seguridad se integran con [Azure Security Center](https://azure.microsoft.com/services/security-center/)y también se envían por correo electrónico a los administradores de suscripciones con detalles de actividad sospechosa y recomendaciones sobre cómo investigar y solucionar las amenazas.

> [!NOTE]
> * Protección contra amenazas avanzada de Azure Storage actualmente solo está disponible para el almacenamiento de blobs.
> * Para obtener más información, incluida una evaluación gratuita de 30 días, consulte el [página de precios de Azure Security Center]( https://azure.microsoft.com/pricing/details/security-center/).
> * ATP para la característica de almacenamiento de Azure actualmente no está disponible en Azure government y las regiones de nube soberana.

Protección contra amenazas avanzada para el almacenamiento de Azure ingiere los registros de diagnóstico de lectura, escritura y solicitudes de eliminación para el almacenamiento de blobs para la detección de amenazas. Para investigar las alertas de protección contra amenazas avanzada, puede ver la actividad de almacenamiento relacionada con el registro de Storage Analytics. Para obtener más información, vea cómo [configurar registro de Storage Analytics](storage-monitor-storage-account.md#configure-logging).

## <a name="set-up-advanced-threat-protection"></a>Configurar la protección contra amenazas avanzada 

### <a name="using-the-portal"></a>Uso del portal

1. Inicie Azure Portal en [https://portal.azure.com](https://portal.azure.com/).

2. Vaya a la página de configuración de la cuenta de Azure Storage que desee proteger. En la página **Configuración**, seleccione **Protección contra amenazas avanzada**.

3. En la hoja de configuración de **Protección contra amenazas avanzada**
    * **Active** *Protección contra amenazas avanzada*
    * Haga clic en **Guardar** para guardar la directiva de Protección contra amenazas avanzada nueva o actualizada. (Los precios en la imagen por ejemplo son solo con fines).

![Activar Protección contra amenazas avanzada de Azure Storage](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-turn-on.png)

### <a name="using-azure-security-center"></a>Uso del Centro de seguridad de Azure
Cuando se suscribe al nivel estándar en Azure Security Center, se configura protección contra amenazas avanzada en las cuentas de almacenamiento. Para obtener más información, consulte [actualizar al nivel estándar de Security Center para mejorar la seguridad](https://docs.microsoft.com/azure/security-center/security-center-pricing). (Los precios en la imagen por ejemplo son solo con fines).

![Nivel estándar en ASC](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-pricing.png)

### <a name="using-azure-resource-manager-templates"></a>Uso de plantillas del Administrador de recursos de Azure

Use una plantilla de Azure Resource Manager para implementar una cuenta de almacenamiento de Azure con la protección contra amenazas avanzada habilitada.
Para obtener más información, consulte [cuenta de almacenamiento con protección contra amenazas avanzada](https://azure.microsoft.com/resources/templates/201-storage-advanced-threat-protection-create/).

### <a name="using-azure-policy"></a>Mediante la directiva de Azure

Usar una directiva de Azure para habilitar la protección contra amenazas avanzada a través de las cuentas de almacenamiento en un grupo específico de recursos o suscripción.

1. Comer Azure **Policy - definiciones** página.

1. Busque el **implementar protección contra amenazas avanzada en las cuentas de almacenamiento** directiva.

     ![Directiva de búsqueda](./media/storage-advanced-threat-protection/storage-atp-policy-definitions.png)
  
1. Seleccione un grupo de recursos o suscripción de Azure.

    ![Seleccione la suscripción o grupo](./media/storage-advanced-threat-protection/storage-atp-policy2.png)

1. Asigne la directiva.

    ![Página de definiciones de directiva](./media/storage-advanced-threat-protection/storage-atp-policy1.png)

### <a name="using-rest-api"></a>Uso de la API de REST
Usar comandos de la API de Rest para crear, actualizar u obtener la configuración de protección contra amenazas avanzada para una cuenta de almacenamiento específica.

* [Creación de Advanced Threat Protection:](https://docs.microsoft.com/rest/api/securitycenter/advancedthreatprotection/create)
* [Obtenga protección avanzada de amenazas:](https://docs.microsoft.com/rest/api/securitycenter/advancedthreatprotection/get)

### <a name="using-azure-powershell"></a>Uso de Azure PowerShell

Use los siguientes cmdlets de PowerShell:

  * [Habilitar protección contra amenazas avanzada](https://docs.microsoft.com/powershell/module/az.security/enable-azsecurityadvancedthreatprotection)
  * [Obtener protección contra amenazas avanzada](https://docs.microsoft.com/powershell/module/az.security/get-azsecurityadvancedthreatprotection)
  * [Deshabilitar protección contra amenazas avanzada](https://docs.microsoft.com/powershell/module/az.security/disable-azsecurityadvancedthreatprotection)

## <a name="explore-security-anomalies"></a>Explore anomalías de seguridad

Cuando se producen anomalías en la actividad de almacenamiento, recibe una notificación por correo electrónico con información acerca del evento de seguridad sospechoso. Los detalles del evento incluyen:

* La naturaleza de la anomalía
* El nombre de la cuenta de almacenamiento
* La hora del evento
* El tipo de almacenamiento
* Las posibles causas 
* Los pasos de investigación
* Los pasos de corrección


El correo electrónico también incluye detalles acerca de las posibles causas y las medidas recomendadas para investigar y mitigar la potencial amenaza.

![Correo electrónico de alerta de Protección contra amenazas avanzada de Azure Storage](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-alert-email.png)

Las alertas de seguridad actuales se pueden revisar y administrar desde el [icono de alertas de seguridad](../../security-center/security-center-managing-and-responding-alerts.md#managing-security-alerts) de Azure Security Center. Al hacer clic en una alerta específica se proporcionan detalles y acciones para investigar la amenaza actual y afrontar las amenazas futuras.

![Correo electrónico de alerta de Protección contra amenazas avanzada de Azure Storage](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-alert.png)

## <a name="protection-alerts"></a>Alertas de protección

Las alertas las generan los intentos inusuales y potencialmente dañinos de acceso o aprovechamiento de cuentas de almacenamiento. Estos eventos pueden desencadenar las siguientes alertas:

### <a name="anomalous-access-pattern-alerts"></a>Alertas de patrón de acceso anómalos

* **Acceso desde una ubicación inusual**: Esta alerta se desencadena cuando se produce un cambio en el patrón de acceso a una cuenta de almacenamiento. Por ejemplo, cuando alguien ha accedido a una cuenta de almacenamiento desde una ubicación geográfica inusual.
Causas posibles:
   * Un atacante tiene acceso a la cuenta de almacenamiento
   * Un usuario legítimo ha accedido a tu cuenta de almacenamiento desde una ubicación nueva
 
* **Anomalías de la aplicación**: Esta alerta indica que una aplicación inusual accedió a esta cuenta de almacenamiento. Causas posibles:
   * Un atacante accedió a su cuenta de almacenamiento mediante una nueva aplicación.
   * Un usuario legítimo ha usado un nuevo explorador o aplicación para tener acceso a la cuenta de almacenamiento.

* **Acceso anónimo**: Esta alerta indica que hay un cambio en el patrón de acceso a una cuenta de almacenamiento. Por ejemplo, ha sido esta cuenta de acceso anónimo (es decir, sin autenticación), que se esperaba compara con el patrón de acceso reciente en esta cuenta.
Causas posibles:
   * Un atacante haya aprovechado el acceso de lectura público a un contenedor.
   * Un usuario legítimo o una aplicación ha usado el acceso de lectura público a un contenedor.

### <a name="anomalous-extractupload-alerts"></a>Alertas de extracción o carga anómalas

* **Exfiltración de datos**: Esta alerta indica que una cantidad inusualmente grande de datos se ha extraído en comparación con la actividad reciente de este contenedor de almacenamiento. Causas posibles:
   * Un atacante ha extraído una gran cantidad de datos de un contenedor. (Por ejemplo: exfiltración/vulneración de datos, la transferencia no autorizada de datos)
   * Un usuario legítimo o una aplicación ha extraído un importe inusual de los datos de un contenedor. (Por ejemplo: actividad de mantenimiento)

* **Eliminar inesperado**: Esta alerta indica que se ha producido una o varias operaciones de eliminación inesperada en una cuenta de almacenamiento, en comparación con la actividad reciente de esta cuenta. Causas posibles:
   * Un atacante ha eliminado los datos de la cuenta de almacenamiento.
   * Un usuario legítimo ha realizado una eliminación inusual.

* **Cargar paquete de servicio de nube de Azure**: Esta alerta indica que se ha cargado un paquete de servicio en la nube de Azure (archivo .cspkg) para una cuenta de almacenamiento de manera inusual, en comparación con la actividad reciente de esta cuenta. Causas posibles: 
   * Un atacante ha sido prepararse para implementar código malintencionado desde la cuenta de almacenamiento a un servicio de nube de Azure.
   * Un usuario legítimo ha preparando para una implementación de servicio legítimo.

### <a name="suspicious-storage-activities-alerts"></a>Alertas de actividades sospechosas de almacenamiento

* **Acceder a los cambios de permiso**: Esta alerta indica que se han cambiado los permisos de acceso de este contenedor de almacenamiento en un modo no habitual. Causas posibles: 
   * Un atacante ha cambiado los permisos del contenedor para debilitar su seguridad.
   * Un usuario legítimo ha cambiado los permisos del contenedor.

* **Obtener acceso a la inspección**: Esta alerta indica que los permisos de acceso de una cuenta de almacenamiento han inspeccionado en modo no habitual, en comparación con la actividad reciente de esta cuenta. Causas posibles: 
   * Un atacante que haya realizado reconocimiento para un ataque futuro.
   * Un usuario legítimo ha realizado mantenimiento en la cuenta de almacenamiento.

* **Exploración de datos**: Esta alerta indica que se han enumerado los blobs o contenedores en una cuenta de almacenamiento de manera inusual, en comparación con la actividad reciente de esta cuenta. Causas posibles: 
   * Un atacante que haya realizado reconocimiento para un ataque futuro.
   * Un usuario legítimo o lógica de la aplicación se han explorado los datos dentro de la cuenta de almacenamiento.






## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre [inicia sesión en cuentas de almacenamiento de Azure](/rest/api/storageservices/About-Storage-Analytics-Logging)

* Más información acerca de [Azure Security Center](../../security-center/security-center-intro.md)
