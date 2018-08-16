---
title: Análisis de datos de sensor mediante Hive y Hadoop (Azure HDInsight)
description: Aprenda a analizar datos de sensor usando la Consola de consultas de Hive con HDInsight (Hadoop) y a visualizar los datos en Microsoft Excel con PowerView.
services: hdinsight
ms.service: hdinsight
author: jasonwhowell
ms.author: jasonh
editor: jasonwhowell
ms.topic: conceptual
ms.date: 04/14/2017
ROBOTS: NOINDEX
ms.openlocfilehash: 412942aa41e7884c6315d921b0b272b033386d17
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2018
ms.locfileid: "39590252"
---
# <a name="analyze-sensor-data-using-the-hive-query-console-on-hadoop-in-hdinsight"></a>Análisis de datos de sensor mediante la consola de consultas de Hive en Hadoop con HDInsight 

Aprenda a analizar datos de sensor usando la consola de consultas de Hive con HDInsight (Hadoop) y a visualizar los datos en Microsoft Excel con Power View.

> [!IMPORTANT]
> Los pasos de este tutorial solo se aplican a los clústeres de HDInsight basados en Windows. HDInsight solo está disponible en Windows en versiones inferiores a la 3.4. Linux es el único sistema operativo que se usa en la versión 3.4 de HDInsight, o en las superiores. Consulte la información sobre la [retirada de HDInsight en Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement).


En esta muestra, usa Hive para procesar datos históricos e identificar problemas con sistemas de calefacción y aire acondicionado. En concreto, se puedan identificar sistemas que no son capaces de mantener de forma confiable una temperatura fijada mediante la realización de las tareas siguientes:

* Crear tablas de HIVE para consultar datos almacenados en archivos de valores separados por comas (CSV).
* Crear consultas de HIVE para analizar los datos.
* Para recuperar los datos analizados, use Microsoft Excel para conectarse a HDInsight.
* Para visualizar los datos, use Power View.

![Diagrama de la arquitectura de la solución](./media/apache-hive-analyze-sensor-data/hvac-architecture.png)

## <a name="prerequisites"></a>Requisitos previos

* Un clúster de HDInsight (Hadoop): consulte [Creación de clústeres de Hadoop en HDInsight](../hdinsight-hadoop-provision-linux-clusters.md) para obtener información sobre la creación de un clúster.
* Microsoft Excel 2013

  > [!NOTE]
  > Microsoft Excel se usa para la visualización de datos con [Power View](https://support.office.com/Article/Power-View-Explore-visualize-and-present-your-data-98268d31-97e2-42aa-a52b-a68cf460472e?ui=en-US&rs=en-US&ad=US).

* [Microsoft Hive ODBC Driver](http://www.microsoft.com/download/details.aspx?id=40886)

## <a name="to-run-the-sample"></a>Para ejecutar el ejemplo

1. Desde el explorador web, navegue a la siguiente dirección URL: 

         https://<clustername>.azurehdinsight.net

    Reemplace `<clustername>` por el nombre del clúster de HDInsight.

    Cuando se le pida, autentíquese con el nombre de usuario y la contraseña de administrador que usó cuando aprovisionó este clúster.

2. En la página web que se abre, haga clic en la pestaña **Getting Started Gallery** (Galería de introducción) y, a continuación, en la categoría **Solutions with Sample Data** (Soluciones con datos de ejemplo), haga clic en el ejemplo **Análisis de datos del sensor**.

    ![Introducción a la galería de imágenes](./media/apache-hive-analyze-sensor-data/getting-started-gallery.png)

3. Siga las instrucciones que se proporcionan en la página web para finalizar el ejemplo.
