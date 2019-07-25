---
title: Prueba de las consultas de Azure Stream Analytics localmente con Visual Studio Code (versión preliminar)
description: En este artículo se describe cómo probar las consultas localmente con las herramientas de Azure Stream Analytics para Visual Studio Code.
ms.service: stream-analytics
author: su-jie
ms.author: sujie
ms.date: 05/15/2019
ms.topic: conceptual
ms.openlocfilehash: f477a0f99c3eaa82568d8188bfaae03818fb72dc
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65827956"
---
# <a name="test-stream-analytics-queries-locally-with-visual-studio-code"></a>Prueba de las consultas de Azure Stream Analytics localmente con Visual Studio Code

Puede usar las herramientas de Azure Stream Analytics para Visual Studio Code para probar los trabajos de Stream Analytics localmente con datos de ejemplo.

Use este [inicio rápido](quick-create-vs-code.md) para aprender a crear un trabajo de Stream Analytics mediante Visual Studio Code.

## <a name="run-queries-locally"></a>Ejecutar consultas de forma local

Puede usar la extensión de Azure Stream Analytics para Visual Studio Code para probar los trabajos de Stream Analytics localmente con datos de ejemplo.

1. Una vez creado el trabajo de Stream Analytics, use **CTRL + Mayús + P** para abrir la paleta de comandos. A continuación, escriba y seleccione **ASA: Agregar entrada**.

    ![Adición de una entrada de ASA en Visual Studio Code](./media/vscode-local-run/add-input.png)

2. Seleccione una **entrada local**.

    ![Adición de una entrada local de ASA en Visual Studio Code](./media/vscode-local-run/add-local-input.png)

3. Seleccione **+ Nueva entrada local**.

    ![Adición de una nueva entrada local de ASA en Visual Studio Code](./media/vscode-local-run/add-new-local-input.png)

4. Escriba el mismo alias de entrada que usó en la consulta.

    ![Adición de un nuevo alias de entrada local de ASA](./media/vscode-local-run/new-local-input-alias.png)

5. En el archivo **LocalInput_DefaultLocalStream.json**, escriba la ruta de acceso del archivo donde se encuentra el archivo de datos local.

    ![Entrada de la ruta de acceso de archivo local en Visual Studio](./media/vscode-local-run/local-file-path.png)

6. Vuelva al editor de consultas y seleccione **Ejecutar localmente**.

    ![Selección de la opción Ejecutar localmente en el editor de consultas](./media/vscode-local-run/run-locally.png)

## <a name="next-steps"></a>Pasos siguientes

* [Creación de un trabajo en la nube de Azure Stream Analytics en Visual Studio Code (versión preliminar)](quick-create-vs-code.md)

* [Exploración de trabajos de Azure Stream Analytics con Visual Studio Code (versión preliminar)](vscode-explore-jobs.md)
