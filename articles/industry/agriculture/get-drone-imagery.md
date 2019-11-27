---
title: Obtención de imágenes con drones
description: Describe cómo obtener imágenes con drones de los asociados
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: 6ee4391369744f966ce273697e5ba9e7f0590edd
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2019
ms.locfileid: "73890992"
---
# <a name="get-drone-imagery-from-drone-partners"></a>Obtención de imágenes con drones de asociados

En este artículo se describe cómo puede incorporar los datos del ortomosaico de las imágenes con drones de los asociados al centro de datos de Azure FarmBeats. Un ortomosaico es una imagen o ilustración aérea que se corrige geométricamente y se ensambla a partir de los datos recopilados por los drones.

Actualmente se admiten los siguientes partners de imágenes.

  ![Proyecto de FarmBeats](./media/get-drone-imagery-from-drone-partner/drone-partner-1.png)

La integración de datos de imágenes de dron con Azure FarmBeats ayuda a obtener datos del ortomosaico de los vuelos de dron que realiza en la granja en el centro de datos. Una vez que los datos están disponibles, puede verlos en el acelerador de FarmBeats y se pueden usar para la fusión de datos y la compilación de modelos de inteligencia artificial y aprendizaje automático.

## <a name="before-you-begin"></a>Antes de empezar

  - Asegúrese de que ha implementado Azure FarmBeats. Para la implementación, visite [Implementación de FarmBeats](prepare-for-deployment.md).
  - Asegúrese de que tiene la granja (para la que desea las imágenes de dron) definida en el sistema de FarmBeats.

## <a name="enable-drone-imagery-integration-with-farmbeats"></a>Habilitación de la integración de imágenes de dron con FarmBeats   

Debe proporcionar la siguiente información al proveedor de dispositivos para habilitar la integración con FarmBeats:  
 - Punto de conexión de API  
 - Id. de inquilino  
 - Id. de cliente  
 - Secreto del cliente  

Para ello, siga los pasos que se describen a continuación:

1. Descargue este [script](https://aka.ms/farmbeatspartnerscript) y extráigalo en la unidad local. Encontrará dos archivos en este archivo ZIP.  
2. Inicie sesión en [Azure Portal](https://portal.azure.com/) y abra Cloud Shell (esta opción está disponible en la barra superior derecha del portal).   

    ![Proyecto de FarmBeats](./media/get-drone-imagery-from-drone-partner/navigation-bar-1.png)

3. Asegurarse de que el entorno está establecido en **PowerShell**

    ![Proyecto de FarmBeats](./media/get-drone-imagery-from-drone-partner/power-shell-new-1.png)

4. Cargue los dos archivos que descargó (del paso 1 anterior) en Cloud Shell.  

    ![Proyecto de FarmBeats](./media/get-drone-imagery-from-drone-partner/power-shell-two-1.png)

5. Vaya al directorio donde se cargaron los archivos. (De forma predeterminada, se carga en el directorio principal > nombre de usuario).  
6. Ejecute el siguiente script:

    ```azurepowershell-interactive 

    ./generateCredentials.ps1   

    ```

7. Siga las instrucciones en pantalla para capturar los valores del punto de conexión de API, el identificador de inquilino, el identificador de cliente, el secreto de cliente y la cadena de conexión de EventHub.

    Cuando haya escrito las credenciales necesarias en el sistema de software de drones del asociado, podrá importar todas las granjas del sistema de FarmBeats y usar los detalles de la granja para planear la ruta de vuelo y recopilar imágenes con drones.

    Una vez que el software de los proveedores de los drones haya procesado las imágenes sin procesar, el sistema del software de drones carga el ortomosaico unido y otras imágenes procesadas en el centro de datos.

## <a name="view-drone-imagery"></a>Visualización de imágenes con drones

Una vez que los datos se hayan enviado al centro de datos de FarmBeats, debería poder consultar el almacén de escenas mediante las API del centro de datos de FarmBeats.

Como alternativa, debería poder ver la imagen de dron más reciente en la página **Farm Details** (Detalles de la granja). Para la visualización, siga estos pasos:  

1. Seleccione la granja en la que se han cargado las imágenes. Se mostrará la página de detalles de **Farm** (Granja).
2. Desplácese hacia abajo hasta la sección **Precision Maps** (Mapas de precisión).
3. Debe poder ver la imagen en la sección **Drone Imagery** (Imágenes de dron).

    ![Proyecto de FarmBeats](./media/get-drone-imagery-from-drone-partner/drone-imagery-1.png)

## <a name="download-drone-imagery"></a>Descarga de imágenes de dron

Al seleccionar la sección de imágenes de dron, se abre un elemento emergente que muestra una imagen de alta resolución del ortomosaico del dron.

![Proyecto de FarmBeats](./media/get-drone-imagery-from-drone-partner/download-drone-imagery-1.png)

## <a name="view-all-drone-maps"></a>Visualización de todos los mapas de dron

Los archivos y las imágenes cargados por el proveedor de drones aparecen en la sección Maps (Mapas). Seleccione la sección **Maps** (Mapas), filtre por **Farm** (Granja) y elija los archivos que desee ver y descargar:

  ![Proyecto de FarmBeats](./media/get-drone-imagery-from-drone-partner/view-drone-maps-1.png)

## <a name="next-steps"></a>Pasos siguientes

Aprenda a usar las [API](references-for-farmbeats.md#rest-api) del centro de datos de FarmBeats para obtener las imágenes de dron.
