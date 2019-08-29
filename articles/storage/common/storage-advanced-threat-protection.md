---
title: Advanced Threat Protection para Azure Storage
description: Configure Protección contra amenazas avanzada de Azure Storage para detectar anomalías en la actividad de la cuenta y enviarle notificaciones si hay intentos potencialmente dañinos de acceso a su cuenta.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 04/03/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.openlocfilehash: 2a88e778458da3c5faace401863998dda746ac75
ms.sourcegitcommit: bba811bd615077dc0610c7435e4513b184fbed19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2019
ms.locfileid: "70051496"
---
# <a name="advanced-threat-protection-for-azure-storage"></a>Advanced Threat Protection para Azure Storage

Advanced Threat Protection para Azure Storage proporciona un nivel adicional de inteligencia de seguridad que detecta intentos poco habituales y potencialmente peligrosos de acceder a las cuentas de almacenamiento o vulnerarlas. Esta capa de protección le permite afrontar las amenazas sin necesidad de ser un experto en seguridad ni administrar sistemas de supervisión de la seguridad. 

Las alertas de seguridad se desencadenan cuando se producen anomalías en una actividad.  Estas alertas de seguridad se integran en [Azure Security Center](https://azure.microsoft.com/services/security-center/) y también se envían por correo electrónico a los administradores de las suscripciones, con detalles de la actividad sospechosa y recomendaciones sobre cómo investigar y solucionar las amenazas.

> [!NOTE]
> * Advanced Threat Protection para Azure Storage está actualmente disponible solo para Blob Storage.
> * Para obtener más información, incluida una evaluación gratuita de 30 días, consulte la [página de precios de Azure Security Center]( https://azure.microsoft.com/pricing/details/security-center/).
> * La característica ATP para Azure Storage no está disponible actualmente en Azure Government ni en regiones de nube soberana.

Advanced Threat Protection de Azure Storage ingiere los registros de diagnóstico de las solicitudes de lectura, escritura y eliminación en Blob Storage para la detección de amenazas. Para investigar las alertas de Advanced Threat Protection, puede ver la actividad de almacenamiento relacionada mediante el registro de Storage Analytics. Para más información, vea cómo [configurar el registro de Storage Analytics](storage-monitor-storage-account.md#configure-logging).

## <a name="set-up-advanced-threat-protection"></a>Configuración de Advanced Threat Protection 

### <a name="using-the-portal"></a>Uso del portal

1. Inicie Azure Portal en [https://portal.azure.com](https://portal.azure.com/).

2. Vaya a la página de configuración de la cuenta de Azure Storage que desee proteger. En la página **Configuración**, seleccione **Protección contra amenazas avanzada**.

3. En la hoja de configuración de **Protección contra amenazas avanzada**
    * **Active** *Protección contra amenazas avanzada*
    * Haga clic en **Guardar** para guardar la directiva de Protección contra amenazas avanzada nueva o actualizada. (Los precios de la imagen son solo para los fines de este ejemplo).

![Activar Protección contra amenazas avanzada de Azure Storage](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-turn-on.png)

### <a name="using-azure-security-center"></a>Uso del Centro de seguridad de Azure

Cuando se suscribe al nivel Estándar en Azure Security Center, se configura automáticamente Advanced Threat Protection en las cuentas de almacenamiento. Puede habilitar o deshabilitar Advanced Threat Protection para las cuentas de almacenamiento en una suscripción específica de la manera siguiente:

1. Inicie **Azure Security Center** en [Azure Portal](https://portal.azure.com).
1. En el menú principal, haga clic en **Pricing & settings** (Precios y configuración).
1. Haga clic en la suscripción que desea habilitar o deshabilitar la protección contra amenazas para las cuentas de almacenamiento.

    ![Selección de la suscripción](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-subscription.png)

1. Haga clic en **Plan de tarifa**.
1. En la sección **Seleccione el plan de tarifa por tipo de recurso**, en la fila **Cuentas de almacenamiento**, haga clic en **Habilitado** o **Deshabilitado**.

    ![Habilitación de ATP en Security Center](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-pricing2.png)
1. Haga clic en **Save**(Guardar).

### <a name="using-azure-resource-manager-templates"></a>Uso de plantillas del Administrador de recursos de Azure

Use una plantilla de Azure Resource Manager para implementar una cuenta de Azure Storage con Advanced Threat Protection habilitada. Para obtener más información, consulte [Storage account with Advanced Threat Protection](https://azure.microsoft.com/resources/templates/201-storage-advanced-threat-protection-create/) (Cuenta de Storage con Advanced Threat Protection).

### <a name="using-azure-policy"></a>Uso de Azure Policy

Use una instancia de Azure Policy para habilitar Advanced Threat Protection en las cuentas de almacenamiento de un grupo de recursos o suscripción específicos.

1. Inicie la página **Directiva: Definiciones** de Azure.

1. Busque la directiva **Implementar Advanced Threat Protection en las cuentas de almacenamiento**.

     ![Búsqueda de directiva](./media/storage-advanced-threat-protection/storage-atp-policy-definitions.png)
  
1. Seleccione una suscripción a Azure o un grupo de recursos.

    ![Selección de suscripción o grupo](./media/storage-advanced-threat-protection/storage-atp-policy2.png)

1. Asigne la directiva.

    ![Página de definiciones de directiva](./media/storage-advanced-threat-protection/storage-atp-policy1.png)

### <a name="using-rest-api"></a>Uso de la API de REST
Use comandos de la API de REST para crear, actualizar u obtener la configuración de Advanced Threat Protection para una cuenta de almacenamiento específica.

* [Creación de Advanced Threat Protection](https://docs.microsoft.com/rest/api/securitycenter/advancedthreatprotection/create)
* [Obtención de Advanced Threat Protection](https://docs.microsoft.com/rest/api/securitycenter/advancedthreatprotection/get)

### <a name="using-azure-powershell"></a>Uso de Azure PowerShell

Use los siguientes cmdlets de PowerShell:

  * [Habilitación de Advanced Threat Protection](https://docs.microsoft.com/powershell/module/az.security/enable-azsecurityadvancedthreatprotection)
  * [Obtención de Advanced Threat Protection](https://docs.microsoft.com/powershell/module/az.security/get-azsecurityadvancedthreatprotection)
  * [Deshabilitación de Advanced Threat Protection](https://docs.microsoft.com/powershell/module/az.security/disable-azsecurityadvancedthreatprotection)

## <a name="explore-security-anomalies"></a>Exploración de anomalías de seguridad

Cuando se producen anomalías en la actividad de almacenamiento, recibe una notificación por correo electrónico con información acerca del evento de seguridad sospechoso. Los detalles del evento incluyen:

* La naturaleza de la anomalía
* El nombre de la cuenta de almacenamiento
* La hora del evento
* El tipo de almacenamiento
* Las causas posibles 
* Los pasos de investigación
* Los pasos para la corrección


El correo electrónico también incluye detalles acerca de las posibles causas y las medidas recomendadas para investigar y mitigar la potencial amenaza.

![Correo electrónico de alerta de Protección contra amenazas avanzada de Azure Storage](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-alert-email.png)

Las alertas de seguridad actuales se pueden revisar y administrar desde el [icono de alertas de seguridad](../../security-center/security-center-managing-and-responding-alerts.md) de Azure Security Center. Al hacer clic en una alerta específica se proporcionan detalles y acciones para investigar la amenaza actual y afrontar las amenazas futuras.

![Correo electrónico de alerta de Protección contra amenazas avanzada de Azure Storage](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-alert.png)

## <a name="protection-alerts"></a>Alertas de protección

Las alertas las generan los intentos inusuales y potencialmente dañinos de acceso o aprovechamiento de cuentas de almacenamiento. Para una lista de estas alertas, consulte las alertas de [Azure Storage](../../security-center/security-center-alerts-data-services.md#azure-storage).

## <a name="next-steps"></a>Pasos siguientes

* Más información acerca de [Registros en cuentas de Azure Storage](/rest/api/storageservices/About-Storage-Analytics-Logging)

* Más información acerca de [Azure Security Center](../../security-center/security-center-intro.md)
