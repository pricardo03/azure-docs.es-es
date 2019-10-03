---
title: 'Habilitación de la creación automática de temas en Apache Kafka: Azure HDInsight'
description: Obtenga información sobre cómo configurar Apache Kafka en HDInsight para crear automáticamente los temas. Puede configurar Kafka estableciendo auto.create.topics.enable en true con Ambari o durante la creación del clúster mediante las plantillas de Resource Manager o PowerShell.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/18/2018
ms.openlocfilehash: 393087f4d5c5e7a52fd2dd10d20362a045a0075b
ms.sourcegitcommit: fad368d47a83dadc85523d86126941c1250b14e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2019
ms.locfileid: "71122662"
---
# <a name="how-to-configure-apache-kafka-on-hdinsight-to-automatically-create-topics"></a>Configuración de Apache Kafka en HDInsight para crear automáticamente los temas

De forma predeterminada, [Apache Kafka](https://kafka.apache.org/) en HDInsight no habilita la creación automática de temas. Puede habilitar la creación automática de temas de clústeres existentes con [Apache Ambari](https://ambari.apache.org/). También puede habilitar la creación automática de temas al crear un clúster de Kafka mediante una plantilla de Azure Resource Manager.

## <a name="apache-ambari-web-ui"></a>Interfaz de usuario web de Apache Ambari

Para habilitar la creación automática de temas en un clúster existente mediante la interfaz de usuario web de Ambari, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com), seleccione el clúster de Kafka.

2. En la __información general del clúster__, seleccione __Panel de clúster__.

    ![Imagen Azure Portal con el panel de clúster seleccionado](./media/apache-kafka-auto-create-topics/kafka-cluster-overview.png)

3. A continuación, seleccione __Panel de clúster de HDInsight__. Cuando se le solicite, realice la autenticación mediante las credenciales de inicio de sesión (administrador) del clúster.

    ![Imagen de la entrada del panel de clúster de HDInsight](./media/apache-kafka-auto-create-topics/hdinsight-cluster-dashboard.png)

3. Seleccione el servicio Kafka en la lista de la parte izquierda de la página.

    ![Servicio de Apache Ambari: pestaña de lista](./media/apache-kafka-auto-create-topics/hdinsight-service-list.png)

4. Seleccione Configuraciones en el medio de la página.

    ![Servicio de Apache Ambari: pestaña de configuración](./media/apache-kafka-auto-create-topics/hdinsight-service-config.png)

5. En el campo Filtro, escriba un valor de `auto.create`.

    ![Apache Ambari: campo de filtro de búsqueda](./media/apache-kafka-auto-create-topics/hdinsight-filter-field.png)

    La lista de propiedades se filtra y se muestra el valor `auto.create.topics.enable`.

6. Cambie el valor de `auto.create.topics.enable` a `true` y luego seleccione Guardar. Agregue una nota y después vuelva a seleccionar Guardar.

    ![Imagen de la entrada de auto.create.topics.enable](./media/apache-kafka-auto-create-topics/auto-create-topics-enable.png)

7. Seleccione el servicio __Kafka__, seleccione __Reiniciar__ y luego seleccione Restart all affected (Reiniciar todos los afectados). Cuando se le solicite, seleccione __Confirm Restart All__ (Confirmar reiniciar todo).

    ![Apache Ambari: reinicar todos los afectados](./media/apache-kafka-auto-create-topics/restart-all-affected.png)

> [!NOTE]  
> También puede establecer valores de Ambari mediante la API de REST de Ambari. Esto suele ser más difícil, ya que tiene que hacer varias llamadas de REST para recuperar la configuración actual, modificarla, etc. Para obtener más información, consulte el documento [Manage HDInsight clusters using the Apache Ambari REST API](../hdinsight-hadoop-manage-ambari-rest-api.md) (Administrar clústeres de HDInsight con la API REST de Apache Ambari).

## <a name="resource-manager-templates"></a>Plantillas de Resource Manager

Al crear un clúster de Kafka mediante una plantilla de Azure Resource Manager, puede establecer directamente `auto.create.topics.enable` al agregarla en `kafka-broker`. El siguiente fragmento de código JSON muestra cómo establecer este valor en `true`:

```json
"clusterDefinition": {
    "kind": "kafka",
    "configurations": {
    "gateway": {
        "restAuthCredential.isEnabled": true,
        "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
        "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
    },
    "kafka-broker": {
        "auto.create.topics.enable": "true"
    }
}
```

## <a name="next-steps"></a>Pasos siguientes

En este documento, ha aprendido a habilitar la creación automática de temas para Apache Kafka en HDInsight. Para más información sobre cómo trabajar con Kafka, vea los siguientes vínculos:

* [Análisis de registros de Apache Kafka](apache-kafka-log-analytics-operations-management.md)
* [Réplica de datos entre clústeres de Apache Kafka](apache-kafka-mirroring.md)
