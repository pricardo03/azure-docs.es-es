---
title: 'Azure HDInsight: preguntas más frecuentes'
description: Proporciona información general sobre Azure HDInsight.
author: Ramakoni1
ms.author: ramakoni
ms.reviewer: v-miegge
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: 4accd8c2f58dd89fb5f918ca6e58e71caeaed57f
ms.sourcegitcommit: 7df70220062f1f09738f113f860fad7ab5736e88
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2019
ms.locfileid: "71213872"
---
# <a name="azure-hdinsight-frequently-asked-questions"></a>Azure HDInsight: Preguntas más frecuentes

En este artículo se proporcionan respuestas a algunas de las preguntas más comunes sobre la ejecución de [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/).

## <a name="creating-or-deleting-hdinsight-clusters"></a>Creación o eliminación de clústeres de HDInsight

### <a name="how-do-i-provision-an-hdinsight-cluster"></a>¿Cómo aprovisiono un clúster de HDInsight?

Para revisar los tipos de clústeres de HDInsight disponibles y los métodos de aprovisionamiento, consulte [Configuración de clústeres en HDInsight con Apache Hadoop, Apache Spark, Apache Kafka, etc](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-provision-linux-clusters).

### <a name="how-do-i-delete-an-existing-hdinsight-cluster"></a>¿Cómo elimino un clúster de HDInsight existente?

Para más información sobre cómo eliminar un clúster que ya no se usa, consulte [Eliminación de un clúster de HDInsight con el explorador, PowerShell o la CLI de Azure](https://docs.microsoft.com/azure/hdinsight/hdinsight-delete-cluster). Se recomienda que transcurran al menos de 30 a 60 minutos entre las operaciones de creación y de eliminación de un clúster. De lo contrario, puede producirse un error en la operación con el siguiente mensaje de error:

``Conflict (HTTP Status Code: 409) error when attempting to delete a cluster immediately after creation of a cluster. If you encounter this error, wait until the newly created cluster is in operational state before attempting to delete it.``

### <a name="how-do-i-select-the-correct-number-of-cores-or-nodes-for-my-workload"></a>¿Cómo selecciono el número correcto de núcleos o nodos para mi carga de trabajo?

El número adecuado de núcleos y otras opciones de configuración dependen de diversos factores.

Para más información, consulte [Planeamiento de la capacidad de los clústeres de HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-capacity-planning).

### <a name="what-can-i-do-when-cluster-provisioning-fails-because-of-a-capacity-issue"></a>¿Qué puedo hacer cuando se produce un error en el aprovisionamiento del clúster debido a un problema de capacidad?

En esta sección se indican errores comunes debidos a problemas de capacidad, así como técnicas para mitigarlos.

#### <a name="error-the-deployment-would-exceed-the-quota-of-800"></a>Error: the deployment would exceed the quota of '800' (la implementación superaría la cuota de '800')

Azure tiene una cuota máxima de 800 implementaciones por grupo de recursos. Se aplican diferentes cuotas por grupo de recursos, suscripciones, cuentas u otros ámbitos. Por ejemplo, la suscripción puede configurarse para limitar el número de núcleos para una región. Si trata de implementar una máquina virtual con más núcleos de los permitidos, recibirá un mensaje de error que indica que se ha superado la cuota.

Para resolver este problema, elimine las implementaciones que ya no son necesarias mediante Azure Portal, la CLI o PowerShell.

Para más información, consulte [Resolver errores de cuota de recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quota-errors).

#### <a name="error-the-maximum-node-exceeded-the-available-cores-in-this-region"></a>Error: the maximum node exceeded the available cores in this region (el nodo máximo superó los núcleos disponibles en esta región)

La suscripción puede configurarse para limitar el número de núcleos para una región. Si trata de implementar un recurso con más núcleos de los permitidos, recibirá un mensaje de error que indica que se ha superado la cuota.

Para solicitar un aumento de la cuota, siga estos pasos:

1. Vaya a Azure Portal y seleccione Ayuda y soporte técnico.
2. Seleccione Nueva solicitud de soporte técnico.
3. En la página Nueva solicitud de soporte técnico, en la pestaña Datos básicos, seleccione las opciones siguientes:

   * Tipo de problema: Límites de servicio y suscripción (cuotas)
   * Suscripción: seleccione la suscripción que quiere modificar.
   * Tipo de cuota: HDInsight

Para más información, consulte [Creación de una incidencia de soporte técnico para aumentar los núcleos](https://docs.microsoft.com/azure/hdinsight/hdinsight-capacity-planning#quotas).

### <a name="what-are-the-various-types-of-nodes-in-an-hdinsight-cluster"></a>¿Cuáles son los distintos tipos de nodos de un clúster de HDInsight?

Los clústeres de Azure HDInsight tienen distintos tipos de máquinas virtuales o nodos. Cada tipo de nodo desempeña un papel en el funcionamiento del sistema.

Para más información, consulte [Tipos de recursos en clústeres de Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-virtual-network-architecture#resource-types-in-azure-hdinsight-clusters).

## <a name="individual-components"></a>Componentes individuales

### <a name="can-i-install-additional-components-on-my-cluster"></a>¿Puedo instalar componentes adicionales en mi clúster?

Sí, puede instalar componentes adicionales o personalizar la configuración del clúster mediante cualquiera de los siguientes métodos:

* Puede usar scripts durante o después de la creación. Estos scripts se invocan a través de una [acción de script](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux). Esta opción de configuración se puede usar desde Azure Portal, los cmdlets de Windows PowerShell para HDInsight o el SDK de HDInsight para .NET. 
* Puede usar sudo u otros métodos después de aprovisionar el clúster.
* Puede usar la [plataforma de aplicaciones HDInsight](https://azure.microsoft.com/services/hdinsight/partner-ecosystem/) para instalar aplicaciones del ecosistema.

Sin embargo, los equipos de Soporte técnico de Microsoft solo pueden ofrecer soporte técnico para las siguientes situaciones:

* Problemas o errores que se producen al cargar el script. Cualquier error que se produce durante la ejecución de los scripts personalizados está fuera del ámbito de una incidencia de soporte técnico. 
* Aplicaciones adicionales que forman parte del proceso de creación del clúster. 

Para acceder a una lista de los componentes admitidos, consulte [¿Cuáles son los componentes y versiones de Apache Hadoop disponibles con HDInsight?](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#apache-hadoop-components-available-with-different-hdinsight-versions)

La compatibilidad con componentes individuales también puede variar según el tipo de clúster. Por ejemplo, Spark no se admite en un clúster de Kafka y viceversa.

En el caso de aplicaciones o servicios que no forman parte del proceso de creación del clúster, póngase en contacto con el proveedor o el proveedor de servicios correspondiente para obtener soporte técnico. También puede usar sitios de la comunidad y conseguir soporte técnico para estas acciones. Hay muchos sitios de la comunidad disponibles; por ejemplo, el [foro de MSDN para HDInsight](https://social.msdn.microsoft.com/Forums/home?forum=hdinsight) y [Stack Overflow](https://stackoverflow.com/questions/tagged/hdinsight). Los proyectos de Apache también tienen sitios de proyecto en el [sitio web de Apache](https://apache.org/). Un ejemplo es [Hadoop](https://hadoop.apache.org/).

 Si tiene más preguntas relacionadas con el soporte técnico de Azure, revise las [preguntas más frecuentes de soporte técnico de Azure](https://azure.microsoft.com/en-us/support/faq/).

### <a name="can-i-upgrade-the-individual-components-that-are-pre-installed-on-the-cluster"></a>¿Puedo actualizar los componentes individuales que están preinstalados en el clúster?

Si actualiza los componentes integrados o las aplicaciones que están preinstaladas en el clúster, Microsoft no dará soporte técnico a la configuración resultante. Microsoft no ha probado estas configuraciones del sistema. Intente usar una versión diferente del clúster de HDInsight; puede que ya tenga la versión actualizada del componente preinstalado.

Por ejemplo, no se admite la actualización de Hive como componente individual. HDInsight es un servicio administrado y muchos servicios se integran con el servidor de Ambari y se prueban. La actualización independiente de Hive hace que los archivos binarios indexados de otros componentes cambien, lo que provocará problemas de integración de componentes en el clúster.

### <a name="can-spark-and-kafka-run-on-the-same-hdinsight-cluster"></a>¿Se pueden ejecutar Spark y Kafka en el mismo clúster de HDInsight?

No, no es posible ejecutar Apache Kafka y Apache Spark en el mismo clúster de HDInsight. Los usuarios deben crear clústeres independientes para cada uno de ellos, con el fin de evitar problemas de contención de recursos.

### <a name="how-do-i-change-timezone-in-ambari"></a>¿Cómo cambio la zona horaria en Ambari?

1. Abra la interfaz de usuario de Ambari Web en https://CLUSTERNAME.azurehdinsight.net, donde CLUSTERNAME es el nombre del clúster.
2. En la esquina superior derecha, seleccione admin | Settings (Administrador | Configuración). 

   ![Configuración de Ambari](media/hdinsight-faq/ambari-settings.png)

3. En la ventana User Settings (Configuración del usuario), seleccione la nueva zona horaria en la lista desplegable Timezone (Zona horaria) y, a continuación, haga clic en Save (Guardar).

   ![Configuración de usuario de Ambari](media/hdinsight-faq/ambari-user-settings.png)

## <a name="metastore"></a>Tienda de metadatos

### <a name="how-can-i-migrate-from-the-existing-metastore-to-azure-sql-server"></a>¿Cómo puedo migrar desde el metastore existente a Azure SQL Server? 

Para migrar de SQL Server a Azure SQL Server, consulte [Tutorial: Migración de SQL Server a una base de datos única o agrupada en Azure SQL Database sin conexión mediante DMS](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-azure-sql).

### <a name="is-the-hive-metastore-deleted-when-the-cluster-is-deleted"></a>¿El metastore de Hive se elimina cuando se elimina el clúster?

Depende del tipo de metastore que el clúster tenga configurado para usar.

Si se trata del metastore predeterminado: el metastore predeterminado forma parte del ciclo de vida del clúster. Al eliminar un clúster, también se eliminan la tienda de metadatos y los metadatos correspondientes.

Si se trata de un metastore personalizado: el ciclo de vida del metastore no está asociado al ciclo de vida de un clúster. Por lo tanto, puede crear y eliminar clústeres sin perder los metadatos. Los metadatos, como los esquemas de Hive, se conservan incluso después de eliminar y volver a crear el clúster de HDInsight.

Para más información, consulte [Use external metadata stores in Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-use-external-metadata-stores) (Uso de almacenes externos de metadatos en Azure HDInsight).

### <a name="does-migrating-a-hive-metastore-also-migrate-the-default-policies-of-the-ranger-database"></a>¿La migración de un metastore de Hive también migra las directivas predeterminadas de la base de datos de Ranger?

No, la definición de la directiva se encuentra en la base de datos de Ranger. Por lo tanto, la migración de un metastore de Hive no migra la directiva.

### <a name="can-you-migrate-a-hive-metastore-from-an-enterprise-security-package-esp-cluster-to-a-non-esp-cluster-and-vice-versa"></a>¿Se puede migrar un metastore de Hive desde un clúster de Enterprise Security Package (ESP) a un clúster que no sea ESP y viceversa?

Sí, puede migrar un metastore de Hive desde un clúster ESP a un clúster que no sea ESP.

### <a name="how-can-i-estimate-the-size-of-a-hive-metastore-database"></a>¿Cómo puedo calcular el tamaño de una base de datos de metastore de Hive?

El metastore de Hive se utiliza para almacenar los metadatos de los orígenes de datos que usa el servidor de Hive. Los requisitos de tamaño se ven afectados por el número de orígenes de datos que puede tener que usar para el servidor de Hive y por el grado de complejidad de estos orígenes. Por lo tanto, no se puede hacer un cálculo por adelantado. Tal y como se describe en [Procedimientos recomendados de Hive Metastore](https://docs.microsoft.com/azure/hdinsight/hdinsight-use-external-metadata-stores#hive-metastore-best-practices), comenzar con un nivel S2 proporciona 50 DTU y 250 GB de almacenamiento. Si se produce un cuello de botella, puede escalar verticalmente la base de datos.

### <a name="do-you-support-any-other-database-other-than-azure-sql-database-as-an-external-metastore"></a>¿Se admite alguna otra base de datos que no sea Azure SQL Database como metastore externo?

No, Microsoft solo admite Azure SQL Database como metastore personalizado externo.

### <a name="can-i-share-a-metastore-across-multiple-clusters"></a>¿Puedo compartir un metastore entre varios clústeres?

Sí, puede compartir un metastore personalizado entre varios clústeres, siempre que usen la misma versión de HDInsight.

## <a name="connectivity-and-virtual-networks"></a>Conectividad y redes virtuales  

### <a name="what-are-the-implications-of-blocking-ports-22-and-23-on-my-network"></a>¿Cuáles son las implicaciones de bloquear los puertos 22 y 23 en mi red?

Si bloquea los puertos 22 y 23, no tendrá acceso SSH al clúster. El servicio HDInsight no usa estos puertos.

Para obtener más información, vea los documentos siguientes:

* [Control del tráfico de red](https://docs.microsoft.com/azure/hdinsight/hdinsight-plan-virtual-network-deployment#networktraffic)
* [Protección del tráfico entrante a los clústeres de HDInsight en una red virtual con un punto de conexión privado](https://azure.microsoft.com/blog/secure-incoming-traffic-to-hdinsight-clusters-in-a-vnet-with-private-endpoint/)
* [Direcciones IP de administración de HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-management-ip-addresses)

### <a name="can-i-deploy-an-additional-virtual-machine-within-the-same-subnet-as-an-hdinsight-cluster"></a>¿Puedo implementar una máquina virtual adicional dentro de la misma subred que un clúster de HDInsight?

Sí, puede implementar una máquina virtual adicional en la misma subred que un clúster de HDInsight. Se admiten las siguientes configuraciones:

* Nodos perimetrales: puede agregar otro nodo perimetral al clúster, tal y como se describe en [Uso de nodos perimetrales vacíos en clústeres de Apache Hadoop en HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-apps-use-edge-node). 
* Nodos independientes:  puede agregar una máquina virtual independiente a la misma subred y acceder al clúster desde esa máquina virtual mediante el punto de conexión privado `https://<CLUSTERNAME>-int.azurehdinsight.net`. Para más información, consulte [Control del tráfico de red](https://docs.microsoft.com/azure/hdinsight/hdinsight-plan-virtual-network-deployment#networktraffic).

### <a name="can-i-add-an-existing-hdinsight-cluster-to-another-virtual-network"></a>¿Puedo agregar un clúster de HDInsight existente a otra red virtual?

No, no puede agregar un clúster de HDInsight existente a otra red virtual. La red virtual debe especificarse en el momento del aprovisionamiento. Si no se especifica ninguna red virtual durante el aprovisionamiento, la implementación crea una red interna a la que no se puede acceder desde el exterior. Para más información, consulte [Agregar HDInsight a una red virtual existente](https://docs.microsoft.com/azure/hdinsight/hdinsight-plan-virtual-network-deployment#existingvnet).

## <a name="security-and-certificates"></a>Seguridad y certificados

¿Cuáles son las recomendaciones de Microsoft para la protección contra malware en los clústeres de Azure HDInsight?

Para información sobre la protección contra malware, consulte [Microsoft Antimalware para Azure Cloud Services y Virtual Machines](https://docs.microsoft.com/azure/security/fundamentals/antimalware).

### <a name="how-do-i-create-a-keytab-for-an-hdinsight-esp-cluster"></a>¿Cómo creo un archivo keytab para un clúster ESP de HDInsight?

Puede crear un archivo keytab de Kerberos para el nombre de usuario del dominio. Posteriormente, puede usar este archivo para autenticarse en clústeres unidos a un dominio remoto sin necesidad de escribir una contraseña. Observe los caracteres en MAYÚSCULAS en el nombre de dominio:

```
o ktutil 
o ktutil: addent -password -p stXX@ DOMAIN.COM -k 1 -e RC4-HMAC 
o Password for stXX@DOMAIN.COM: provided by lab instructors 
o ktutil: wkt stXX.keytab 
o ktutil: q
```

### <a name="can-i-use-an-existing-azure-active-directory-tenant-to-create-an-hdinsight-cluster-that-has-the-esp"></a>¿Puedo usar un inquilino de Azure Active Directory existente para crear un clúster de HDInsight que tenga ESP?

La habilitación de Azure Active Directory Domain Services (AAD-DS) es un requisito previo para poder crear un clúster de HDInsight que tenga ESP. Hadoop de código abierto se basa en Kerberos (en lugar de OAuth) para proporcionar autenticación.

Para unir máquinas virtuales a un dominio, se requiere un controlador de dominio. AAD-DS es el controlador de dominio administrado. Se considera una extensión de AAD que proporciona todos los requisitos de Kerberos para compilar un clúster de Hadoop seguro de forma administrada. HDInsight como servicio administrado se integra con AAD-DS para proporcionar la seguridad de un extremo a otro.

### <a name="can-i-use-a-self-signed-certificate-in-an-aad-ds-secure-ldap-setup-and-provision-an-esp-cluster"></a>¿Puedo usar un certificado autofirmado en una instalación de LDAP seguro de AAD-DS y aprovisionar un clúster ESP?

Aunque se recomienda el uso de un certificado emitido por una entidad de certificación (CA), también se admite el uso de un certificado autofirmado en ESP.

Para más información, consulte:

* [Habilitar Azure AD DS](https://docs.microsoft.com/azure/hdinsight/domain-joined/apache-domain-joined-configure-using-azure-adds#enable-azure-ad-ds)
* [Tutorial: Configuración de LDAP seguro para un dominio administrado de Azure Active Directory Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/tutorial-configure-ldaps)

### <a name="how-can-i-pull-login-activity-shown-in-ranger"></a>¿Cómo puedo extraer la actividad de inicio de sesión que se muestra en Ranger?

En cuanto a los requisitos de auditoría, Microsoft recomienda habilitar los registros de Azure Monitor, tal como se describe en [Uso de los registros de Azure Monitor para supervisar clústeres de HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-oms-log-analytics-tutorial).

### <a name="can-i-disable-clamscan-on-my-cluster"></a>¿Puedo deshabilitar Clamscan en mi clúster?

Clamscan es el software antivirus que se ejecuta en el clúster de HDInsight y se usa en la seguridad de Azure (azsecd) para proteger los clústeres ante los ataques de virus. Microsoft recomienda encarecidamente que los usuarios no realicen ningún cambio en la configuración predeterminada de Clamscan.

Este proceso no interfiere con otros procesos ni les retira ciclos. Siempre se suspenderá ante otro proceso. Los picos de CPU de Clamscan solo deben aparecer cuando el sistema está inactivo.  

En los escenarios en los que debe controlar la programación, puede seguir estos pasos:

1. Deshabilite la ejecución automática con el siguiente comando:

   ``/usr/local/vbin/azsecd config -s clamav -d Disabled``

2. Cree un trabajo Cron que ejecute este comando como raíz:

   ``/usr/local/bin/azsecd manual -s clamav``

Para más información sobre la configuración y ejecución de un trabajo Cron, consulte [How do I set up a Cron job?](https://askubuntu.com/questions/2368/how-do-i-set-up-a-cron-job) (¿Cómo configuro un trabajo Cron?).

## <a name="storage"></a>Storage

### <a name="can-i-add-an-azure-data-lake-storage-gen2-to-an-existing-hdinsight-cluster-as-an-additional-storage-account"></a>¿Puedo agregar Azure Data Lake Storage Gen2 a un clúster de HDInsight existente como una cuenta de almacenamiento adicional?

No. Actualmente, no se puede agregar Azure Data Lake Storage Gen2 a un clúster de HDInsight existente que use el almacenamiento de blobs como almacenamiento principal. Para más información, consulte [Comparación de opciones de almacenamiento para los clústeres de Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-compare-storage-options). 

### <a name="how-can-i-find-the-currently-linked-service-principal-for-a-data-lake-storage-account"></a>¿Cómo puedo encontrar la entidad de servicio vinculada actualmente para una cuenta de Data Lake Storage?

Puede comprobar esta configuración revisando el acceso de Data Lake Storage Gen1 en las propiedades del clúster del portal. Para más información, consulte [Comprobación de la configuración del clúster](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-hdinsight-hadoop-use-portal#verify-cluster-set-up).
 
¿Cómo puedo calcular el uso de cuentas de almacenamiento y contenedores de blobs para mis clústeres de HDInsight?
Puede usar uno de los siguientes procedimientos:

* [Uso de PowerShell](https://docs.microsoft.com/azure/storage/scripts/storage-blobs-container-calculate-size-powershell)
* También puede conocer el tamaño de la carpeta /user/hive/.Trash/ en los clústeres de HDI mediante el siguiente comando:

   ``hdfs dfs -du -h /user/hive/.Trash/``

### <a name="how-can-i-set-up-auditing-for-my-blob-storage-account"></a>¿Cómo puedo configurar la auditoría para mi cuenta de almacenamiento de blobs?

Para auditar cuentas de almacenamiento de blobs, tiene que configurar la supervisión de la cuenta de almacenamiento de blobs mediante el procedimiento que se documenta en [Supervisión de una cuenta de almacenamiento en Azure Portal](https://docs.microsoft.com/azure/storage/common/storage-monitor-storage-account). Un registro de auditoría de HDFS solo proporciona información de auditoría para el sistema de archivos HDFS local (hdfs://mycluster).  No incluye las operaciones que se realizan en el almacenamiento remoto.

### <a name="how-can-i-transfer-files-between-a-blob-container-and-an-hdinsight-head-node"></a>¿Cómo puedo transferir archivos entre un contenedor de blobs y un nodo principal de HDInsight?

Puede transferir archivos entre un contenedor de blobs y un nodo principal de HDInsight mediante la ejecución de un script de Shell en el nodo principal, similar al ejemplo siguiente:

```
for i in cat filenames.txt

do
         hadoop fs -get $i <local destination>
done
```
 
> [!NOTE]
> El archivo *filenames.txt* incluirá la ruta de acceso absoluta de los archivos de los contenedores de blobs.
 
### <a name="are-there-any-ranger-plugins-for-storage"></a>¿Hay algún complemento de Ranger para el almacenamiento?

Actualmente no existe ningún complemento de Ranger para el almacenamiento de blobs, Azure Data Lake Storage (ADLS) Gen1 ni Azure Data Lake Storage Gen2. En el caso de los clústeres ESP, use ADLS como procedimiento recomendado y establezca los permisos específicos de forma manual en el nivel del sistema de archivos mediante las herramientas de HDFS. Además, cuando se usa ADLS, los clústeres ESP realizan parte del control de acceso al sistema de archivos mediante AAD en el nivel de clúster. 

Debe poder usar el Explorador de Azure Storage para asignar directivas de acceso a datos a los grupos de seguridad en los que se encuentran los usuarios, mediante los procedimientos que se describen en los siguientes artículos:

* [¿Cómo puedo establecer los permisos para que los usuarios de Azure AD consulten los datos en Data Lake Storage Gen2 mediante Hive u otros servicios?](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2#how-do-i-set-permissions-for-azure-ad-users-to-query-data-in-data-lake-storage-gen2-by-using-hive-or-other-services)
* [Establecimiento de permisos en el nivel de archivo y directorio mediante el Explorador de Azure Storage con Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-how-to-set-permissions-storage-explorer)

### <a name="can-i-increase-hdfs-storage-on-a-cluster-without-increasing-the-disk-size-of-worker-nodes"></a>¿Puedo aumentar el almacenamiento de HDFS en un clúster sin aumentar el tamaño del disco de los nodos de trabajo?

No, no puede aumentar el tamaño del disco de ningún nodo de trabajo. La única manera de aumentar el tamaño del disco es quitar el clúster y volver a crearlo en máquinas virtuales de trabajo de mayor tamaño.

Microsoft no recomienda usar HDFS para almacenar los datos de HDInsight, ya que se eliminan cuando se elimina el clúster. En su lugar, se recomienda almacenar los datos en Azure. El escalado vertical del clúster también puede agregar más capacidad al clúster de HDInsight.

## <a name="edge-nodes"></a>Nodos perimetrales

### <a name="can-i-add-an-edge-node-after-the-cluster-has-been-created"></a>¿Puedo agregar un nodo perimetral una vez creado el clúster?

Sí, puede agregar un nodo perimetral vacío a un clúster de HDInsight existente o a uno nuevo al crear el clúster.

Para más información, consulte [Uso de nodos perimetrales vacíos en clústeres de Apache Hadoop en HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-apps-use-edge-node).

### <a name="how-can-i-connect-to-an-edge-node"></a>¿Cómo puedo conectarme a un nodo perimetral?

Después de crear un nodo perimetral, puede conectarse a él mediante SSH en el puerto 22. Puede encontrar el nombre del nodo perimetral en el portal del clúster (por lo general, los nombres terminan en "-ed").

### <a name="why-are-persisted-scripts-not-running-automatically-on-newly-created-edge-nodes"></a>¿Por qué los scripts persistentes no se ejecutan automáticamente en los nodos perimetrales recién creados?

Los scripts persistentes se usan para personalizar los nuevos nodos de trabajo que se agregan al clúster a través de operaciones de escalado y no se aplican a los nodos perimetrales.

## <a name="rest-api"></a>API DE REST

### <a name="what-are-the-rest-api-calls-to-pull-a-tez-query-view-from-the-cluster"></a>¿Cuáles son las llamadas de la API REST para extraer una vista de consulta de Tez del clúster?

Puede usar los siguientes puntos de conexión de REST para extraer la información necesaria; la respuesta estará en formato JSON. Se pueden usar los encabezados de autenticación básica para realizar estas solicitudes.

* Vista de consulta de Tez: https://`<cluster name>`.azurehdinsight.net/ws/v1/timeline/HIVE_QUERY_ID/

* Vista de DAG de Tez: https://`<cluster name>`.azurehdinsight.net/ws/v1/timeline/TEZ_DAG_ID/

### <a name="how-do-i-retrieve-the-configuration-details-from-hdi-cluster-by-using-an-azure-active-directory-user"></a>¿Cómo recupero los detalles de configuración del clúster de HDI con un usuario de Azure Active Directory?

Para negociar los tokens de autenticación adecuados con el usuario de AAD, vaya a través de la puerta de enlace con el siguiente formato:

* https://`<cluster dnsname>`.azurehdinsight.net/api/v1/clusters/testclusterdem/stack_versions/1/repository_versions/1 

### <a name="how-do-i-use-ambari-restful-api-to-monitor-yarn-performance"></a>¿Cómo uso la API RESTful de Ambari para supervisar el rendimiento de YARN?

Si llama al comando curl en la misma red virtual o en una red virtual emparejada, use el siguiente comando:

``curl -u <cluster login username> -sS -G http://headnodehost:8080/api/v1/clusters/anssenllap/services/YARN/components/NODEMANAGER?fields=metrics/cpu``
 
Si llama al comando desde fuera de la red virtual o desde una red virtual no emparejada, el formato del comando es:
 
* Para un clúster que no sea ESP: ``curl -u <cluster login username> -sS -G https://ClusterName.azurehdinsight.net/api/v1/clusters/ClusterName/services/YARN/components/NODEMANAGER?fields=metrics/cpu``
 
* Para un clúster ESP: ``curl -u <cluster login username> -sS -G https://ClusterName.azurehdinsight.net/api/v1/clusters/ClusterName/services/YARN/components/NODEMANAGER?fields=metrics/cpu``

> [!NOTE]
> Curl le solicitará una contraseña. Debe escribir una contraseña válida para el nombre de usuario de inicio de sesión del clúster.

## <a name="billing"></a>Facturación

### <a name="how-much-does-it-cost-to-deploy-an-hdinsight-cluster"></a>¿Cuánto cuesta implementar un clúster de HDInsight?

Para más información sobre los precios y las preguntas más frecuentes relacionadas con la facturación, consulte la página [Precios de Azure HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).

### <a name="how-do-i-cancel-my-subscription"></a>¿Cómo cancelo mi suscripción?

Para información sobre cómo cancelar la suscripción, consulte [Cancelación de su suscripción de Azure](https://docs.microsoft.com/azure/billing/billing-how-to-cancel-azure-subscription).

### <a name="for-pay-as-you-go-subscriptions-what-happens-after-i-cancel-my-subscription"></a>En el caso de las suscripciones de pago por uso, ¿qué ocurre después de cancelar mi suscripción?

Para información sobre la suscripción después de su cancelación, consulte [¿Qué ocurre después de cancelar la suscripción?](https://docs.microsoft.com/azure/billing/billing-how-to-cancel-azure-subscription#what-happens-after-i-cancel-my-subscription)

## <a name="hive"></a>Hive

### <a name="why-does-the-hive-version-appear-as-121000-instead-of-21-in-the-ambari-ui-even-though-i-am-running-an-hdinsight-36-cluster"></a>¿Por qué aparece la versión de Hive como 1.2.1000 en lugar de 2.1 en la interfaz de usuario de Ambari, a pesar de ejecutar un clúster de HDInsight 3.6?

Aunque solo aparece 1.2 en la interfaz de usuario de Ambari, HDInsight 3.6 contiene Hive 1.2 y Hive 2.1.

## <a name="other-faq"></a>Otras preguntas más frecuentes

### <a name="what-does-hdinsight-offer-in-terms-of-real-time-stream-processing-capabilities"></a>¿Qué ofrece HDInsight en cuanto a las capacidades de procesamiento de flujos en tiempo real?

Para información sobre las capacidades de integración del procesamiento de flujos en Azure HDInsight, consulte [Selección de una tecnología de procesamiento de flujos en Azure](https://docs.microsoft.com/azure/architecture/data-guide/technology-choices/stream-processing).

### <a name="is-there-a-way-to-dynamically-terminate-the-head-node-of-the-cluster-when-the-cluster-is-idle-for-a-specific-period"></a>¿Hay alguna manera de terminar de forma dinámica el nodo principal del clúster cuando este está inactivo durante un período específico?

No, no puede terminar de forma dinámica el nodo principal del clúster. Puede usar Azure Data Factory para este escenario.
