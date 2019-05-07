---
title: Explorar Azure Stream Analytics con Visual Studio Code (versión preliminar)
description: Este artículo muestra cómo exportar un trabajo de Azure Stream Analytics a un proyecto local, la lista de trabajos y las entidades job de vista y configurar una canalización de CI/CD para el trabajo de Stream Analytics.
ms.service: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.date: 05/06/2019
ms.topic: conceptual
ms.openlocfilehash: 640a81fcb94194e2e907e9339f016bd35e279fdd
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65079216"
---
# <a name="explore-azure-stream-analytics-with-visual-studio-code-preview"></a>Explorar Azure Stream Analytics con Visual Studio Code (versión preliminar)

Azure Stream Analytics para la extensión de Visual Studio Code ofrece a los desarrolladores una experiencia ligera para administrar sus trabajos de Stream Analytics. Con la extensión de Azure Stream Analytics, hacer lo siguiente:

- [Crear](quick-create-vs-code.md), iniciar y detener trabajos
- Exportar los trabajos existentes a un proyecto local
- Ejecutar consultas de forma local
- Configurar una canalización de CI/CD

## <a name="export-a-job-to-a-local-project"></a>Exportar un trabajo a un proyecto local

Para exportar un trabajo a un proyecto local, busque el trabajo que desea exportar en el **Stream Analytics Explorer** en Visual Studio code. A continuación, seleccione una carpeta para el proyecto. El proyecto se exporta a la carpeta que seleccione, y puede seguir administrar el trabajo desde Visual Studio Code. Para obtener más información sobre el uso de código de Visual Studio para administrar los trabajos de Stream Analytics, consulte el código de Visual Studio [quickstart](quick-create-vs-code.md).

![Exportar trabajo de ASA en Visual Studio Code](./media/vs-code-how-to/export-job.png)

## <a name="run-queries-locally"></a>Ejecutar consultas de forma local

Puede usar la extensión de Azure Stream Analytics para Visual Studio Code para probar los trabajos de Stream Analytics localmente con datos de ejemplo.

1. Una vez que ha creado el trabajo de Stream Analytics, use **Ctrl + Mayús + P** para abrir la paleta de comandos. A continuación, escriba y seleccione **ASA: Agregar entrada**.

    ![Agregar entrada de ASA en Visual Studio code](./media/vs-code-how-to/add-input.png)

2. Seleccione **entrada Local**.

    ![Agregar entrada local de ASA en Visual Studio code](./media/vs-code-how-to/add-local-input.png)

3. Seleccione **+ nueva entrada Local**.

    ![Agregar un nuevo ASA local de entrada en el código de Visual Studio](./media/vs-code-how-to/add-new-local-input.png)

4. Escriba el mismo alias de entrada que usó en la consulta.

    ![Agregar un nuevo alias de entrada local de ASA](./media/vs-code-how-to/new-local-input-alias.png)

5. En el **LocalInput_DefaultLocalStream.json** de archivo, escriba la ruta de acceso del archivo donde se encuentra el archivo de datos local.

    ![Escriba la ruta de acceso local en Visual Studio](./media/vs-code-how-to/local-file-path.png)

6. Vuelva al editor de consultas y seleccione **ejecutar localmente**.

    ![Seleccione ejecutar localmente en el editor de consultas](./media/vs-code-how-to/run-locally.png)

## <a name="set-up-a-cicd-pipeline"></a>Configurar una canalización de CI/CD

Puede habilitar la integración continua e implementación para los trabajos de Azure Stream Analytics mediante el **asa cicd herramientas** paquete NPM. El paquete de NPM proporciona las herramientas para la implementación automática de proyectos de Visual Studio Code de Stream Analytics. Se puede usar en Windows, macOS y Linux sin necesidad de instalar Visual Studio Code.

Una vez que tenga [descargado el paquete](https://usqldownload.blob.core.windows.net/ext/asa/asa-cicd-0.0.1-preview-beta.tar), use el siguiente comando para generar las plantillas de Azure Resource Manager. Si el **outputPath** no se especifica, las plantillas se colocarán en el **implementar** carpeta bajo el proyecto **bin** carpeta.

```powershell
asa-cicd build -scriptPath <scriptFullPath> -outputPath <outputPath>
```

## <a name="next-steps"></a>Pasos siguientes

* [Crear un trabajo en la nube de Azure Stream Analytics en Visual Studio Code (versión preliminar)](quick-create-vs-code.md)