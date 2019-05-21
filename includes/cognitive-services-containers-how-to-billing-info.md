---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 04/16/2019
ms.openlocfilehash: 05961f8dd2788179a15e6bfe094484cf4770067b
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65885044"
---
Las consultas en el contenedor se facturan con el plan de tarifa del recurso de Azure que se usa para `<ApiKey>`.

Los contenedores de Azure Cognitive Services no tienen licencia para ejecutarse si no están conectados al punto de conexión de facturación para las mediciones. Debe habilitar los contenedores para que comuniquen la información de facturación al punto de conexión de facturación en todo momento. Los contenedores de Cognitive Services no envían datos de los clientes (por ejemplo, la imagen o el texto que se está analizando) a Microsoft. 

### <a name="connect-to-azure"></a>Conexión a Azure

El contenedor necesita que se ejecuten los valores del argumento de facturación. Estos valores permiten al contenedor conectarse al punto de conexión de facturación. El contenedor informa sobre el uso cada 10 a 15 minutos. Si el contenedor no se conecta a Azure en la ventana de tiempo permitida, continuará ejecutándose pero no atenderá las consultas hasta que se restaure el punto de conexión de facturación. Se intenta 10 veces la conexión en el mismo intervalo de tiempo de 10 a 15 minutos. Si no se puede conectar con el punto de conexión de facturación en esos 10 intentos, el contenedor deja de ejecutarse. 

### <a name="billing-arguments"></a>Argumentos de facturación

Las tres opciones siguientes se deben especificar con valores válidos para que el comando `docker run` inicie el contenedor.

| Opción | DESCRIPCIÓN |
|--------|-------------|
| `ApiKey` | La clave de API del recurso de Cognitive Services usado para realizar un seguimiento de la información de facturación.<br/>El valor de esta opción debe establecerse en una clave de API del recurso aprovisionado especificado en `Billing`. |
| `Billing` | El punto de conexión del recurso de Cognitive Services usado para realizar el seguimiento de la información de facturación.<br/>El valor de esta opción debe establecerse en el URI del punto de conexión de un recurso aprovisionado de Azure.|
| `Eula` | Indica que ha aceptado la licencia del contenedor.<br/>El valor de esta opción debe establecerse en `accept`. |


