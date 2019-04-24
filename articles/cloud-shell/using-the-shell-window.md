---
title: Usar la ventana Azure Cloud Shell | Microsoft Docs
description: Información general sobre cómo usar la ventana Azure Cloud Shell.
services: azure
documentationcenter: ''
author: maertendMSFT
manager: timlt
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 04/15/2019
ms.author: damaerte
ms.openlocfilehash: 2511f2c8fb706e232cde9ee4c02c7f8114bd3a2b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60200707"
---
# <a name="using-the-azure-cloud-shell-window"></a>Uso de la ventana Azure Cloud Shell

En este documento se explica cómo usar la ventana Cloud Shell.

## <a name="swap-between-bash-and-powershell-environments"></a>Intercambio entre los entornos Bash y PowerShell

Use el selector de entorno de la barra de herramientas de Cloud Shell para cambiar entre los entornos Bash y PowerShell.  
![Seleccionar entorno](media/using-the-shell-window/env-selector.png)

## <a name="restart-cloud-shell"></a>Reinicio de Cloud Shell
Haga clic en el icono de reinicio de la barra de herramientas de Cloud Shell para restablecer el estado de la máquina.  
![Reinicio de Cloud Shell](media/using-the-shell-window/restart.png)
> [!WARNING]
> Al reiniciar Cloud Shell se restablecerá el estado de la máquina y todos los archivos que no conserve el recurso compartido de archivos de Azure se perderán.

## <a name="change-the-text-size"></a>Cambiar el tamaño del texto
Haga clic en el icono de configuración que se encuentra en la esquina superior izquierda de la ventana y, luego, mantenga el puntero sobre la opción "Tamaño de texto" y seleccione el tamaño de texto que prefiera. La selección se conservará de una sesión a otra.
![Tamaño del texto](media/using-the-shell-window/text-size.png)

## <a name="change-the-font"></a>Cambiar la fuente
Haga clic en el icono de configuración en la parte superior izquierdo de la ventana y, a continuación, mantenga el mouse sobre la opción "Fuente" y seleccione la fuente deseado.  La selección se conservará de una sesión a otra.
![Font](media/using-the-shell-window/text-font.png)

## <a name="upload-and-download-files"></a>Carga y descarga de archivos
Haga clic en el icono de carga y descarga de archivos en la parte superior izquierdo de la ventana, a continuación, seleccione la carga o descarga.  
![Cargar/descargar archivos](media/using-the-shell-window/uploaddownload.png)
* Para cargar archivos, use el elemento emergente para buscar el archivo en el equipo local, seleccione el archivo que desee y haga clic en el botón "Abrir".  El archivo se cargará en el `/home/user` directory.
* Para descargar archivos, escriba la ruta de acceso completa del archivo en la ventana emergente y seleccione el botón "Descargar".  
> [!NOTE] 
> Los archivos y rutas de acceso de archivo distinguen mayúsculas de minúsculas en Cloud Shell. Compruebe el uso de mayúsculas y minúsculas en la ruta de acceso de archivo.

## <a name="open-another-cloud-shell-window"></a>Abrir otra ventana del Shell en la nube
Cloud Shell permite varias sesiones simultáneas en pestañas del explorador posibilitando que cada sesión exista como un proceso independiente.
Si sale de una sesión, asegúrese de salir de cada ventana de sesión, ya que cada proceso se ejecuta de forma independiente aunque se ejecuten en la misma máquina.  
Haga clic en el icono de la sesión abierta de nuevo en la parte superior izquierdo de la ventana. Se abrirá una nueva pestaña con otra sesión conectada al contenedor existente.
![Abrir nueva sesión](media/using-the-shell-window/newsession.png)

## <a name="cloud-shell-editor"></a>Editor de Cloud Shell
* Hacer referencia a la [con el editor de Azure Cloud Shell](using-cloud-shell-editor.md) página.

## <a name="web-preview"></a>Vista previa web
Haga clic en el icono de vista previa web en la parte superior izquierda de la ventana, seleccione "Configurar", especifique el puerto deseado al abrir.  Seleccione cualquier "puerto abierto" solo para abre el puerto, o "abrir y examinar" para abrir el puerto y el puerto en una nueva pestaña de vista previa.  
![Vista previa Web](media/using-the-shell-window/preview.png)  
<br>
![Configurar puerto](media/using-the-shell-window/preview-configure.png)  
Haga clic en el icono de vista previa web en la parte superior izquierdo de la ventana, seleccione "Vista previa de puerto..." Para obtener una vista previa de un puerto abierto en una nueva pestaña. Haga clic en el icono de vista previa web en la parte superior izquierdo de la ventana, seleccione "Cerrar puerto..." para cerrar el puerto abierto.  
![Vista previa o cerrar puerto](media/using-the-shell-window/preview-options.png)

## <a name="minimize--maximize-cloud-shell-window"></a>Minimizar y maximizar la ventana Cloud Shell
Haga clic en el icono Minimizar situado en la parte superior derecha de la ventana para ocultarla. Haga clic en el icono Cloud Shell de nuevo para mostrarla.
Haga clic en el icono Maximizar para establecer la ventana en la altura máxima. Para restablecer el tamaño anterior de la ventana, haga clic en Restaurar.  
![Minimizar o maximizar la ventana](media/using-the-shell-window/minmax.png)

## <a name="copy-and-paste"></a>Copiar y pegar
[!INCLUDE [copy-paste](../../includes/cloud-shell-copy-paste.md)]

## <a name="resize-cloud-shell-window"></a>Cambio del tamaño de la ventana Cloud Shell
Haga clic y arrastre el borde superior de la barra de herramientas hacia arriba o hacia abajo para cambiar el tamaño de la ventana Cloud Shell.

## <a name="scrolling-text-display"></a>Desplazamiento de la presentación del texto
Desplácese con el mouse o panel táctil para mover texto terminal.

## <a name="exit-command"></a>Comando Exit
Al ejecutar `exit` se terminará la sesión activa. Este comportamiento se produce de forma predeterminada después de 20 minutos sin interacción.

## <a name="next-steps"></a>Pasos siguientes

[Guía de inicio rápido de Bash en Cloud Shell](quickstart.md) <br>
[Guía de inicio rápido de PowerShell en Cloud Shell](quickstart-powershell.md)