---
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 06/18/2019
ms.author: cephalin
ms.openlocfilehash: 0691b1a531ffebbb2c368bdb37dd4d8025fb4a4e
ms.sourcegitcommit: 55e0c33b84f2579b7aad48a420a21141854bc9e3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/19/2019
ms.locfileid: "69623703"
---
Cuando crea un clon de la configuración de otro espacio de implementación, la configuración clonada se puede editar. Algunos elementos de configuración siguen al contenido en los intercambios (no son específicos de la ranura), mientras que otros permanecen en la misma ranura después de este (específicos). Las listas siguientes muestran la configuración que cambia cuando se intercambian las ranuras.

**Configuraciones que se intercambian**:

* Configuración general: por ejemplo, versión de Framework, 32 o 64 bits, Web Sockets
* Configuración de la aplicación (puede configurarse para ajustarse a un espacio)
* Cadenas de conexión (puede configurarse para ajustarse a un espacio)
* Asignaciones de controlador
* Configuración de supervisión y diagnóstico
* Certificados públicos
* Contenido de WebJobs
* Conexiones híbridas *
* Integración de la red virtual *
* Puntos de conexión de servicio *
* Azure Content Delivery Network *

Se prevé que las características marcadas con un asterisco (*) se ajusten a la ranura. 

**Valores que no se intercambian**:

* Extremos de publicación
* Nombres de dominio personalizados
* Certificados privados y enlaces SSL
* Configuración de escala
* Programadores de WebJobs
* Restricciones de IP
* Always On
* Configuración del protocolo (HTTPS, versión de TLS, certificados de cliente)
* Configuración del registro de diagnóstico
* Uso compartido de recursos entre orígenes (CORS)

<!-- VNET and hybrid connections not yet sticky to slot -->