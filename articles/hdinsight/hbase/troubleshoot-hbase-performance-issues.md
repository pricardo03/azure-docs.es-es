---
title: Solución de problemas de rendimiento de Apache HBase en Azure HDInsight
description: Diversas instrucciones y sugerencias de ajuste del rendimiento de Apache HBase para obtener un rendimiento óptimo en Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 09/24/2019
ms.openlocfilehash: c67f21a6ed8a7697977bb7737f0e46348efb2530
ms.sourcegitcommit: 3f22ae300425fb30be47992c7e46f0abc2e68478
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2019
ms.locfileid: "71266505"
---
# <a name="troubleshoot-apache-hbase-performance-issues-on-azure-hdinsight"></a>Solución de problemas de rendimiento de Apache HBase en Azure HDInsight

En este documento se describen diversas instrucciones y sugerencias de ajuste del rendimiento de Apache HBase para obtener un rendimiento óptimo en Azure HDInsight. Muchas de estas sugerencias dependen de la carga de trabajo y del patrón de lectura, escritura o examen concretos. Realice pruebas exhaustivas de los cambios de configuración antes de aplicarlos en un entorno de producción.

## <a name="hdinsight-hbase-performance-insights"></a>Información de rendimiento de HDInsight HBase

El principal cuello de botella en la mayoría de las cargas de trabajo de HBase es el registro de escritura previa (WAL). Afecta de manera importante al rendimiento de escritura. HDInsight HBase tiene un modelo de almacenamiento y proceso separados; es decir, los datos se almacenan de forma remota en Azure Storage, pero los servidores regionales se hospedan en las máquinas virtuales. Hasta hace poco, el registro de escritura previa también se escribía en Azure Storage, lo que aumentaba el cuello de botella en el caso de HDInsight. La característica [Escrituras aceleradas](./apache-hbase-accelerated-writes.md) se ha diseñado para resolver este problema, al escribir el registro de escritura previa en discos administrados SSD Premium de Azure. Esto supone una sustancial mejora en el rendimiento de la escritura y ayuda a resolver muchos problemas con los que se encuentran algunas de las cargas de trabajo de escritura intensiva.

Use una cuenta de almacenamiento de blobs en bloques Premium como almacenamiento remoto para obtener una mejora significativa en las operaciones de lectura. Esta opción solo es posible si está habilitada la característica de registros de escritura previa.

## <a name="compaction"></a>Compactación

La compactación es otro posible cuello de botella fundamentalmente acordado en la comunidad.  De forma predeterminada, la compactación mayor está deshabilitada en los clústeres de HDInsight HBase. Esto se debe a que, dado que se trata de un proceso que consume muchos recursos, queremos permitir que los clientes cuenten con toda la flexibilidad necesaria para programarla según sus características de carga de trabajo, es decir, fuera de las horas punta. Hay que tener en cuenta también que nuestro almacenamiento es remoto (respaldado por Azure Storage) en lugar de usar un sistema HDFS local, habitual en la mayoría de las instancias locales; por tanto, no se da el problema de localidad de los datos, que es uno de los principales objetivos de la compactación mayor.

Se supone que el cliente se encargará de programar la compactación mayor según sus intereses. Si no se realiza este mantenimiento, la compactación afectará significativamente al rendimiento de lectura a largo plazo.

En particular en el caso de las operaciones de examen, las latencias medias muy superiores a 100 ms deben ser motivo de preocupación. Compruebe si la compactación mayor se ha programado de manera adecuada.

## <a name="know-your-apache-phoenix-workload"></a>Conozca la carga de trabajo de Apache Phoenix

La respuesta a las siguientes preguntas le ayudará a comprender mejor la carga de trabajo de Apache Phoenix:

* ¿Todas las "lecturas" se convierten en exámenes?
    * Si es así, ¿cuáles son las características de estos exámenes?
    * ¿Ha optimizado el esquema de tabla de Phoenix para estos exámenes, incluida una indexación adecuada?
* ¿Ha utilizado la instrucción `EXPLAIN` para comprender los planes de consulta que generan las "lecturas"?
* ¿Las escrituras son instrucciones "upsert-select"?
    * Si es así, también estarán realizando exámenes. La latencia esperada de los exámenes se sitúa en el orden de 100 ms de media, en lugar de 10 ms para los get de punto en HBase.  

## <a name="test-methodology-and-metrics-monitoring"></a>Metodología de prueba y supervisión de métricas

Si usa bancos de pruebas como YCSB, JMeter o Pherf para probar y ajustar el rendimiento, asegúrese de lo siguiente:

1. Los equipos cliente no se convierten en un cuello de botella (compruebe el uso de CPU en los equipos cliente).

1. Las configuraciones de cliente (como el número de subprocesos, etc.) se ajustan adecuadamente para saturar el ancho de banda del cliente.

1. Los resultados de las pruebas se registran de forma precisa y sistemática.

Si las consultas empiezan a mostrar un rendimiento mucho peor que antes, compruebe si hay errores en el código de la aplicación: ¿está generando repentinamente grandes cantidades de datos no válidos, lo que aumenta de forma natural las latencias de lectura?

## <a name="migration-issues"></a>Problemas de migración

Si va a migrar a Azure HDInsight, asegúrese de que la migración se realiza de forma sistemática y precisa, preferiblemente mediante automatización. Evite la migración manual. Asegúrese de lo siguiente:

1. Los atributos de tabla (como la compresión, los filtros de Bloom, etc.) se deben migrar con precisión.

1. En el caso de las tablas de Phoenix, la configuración del cifrado con sal debe adaptarse de manera adecuada al nuevo tamaño del clúster. Por ejemplo, se recomienda que el número de cubos de sal sea un múltiplo del número de nodos de trabajo del clúster; el múltiplo en cuestión es un factor del número de zonas activas observadas.  

## <a name="server-side-config-tunings"></a>Ajustes de la configuración del servidor

En HDInsight HBase, los HFiles se guardan en el almacenamiento remoto; por lo tanto, cuando se produce un error de caché, el costo de las lecturas es sin duda más alto que en los sistemas locales (cuyos datos cuentan con respaldo de HDFS local) debido a la latencia de red implicada. En la mayoría de los escenarios, se ha diseñado un uso inteligente de las memorias caché de HBase (caché de bloques y caché de cubos) para evitar este problema. Sin embargo, seguirán existiendo casos esporádicos en los que esto podría suponer un problema para el cliente. El uso de una cuenta de blobs en bloques Premium ha supuesto cierta ayuda. Sin embargo, dado que el blob WASB (controlador de Windows Azure Storage) se basa en determinadas propiedades, para capturar los datos en bloques, en función de los cuales determina el modo de lectura (secuencial o aleatorio), es posible que se sigan produciendo casos de latencias mayores con operaciones de lectura. A partir de experimentos prácticos, hemos determinado que establecer el tamaño del bloque de solicitud de lectura (`fs.azure.read.request.size`) en 512 KB y hacer coincidir con él el tamaño de bloque de las tablas de HBase ofrece los mejores resultados.

HDInsight HBase, en la mayoría de los clústeres de nodos de gran tamaño, proporciona `bucketcache` como archivo en un disco SSD local conectado a la máquina virtual, que ejecuta las instancias `regionservers`. En ocasiones, el uso en su lugar de memoria caché fuera del montón puede suponer alguna mejora. Esto tiene la limitación de que usa la memoria disponible y puede tener un tamaño menor que el de la memoria caché basada en archivos, por lo que es posible que no sea siempre la mejor opción.

A continuación indicamos otros parámetros específicos que ajustamos y que parecen haber ayudado en distintos grados, con alguna explicación:

1. Aumentar el tamaño de `memstore` del valor predeterminado de 128 MB a 256 MB. Esta configuración se suele recomendar para escenarios de escritura intensiva.

1. Aumentar el número de subprocesos dedicados a la compactación, pasando del valor predeterminado de 1 a 4. Esta configuración es pertinente si observamos frecuentes compactaciones menores.

1. Evitar el bloqueo del vaciado de `memstore` debido al límite del almacén. `Hbase.hstore.blockingStoreFiles` se puede aumentar hasta 100 para proporcionar este almacenamiento en búfer.

1. Para controlar los vaciados, se pueden modificar los valores predeterminados como se indica a continuación:

    1. `Hbase.regionserver.maxlogs` se puede subir de 32 a 140 (evitando vaciados debido a los límites de WAL).

    1. `Hbase.regionserver.global.memstore.lowerLimit` = 0.55.

    1. `Hbase.regionserver.global.memstore.upperLimit` = 0.60.

1. Configuraciones específicas de Phoenix para el ajuste de grupos de subprocesos:

    1. `Phoenix.query.queuesize` se puede aumentar hasta 10000.

    1. `Phoenix.query.threadpoolsize` se puede aumentar hasta 512.

1. Otras configuraciones específicas de Phoenix:

    1. `Phoenix.rpc.index.handler.count` se puede establecer en 50 si hay grandes o muchas búsquedas de índice.

    1. `Phoenix.stats.updateFrequency` se puede subir desde el valor predeterminado de 15 minutos hasta 1 hora.

    1. `Phoenix.coprocessor.maxmetadatacachetimetolivems` se puede subir desde el valor de 30 minutos hasta 1 hora.

    1. `Phoenix.coprocessor.maxmetadatacachesize` se puede subir desde el valor de 20 MB hasta 50 MB.

1. Tiempos de espera de RPC: el tiempo de espera de RPC de HBase, el tiempo de espera del escáner de cliente de HBase y el tiempo de espera de consulta de Phoenix pueden aumentarse a 3 minutos. Aquí es importante tener en cuenta que el parámetro `hbase.client.scanner.caching` se establece en un valor que coincide en los lados de servidor y cliente. De lo contrario, esta configuración conduce a errores relacionados con `OutOfOrderScannerException` en el cliente. Esta opción debe establecerse en un valor bajo para exámenes grandes. Establecemos este valor en 100.

## <a name="other-considerations"></a>Otras consideraciones

Existen otros parámetros que se pueden tener en cuenta para el ajuste:

1. `Hbase.rs.cacheblocksonwrite`: este valor se establece en true de forma predeterminada en HDI.

1. Configuración que permite diferir la compactación menor para más adelante.

1. Configuración experimental, como el ajuste de porcentajes de colas reservadas para solicitudes de lectura y escritura.

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

- Obtenga respuestas de expertos de Azure mediante el [soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).

- Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente. Esta cuenta pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.

- Si necesita más ayuda, puede enviar una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Seleccione **Soporte técnico** en la barra de menús o abra la central **Ayuda + soporte técnico**. Para obtener información más detallada, revise [Creación de una solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). La suscripción a Microsoft Azure incluye acceso al soporte técnico para facturación y administración de suscripciones. El soporte técnico se proporciona a través de uno de los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).
