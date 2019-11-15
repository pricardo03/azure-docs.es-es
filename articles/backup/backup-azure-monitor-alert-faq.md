---
title: Preguntas frecuentes sobre las alertas de supervisión de Azure Backup
description: En este artículo, encontrará respuestas a preguntas habituales sobre los informes de Azure Backup Monitoring Alert y Azure Backup.
ms.reviewer: srinathv
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 07/08/2019
ms.author: dacurwin
ms.openlocfilehash: dc0225092d23371ca97dfedd48a2d3ffcf85a9be
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2019
ms.locfileid: "73747385"
---
# <a name="azure-backup-monitoring-alert---faq"></a>Preguntas frecuentes sobre las alertas de supervisión de Azure Backup

Este artículo contiene respuestas a algunas preguntas frecuentes sobre las alertas de supervisión de Azure.

## <a name="configure-azure-backup-reports"></a>Configuración de informes de Azure Backup

### <a name="how-do-i-check-if-reporting-data-has-started-flowing-into-a-storage-account"></a>¿Cómo se comprueba si los datos de informes han empezado a fluir en una cuenta de almacenamiento?

Puede ir a la cuenta de almacenamiento configurada y seleccione Containers. Si el contenedor tiene una entrada para insights-logs-azurebackupreport, esto indica que los datos de informes han empezado a fluir.

### <a name="what-is-the-frequency-of-data-push-to-a-storage-account-and-the-azure-backup-content-pack-in-power-bi"></a>¿Cuál es la frecuencia de inserción de datos en la cuenta de almacenamiento y en el paquete de contenido de Azure Backup en Power BI?

  Para los usuarios del día 0, la inserción de datos en la cuenta de almacenamiento tarda unas 24 horas aproximadamente. Cuando finalice esta inserción inicial, los datos se actualizan con la frecuencia mostrada en la ilustración siguiente.

* Los datos relacionados con **trabajos**, **alertas**, **elementos de copia de seguridad**, **almacenes**, **servidores protegidos** y **directivas** se insertan en la cuenta de almacenamiento del cliente cuando esta se registra.

* Los datos relacionados con **Storage** se insertan en la cuenta de almacenamiento del cliente cada 24 horas.

    ![Frecuencia de inserción de datos de informes de Azure Backup](./media/backup-azure-configure-reports/reports-data-refresh-cycle.png)

* Power BI tiene una [actualización programada una vez al día](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/#what-can-be-refreshed). Puede realizar una actualización manual de los datos en Power BI para el paquete de contenido.

### <a name="how-long-can-i-retain-reports"></a>¿Cuánto tiempo se pueden conservar los informes?

Al configurar una cuenta de almacenamiento, puede seleccionar un período de retención para los datos de informe en la cuenta de almacenamiento. Siga el paso 6 de la sección [Configuración de la cuenta de almacenamiento para los informes](backup-azure-configure-reports.md#configure-storage-account-for-reports). También puede [analizar informes en Excel](https://powerbi.microsoft.com/documentation/powerbi-service-analyze-in-excel/) y guardarlos durante un período de retención más prolongado, según sus necesidades.

### <a name="will-i-see-all-my-data-in-reports-after-i-configure-the-storage-account"></a>¿Aparecerán todos los datos en los informes después de configurar la cuenta de almacenamiento?

 Todos los datos generados después de configurar la cuenta de almacenamiento se insertarán en dicha cuenta y estarán disponibles en los informes. Los trabajos en curso no se insertan para los informes. Después de que el trabajo termina o se produce un error, se envía a los informes.

### <a name="if-i-already-configured-the-storage-account-to-view-reports-can-i-change-the-configuration-to-use-another-storage-account"></a>Si ya se ha configurado la cuenta de almacenamiento para ver los informes, ¿se puede cambiar la configuración para usar otra cuenta de almacenamiento?

Sí, puede cambiar la configuración para que se remita a una cuenta de Storage distinta. Use la cuenta de almacenamiento que acaba de configurar al conectarse al paquete de contenido de Azure Backup. Además, después de configurar una cuenta de Storage distinta, los nuevos datos fluyen a esta cuenta de almacenamiento. Los datos más antiguos (antes de cambiar la configuración) permanecen aún en la cuenta de almacenamiento anterior.

### <a name="can-i-view-reports-across-vaults-and-subscriptions"></a>¿Se pueden ver los informes en los almacenes y las suscripciones?

Sí, puede configurar la misma cuenta de Storage en varios almacenes para ver los informes entre ellos. Además, puede configurar la misma cuenta de Storage para almacenes a través de las suscripciones. Después, puede usar esta cuenta de almacenamiento al conectarse al paquete de contenido de Azure Backup en Power BI para ver los informes. La cuenta de almacenamiento seleccionada debe estar en la misma región que el almacén de Recovery Services.

### <a name="how-long-does-it-take-for-the-azure-backup-agent-job-status-to-reflect-in-the-portal"></a>¿Cuánto tarda el estado del trabajo del agente de copia de seguridad de Azure en reflejarse en el portal?

Azure Portal puede tardar hasta 15 minutos en reflejar el estado del trabajo del agente de copia de seguridad de Azure.

### <a name="when-a-backup-job-fails-how-long-does-it-take-to-raise-an-alert"></a>Cuando se produce un error en un trabajo de copia de seguridad, ¿cuánto tarda en generarse una alerta?

Se genera una alerta en menos de 20 minutos desde que se produce el error en la copia de seguridad de Azure.

### <a name="is-there-a-case-where-an-email-wont-be-sent-if-notifications-are-configured"></a>¿Hay algún caso en el que no se envíe ningún correo electrónico si se configuran las notificaciones?

Sí. En las siguientes situaciones no se envían notificaciones.

* Si se configuran las notificaciones por horas y una alerta se genera y se resuelve en menos de una hora
* Cuando se cancela un trabajo
* Si se produce un error en el segundo trabajo de copia de seguridad porque el original está en curso

## <a name="recovery-services-vault"></a>Almacén de Recovery Services

### <a name="how-long-does-it-take-for-the-azure-backup-agent-job-status-to-reflect-in-the-portal"></a>¿Cuánto tarda el estado del trabajo del agente de copia de seguridad de Azure en reflejarse en el portal?

Azure Portal puede tardar hasta 15 minutos en reflejar el estado del trabajo del agente de copia de seguridad de Azure.

### <a name="when-a-backup-job-fails-how-long-does-it-take-to-raise-an-alert"></a>Cuando se produce un error en un trabajo de copia de seguridad, ¿cuánto tarda en generarse una alerta?

Se genera una alerta en menos de 20 minutos desde que se produce el error en la copia de seguridad de Azure.

### <a name="is-there-a-case-where-an-email-wont-be-sent-if-notifications-are-configured"></a>¿Hay algún caso en el que no se envíe ningún correo electrónico si se configuran las notificaciones?

Sí. En las siguientes situaciones no se envían notificaciones:

* Si se configuran las notificaciones por horas y una alerta se genera y se resuelve en menos de una hora
* Cuando se cancela un trabajo
* Si se produce un error en el segundo trabajo de copia de seguridad porque el original está en curso

## <a name="next-steps"></a>Pasos siguientes

Lea las otras preguntas más frecuentes:

* [Preguntas comunes](backup-azure-vm-backup-faq.md) sobre las copias de seguridad de máquinas virtuales de Azure
* [Preguntas comunes](backup-azure-file-folder-backup-faq.md) sobre el agente de Azure Backup
