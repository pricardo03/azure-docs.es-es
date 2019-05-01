---
title: Configuración de herramientas de Azure Stream Analytics para Visual Studio
description: En este artículo se describen los requisitos de instalación de las herramientas de Azure Stream Analytics para Visual Studio y cómo configurarlas.
services: stream-analytics
author: su-jie
ms.author: sujie
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/22/2018
ms.openlocfilehash: 673f4935dce28b30c10e6abf4c7d22e00c1dd73a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60762242"
---
# <a name="install-azure-stream-analytics-tools-for-visual-studio"></a>Instalación de las herramientas de Azure Stream Analytics para Visual Studio
Las herramientas de Azure Stream Analytics son compatibles con Visual Studio 2017, 2015 y 2013. En este artículo se describe cómo instalar y desinstalar las herramientas.

Para más información sobre el uso de las herramientas, consulte [Uso de herramientas de Azure Stream Analytics para Visual Studio](stream-analytics-quick-create-vs.md).

## <a name="install"></a>Instalación
### <a name="recommended-visual-studio-2019-and-2017"></a>Se recomienda: Visual Studio 2019 y 2017
* Descargue [Visual Studio 2019 (versión preliminar 2 o posterior) y Visual Studio 2017 (15.3 o posterior)](https://www.visualstudio.com/). Se admiten las ediciones Enterprise (Ultimate y Premium), Professional y Community. No se admite la edición Express. No se admite Visual Studio 2017 en Mac. 
* Las herramientas de Stream Analytics forman parte de las cargas de trabajo de **almacenamiento y procesamiento de datos** y **desarrollo de Azure** en Visual Studio 2017. Habilite cualquiera de estas dos cargas como parte de la instalación de Visual Studio.

Habilite la carga de trabajo de **procesamiento y almacenamiento de datos** como se muestra:

![Se ha seleccionado la carga de trabajo de procesamiento y el almacenamiento de datos](./media/stream-analytics-tools-for-visual-studio-install/stream-analytics-tools-for-vs-2017-install-01.png)

Habilite la carga de trabajo de **desarrollo de Azure** como se muestra:

![Se ha seleccionado la carga de trabajo de desarrollo de Azure](./media/stream-analytics-tools-for-visual-studio-install/stream-analytics-tools-for-vs-2017-install-02.png)

* En el menú Herramientas, seleccione **Extensiones y actualizaciones**. Busque las herramientas de Azure Data Lake y Stream Analytics en las extensiones instaladas y haga clic en **Actualizar** para instalar la extensión más reciente. 

![Extensiones y actualizaciones de Visual Studio](./media/stream-analytics-tools-for-visual-studio-install/stream-analytics-tools-for-vs-extensions-updates.png)

### <a name="visual-studio-2015-2013"></a>Visual Studio 2015, 2013
* Instale Visual Studio 2015 o Visual Studio 2013 Update 4. Se admiten las ediciones Enterprise (Ultimate y Premium), Professional y Community. No se admite la edición Express. 
* Instale Microsoft Azure SDK para .NET versión 2.7.1 o superior con el [instalador de plataforma web](https://www.microsoft.com/web/downloads/platform.aspx).
* Instale [Herramientas de Azure Stream Analytics para Visual Studio](https://www.microsoft.com/en-us/download/details.aspx?id=49504).

## <a name="update"></a>Actualizar

### <a name="visual-studio-2019-and-2017"></a>Visual Studio 2019 y 2017
El recordatorio de la versión nueva aparece en la notificación de Visual Studio.

![Aviso de la nueva versión de Visual Studio](./media/stream-analytics-tools-for-visual-studio-install/stream-analytics-new-version-reminder-vs-tools.png)

### <a name="visual-studio-2015-and-2013"></a>Visual Studio 2015 y 2013
Las herramientas instaladas de Stream Analytics para Visual Studio buscan automáticamente nuevas versiones. Siga las instrucciones en la ventana emergente para instalar la última versión. 


## <a name="uninstall"></a>Desinstalación

### <a name="visual-studio-2019-and-2017"></a>Visual Studio 2019 y 2017
Haga doble clic en el instalador de Visual Studio y seleccione **Modificar**. Desactive la casilla **Herramientas de Azure Data Lake y Stream Analytics** en la carga de trabajo de **procesamiento y almacenamiento de datos** o de **desarrollo de Azure**.

### <a name="visual-studio-2015-and-2013"></a>Visual Studio 2015 y 2013
Vaya al Panel de control y desinstale **Herramientas de Microsoft Azure Data Lake y Stream Analytics**.





