---
title: Solución de problemas de acceso a Azure Functions Runtime
description: Aprenda a solucionar los problemas con una cuenta de almacenamiento no válida.
author: alexkarcher-msft
ms.topic: article
ms.date: 09/05/2018
ms.author: alkarche
ms.openlocfilehash: 910b582cb40b9f8aff6a553621b4677d6b019826
ms.sourcegitcommit: 42517355cc32890b1686de996c7913c98634e348
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2020
ms.locfileid: "76963893"
---
# <a name="how-to-troubleshoot-functions-runtime-is-unreachable"></a>Solución de problemas de acceso a Azure Functions Runtime

El objetivo de este artículo es solucionar los problemas relacionados con el mensaje de error que indica que no se puede acceder al entorno de ejecución de Azure Functions cuando este aparece en Azure Portal. Cuando se produce este error, aparece el siguiente mensaje de error en el portal.

`Error: Azure Functions Runtime is unreachable. Click here for details on storage configuration`

Este problema se produce cuando no se puede iniciar Azure Functions Runtime. La razón más común para que se produzca este error es que la aplicación de función pierde el acceso a su cuenta de almacenamiento. Para más información, consulte [Requisitos de la cuenta de almacenamiento](storage-considerations.md#storage-account-requirements).

Las demás secciones de este artículo le ayudarán a solucionar las siguientes causas del error, así como a identificar y a resolver cada caso.

+ [Eliminación de la cuenta de almacenamiento](#storage-account-deleted)
+ [Eliminación de la configuración de la aplicación de la cuenta de almacenamiento](#storage-account-application-settings-deleted)
+ [Credenciales de la cuenta de almacenamiento no válidas](#storage-account-credentials-invalid)
+ [Falta de acceso a la cuenta de almacenamiento](#storage-account-inaccessible)
+ [Cuota de ejecución diaria superada](#daily-execution-quota-full)
+ [La aplicación está detrás de un firewall](#app-is-behind-a-firewall)


## <a name="storage-account-deleted"></a>Eliminación de la cuenta de almacenamiento

Cada aplicación de función requiere una cuenta de almacenamiento para funcionar. Si esa cuenta se elimina, la función no funciona.

### <a name="how-to-find-your-storage-account"></a>Búsqueda de la cuenta de almacenamiento

Empiece por buscar el nombre de cuenta de almacenamiento en Configuración de la aplicación. `AzureWebJobsStorage` o `WEBSITE_CONTENTAZUREFILECONNECTIONSTRING` contendrán el nombre de la cuenta de almacenamiento incluida en una cadena de conexión. Lea información más específica en esta [referencia de configuración de aplicación](https://docs.microsoft.com/azure/azure-functions/functions-app-settings#azurewebjobsstorage).

Busque la cuenta de almacenamiento en Azure Portal para ver si sigue existiendo. Si se ha eliminado, deberá volver a crear una cuenta de almacenamiento y reemplazar las cadenas de conexión de almacenamiento. El código de función se pierde y debe volver a implementarlo.

## <a name="storage-account-application-settings-deleted"></a>Eliminación de la configuración de la aplicación de la cuenta de almacenamiento

En el paso anterior, si no tenía una cadena de conexión de la cuenta de almacenamiento, es probable que se eliminara o se sobrescribiera. La configuración de la aplicación se suele eliminar cuando se usan ranuras de implementación o scripts de Azure Resource Manager para la configuración de la aplicación.

### <a name="required-application-settings"></a>Configuración de aplicación necesaria

* Obligatorio
    * [`AzureWebJobsStorage`](https://docs.microsoft.com/azure/azure-functions/functions-app-settings#azurewebjobsstorage)
* Obligatorio para las funciones del plan de consumo
    * [`WEBSITE_CONTENTAZUREFILECONNECTIONSTRING`](https://docs.microsoft.com/azure/azure-functions/functions-app-settings)
    * [`WEBSITE_CONTENTSHARE`](https://docs.microsoft.com/azure/azure-functions/functions-app-settings)

[Más información sobre estos valores de configuración de la aplicación](https://docs.microsoft.com/azure/azure-functions/functions-app-settings).

### <a name="guidance"></a>Guía

* No seleccione "Configuración de ranuras" en ninguna de estas opciones. Si se intercambian las ranuras de implementación, la aplicación de funciones se interrumpe.
* No modifique esta configuración durante las implementaciones automatizadas.
* Esta configuración debe ser válida y proporcionarse en el momento de la creación. Si una implementación automatizada no tuviera esta configuración, la aplicación de funciones no se ejecutaría, ni siquiera aunque esta configuración se agregara más adelante.

## <a name="storage-account-credentials-invalid"></a>Credenciales de la cuenta de almacenamiento no válidas

Al volver a generar las claves de almacenamiento deben actualizarse las cadenas de conexión de la cuenta de almacenamiento anteriores. [Más información sobre la administración de las claves de almacenamiento](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account).

## <a name="storage-account-inaccessible"></a>Falta de acceso a la cuenta de almacenamiento

Es necesario que la aplicación de funciones pueda acceder a la cuenta de almacenamiento. Estos son los problemas comunes de bloqueo del acceso de Functions a la cuenta de almacenamiento:

+ Las aplicaciones de funciones están implementadas en instancias de App Service Environment sin las reglas de red adecuadas para permitir el tráfico de entrada y salida de la cuenta de almacenamiento.

+ El firewall de la cuenta de almacenamiento está habilitado, pero no está configurado para permitir el tráfico de entrada y salida de Functions. Para más información, consulte [Configuración de redes virtuales y firewalls de Azure Storage](../storage/common/storage-network-security.md).

## <a name="daily-execution-quota-full"></a>Se ha alcanzado la cuota de ejecución diaria

Si tiene configurada una cuota de ejecución diaria, la aplicación de funciones se deshabilitará temporalmente y muchos de los controles del portal dejarán de estar disponibles. 

+ Para comprobarlo en [Azure Portal](https://portal.azure.com), abra **Características de la plataforma** > **Configuración de aplicación de funciones** en la aplicación de funciones. Si supera el valor establecido en **Cuota de uso diario**, aparecerá el siguiente mensaje:

    `The function app has reached daily usage quota and has been stopped until the next 24 hours time frame.`

+ Para resolver este problema, quite o aumente la cuota diaria y reinicie la aplicación. De lo contrario, la ejecución de la aplicación se bloqueará hasta el día siguiente.

## <a name="app-is-behind-a-firewall"></a>Aplicación detrás de un firewall

El entorno de ejecución de la función no estará accesible si la aplicación de funciones está hospedada en una instancia de [App Service Environment con equilibro de carga interno](../app-service/environment/create-ilb-ase.md) y está configurada para bloquear el tráfico entrante de Internet o tiene [restricciones de IP de entrada](functions-networking-options.md#inbound-ip-restrictions) configuradas para bloquear el acceso a Internet. Azure Portal realiza llamadas directamente a la aplicación en ejecución para obtener la lista de funciones y también realiza llamadas HTTP a un punto de conexión de KUDU. La configuración de nivel de plataforma en la pestaña `Platform Features` seguirá estando disponible.

Para comprobar la configuración de ASE, vaya al grupo de seguridad de red de la subred donde se encuentra ASE y valide las reglas de entrada para permitir el tráfico procedente de la dirección IP pública del equipo desde el que se accede a la aplicación. También puede usar el portal desde un equipo conectado a la red virtual que ejecuta la aplicación o una máquina virtual que se ejecuta en la red virtual. [Obtenga más información sobre la configuración de reglas de entrada aquí](../app-service/environment/network-info.md#network-security-groups).

## <a name="next-steps"></a>Pasos siguientes

Más información sobre la supervisión de las aplicaciones de funciones:

> [!div class="nextstepaction"]
> [Monitor Azure Functions](functions-monitoring.md)
