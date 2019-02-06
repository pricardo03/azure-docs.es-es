---
title: Uso de Azure Data Lake Storage Gen2 con clústeres de Azure HDInsight
description: Obtenga información sobre cómo usar Azure Data Lake Storage Gen2 con clústeres de Azure HDInsight.
services: hdinsight
author: hrasheed-msft
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: howto
ms.date: 01/10/2019
ms.author: hrasheed
ms.openlocfilehash: a44e53d7a32ab151fa951d1bc89b741390a70dfb
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2019
ms.locfileid: "55464796"
---
# <a name="use-azure-data-lake-storage-gen2-with-azure-hdinsight-clusters"></a>Uso de Azure Data Lake Storage Gen2 con clústeres de Azure HDInsight

Azure Data Lake Storage (Data Lake Storage) Gen2 es un conjunto de funcionalidades dedicado al análisis de macrodatos, basado en Azure Blob Storage. Data Lake Storage Gen2 es el resultado de la combinación de las funcionalidades de Azure Blob Storage y Azure Data Lake Storage Gen1. El resultado es un servicio que ofrece características de Azure Data Lake Storage Gen1, como la semántica del sistema de archivos, la seguridad de nivel de directorio y archivo y la escala junto con las funcionalidades de recuperación ante desastres o alta disponibilidad, y de almacenamiento por niveles de bajo costo de Azure Blob Storage.

## <a name="data-lake-storage-gen2-availability"></a>Disponibilidad de Data Lake Storage Gen2

Azure Data Lake Storage Gen2 está disponible como opción de almacenamiento para casi todos los tipos de clúster de Azure HDInsight como una cuenta de almacenamiento predeterminada o adicional. Sin embargo, HBase solo puede tener una cuenta de Data Lake Storage Gen2.

> [!Note] 
> Una vez que selecciona Data Lake Storage Gen2 como el **tipo de almacenamiento principal**, no puede seleccionar una cuenta de Data Lake Storage Gen1 como almacenamiento adicional.

## <a name="creating-an-hdinsight-cluster-with-data-lake-storage-gen2"></a>Creación de un clúster de HDInsight con Data Lake Storage Gen2

## <a name="using-the-azure-portal"></a>Uso de Azure Portal

Para crear un clúster de HDInsight, que usa Data Lake Storage Gen2 para almacenamiento, use estos pasos para crear una cuenta de Data Lake Storage Gen2 correctamente configurada.

1. Cree una identidad administrada asignada por el usuario si todavía no tiene una. Consulte [Creación, enumeración, eliminación o asignación de un rol a una identidad administrada asignada por el usuario mediante Azure Portal](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md#create-a-user-assigned-managed-identity).

    ![Crear una identidad administrada asignada por el usuario](./media/hdinsight-hadoop-data-lake-storage-gen2/create-user-assigned-managed-identity-portal.png)

1. Cree una cuenta de almacenamiento de Azure Data Lake Storage Gen2. Asegúrese de que esté habilitada la opción **Sistema de archivos jerárquico**. Consulte [Quickstart: Creación de una cuenta de almacenamiento de Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-quickstart-create-account.md) para más detalles.

    ![Captura de pantalla en la que se muestra la creación de una cuenta de almacenamiento en Azure Portal](./media/hdinsight-hadoop-data-lake-storage-gen2/azure-data-lake-storage-account-create-advanced.png)
 
1. Asigne la identidad administrada al rol **Colaborador de dato de blobs de almacenamiento (versión preliminar)** de la cuenta de almacenamiento. Consulte [Administración de los derechos de acceso a los datos de la cola y el blob de Azure con RBAC (versión preliminar)](../storage/common/storage-auth-aad-rbac.md#assign-a-role-scoped-to-the-storage-account-in-the-azure-portal)

    1. En [Azure Portal](https://portal.azure.com), vaya a la cuenta de almacenamiento.
    1. Seleccione la cuenta de almacenamiento y, después, seleccione **Control de acceso (IAM)** para mostrar la configuración de control de acceso de la cuenta. Seleccione la pestaña **Asignaciones de roles** para ver la lista de asignaciones de roles.
    
        ![Captura de pantalla que muestra la configuración de control de acceso del almacenamiento](./media/hdinsight-hadoop-data-lake-storage-gen2/portal-access-control.png)
    
    1. Haga clic en el botón **Agregar asignación de roles** para agregar un rol nuevo.
    1. En la ventana **Agregar asignación de roles**, seleccione el rol **Colaborador de datos de blobs de almacenamiento (versión preliminar)**. Luego, seleccione la suscripción que tiene la identidad administrada y la cuenta de almacenamiento. A continuación, busque la identidad administrada asignada por el usuario que creó anteriormente. Por último, seleccione la identidad administrada y esta se mostrará en **Miembros seleccionados**.
    
        ![Captura de pantalla que muestra cómo asignar un rol de RBAC](./media/hdinsight-hadoop-data-lake-storage-gen2/add-rbac-role2.png)
    
    1. Haga clic en **Save**(Guardar). La identidad asignada por el usuario que seleccionó ahora aparece en el rol **Colaborador**.

    1. Una vez completada la configuración inicial, puede crear un clúster a través del portal. El clúster debe estar en la misma región de Azure que la cuenta de almacenamiento. En la sección **Almacenamiento** del menú de creación del clúster, seleccione estas opciones:
        
        * En **Tipo de almacenamiento principal**, haga clic en **Azure Data Lake Storage Gen2**.
        * En **Seleccionar una cuenta de almacenamiento**, busque y seleccione la cuenta de almacenamiento de Data Lake Storage Gen2 recién creada.
        
            ![Configuración de almacenamiento para usar Data Lake Storage Gen2 con Azure HDInsight](./media/hdinsight-hadoop-data-lake-storage-gen2/primary-storage-type-adls-gen2.png)
        
        * En **Identidad**, seleccione la suscripción correcta y la identidad administrada asignada por el usuario recién creada.
        
            ![Configuración de identidad para usar Data Lake Storage Gen2 con Azure HDInsight](./media/hdinsight-hadoop-data-lake-storage-gen2/managed-identity-cluster-creation.png)

### <a name="using-a-resource-manager-template-deployed-with-azure-cli"></a>Uso de una plantilla de Resource Manager implementada con la CLI de Azure

Puede descargar un [archivo de plantilla de ejemplo aquí](https://github.com/Azure-Samples/hdinsight-data-lake-storage-gen2-templates/blob/master/hdinsight-adls-gen2-template.json) y un [archivo de parámetros de ejemplo aquí](https://github.com/Azure-Samples/hdinsight-data-lake-storage-gen2-templates/blob/master/parameters.json). Antes de usar la plantilla, sustituya el identificador real de la suscripción de Azure para la cadena `<SUBSCRIPTION_ID>`. Además, sustituya la contraseña elegida para la cadena `<PASSWORD>` para establecer tanto la contraseña de inicio de sesión que va a usar para iniciar sesión en el clúster, como la contraseña SSH.

El siguiente fragmento de código realiza los pasos iniciales siguientes:

1. Inicie sesión en su cuenta de Azure.
1. Establezca la suscripción activa donde se realizarán las operaciones de creación.
1. Cree un nuevo grupo de recursos para las nuevas actividades de implementación `hdinsight-deployment-rg`.
1. Cree un usuario de Managed Service Identity (MSI) `test-hdinsight-msi`.
1. Agregue una extensión a la CLI de Azure para usar las características para Data Lake Storage Gen2.
1. Cree una nueva cuenta de Data Lake Storage Gen2 `hdinsightadlsgen2`, usando la marca `--hierarchical-namespace true`.

```azurecli
az login
az account set --subscription <subscription_id>

#create resource group
az group create --name hdinsight-deployment-rg --location eastus

# Create managed identity
az identity create -g hdinsight-deployment-rg -n test-hdinsight-msi

az extension add --name storage-preview

az storage account create --name hdinsightadlsgen2 \
    --resource-group hdinsight-deployment-rg \
    --location eastus --sku Standard_LRS \
    --kind StorageV2 --hierarchical-namespace true
```

Luego, inicie sesión en el portal y agregue el nuevo MSI al rol **Colaborador de datos de blobs de almacenamiento (versión preliminar)** en la cuenta de almacenamiento, como se describe en el paso 3 anterior en [Uso de Azure Portal](hdinsight-hadoop-use-data-lake-storage-gen2.md#using-the-azure-portal).

Después de completar la asignación de roles de MSI en el portal, implemente la plantilla con el siguiente fragmento de código.

```azurecli
az group deployment create --name HDInsightADLSGen2Deployment \
    --resource-group hdinsight-deployment-rg \
    --template-file hdinsight-adls-gen2-template.json \
    --parameters parameters.json
```

## <a name="access-control-for-data-lake-storage-gen2-in-hdinsight"></a>Control de acceso de Data Lake Storage Gen2 en HDInsight

### <a name="what-kinds-of-permissions-does-data-lake-storage-gen2-support"></a>¿Qué tipos de permisos admite Data Lake Storage Gen2?

Azure Data Lake Storage Gen2 implementa un modelo de control de acceso compatible con el control de acceso basado en rol (RBAC) de Azure y las listas de control de acceso (ACL) tipo POSIX. Data Lake Storage Gen1 solo admitía las listas de control de acceso para controlar el acceso a los datos.

El control de acceso basado en rol (RBAC) de Azure usa las asignaciones de roles para aplicar de forma eficaz conjuntos de permisos a usuarios, grupos y entidades de servicio para los recursos de Azure. Normalmente, los recursos de Azure están limitados a los recursos de nivel superior (por ejemplo, cuentas de Azure Storage). En el caso de Azure Storage, y también Azure Data Lake Storage Gen2, este mecanismo se ha ampliado al recurso de sistema de archivos.

 Para más información sobre los permisos de archivo con RBAC, consulte [Control de acceso basado en rol (RBAC) de Azure](../storage/blobs/data-lake-storage-access-control.md#azure-role-based-access-control-rbac).

Para más información sobre los permisos de archivo con las listas de control de acceso, consulte [Listas de control de acceso en archivos y directorios](../storage/blobs/data-lake-storage-access-control.md#access-control-lists-on-files-and-directories).


### <a name="how-do-i-control-access-to-my-data-in-gen2"></a>¿Cómo puedo controlar el acceso a mis datos en Gen2?

La capacidad que el clúster de HDInsight tiene de acceder a los archivos en Data Lake Storage Gen2 se controla mediante las identidades administradas. Una identidad administrada es una identidad registrada en Azure AD con credenciales administradas por Azure. No es necesario registrar entidades de servicio en Azure AD ni mantener credenciales, tales como los certificados.

Existen dos tipos de identidades administradas para los servicios de Azure: asignadas por el sistema y asignadas por el usuario. Azure HDInsight usa identidades administradas asignadas por el usuario para acceder a Azure Data Lake Storage Gen2. Las identidades administrada asignadas por el usuario se crean como recursos de Azure independientes. Mediante un proceso de creación, Azure crea una identidad en el inquilino de Azure AD de confianza para la suscripción que se utiliza. Una vez creada la identidad, esta puede asignarse a una o varias instancias de servicio de Azure. El ciclo de vida de una identidad asignada por el usuario no se administra junto con el ciclo de vida de las instancias de servicio de Azure a las que se asigna. Para más información sobre las identidades administradas, consulte [¿Cómo funcionan las identidades administradas para los recursos de Azure?](../active-directory/managed-identities-azure-resources/overview.md#how-does-the-managed-identities-for-azure-resources-worka-namehow-does-it-worka).

### <a name="how-do-i-set-permissions-for-azure-ad-users-to-query-data-in-data-lake-storage-gen2-using-hive-or-other-services"></a>¿Cómo puedo establecer permisos para que los usuarios de Azure AD consulten datos en Data Lake Storage Gen2 mediante Hive u otros servicios?

Use grupos de seguridad de Azure AD como la entidad de seguridad asignada en las listas de control de acceso. No asigne directamente permisos de acceso a archivos a usuarios individuales ni a entidades de servicio. Cuando usa los grupos de seguridad de AD para controlar el flujo de los permisos, puede agregar y quitar usuarios o entidades de servicio sin volver a aplicar las ACL a la estructura de todo un directorio. Solo tiene que agregar o quitar los usuarios del grupo de seguridad de Azure AD adecuado. Las ACL no se heredan y, por ello, para volver a aplicar las ACL es necesario actualizarlas en cada archivo y subdirectorio.

## <a name="next-steps"></a>Pasos siguientes

* [Uso de Azure Data Lake Storage Gen2 (versión preliminar) con clústeres de Azure HDInsight](../storage/blobs/data-lake-storage-use-hdi-cluster.md)
* [Azure HDInsight integration with Data Lake Storage Gen2 preview - ACL and security update](https://azure.microsoft.com/blog/azure-hdinsight-integration-with-data-lake-storage-gen-2-preview-acl-and-security-update/) (Integración de Azure HDInsight con Data Lake Storage Gen2 [versión preliminar]: ACL y actualización de seguridad)
* [Introducción a Azure Data Lake Storage Gen2 (versión preliminar)](../storage/blobs/data-lake-storage-introduction.md)
