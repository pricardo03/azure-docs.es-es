---
title: Uso de la CLI de Azure multiplataforma para crear alertas clásicas de los servicios de Azure | Microsoft Docs
description: Desencadene correos electrónicos o notificaciones o llamadas a direcciones URL de sitios web (webhooks) o a la automatización cuando se cumplen las condiciones especificadas.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 10/24/2016
ms.author: robb
ms.component: alerts
ms.openlocfilehash: 8112b868bc8d2ca2a9478d38ee702d8b3350d48e
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36287119"
---
# <a name="use-the-cross-platform-azure-cli-to-create-classic-metric-alerts-in-azure-monitor-for-azure-services"></a>Uso de la CLI de Azure multiplataforma para crear alertas de métricas clásicas en Azure Monitor para los servicios de Azure 

> [!div class="op_single_selector"]
> * [Portal](insights-alerts-portal.md)
> * [PowerShell](insights-alerts-powershell.md)
> * [CLI](insights-alerts-command-line-interface.md)
>
>
> [!NOTE]
> En este artículo se describe cómo crear alertas de métrica clásicas más antiguas. Azure Monitor admite ahora [nuevas y mejores alertas de métricas](monitoring-near-real-time-metric-alerts.md). Estas alertas pueden supervisar varias métricas y permiten alertas relacionadas con métricas de dimensiones. Próximamente estará disponible la compatibilidad de la CLI de Azure con alertas de métricas más recientes.
>
>

En este artículo se muestra cómo configurar alertas de métricas clásicas de Azure con la interfaz de la línea de comandos (CLI de Azure) multiplataforma.

> [!NOTE]
> Azure Monitor es el nuevo nombre de lo que se conocía como "Azure Insights" hasta el 25 de septiembre de 2016. Sin embargo, los espacios de nombres y, por tanto, los comandos que se describen aquí, aún contienen el término "insights".

Puede recibir una alerta basada en métricas de los servicios de Azure o basadas en eventos que se producen en Azure.

* **Valores de métrica**: la alerta se desencadena cuando el valor de una métrica específica cruza un umbral asignado en cualquier dirección. Es decir, se desencadena tanto la primera vez que se cumple la condición como después, cuando esa condición ya deja de cumplirse.    

* **Eventos de registro de actividades**: una alerta puede desencadenarse con *cada* evento o cuando se producen ciertos eventos concretos. Para más información acerca de los registros de actividad, consulte [Creación de alertas del registro de actividad (clásica)](monitoring-activity-log-alerts.md). 

Puede configurar una alerta de métrica clásica para hacer lo siguiente cuando se desencadena:

* Enviar notificaciones de correo electrónico al administrador y los coadministradores del servicio. 
* Enviar un correo electrónico a direcciones de correo electrónico que especifique.
* Llamar a un webhook.
* Iniciar la ejecución de un runbook de Azure (en este momento, solo desde Azure Portal).

Puede obtener información sobre las reglas de alerta de métricas clásicas y configurarlas mediante: 

* [Portal de Azure](insights-alerts-portal.md)
* [PowerShell](insights-alerts-powershell.md)
* [CLI de Azure](insights-alerts-command-line-interface.md)
* [API de REST de Azure Monitor](https://msdn.microsoft.com/library/azure/dn931945.aspx)

También puede obtener ayuda sobre los comandos, escribiendo un comando con **-help** al final. El siguiente es un ejemplo: 

```console
 azure insights alerts -help
 azure insights alerts actions email create -help
 ```

## <a name="create-alert-rules-by-using-azure-cli"></a>Creación de reglas de alerta mediante la CLI de Azure
1. Después de instalar los requisitos previos, inicie sesión en Azure. Consulte los [ejemplos de CLI de Azure Monitor](insights-cli-samples.md) para los comandos que necesita para empezar a trabajar. Estos comandos le ayudarán a iniciar sesión, a ver la suscripción que usa y a prepararse para ejecutar comandos de Azure Monitor.

    ```console
    azure login
    azure account show
    azure config mode arm

    ```

2. Para ver las reglas existentes en un grupo de recursos, use el formato siguiente: 

   **azure insights alerts rule list** *[opciones] &lt;GrupoDeRecursos&gt;*

   ```console
   azure insights alerts rule list myresourcegroupname

   ```
3. Para crear una regla, primero debe contar con varios datos importantes.
    * El **identificador del recurso** para el que quiere establecer una alerta.
    * Las **definiciones de métricas** disponibles para ese recurso.

     Una forma de obtener el identificador de recurso es usar Azure Portal. Seleccione el recurso en el portal, suponiendo que ya existe. En la sección **Configuración** de la hoja siguiente, seleccione **Propiedades**. El campo **RESOURCE ID** está en la hoja siguiente. 
     
     También puede obtener el identificador de recurso mediante [Azure Resource Explorer](https://resources.azure.com/).

     A continuación se muestra un ejemplo de identificador de recurso de una aplicación web:

     ```console
     /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
     ```

     Para obtener una lista de las métricas disponibles y las unidades de esas métricas para el ejemplo de recurso anterior, use el siguiente comando de la CLI de Azure:  

     ```console
     azure insights metrics list /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename PT1M
     ```

     *PT1M* es la granularidad de la medida disponible (en intervalos de 1 minuto). El uso de distintas granularidades le brinda opciones de métricas diferentes.
     
4. Para crear una regla de alerta basada en métricas, use un comando con el siguiente formato:

    **azure insights alerts rule metric set***[opciones] &lt;nombreRegla&gt;&lt;ubicación&gt;&lt;grupoRecursos&gt;&lt;tamañoVentana&gt;&lt;operador&gt;&lt;umbral&gt;&lt;idRecursoObjetivo&gt;&lt;nombreMétrica&gt;&lt;horaInserciónOperador&gt;*

    En el ejemplo siguiente se configura una alerta en un recurso de sitio web. La alerta se desencadena cada vez que se recibe constantemente cualquier tráfico durante 5 minutos y nuevamente cuando no se recibe tráfico durante 5 minutos.

    ```console
    azure insights alerts rule metric set myrule eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total

    ```
5. Para crear un webhook o enviar un correo electrónico cuando se active una alerta de métrica clásica, debe crear antes el correo electrónico o el webhook. Cree la regla inmediatamente después. No se pueden asociar webhooks o mensajes de correo electrónico con reglas que ya haya creado.

    ```console
    azure insights alerts actions email create --customEmails myemail@contoso.com

    azure insights alerts actions webhook create https://www.contoso.com

    azure insights alerts rule metric set myrulewithwebhookandemail eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total
    ```

6. Observe una regla individual para comprobar que las alertas se crearon correctamente.

    ```console
    azure insights alerts rule list myresourcegroup --ruleName myrule
    ```
7. Para eliminar reglas, use un comando con el siguiente formato:

    **insights alerts rule delete** [opciones] &lt;grupoRecursos&gt;&lt;nombreRegla&gt;

    Estos comandos eliminan las reglas que se crearon anteriormente en este artículo.

    ```console
    azure insights alerts rule delete myresourcegroup myrule
    azure insights alerts rule delete myresourcegroup myrulewithwebhookandemail
    azure insights alerts rule delete myresourcegroup myActivityLogRule
    ```

## <a name="next-steps"></a>Pasos siguientes
* [Obtenga información general sobre la supervisión de Azure](monitoring-overview.md), incluidos los tipos de información que puede recopilar y supervisar.
* Obtenga más información sobre cómo [configurar webhooks en las alertas](insights-webhooks-alerts.md).
* Obtenga más información sobre la [configuración de alertas sobre los eventos de registro de actividad](monitoring-activity-log-alerts.md).
* Obtenga más información sobre los [runbooks de Azure Automation](../automation/automation-starting-a-runbook.md).
* Obtenga [información general sobre la colección de registros de diagnóstico](monitoring-overview-of-diagnostic-logs.md) para recopilar métricas detalladas de alta frecuencia para el servicio.
* Obtenga [información general sobre la colección de métricas](insights-how-to-customize-monitoring.md) para garantizar que el servicio está disponible y que responder adecuadamente.
