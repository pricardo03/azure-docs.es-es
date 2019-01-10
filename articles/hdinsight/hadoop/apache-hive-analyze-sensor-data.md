---
title: 'Análisis de datos de sensor con Apache Hive y Apache Hadoop: Azure HDInsight'
description: Aprenda a analizar datos de sensor usando la consola de consultas de Apache Hive con HDInsight (Hadoop) y a visualizar los datos en Microsoft Excel con PowerView.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 04/14/2017
ROBOTS: NOINDEX
ms.openlocfilehash: b9c8f1af612c220534e45c5c66651f0ad8600826
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/19/2018
ms.locfileid: "53628193"
---
# <a name="analyze-sensor-data-using-the-apache-hive-query-console-on-apache-hadoop-in-hdinsight"></a>Análisis de datos de sensor mediante la consola de consultas de Apache Hive en Apache Hadoop con HDInsight

Aprenda a analizar datos de sensor usando la consola de consultas de Apache Hive con HDInsight (Apache Hadoop) y a visualizar los datos en Microsoft Excel con Power View.

> [!IMPORTANT]  
> Los pasos de este tutorial solo se aplican a los clústeres de HDInsight basados en Windows. HDInsight solo está disponible en Windows en versiones inferiores a la 3.4. Linux es el único sistema operativo que se usa en la versión 3.4 de HDInsight, o en las superiores. Consulte la información sobre la [retirada de HDInsight en Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement).


En esta muestra, usa Hive para procesar datos históricos e identificar problemas con sistemas de calefacción y aire acondicionado. En concreto, se puedan identificar sistemas que no son capaces de mantener de forma confiable una temperatura fijada mediante la realización de las tareas siguientes:

* Crear tablas de HIVE para consultar datos almacenados en archivos de valores separados por comas (CSV).
* Crear consultas de HIVE para analizar los datos.
* Para recuperar los datos analizados, use Microsoft Excel para conectarse a HDInsight.
* Para visualizar los datos, use Power View.

![Diagrama de la arquitectura de la solución](./media/apache-hive-analyze-sensor-data/hvac-architecture.png)

## <a name="prerequisites"></a>Requisitos previos

* Un clúster de HDInsight (Hadoop): Consulte [Create Apache Hadoop clusters in HDInsight](../hdinsight-hadoop-provision-linux-clusters.md) (Creación de clústeres de Apache Hadoop en HDInsight) para obtener información sobre cómo crear un clúster.
* Microsoft Excel 2013

  > [!NOTE]  
  > Microsoft Excel se usa para la visualización de datos con [Power View](https://support.office.com/Article/Power-View-Explore-visualize-and-present-your-data-98268d31-97e2-42aa-a52b-a68cf460472e?ui=en-US&rs=en-US&ad=US).

* [Microsoft Hive ODBC Driver](https://www.microsoft.com/download/details.aspx?id=40886)

## <a name="to-run-the-sample"></a>Para ejecutar el ejemplo

1. Desde el explorador web, navegue a la siguiente dirección URL: 

         https://<clustername>.azurehdinsight.net

    Reemplace `<clustername>` por el nombre del clúster de HDInsight.

    Cuando se le pida, autentíquese con el nombre de usuario y la contraseña de administrador que usó cuando aprovisionó este clúster.

2. En la página web que se abre, haga clic en la pestaña **Getting Started Gallery** (Galería de introducción) y, a continuación, en la categoría **Solutions with Sample Data** (Soluciones con datos de ejemplo), haga clic en el ejemplo **Análisis de datos del sensor**.

    ![Introducción a la galería de imágenes](./media/apache-hive-analyze-sensor-data/getting-started-gallery.png)

3. Siga las instrucciones que se proporcionan en la página web para finalizar el ejemplo.
