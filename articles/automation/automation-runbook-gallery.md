---
title: Galerías de runbooks y módulos para Azure Automation
description: Dispone de runbooks y módulos de Microsoft y la comunidad para instalarlos y usarlos en su entorno de Azure Automation.  En este artículo, se describe cómo acceder a estos recursos y contribuir sus runbooks a la galería.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 09/11/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 68eb3f3e5c568bb518251aca2a4e76932ce2eee5
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2019
ms.locfileid: "56416215"
---
# <a name="runbook-and-module-galleries-for-azure-automation"></a>Galerías de runbooks y módulos para Azure Automation

En lugar de crear sus propios runbooks y módulos en Azure Automation, dispone de acceso a escenarios ya compilados por Microsoft y la comunidad.

Puede obtener runbooks en la [Galería de Runbook](#runbooks-in-runbook-gallery) y módulos en la [Galería de PowerShell](#modules-in-powerShell-gallery).  También puede compartir los escenarios que desarrolle para contribuir a la comunidad; para ello, consulte [Adición de un runbook a la Galería de runbooks](automation-runbook-gallery.md#adding-a-runbook-to-the-runbook-gallery).

## <a name="runbooks-in-runbook-gallery"></a>Runbooks en la Galería de runbooks

La [Galería de runbooks](https://gallery.technet.microsoft.com/scriptcenter/site/search?f\[0\].Type=RootCategory&f\[0\].Value=WindowsAzure&f\[1\].Type=SubCategory&f\[1\].Value=WindowsAzure_automation&f\[1\].Text=Automation) ofrece diversos runbooks de Microsoft y la comunidad que se pueden importar a Azure Automation. Puede usar uno al descargarlo de la galería, que se hospeda en el [Centro de scripts de TechNet](https://gallery.technet.microsoft.com/scriptcenter/site/upload) o puede importar runbooks directamente desde la galería de Azure Portal.

Solo se puede importar directamente desde la Galería de Runbooks mediante Azure Portal. No se puede llevar a cabo esta función mediante Windows PowerShell.

> [!NOTE]
> Debería validar el contenido de los runbooks que obtenga de la Galería de runbooks y extremar el cuidado al instalarlos y ejecutarlos en un entorno de producción.

### <a name="to-import-a-runbook-from-the-runbook-gallery-with-the-azure-portal"></a>Para importar un runbook de la Galería de runbooks con el Portal de Azure

1. En Azure Portal, abra su cuenta de Automation.
2. En **Automatización de procesos**, haga clic en **Galería de runbooks**
3. Busque el elemento de la galería que desee y selecciónelo para ver sus detalles. En la parte izquierda puede especificar otros parámetros de búsqueda para el anunciante y el tipo.

   ![Examinar galería](media/automation-runbook-gallery/browse-gallery.png)

4. Haga clic en **Ver proyecto de origen** para ver el elemento en el [Centro de scripts de TechNet](https://gallery.technet.microsoft.com/).
5. Para importar un elemento, haga clic en él para ver sus detalles y después haga clic en el botón **Importar** .

   ![Botón Importar](media/automation-runbook-gallery/gallery-item-detail.png)

6. Si lo desea, cambie el nombre del runbook y después haga clic en **Aceptar** para importarlo.
7. El runbook aparecerá en la pestaña **Runbooks** para la cuenta de Automation.

### <a name="adding-a-runbook-to-the-runbook-gallery"></a>Adición de un runbook a la Galería de runbooks

Microsoft recomienda agregar a la Galería de Runbooks aquellos runbooks que piense que podrían ser útiles para otros clientes.  Puede agregar un runbook si [lo carga al Centro de scripts](https://gallery.technet.microsoft.com/site/upload) teniendo en cuenta los siguientes detalles.

* Necesita especificar *Windows Azure* como **categoría** y *Automation* como **subcategoría** para el runbook que se mostrará en el asistente.  
* La carga debe ser un archivo `.ps1` o `.graphrunbook` único.  Si el runbook requiere módulos, runbooks secundarios o recursos, debe enumerarlos en la descripción del envío y en la sección de comentarios del runbook.  Si tiene un escenario que requiere varios runbooks, cargue cada uno por separado e indique los nombres de los runbooks relacionados en cada una de sus descripciones. Asegúrese de usar las mismas etiquetas para que se muestren en la misma categoría. Un usuario tendrá que leer la descripción para saber que se requieren otros runbooks para que funcione el escenario.
* Agregue la etiqueta "GraphicalPS" si va a publicar un **runbook gráfico** (no en un flujo de trabajo gráfico).
* Inserte un fragmento de código de Flujo de trabajo de PowerShell o PowerShell en la descripción con el icono **Insertar sección de código** .
* El resumen de la descarga se mostrará en los resultados de la Galería de runbooks, por lo que debería proporcionar información detallada que ayude a un usuario a identificar la funcionalidad del runbook.
* Debería asignar entre una y tres de las etiquetas siguientes a la carga.  El runbook se mostrará en el asistente en las categorías que coincidan con sus etiquetas.  El asistente pasará por alto las etiquetas que no aparezcan en esta lista. Si no especifica ninguna etiqueta coincidente, el runbook se incluirá en la categoría Otros.
  
  * Copia de seguridad
  * Administración de la capacidad
  * Control de cambios
  * Cumplimiento normativo
  * Desarrollo y entornos de prueba
  * Recuperación ante desastres
  * Supervisión
  * Aplicación de revisiones
  * Aprovisionamiento
  * Corrección
  * Administración del ciclo de vida de VM

* Automation actualiza la galería una vez a la hora, por lo que no verá sus contribuciones de inmediato.

## <a name="modules-in-powershell-gallery"></a>Módulos en la Galería de PowerShell

Los módulos de PowerShell contienen cmdlets que puede usar en sus runbooks; los módulos existentes que se pueden instalar en Azure Automation están disponibles en la [Galería de PowerShell](https://www.powershellgallery.com). Puede iniciar esta galería desde Azure Portal e instalarlos directamente en Azure Automation o puede descargarlos e instalarlos de forma manual.  

### <a name="to-import-a-module-from-the-automation-module-gallery-with-the-azure-portal"></a>Para importar un módulo de la Galería de módulos de automatización con el Portal de Azure

1. En Azure Portal, abra su cuenta de Automation.
2. Seleccione **Módulos** en **Recursos compartidos** para abrir la lista de módulos.
3. Haga clic en **Examinar la galería** en la parte superior de la página.

   ![Galería de módulos](media/automation-runbook-gallery/modules-blade.png)

4. En la página **Examinar galería**, puede buscar por los campos siguientes:

   * Nombre de módulo
   * Etiquetas
   * Autor
   * Nombre de cmdlet/recurso de DSC

5. Busque un módulo que le interese y selecciónelo para ver sus detalles.  

   Al explorar un módulo específico, puede ver más información. Esta información incluye un vínculo a la Galería de PowerShell, las dependencias necesarias y todos los cmdlets o recursos de DSC que contiene el módulo.

   ![Detalles del módulo de PowerShell](media/automation-runbook-gallery/gallery-item-details-blade.png)

6. Para instalar el módulo directamente en Azure Automation, haga clic en el botón **Importar** .
7. Al hacer clic en el botón Importar, en el panel **Importar**, verá el nombre del módulo que se va a importar. Si están instaladas todas las dependencias, el botón **Aceptar** estará activo. Si faltan dependencias, debe importarlas antes de importar este módulo.
8. En la página **Importar**, haga clic en **Aceptar** para importar el módulo. Cuando Azure Automation importa un módulo en la cuenta, extrae metadatos sobre el módulo y los cmdlets. Esta acción puede tardar un par de minutos, ya que deben extraerse todas las actividades.
9.  Recibirá una notificación inicial indicando que el módulo se está implementado y otra notificación cuando se haya completado.
10. Una vez importado el módulo, puede ver las actividades disponibles. Puede usar sus recursos en los runbooks y la configuración de estado deseada.

> [!NOTE]
> Los módulos que solo admiten PowerShell Core no son compatibles con Azure Automation y no pueden importarse en Azure Portal, ni implementarse directamente desde la Galería de PowerShell.

## <a name="python-runbooks"></a>Runbooks de Python

Los runbooks de Python están disponibles en la [galería del Centro de scripts](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&f%5B0%5D.Value=WindowsAzure&f%5B1%5D.Type=ProgrammingLanguage&f%5B1%5D.Value=Python&f%5B1%5D.Text=Python&sortBy=Date&username=). Puede contribuir con runbooks de Python para la galería del Centro de scripts. Al hacerlo, asegúrese de agregar la etiqueta **Python** al cargar su contribución.

## <a name="requesting-a-runbook-or-module"></a>Solicitud de un runbook o módulo

Puede enviar solicitudes a [Voz de usuario](https://feedback.azure.com/forums/246290-azure-automation/).  Si necesita ayuda para escribir un runbook o se plantea preguntas acerca de PowerShell, publique una pregunta en nuestro [foro](https://social.msdn.microsoft.com/Forums/windowsazure/home?forum=azureautomation&filter=alltypes&sort=lastpostdesc).

## <a name="next-steps"></a>Pasos siguientes

* Para empezar a trabajar con runbooks, consulte el artículo sobre la [Administración de un runbook en Azure Automation](manage-runbooks.md)
* Para comprender las diferencias entre PowerShell y los flujos de trabajo de PowerShell para runbooks, consulte [Aprendizaje del flujo de trabajo de Windows PowerShell](automation-powershell-workflow.md)