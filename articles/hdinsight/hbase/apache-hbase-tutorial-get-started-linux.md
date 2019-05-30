---
title: Introducción a un ejemplo de HBase en HDInsight (Azure)
description: Siga este ejemplo de Apache HBase para empezar a usar Hadoop en HDInsight. Cree tablas desde el shell de HBase y consúltelas mediante Hive.
keywords: hbasecommand, ejemplo de hbase
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 05/27/2019
ms.author: hrasheed
ms.openlocfilehash: 9d94a976c08cdb5184ea4c5e2cd70ac039d78378
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66384697"
---
# <a name="get-started-with-an-apache-hbase-example-in-hdinsight"></a>Introducción a un ejemplo de Apache HBase en HDInsight

Aprenda a crear un clúster de [Apache HBase](https://hbase.apache.org/) en HDInsight, a crear tablas de HBase y a consultar tablas mediante [Apache Hive](https://hive.apache.org/).  Para obtener información general de HBase, consulte [información general de HDInsight HBase](./apache-hbase-overview.md).

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

* Un cliente SSH. Para más información, consulte [Conexión a través de SSH con HDInsight (Apache Hadoop)](../hdinsight-hadoop-linux-use-ssh-unix.md).

* Bash. Los ejemplos de este artículo usan el shell de Bash en Windows 10 para los comandos de curl. Consulte [subsistema de Windows para la Guía de instalación de Linux para Windows 10](https://docs.microsoft.com/windows/wsl/install-win10) para conocer los pasos de instalación.  Otros [shells de Unix](https://www.gnu.org/software/bash/) también funcionan correctamente.  Los ejemplos de curl, con algunas pequeñas modificaciones, pueden trabajar en un símbolo del sistema Windows.  Como alternativa, puede usar el cmdlet de Windows PowerShell [Invoke-RestMethod](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/invoke-restmethod).


## <a name="create-apache-hbase-cluster"></a>Crear clústeres de Apache HBase

El siguiente procedimiento usa una plantilla de Azure Resource Manager para crear un clúster de HBase y la cuenta de Azure Storage predeterminada dependiente. Para comprender los parámetros utilizados en el procedimiento y otros métodos de creación del clúster, consulte [Creación de clústeres de Hadoop basados en Linux en HDInsight](../hdinsight-hadoop-provision-linux-clusters.md). Para más información sobre el uso de Data Lake Storage Gen2, consulte [Guía de inicio rápido: Configuración de clústeres en HDInsight](../../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md).

1. Seleccione la imagen siguiente para abrir la plantilla en el portal de Azure. La plantilla se encuentra en [plantillas de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/).

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-linux%2Fazuredeploy.json" target="_blank"><img src="./media/apache-hbase-tutorial-get-started-linux/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. En la hoja **Implementación personalizada**, escriba los valores siguientes:

    |Propiedad |DESCRIPCIÓN |
    |---|---|
    |Subscription|seleccione la suscripción de Azure que usa para crear este clúster.|
    |Grupos de recursos|cree un grupo de administración de recursos de Azure o use uno existente.|
    |Ubicación|especifique la ubicación del grupo de recursos. |
    |ClusterName|escriba el nombre del clúster de HBase.|
    |Nombre de inicio de sesión de clúster y la contraseña|El nombre de inicio de sesión predeterminado es **admin**.|
    |Nombre de usuario SSH y contraseña|El nombre de usuario predeterminado es **sshuser**.|

    Otros parámetros son opcionales.  

    Cada clúster tiene una dependencia de cuenta de Azure Storage. Después de eliminar un clúster, los datos permanecen en la cuenta de almacenamiento. El nombre de cuenta de almacenamiento de clúster predeterminado es el nombre del clúster con "store" anexado. Está codificado en la sección de variables de plantilla.

3. Seleccione **Acepto los términos y condiciones indicadas anteriormente** y, después, seleccione **Comprar**. Se tarda aproximadamente 20 minutos en crear un clúster.

> [!NOTE]  
> Después de que se elimine un clúster de HBase, puede crear otro clúster de HBase mediante el mismo contenedor de blobs predeterminado. El nuevo clúster selecciona las tablas de HBase que creó en el clúster original. Para evitar incoherencias, recomendamos deshabilitar las tablas de HBase antes de eliminar el clúster.

## <a name="create-tables-and-insert-data"></a>Creación de tablas e inserción de datos

Puede usar SSH para conectarse a los clústeres de HBase y, después, usar el [shell de Apache HBase](https://hbase.apache.org/0.94/book/shell.html) para crear tablas de HBase e insertar y consultar datos. Para más información, consulte [Uso SSH con HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

Para la mayoría de las personas, los datos aparecen en formato tabular:

![Datos tabulares de HBase de HDInsight](./media/apache-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-tabular.png)

En HBase (una implementación de [Cloud BigTable](https://cloud.google.com/bigtable/)), los mismos datos tienen un aspecto similar al siguiente:

![Datos de HDInsight HBase BigTable](./media/apache-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-bigtable.png)

**Para usar el shell de HBase, siga estos pasos:**

1. Use el comando `ssh` para conectarse al clúster de HBase. Modifique el comando siguiente: reemplace `CLUSTERNAME` por el nombre del clúster y, luego, escriba el comando:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. Use el comando `hbase shell` para iniciar el shell interactivo de HBase. Escriba el siguiente comando en la conexión SSH:

    ```bash
    hbase shell
    ```

1. Use el comando `create` para crear una tabla de HBase con dos familias de columnas. Los nombres de columna y tabla distinguen mayúsculas de minúsculas. Escriba el comando siguiente:

    ```hbaseshell
    create 'Contacts', 'Personal', 'Office'
    ```

1. Use el comando `list` para mostrar todas las tablas de HBase. Escriba el comando siguiente:

    ```hbase
    list
    ```

1. Use el comando `put` para insertar valores en una columna especificada en una fila especificada de una tabla determinada. Escriba los siguientes comandos:

    ```hbaseshell
    put 'Contacts', '1000', 'Personal:Name', 'John Dole'
    put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
    put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
    put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
    ```

1. Use el comando `scan` para buscar y devolver los datos de tabla `Contacts`. Escriba el comando siguiente:

    ```hbase
    scan 'Contacts'
    ```

    ![Shell de HDInsight Hadoop HBase](./media/apache-hbase-tutorial-get-started-linux/hdinsight-hbase-shell.png)

1. Use el comando `get` para capturar el contenido de una fila. Escriba el comando siguiente:

    ```hbaseshell
    get 'Contacts', '1000'
    ```

    Observará que los resultados son similares al uso del comando `scan` porque solo hay una fila.

    Para más información sobre el esquema de tabla de HBase, consulte [Introduction to Apache HBase Schema Design](http://0b4af6cdc2f0c5998459-c0245c5c937c5dedcca3f1764ecc9b2f.r43.cf2.rackcdn.com/9353-login1210_khurana.pdf) (Introducción al diseño de esquema de Apache HBase). Para ver más comandos de HBase, consulte [Guía de referencia de Apache HBase](https://hbase.apache.org/book.html#quickstart).

1. Use el comando `exit` para detener el shell interactivo de HBase. Escriba el comando siguiente:

    ```hbaseshell
    exit
    ```

**Para cargar datos de forma masiva en la tabla HBase de contactos**

HBase incluye varios métodos de carga de datos en las tablas.  Para obtener más información, vea [Carga masiva](https://hbase.apache.org/book.html#arch.bulk.load).

Un archivo de datos de ejemplo puede encontrarse en un contenedor de blobs público, `wasb://hbasecontacts\@hditutorialdata.blob.core.windows.net/contacts.txt`.  El contenido del archivo de datos es:

    8396    Calvin Raji      230-555-0191    230-555-0191    5415 San Gabriel Dr.
    16600   Karen Wu         646-555-0113    230-555-0192    9265 La Paz
    4324    Karl Xie         508-555-0163    230-555-0193    4912 La Vuelta
    16891   Jonn Jackson     674-555-0110    230-555-0194    40 Ellis St.
    3273    Miguel Miller    397-555-0155    230-555-0195    6696 Anchor Drive
    3588    Osa Agbonile     592-555-0152    230-555-0196    1873 Lion Circle
    10272   Julia Lee        870-555-0110    230-555-0197    3148 Rose Street
    4868    Jose Hayes       599-555-0171    230-555-0198    793 Crawford Street
    4761    Caleb Alexander  670-555-0141    230-555-0199    4775 Kentucky Dr.
    16443   Terry Chander    998-555-0171    230-555-0200    771 Northridge Drive

Opcionalmente, puede crear un archivo de texto y cargarlo en su propia cuenta de almacenamiento. Para obtener instrucciones, consulte [cargar datos para trabajos de Apache Hadoop en HDInsight](../hdinsight-upload-data.md).

> [!NOTE]  
> Este procedimiento usa la tabla HBase de contactos que ha creado en el último procedimiento.

1. Desde su abrir ssh conexión, ejecute el siguiente comando para transformar los datos de archivos a StoreFiles y almacene en una ruta de acceso relativa especificada por `Dimporttsv.bulk.output`.

    ```bash
    hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name,Personal:Phone,Office:Phone,Office:Address" -Dimporttsv.bulk.output="/example/data/storeDataFileOutput" Contacts wasb://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt
    ```

2. Ejecute el siguiente comando para cargar los datos de `/example/data/storeDataFileOutput` a la tabla de HBase:

    ```bash
    hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /example/data/storeDataFileOutput Contacts
    ```

3. Puede abrir el shell de HBase y usar el `scan` comando para enumerar el contenido de la tabla.

## <a name="use-apache-hive-to-query-apache-hbase"></a>Usar Apache Hive para consultar Apache HBase

Puede consultar datos en las tablas de HBase mediante [Apache Hive](https://hive.apache.org/). En esta sección, creará una tabla de Hive que se asigna a la tabla de HBase y la usará para consultar los datos de la tabla de HBase.

1. Desde su abrir la conexión ssh, use el siguiente comando para iniciar Beeline:

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin
    ```

    Para más información sobre Beeline, consulte [Uso de Hive con Hadoop en HDInsight con Beeline](../hadoop/apache-hadoop-use-hive-beeline.md).

1. Ejecute el siguiente script de [HiveQL](https://cwiki.apache.org/confluence/display/Hive/LanguageManual) para crear una tabla de Hive que se asigne a la tabla de HBase. Antes de ejecutar esta instrucción, asegúrese de haber creado la tabla de ejemplo a la que se hace referencia aquí en HBase mediante el shell de HBase.

    ```hiveql
    CREATE EXTERNAL TABLE hbasecontacts(rowkey STRING, name STRING, homephone STRING, officephone STRING, officeaddress STRING)
    STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
    WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,Personal:Name,Personal:Phone,Office:Phone,Office:Address')
    TBLPROPERTIES ('hbase.table.name' = 'Contacts');
    ```

1. Ejecute el siguiente script de HiveQL para consultar los datos de la tabla de HBase:

    ```hiveql
    SELECT count(rowkey) AS rk_count FROM hbasecontacts;
    ```

1. Para salir de Beeline, use `!exit`.

1. Para salir de su ssh conexión, use `exit`.

## <a name="use-hbase-rest-apis-using-curl"></a>Usar las API de REST de HBase con Curl

La API de REST se protege con la [autenticación básica](https://en.wikipedia.org/wiki/Basic_access_authentication). Siempre debe crear solicitudes usando HTTP segura (HTTPS) para así garantizar que las credenciales se envían de manera segura al servidor.

1. Iniciar la variable de entorno para facilitar su uso. Editar los comandos siguientes, reemplazando `MYPASSWORD` con la contraseña de inicio de sesión del clúster. Reemplace `MYCLUSTERNAME` con el nombre del clúster de HBase. A continuación, escriba los comandos.

    ```bash
    export password='MYPASSWORD'
    export clustername=MYCLUSTERNAME
    ```

1. Use el siguiente comando para enumerar las tablas de HBase existentes:

    ```bash
    curl -u admin:$password \
    -G https://$clustername.azurehdinsight.net/hbaserest/
    ```

1. Use el siguiente comando para crear una nueva tabla de HBase con dos familias de columnas:

    ```bash
    curl -u admin:$password \
    -X PUT "https://$clustername.azurehdinsight.net/hbaserest/Contacts1/schema" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d "{\"@name\":\"Contact1\",\"ColumnSchema\":[{\"name\":\"Personal\"},{\"name\":\"Office\"}]}" \
    -v
    ```

    El esquema se ofrece con el formato JSon.
1. Use el siguiente comando para instalar algunos datos:

    ```bash
    curl -u admin:$password \
    -X PUT "https://$clustername.azurehdinsight.net/hbaserest/Contacts1/false-row-key" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d "{\"Row\":[{\"key\":\"MTAwMA==\",\"Cell\": [{\"column\":\"UGVyc29uYWw6TmFtZQ==\", \"$\":\"Sm9obiBEb2xl\"}]}]}" \
    -v
    ```

    Debe codificar en base64 los valores especificados en el modificador -d. En el ejemplo:

   * MTAwMA==: 1000
   * UGVyc29uYWw6TmFtZQ==: Personal:Name
   * Sm9obiBEb2xl: John Doe

     [false-row-key](https://hbase.apache.org/apidocs/org/apache/hadoop/hbase/rest/package-summary.html#operation_cell_store_single) permite insertar varios valores (por lotes).

1. Use el siguiente comando para obtener una fila:

    ```bash
    curl -u admin:$password \
    GET "https://$clustername.azurehdinsight.net/hbaserest/Contacts1/1000" \
    -H "Accept: application/json" \
    -v
    ```

Para más información sobre Rest de HBase, consulte la [guía de referencia de Apache HBase](https://hbase.apache.org/book.html#_rest).

> [!NOTE]  
> Thrift no es compatible con HBase en HDInsight.
>
> Al usar Curl o cualquier otra comunicación REST con WebHCat, debe proporcionar el nombre de usuario y la contraseña del administrador del clúster de HDInsight para autenticar las solicitudes. También debe usar el nombre del clúster como parte del identificador uniforme de recursos (URI) que se utiliza para enviar las solicitudes al servidor:
> 
>   
>        curl -u <UserName>:<Password> \
>        -G https://<ClusterName>.azurehdinsight.net/templeton/v1/status
>   
>    Recibirá una respuesta similar a la siguiente:
>   
>        {"status":"ok","version":"v1"}


## <a name="check-cluster-status"></a>Comprobar el estado del clúster

HBase en HDInsight se incluye con una interfaz de usuario web para la supervisión de clústeres. Mediante la interfaz de usuario web, puede solicitar estadísticas o información acerca de las regiones.

**Para acceder a la interfaz de usuario maestra de HBase**

1. Inicio de sesión en la interfaz de usuario Web de Ambari en `https://Clustername.azurehdinsight.net`.
2. Haga clic en **HBase** en el menú izquierdo.
3. Haga clic en **Quick links** (Vínculos rápidos) en la parte superior de la página, seleccione el vínculo del nodo Zookeeper activo y, después, haga clic en **HBase Master UI** (Interfaz de usuario maestra de HBase).  La interfaz de usuario se abre en otra pestaña del explorador:

   ![Interfaz de usuario maestra de HBase de HDInsight](./media/apache-hbase-tutorial-get-started-linux/hdinsight-hbase-hmaster-ui.png)

   La interfaz de usuario maestra de HBase contiene las siguientes secciones:

   - servidores regionales
   - maestros de copia de seguridad
   - tables
   - tareas
   - atributos de software

## <a name="delete-the-cluster"></a>Eliminación del clúster

Para evitar incoherencias, recomendamos deshabilitar las tablas de HBase antes de eliminar el clúster.

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a>Solución de problemas

Si experimenta problemas con la creación de clústeres de HDInsight, consulte los [requisitos de control de acceso](../hdinsight-hadoop-customize-cluster-linux.md#access-control).

## <a name="next-steps"></a>Pasos siguientes

En este artículo ha aprendido a crear un clúster de Apache HBase, a crear tablas y a ver los datos de esas tablas en el shell de HBase. También ha aprendido a usar una consulta de datos de Hive en las tablas de HBase y a usar las API de REST de C# para HBase para crear una tabla de HBase y recuperar los datos de la tabla.

Para obtener más información, consulte:

* [Información general de HDInsight HBase](./apache-hbase-overview.md): Apache HBase es una base de datos NoSQL de código abierto de Apache basada en Apache Hadoop que proporciona acceso aleatorio y un elevado nivel de coherencia para grandes cantidades de datos no estructurados y semiestructurados.