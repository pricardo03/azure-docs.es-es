---
title: Supervisión de amenazas en Azure Storage
description: Configure Protección contra amenazas avanzada de Azure Storage para detectar anomalías en la actividad de la cuenta y enviarle notificaciones si hay intentos potencialmente dañinos de acceso a su cuenta.
services: storage
author: rmatchoro
ms.service: storage
ms.topic: article
ms.date: 09/24/2018
ms.author: ronmat
ms.manager: shaik
ms.openlocfilehash: 8b2ca2d5d6418d68cab847df80fc437e468249ed
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46995657"
---
# <a name="azure-storage-advanced-threat-protection"></a>Protección contra amenazas avanzada de Azure Storage

Protección contra amenazas avanzada de Azure Storage detecta anomalías en la actividad de la cuenta y le envía notificaciones si hay intentos potencialmente dañinos de acceso a su cuenta. Esta capa de protección le permite afrontar las amenazas sin necesidad de ser un experto en seguridad ni administrar sistemas de supervisión de la seguridad.

Las amenazas se exponen mediante la definición de alertas de seguridad que se desencadenan cuando se producen anomalías en la actividad. Dichas alertas se integran con [Azure Security Center](https://azure.microsoft.com/services/security-center/), que incluye detalles acerca de cualquier actividad sospechosa y recomendaciones acerca de cómo investigar amenazas y mitigarlas. 

> [!NOTE]
> Protección contra amenazas avanzada de Azure Storage está actualmente disponible solo para Blob service. Las alertas de seguridad se integran con Azure Security Center y se envían por correo electrónico a los administradores de suscripciones.

Protección contra amenazas avanzada de Azure Storage ingiere los registros de diagnóstico de las solicitudes de lectura, escritura y eliminación a Blob service para la detección de amenazas. Para investigar las alertas de Protección contra amenazas avanzada, tiene que [configurar los registros de diagnóstico](storage-monitor-storage-account.md#configure-logging) para habilitar todos los niveles de los registros de Blob service.

## <a name="set-up-advanced-threat-protection-in-the-portal"></a>Configuración de Protección contra amenazas avanzada en el portal

1. Inicie Azure Portal en [https://portal.azure.com](https://portal.azure.com/).

2. Vaya a la página de configuración de la cuenta de Azure Storage que desee proteger. En la página **Configuración**, seleccione **Protección contra amenazas avanzada**.

3. En la hoja de configuración de **Protección contra amenazas avanzada**
    * **Active** *Protección contra amenazas avanzada*
    * Haga clic en **Guardar** para guardar la directiva de Protección contra amenazas avanzada nueva o actualizada.

![Activar Protección contra amenazas avanzada de Azure Storage](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-turn-on.png)

## <a name="explore-anomalies"></a>Exploración de anomalías

Cuando se producen anomalías en la actividad de almacenamiento, recibe una notificación por correo electrónico con información acerca del evento de seguridad sospechoso. Los detalles del evento incluyen:

* la naturaleza de la anomalía
* el nombre de cuenta de almacenamiento
* el tipo de almacenamiento
* la hora del evento

El correo electrónico también incluye detalles acerca de las posibles causas y las medidas recomendadas para investigar y mitigar la potencial amenaza.

![Correo electrónico de alerta de Protección contra amenazas avanzada de Azure Storage](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-alert-email.png)

Las alertas de seguridad actuales se pueden revisar y administrar desde el [icono de alertas de seguridad](../../security-center/security-center-managing-and-responding-alerts.md#managing-security-alerts) de Azure Security Center. Al hacer clic en una alerta específica se proporcionan detalles y acciones para investigar la amenaza actual y afrontar las amenazas futuras.

![Correo electrónico de alerta de Protección contra amenazas avanzada de Azure Storage](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-alert.png)

## <a name="protection-alerts"></a>Alertas de protección

Las alertas las generan los intentos inusuales y potencialmente dañinos de acceso o aprovechamiento de cuentas de almacenamiento. Estos eventos pueden desencadenar las siguientes alertas:

* **Acceso desde una ubicación inusual**: esta alerta se desencadena cuando se produce un cambio en el patrón de acceso a una cuenta de almacenamiento. Por ejemplo, cuando alguien ha accedido a una cuenta de almacenamiento desde una ubicación geográfica inusual. En algunos casos, la alerta detecta una acción legítima (una nueva aplicación o una operación de mantenimiento de un desarrollador). En otros casos, la alerta detecta una acción malintencionada (por ejemplo, un antiguo empleado, un atacante externo, etc.).

* **Extracción de datos inusual**: esta alerta se desencadena cuando se produce un cambio en el patrón de extracción de datos de una cuenta de almacenamiento. Por ejemplo, si alguien ha accedido a una cantidad inusual de datos en una cuenta de almacenamiento. En algunos casos, la alerta detecta una acción legítima (actividad de mantenimiento). En otros casos, la alerta detecta una acción malintencionada (filtración o vulneración de datos, transferencia no autorizada de datos).

* **Acceso anónimo inusual**: esta alerta se desencadena cuando se produce un cambio en el patrón de acceso a una cuenta de almacenamiento. Por ejemplo, suponga que alguien ha accedido de forma anónima a una cuenta de almacenamiento. En algunos casos, la alerta detecta un acceso legítimo mediante acceso de lectura público. En otros casos, la alerta detecta un acceso no autorizado que aprovecha el acceso de lectura público a un contenedor y sus blobs.

* **Eliminación inesperada:** esta alerta se desencadena cuando se producen una o varias operaciones de eliminación inesperadas en una cuenta de almacenamiento, según el análisis histórico de la cuenta de almacenamiento. Por ejemplo, suponga que alguien ha realizado una operación de *DeleteBlob* mediante una nueva aplicación y desde una nueva dirección IP. En algunos casos, la alerta detecta una acción legítima (el administrador usó otro explorador durante un viaje de negocios). En otros casos, la alerta detecta una acción malintencionada (un atacante que elimina datos). 
 
* **Cambio de permisos de acceso:** esta alerta se desencadena cuando se produce un cambio inesperado en el permiso de acceso a una cuenta de almacenamiento. Por ejemplo, suponga que alguien ha cambiado el permiso de acceso a una cuenta de almacenamiento mediante una nueva aplicación y desde una nueva dirección IP. En algunos casos, la alerta detecta una acción legítima (el administrador usó otro explorador durante un viaje de negocios). En otros casos, la alerta detecta una acción malintencionada (por ejemplo, que un atacante aumenta los privilegios de una cuenta a la que ha obtenido acceso). 

* **Cargar paquete de Servicio en la nube de Azure:** esta alerta se desencadena cuando se produce una carga inesperada de Servicio en la nube de Azure (archivo *.cspkg*) en una cuenta de almacenamiento. Por ejemplo, suponga que se ha cargado un archivo *.cspkg* desde una dirección IP nueva. En algunos casos, la alerta detecta una acción legítima. En otros, la alerta detecta una acción malintencionada (por ejemplo, se ha cargado un paquete de Servicio en la nube para preparar la implementación de un servicio malintencionado).    
   

## <a name="next-steps"></a>Pasos siguientes

* Más información acerca de [Registros en cuentas de Azure Storage](/rest/api/storageservices/About-Storage-Analytics-Logging)

* Más información acerca de [Azure Security Center](../../security-center/security-center-intro.md)