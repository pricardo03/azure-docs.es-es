---
title: Listado de errores de creación de clústeres
description: Obtenga una lista de los errores que pueden aparecer al crear clústeres.
ms.reviewer: hrasheed
author: karkrish
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: troubleshooting
ms.date: 11/19/2019
ms.author: v-todmc
ms.openlocfilehash: e537014f741196024c24dd6ee98af0d8af2e1b31
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75475934"
---
# <a name="hdinsight-cluster-creation-errors"></a>HDInsight: Errores de creación de clúster

En este artículo se describen las soluciones a los errores que se producen al crear clústeres. 

> [!NOTE]
> Los tres primeros errores de la siguiente lista se producen debido a errores de validación cuando un producto de HDInsight usa la clase CsmDocument_2_0.



## <a name="error-codedeploymentdocument-csmdocument_2_0-failed-the-validation"></a>Código de error: Error de DeploymentDocument "CsmDocument_2_0" para su validación.

### <a name="error-script-action-location-cannot-be-accessed-uriscript-action-url"></a>**Error**: No se puede tener acceso a la ubicación de la acción de script: \<URL DE LA ACCIÓN DE SCRIPT\>

**Mensaje de error: "El servidor remoto devolvió un error: 404 No encontrado."**

### <a name="cause"></a>Causa
No se puede acceder desde el servicio HDInsight a la dirección URL de la acción de script proporcionada como parte de la solicitud de creación del clúster. El mensaje "ErrorMessage" se recibe al intentar acceder a la acción de script.

### <a name="resolution"></a>Solución 
  - En el caso de una dirección URL http/https, esto se puede comprobar intentando abrirla en una ventana del explorador de incógnito. 
  - Con una dirección URL de WASB, asegúrese de que el script existe en la cuenta de almacenamiento que se especifica en la solicitud. Asegúrese de que la clave de almacenamiento de esta cuenta de almacenamiento es exacta. 
  - Si es una dirección URL de ADLS, asegúrese de que el script existe en la cuenta de almacenamiento.

---

## <a name="error-codedeploymentdocument-csmdocument_2_0-failed-the-validation"></a>Código de error: Error de DeploymentDocument "CsmDocument_2_0" para su validación.

### <a name="error-script-action-location-cannot-be-accessed-uri-script-action-url"></a>**Error**: No se puede tener acceso a la ubicación de la acción de script: \<URL DE LA ACCIÓN DE SCRIPT\>

**Mensaje de error: El URI de script especificado \<URI DE SCRIPT\> está en ADLS, pero este clúster no tiene ninguna entidad de seguridad de Data Lake Storage.**

### <a name="cause"></a>Causa
No se puede acceder desde el servicio HDInsight a la dirección URL de la acción de script proporcionada como parte de la solicitud de creación del clúster. El mensaje "ErrorMessage" se recibe al intentar acceder a la acción de script. 

### <a name="resolution"></a>Solución

Asegúrese de que la cuenta de Azure Data Lake Store Gen 1 correspondiente se ha agregado al clúster. La entidad de servicio que se usa para acceder a la cuenta de Azure Data Lake Store Gen 1 también se agrega al clúster. 

---

## <a name="error-code-deploymentdocument-csmdocument_2_0-failed-the-validation"></a>Código de error: Error de DeploymentDocument "CsmDocument_2_0" para su validación.

### <a name="error-vm-size-customer-specified-vm-size-provided-in-the-request-is-invalid-or-not-supported-for-role-role-valid-values-are-valid-vm-size-for-role"></a>**Error**: El tamaño de la máquina virtual "\<TAMAÑO DE MÁQUINA VIRTUAL ESPECIFICADA POR EL CLIENTE\>" proporcionado en la solicitud no es válido o no se admite para el rol "\<ROL\>". Los valores válidos son: \<TAMAÑO DE MÁQUINA VIRTUAL VÁLIDO PARA EL ROL\>.

### <a name="cause"></a>Causa
El tamaño de máquina virtual especificado por el cliente no se permite para el rol. Esto puede ser así porque el valor de tamaño de la máquina virtual no funciona como se esperaba o no es adecuado para el rol de equipo. 

### <a name="resolution"></a>Solución
El mensaje de error muestra los valores válidos de tamaño de máquina virtual. Seleccione uno de estos valores válidos e intente realizar de nuevo la solicitud de creación de clúster. 

---

## <a name="error-codeinvalidvirtualnetworkid"></a>Código de error: InvalidVirtualNetworkId  

### <a name="error-the-virtualnetworkid-is-not-valid-virtualnetworkid-user_virtualnetworkid"></a>**Error**: El parámetro VirtualNetworkId no es válido. VirtualNetworkId "\<VIRTUALNETWORKID_USUARIO\>"*

### <a name="cause"></a>Causa
El valor de **VirtualNetworkId** especificado durante la creación del clúster no tiene el formato correcto. 

### <a name="resolution"></a>Solución
Asegúrese de que **VirtualNetworkId** y la subred tienen el formato correcto. Para obtener el valor de **VirtualnetworkId**, vaya a Azure Portal, seleccione la red virtual y, luego, seleccione **Propiedades** en el menú. La propiedad **ResourceID** es el valor de **VirtualNetworkId**. 

Un ejemplo de identificador de red virtual es este: 

"/subscriptions/c15fd9b8-e2b8-1d4e-aa85-2e668040233b/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/myvnet" 

---

## <a name="error-code-customizationfailederrorcode"></a>Código de error: CustomizationFailedErrorCode

### <a name="error-cluster-deployment-failed-due-to-an-error-in-the-custom-script-action-failed-actions-script_name-please-go-to-ambari-ui-to-further-debug-the-failure"></a>**Error**: No se pudo realizar la implementación de clúster debido a un error en la acción del script personalizado. Failed Actions: \<NOMBRE_SCRIPT\>. Vaya a la interfaz de usuario de Ambari para seguir depurando el error.

### <a name="cause"></a>Causa
El script personalizado del usuario que se proporcionó durante la solicitud de creación de clúster se ejecuta después de que el clúster se haya implementado correctamente. Este código de error indica que se detectó un error al ejecutar este script personalizado con el nombre \<NOMBRE_SCRIPT\>.   

### <a name="resolution"></a>Solución
Dado que se trata del script personalizado del usuario, este deberá solucionar el problema y volver a ejecutar el script si es necesario. Para solucionar el error del script, examine los registros de la carpeta /var/lib/ambari-agent/*. También puede abrir la página de operaciones en la interfaz de usuario de Ambari y seleccionar la operación **run_customscriptionaction** para ver los detalles del error. 

---

## <a name="error-codeinvaliddocumenterrorcode"></a>Código de error: InvalidDocumentErrorCode

### <a name="error-the-meta_store_type-metastore-schema-version-metastore_major_version-in-database-database_name-is-incompatible-with-cluster-version-cluster_version"></a>**Error**: La versión de esquema de metastore de \<TIPO_ALMACÉN_META\> \<VERSIÓN_PRINCIPAL_METASTORE\> en la base de datos \<NOMBRE_BASE_DE_DATOS\> no es compatible con la versión del clúster \<VERSIÓN_CLÚSTER\>

### <a name="cause"></a>Causa
El repositorio metastore personalizado es incompatible con la versión de clúster de HDInsight seleccionada. Actualmente, el clúster de HDInsight 4.0 admite únicamente la versión de metastore 3.*x*, mientras que HDInsight 3.6 no admite la versión de metastore 3.*x* o posterior. 

### <a name="resolution"></a>Solución
Asegúrese de usar solo las versiones de metastore que sean compatibles con cada versión de clúster de HDInsight. Tenga en cuenta que, si no se especifica un repositorio metastore personalizado, HDInsight creará uno internamente, pero este se eliminará de manera automática tras la eliminación del clúster. 

---

## <a name="error-code-failedtoconnectwithclustererrorcode"></a>Código de error: FailedToConnectWithClusterErrorCode 

### <a name="error-unable-to-connect-to-cluster-management-endpoint-to-perform-scaling-operation-verify-that-network-security-rules-are-not-blocking-external-access-to-the-cluster-and-that-the-cluster-manager-ambari-ui-can-be-successfully-accessed"></a>**Error**: No se puede conectar con el punto de conexión de administración de clúster para realizar la operación de escalado. Compruebe que no haya reglas de seguridad de red que bloqueen el acceso externo al clúster y que se pueda acceder correctamente a la interfaz de usuario del administrador de clústeres (Ambari).

### <a name="cause"></a>Causa
Tiene una regla de firewall en el grupo de seguridad de red (NSG) que está bloqueando la comunicación del clúster con los servicios críticos de mantenimiento y administración de Azure. 

### <a name="resolution"></a>Solución
Si tiene previsto usar  **grupos de seguridad de red**  para controlar el tráfico de red, realice las siguientes acciones antes de instalar HDInsight: 
  - Identificar la región de Azure que va a usar para HDInsight. 
  - Identificar las direcciones IP requeridas para HDInsight. Para más información, consulte [Direcciones IP de administración de HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-management-ip-addresses). 
    - Cree o modifique los grupos de seguridad de red de la subred en la que tiene previsto instalar HDInsight. 
    - **Grupos de seguridad de red:** Permita el tráfico  **entrante**  en el puerto  **443**  desde las direcciones IP. De este modo, los servicios de administración de HDInsight se podrán comunicar con el clúster desde fuera de la red virtual. 

---

## <a name="error-code-storagepermissionsblockedformsi"></a>Código de error: StoragePermissionsBlockedForMsi  

### <a name="error-the-managed-identity-does-not-have-permissions-on-the-storage-account-please-verify-that-storage-blob-data-owner-role-is-assigned-to-the-managed-identity-for-the-storage-account-storage-subscriptions-subscription-id-resourcegroups-resource-group-name-providersmicrosoftstoragestorageaccounts-storage-account-name-managed-identity-subscriptions-subscription-id-resourcegroups--resource-group-name-providersmicrosoftmanagedidentityuserassignedidentities-user-managed-identity-name"></a>**Error**: La identidad administrada no tiene permisos en la cuenta de almacenamiento. Compruebe que el rol "Propietario de datos de Storage Blob" está asignado a la identidad administrada en la cuenta de almacenamiento. Almacenamiento: /subscriptions/ \<identificador de suscripción\> /resourceGroups/\<nombre del grupo de recursos\> /providers/Microsoft.Storage/storageAccounts/ \<nombre de la cuenta de almacenamiento\>; identidad administrada: /subscriptions/ \<identificador de suscripción\> /resourceGroups/ /\<nombre del grupo de recursos\> /providers/Microsoft.ManagedIdentity/userAssignedIdentities/ \<nombre de identidad administrada de usuario\>

### <a name="cause"></a>Causa
No se han proporcionado los permisos necesarios para **administrar la identidad**. La **identidad administrada asignada por el usuario** no tenía el rol Colaborador de datos de Storage Blob en la cuenta de almacenamiento de ADLS Gen2. 

### <a name="resolution"></a>Solución

Abra Azure Portal, vaya a la cuenta de almacenamiento, busque en **Control de acceso (IAM)** y asegúrese de que los roles Colaborador de datos de Storage Blob o Propietario de datos de Storage Blob tienen acceso "Asignado" a la  **identidad administrada asignada por el usuario** en la suscripción. Para más información, vea [Configuración de permisos para la identidad administrada en la cuenta de Data Lake Storage Gen2](hdinsight-hadoop-use-data-lake-storage-gen2.md). 

---

## <a name="error-code-invalidnetworksecuritygroupsecurityrules"></a>Código de error: InvalidNetworkSecurityGroupSecurityRules  

### <a name="error-the-security-rules-in-the-network-security-group-subscriptionssubscriptionidresourcegroupsresource-group-name-defaultprovidersmicrosoftnetworknetworksecuritygroupsnetwork-security-group-name-configured-with-subnet-subscriptionssubscriptionidresourcegroupsresource-group-name-rg-westeurope-vnet-tomtom-defaultprovidersmicrosoftnetworkvirtualnetworksvirtual-network-namesubnetssubnet-name"></a>**Error**: Las reglas de seguridad del grupo de seguridad de red /subscriptions/\<identificador de suscripción>\/resourceGroups/<Resource Group name> default/providers/Microsoft.Network/networkSecurityGroups/\<nombre del grupo de seguridad de red\> configuradas con la subred /subscriptions/\<identificador de suscripción>\/ resourceGroups/\<recursos> RG-westeurope-vnet-tomtom-default\/providers/Microsoft.Network/virtualNetworks/\<nombre de red virtual>\/subnets/\<nombre de subred\> 
no permiten la conectividad entrante o saliente necesaria. Para obtener más información, visite [Planificación de una red virtual para Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-plan-virtual-network-deployment) o póngase en contacto con el servicio de soporte técnico.**

### <a name="cause"></a>Causa
Si usa grupos de seguridad de red (NSG) o rutas definidas por el usuario (UDR) para controlar el tráfico entrante al clúster de HDInsight, debe asegurarse de que el clúster puede comunicarse con los servicios críticos de mantenimiento y administración de Azure.

### <a name="resolution"></a>Solución
Si tiene previsto usar  **grupos de seguridad de red**  para controlar el tráfico de red, realice las siguientes acciones antes de instalar HDInsight: 
  - Identifique la región de Azure que va a usar para HDInsight y cree una lista segura de las direcciones IP de su región: [Servicios de mantenimiento y administración: regiones específicas](https://docs.microsoft.com/azure/hdinsight/hdinsight-management-ip-addresses#health-and-management-services-specific-regions).
  - Identifique las direcciones IP que HDInsight necesita. Para más información, vea  [Direcciones IP de administración de HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-management-ip-addresses). 
  - Cree o modifique los grupos de seguridad de red de la subred en la que tiene previsto instalar HDInsight. **Grupos de seguridad de red**: permita el tráfico entrante en el puerto  **443**  desde las direcciones IP. Esto garantizará que los servicios de administración de HDInsight puedan comunicarse con el clúster desde fuera de la red virtual.
  
---

## <a name="error-code-cluster-setup-failed-to-install-components-on-one-or-more-hosts"></a>Código de error: El programa de instalación del clúster no pudo instalar los componentes en uno o más hosts.

###  <a name="errorcluster-setup-failed-to-install-components-on-one-or-more-hosts-please-retry-your-request"></a>**Error**: El programa de instalación del clúster no pudo instalar los componentes en uno o más hosts. Vuelva a intentar realizar la solicitud. 

### <a name="cause"></a>Causa 
Este error suele aparecer cuando hay un problema transitorio o Azure se ha interrumpido. 

### <a name="resolution"></a>Solución

Consulte la página [Estado de Azure](https://status.azure.com/status) para ver si hay posibles interrupciones de Azure que afecten a la implementación del clúster. Si no existen interrupciones, intente implementar el clúster de nuevo. 

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la solución de errores de creación de clústeres, vea [Solución de errores de creación de clústeres con Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/hadoop/hdinsight-troubleshoot-cluster-creation-fails).
