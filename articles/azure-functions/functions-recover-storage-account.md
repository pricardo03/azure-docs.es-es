---
title: Solución de problemas de acceso a Azure Functions Runtime
description: Aprenda a solucionar los problemas con una cuenta de almacenamiento no válida.
author: alexkarcher-msft
ms.topic: article
ms.date: 09/05/2018
ms.author: alkarche
ms.openlocfilehash: 212f10bd33479e5a9f7244d5b2090c0324f937c2
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74226758"
---
# <a name="how-to-troubleshoot-functions-runtime-is-unreachable"></a>Solución de problemas de acceso a Azure Functions Runtime


## <a name="error-text"></a>Texto del error
Este documento está pensado para solucionar el error siguiente que aparece en el portal de Functions.

`Error: Azure Functions Runtime is unreachable. Click here for details on storage configuration`

### <a name="summary"></a>Resumen
Este problema se produce cuando no se puede iniciar Azure Functions Runtime. La razón más común para que se produzca este error es que la aplicación de función pierde el acceso a su cuenta de almacenamiento. [Más información sobre los requisitos de la cuenta de almacenamiento](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal#storage-account-requirements)

### <a name="troubleshooting"></a>solución de problemas
Le guiaremos con los cuatro casos de error más comunes y le ayudaremos a identificar y resolver cada uno de ellos.

1. Eliminación de la cuenta de almacenamiento
1. Eliminación de la configuración de la aplicación de la cuenta de almacenamiento
1. Credenciales de la cuenta de almacenamiento no válidas
1. Falta de acceso a la cuenta de almacenamiento
1. Cuota de ejecución diaria total

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

[Más información sobre estos valores de configuración de la aplicación](https://docs.microsoft.com/azure/azure-functions/functions-app-settings)

### <a name="guidance"></a>Guía

* No busque esta configuración en la configuración de las ranuras. Al intercambiar ranuras de implementación, la función se interrumpe.
* No modifique esta configuración como parte de implementaciones automatizadas.
* Esta configuración debe ser válida y proporcionarse en el momento de la creación. Una implementación automatizada que no contiene esta configuración genera una aplicación no funcional, aunque esta se añada después.

## <a name="storage-account-credentials-invalid"></a>Credenciales de la cuenta de almacenamiento no válidas

Al volver a generar las claves de almacenamiento deben actualizarse las cadenas de conexión de la cuenta de almacenamiento anteriores. [Más información sobre la administración de las claves de almacenamiento](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account)

## <a name="storage-account-inaccessible"></a>Falta de acceso a la cuenta de almacenamiento

La aplicación de función debe poder acceder a la cuenta de almacenamiento. Estos son los problemas comunes de bloqueo del acceso de Functions a la cuenta de almacenamiento:

* Aplicaciones de función implementadas en entornos de App Service sin las reglas de red correctas para permitir el tráfico hacia la cuenta de almacenamiento y desde esta
* El firewall de la cuenta de almacenamiento está habilitado y no está configurado para permitir el tráfico hacia Functions y desde aquí. [Más información sobre la configuración del firewall de la cuenta de almacenamiento](https://docs.microsoft.com/azure/storage/common/storage-network-security?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)

## <a name="daily-execution-quota-full"></a>Cuota de ejecución diaria total

Si tiene una cuota de ejecución diaria configurada, la instancia de Function App se deshabilitará temporalmente y muchos de los controles del portal dejarán de estar disponibles. 

* Para comprobarlo, abra Características de la plataforma > Configuración de Function App en el portal. Verá el siguiente mensaje si supera la cuota
    * `The Function App has reached daily usage quota and has been stopped until the next 24 hours time frame.`
* Elimine la cuota y reinicie la aplicación para resolver el problema.

## <a name="next-steps"></a>Pasos siguientes

Ahora que volvemos a tener la aplicación de función y funciona, veamos las guías de inicio rápido y las referencias para los desarrolladores para volver a prepararlo todo de nuevo.

* [Creación de su primera función de Azure](functions-create-first-azure-function.md)  
  Comience de inmediato y cree su primera función mediante el inicio rápido de Azure Functions. 
* [Referencia para desarrolladores de Azure Functions](functions-reference.md)  
  Proporciona información técnica sobre el tiempo de ejecución de Azure Functions y una referencia para las funciones de codificación y la definición de enlaces y desencadenadores.
* [Prueba de Azure Functions](functions-test-a-function.md)  
  describe las diversas herramientas y técnicas para probar sus funciones.
* [How to scale Azure Functions](functions-scale.md)  
  Trata los planes de servicio disponibles con Azure Functions, incluido el plan de hospedaje de Consumo, y cómo elegir el plan adecuado. 
* [¿Qué es Azure App Service?](../app-service/overview.md)  
  Azure Functions aprovecha Azure App Service para obtener una funcionalidad básica como son las implementaciones, las variables de entorno y los diagnósticos. 
