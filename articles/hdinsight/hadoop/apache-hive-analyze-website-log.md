---
title: 'Uso de Hive con Apache Hadoop para el análisis de registros de sitios web: Azure HDInsight'
description: Vea cómo usar Apache Hive con HDInsight para analizar registros de sitios web. Usará un archivo de registro como entrada en una tabla de HDInsight y HiveQL para consultar los datos.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/17/2016
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: 30bfaad8fcc1a837a37689280149a6dbe20b7c1d
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/19/2018
ms.locfileid: "53628158"
---
# <a name="use-apache-hive-with-windows-based-hdinsight-to-analyze-logs-from-websites"></a>Uso de Apache Hive con HDInsight basado en Windows para analizar registros de sitios web
Vea cómo usar HiveQL con HDInsight para analizar registros de un sitio web. El análisis de registros de sitios web se puede usar para segmentar su público en función de actividades parecidas, clasificar los visitantes a los sitios por datos demográficos, descubrir el contenido que ven, los sitios web de los que proceden, etc.

> [!IMPORTANT]  
> Los pasos de este tutorial solo se aplican a los clústeres de HDInsight basados en Windows. HDInsight solo está disponible en Windows en versiones inferiores a la 3.4. Linux es el único sistema operativo que se usa en la versión 3.4 de HDInsight, o en las superiores. Consulte la información sobre la [retirada de HDInsight en Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

En este ejemplo, usará un clúster de HDInsight para analizar archivos de registro de sitios web a fin de conocer la frecuencia de las visitas a estos en un día desde sitios web externos. También generará un resumen de errores de sitios web que experimentan los usuarios. Aprenderá a:

* Conectarse a una instancia de Azure Blob Storage, que contiene archivos de registro de sitios web.
* Crear tablas de HIVE para consultar esos registros.
* Crear consultas de HIVE para analizar los datos.
* Usar Microsoft Excel para conectarse a HDInsight (usando conectividad abierta de base de datos, ODBC) para recuperar los datos analizados.

![HDI.Samples.Website.Log.Analysis](./media/apache-hive-analyze-website-log/hdinsight-weblogs-sample.png)

## <a name="prerequisites"></a>Requisitos previos
* Debe haber aprovisionado un clúster de Apache Hadoop en Azure HDInsight. Para obtener instrucciones, consulte [Aprovisionamiento de clústeres de HDInsight](../hdinsight-hadoop-provision-linux-clusters.md).
* Debe tener instalado Microsoft Excel 2013 o Excel 2010.
* Debe tener [Microsoft Hive ODBC Driver para](https://www.microsoft.com/download/details.aspx?id=40886) importar datos de Hive en Excel.

## <a name="to-run-the-sample"></a>Para ejecutar el ejemplo
1. En [Azure Portal](https://portal.azure.com/), en el panel de inicio (si ancló el clúster allí), haga clic en el icono de clúster en el que quiera ejecutar el ejemplo.
2. En la hoja del clúster, en **Vínculos rápidos**, haga clic en el **panel del clúster** y luego, en la hoja **Panel de clúster**, haga clic en el **panel del clúster de HDInsight**. También puede abrir directamente el panel con la siguiente dirección URL:

         https://<clustername>.azurehdinsight.net

    Cuando se le pida, autentíquese con el nombre de usuario y la contraseña de administrador que usó cuando aprovisionó el clúster.
3. En la página web que se abre, haga clic en la pestaña **Galería de introducción** y, a continuación, en la categoría **Soluciones con datos de ejemplo**, haga clic en el ejemplo **Análisis de registro del sitio web**.
4. Siga las instrucciones que se proporcionan en la página web para finalizar el ejemplo.

## <a name="next-steps"></a>Pasos siguientes
Pruebe el siguiente ejemplo: [Análisis de datos de sensor usando Hive con HDInsight](apache-hive-analyze-sensor-data.md).

[hdinsight-sensor-data-sample]: ../hdinsight-use-hive-sensor-data-analysis.md
