---
title: 'Inicio rápido: Uso de Apache Zeppelin para ejecutar consultas de Apache Hive en Azure HDInsight'
description: Con este inicio rápido aprenderá a usar Apache Zeppelin para ejecutar consultas de Apache Hive.
keywords: hdinsight,hadoop,hive,interactive query,LLAP
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: quickstart
ms.date: 05/06/2019
ms.author: hrasheed
ms.openlocfilehash: 1642c64b0b14c2e290aad689399b59d896660a28
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67056686"
---
# <a name="quickstart-execute-apache-hive-queries-in-azure-hdinsight-with-apache-zeppelin"></a>Inicio rápido: Uso de Apache Zeppelin para ejecutar consultas de Apache Hive en Azure HDInsight

Con este inicio rápido aprenderá a usar Apache Zeppelin para ejecutar consultas de [Apache Hive](https://hive.apache.org/) en Azure HDInsight. Los clústeres de HDInsight Interactive Query incluyen cuadernos de [Apache Zeppelin](https://zeppelin.apache.org/) con los que puede ejecutar consultas de Hive interactivas.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Un clúster de HDInsight Interactive Query. Consulte [Creación de un clúster](../hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster) para crear un clúster de HDInsight.  Asegúrese de elegir el tipo de clúster **Interactive Query**.

## <a name="create-an-apache-zeppelin-note"></a>Creación de una nota de Apache Zeppelin

1. Reemplace `CLUSTERNAME` por el nombre del clúster en la siguiente dirección URL`https://CLUSTERNAME.azurehdinsight.net/zeppelin`. Después, escriba la dirección URL en un explorador web.

2. Escriba el nombre de usuario y la contraseña de inicio de sesión del clúster. En la página de Zeppelin, puede crear una nota o abrir notas existentes. **HiveSample** contiene algunos ejemplos de consultas de Hive.  

    ![Zeppelin de HDInsight Interactive Query](./media/hdinsight-connect-hive-zeppelin/hdinsight-hive-zeppelin.png)

3. Seleccione **Create new note** (Crear una nota).

4. En el cuadro de diálogo **Create new note** (Crear una nota), escriba o seleccione los valores siguientes:

    - Nombre de la nota: Escriba un nombre para la nota.
    - Intérprete predeterminado: Seleccione **jdbc** en la lista desplegable.

5. Seleccione **Create Note** (Crear nota).

6. Escriba la siguiente consulta de Hive en la sección de código y, después, presione **Mayús + Entrar**:

    ```hive
    %jdbc(hive)
    show tables
    ```

    ![Zeppelin de HDInsight Interactive Query ejecuta una consulta](./media/hdinsight-connect-hive-zeppelin/hdinsight-hive-zeppelin-query.png)

    La instrucción **%jdbc(hive)** en la primera línea indica al bloc de notas que debe usar el intérprete JDBC de Hive.

    La consulta devolverá una tabla de Hive denominada **hivesampletable**.

    Aquí se muestran dos consultas adicionales de Hive que se pueden ejecutar en **hivesampletable**.

    ```hive
    %jdbc(hive)
    select * from hivesampletable limit 10

    %jdbc(hive)
    select ${group_name}, count(*) as total_count
    from hivesampletable
    group by ${group_name=market,market|deviceplatform|devicemake}
    limit ${total_count=10}
    ```

    Comparado con Hive tradicional, los resultados de la consulta se devuelven mucho más rápido.

## <a name="clean-up-resources"></a>Limpieza de recursos

Después de completar el inicio rápido, puede ser conveniente eliminar el clúster. Con HDInsight, los datos se almacenan en Azure Storage, por lo que puede eliminar un clúster de forma segura cuando no se esté usando. También se le cobrará por un clúster de HDInsight aunque no se esté usando. Como en muchas ocasiones los cargos por el clúster son mucho más elevados que los cargos por el almacenamiento, desde el punto de vista económico tiene sentido eliminar clústeres cuando no se estén usando.

Para eliminar un clúster, consulte [Eliminación de un clúster de HDInsight con el explorador, PowerShell o la CLI de Azure](../hdinsight-delete-cluster.md).

## <a name="next-steps"></a>Pasos siguientes

Con este artículo aprenderá a usar Apache Zeppelin para ejecutar consultas de Apache Hive en Azure HDInsight. Para más información sobre las consultas de Hive, el siguiente artículo le mostrará cómo ejecutar consultas con Visual Studio.

> [!div class="nextstepaction"]
> [Conectarse a Azure HDInsight y ejecutar consultas de Apache Hive con Herramientas de Data Lake para Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md)
