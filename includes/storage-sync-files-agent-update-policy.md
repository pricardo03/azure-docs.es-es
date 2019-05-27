---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 12/11/2018
ms.author: tamram
ms.openlocfilehash: 4f59f68c1598f737ea7cb3a0e8046fc0779ed9d3
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66113803"
---
El agente de Azure File Sync se actualiza periódicamente con el fin de agregar nueva funcionalidad y solucionar los problemas. Se recomienda configurar Microsoft Update para obtener todas las actualizaciones del agente de Azure File Sync a medida que están disponibles.

#### <a name="major-vs-minor-agent-versions"></a>Versiones de agente principales y secundarias
* Con frecuencia, las versiones de agente principales contienen nuevas características y en la primera parte del número de versión tienen un número creciente. Por ejemplo: \*2.\*.\*\*
* Las versiones de agente secundarias se llaman también "revisiones" y se lanzan con más frecuencia que las principales. Suelen contener correcciones de errores y mejoras más pequeñas, pero no características nuevas. Por ejemplo: \*\*.3.\*\*

#### <a name="upgrade-paths"></a>Rutas de actualización
Existen cuatro métodos aprobados y probados de instalar las actualizaciones del agente de Azure File Sync. 
1. **(Preferida) Configurar Microsoft Update para descargar e instalar automáticamente las actualizaciones del agente.**  
    Se recomienda realizar siempre todas las actualizaciones de Azure File Sync para asegurarse de que tiene acceso a las correcciones más recientes del agente de servidor. Microsoft Update realiza este proceso íntegramente al descargar e instalar automáticamente estas actualizaciones.
2. **Use AfsUpdater.exe para descargar e instalar las actualizaciones del agente.**  
    AfsUpdater.exe se encuentra en el directorio de instalación del agente. Haga doble clic en el archivo ejecutable para descargar e instalar las actualizaciones del agente. 
3. **Aplicar revisiones a un agente de Azure File Sync existente mediante un archivo de revisiones de Microsoft Update o un archivo ejecutable .msp. La actualización más reciente de Azure File Sync se puede descargar del [Catálogo de Microsoft Update](https://www.catalog.update.microsoft.com/Search.aspx?q=Azure%20File%20Sync).**  
    Al ejecutar un archivo ejecutable .msp, se actualiza la instalación de Azure File Sync con el mismo método usado automáticamente por Microsoft Update en la ruta de actualización anterior. Cuando se aplica una revisión de Microsoft Update, se realiza una actualización local de una instalación de Azure File Sync.
4. **Descargar el instalador de agente de Azure File Sync más reciente desde el [Microsoft Download Center](https://go.microsoft.com/fwlink/?linkid=858257).**  
    Para actualizar una instalación existente del agente de Azure File Sync, desinstale la versión antigua e instale a continuación la versión más reciente del instalador descargado. El registro del servidor, los grupos de sincronización y cualquier otra configuración se mantienen durante la instalación de Azure File Sync.

#### <a name="automatic-agent-lifecycle-management"></a>Administración del ciclo de vida de agente automática
Con la versión 6 del agente, el equipo de sincronización de archivos ha introducido una característica de actualización automática del agente. Puede seleccionar cualquiera de los dos modos y especificar una ventana de mantenimiento en el que debe realizarse la actualización en el servidor. Esta característica está diseñada para ayudarle con la administración del ciclo de vida de agente proporcionando una barandilla protectora evitar que al agente de la expiración o sin problemas, lo que permite mantenerse la configuración actual.
1. El **configuración predeterminada** intentará evitar que el agente de la expiración. En los 21 días de la fecha de expiración registrado de un agente, el agente intentará actualizar automáticamente. Se iniciará un intento de actualizar una vez por semana en los 21 días antes de que expire y, en la ventana de mantenimiento seleccionado. **Esta opción no elimina la necesidad de realizar las revisiones regulares de Microsoft Update.**
2. Si lo desea, puede seleccionar que el agente se actualizará automáticamente propio tan pronto como esté disponible una nueva versión de agente. Esto también se producen durante la ventana de mantenimiento seleccionado y permitir que el servidor para beneficiarse de las nuevas características y mejoras en cuanto estén disponibles con carácter general. Esta es la configuración recomendada, preocupaciones que proporcionará versiones principales del agente, así como revisiones de actualizaciones regulares a su servidor.

#### <a name="agent-lifecycle-and-change-management-guarantees"></a>Garantías de ciclo de vida y administración de cambios del agente
Azure File Sync es un servicio en la nube, lo que introduce continuamente nuevas características y mejoras. Esto significa que una versión específica del agente de Azure File Sync solo se puede admitir por tiempo limitado. Para facilitar la implementación, las siguientes reglas de asegurarse de que tiene suficiente tiempo y notificación para dar cabida a actualizaciones del agente en el proceso de administración de cambios:

- Las versiones principales del agente se admiten durante seis meses como mínimo desde la fecha de lanzamiento inicial.
- Se garantiza que, al menos durante tres meses, el soporte técnico de las versiones principales del agente se solapa. 
- Se emiten advertencias para los servidores registrados mediante un agente "expirará pronto" al menos tres meses antes de la expiración. Puede comprobar si un servidor registrado usa una versión anterior del agente en la sección de servidores registrados de un servicio de sincronización de almacenamiento.
- La duración de una versión secundaria del agente está enlazada a la versión principal asociada. Por ejemplo, cuando se lance la versión 3.0 del agente, se establecerá que las versiones 2.\* expiren todas juntas.

> [!Note]
> Al instalarse una versión del agente con un aviso de expiración se mostrará una advertencia, pero la instalación se realizará. Al intentar instalar una versión expirada del agente, o conectarse a ella, no se admite y se bloqueará.
