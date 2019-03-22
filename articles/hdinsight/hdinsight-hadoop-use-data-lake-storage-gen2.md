---
title: Uso de Azure Data Lake Storage Gen2 con clústeres de Azure HDInsight
description: Aprenda a usar Azure Data Lake Storage Gen2 con clústeres de HDInsight de Azure.
services: hdinsight
author: hrasheed-msft
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: hrasheed
ms.openlocfilehash: 4e8649096d4f7de49c9cf0d569422919f865bb3b
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58094099"
---
# <a name="use-azure-data-lake-storage-gen2-with-azure-hdinsight-clusters"></a>Uso de Azure Data Lake Storage Gen2 con clústeres de Azure HDInsight

Azure Data Lake Storage Gen2 es un servicio de almacenamiento en la nube dedicado para análisis de macrodatos basados en almacenamiento de blobs de Azure. Data Lake Storage Gen2 combina las funcionalidades de almacenamiento de blobs de Azure y Azure Data Lake Storage Gen1. El servicio resultante ofrece características de Azure Data Lake Storage Gen1, por ejemplo, la semántica del sistema de archivos, la seguridad de nivel de archivo y directorio y la escalabilidad, junto con el almacenamiento de bajo costo y en capas, alta disponibilidad y capacidades de recuperación ante desastres desde Azure Blob storage.

## <a name="data-lake-storage-gen2-availability"></a>Disponibilidad de Data Lake Storage Gen2

Data Lake Storage Gen2 está disponible como una opción de almacenamiento para casi todos los tipos de clúster de HDInsight de Azure como un valor predeterminado y una cuenta de almacenamiento adicional. Sin embargo, HBase, puede tener solo una cuenta de Data Lake Storage Gen2.

> [!Note] 
> Después de seleccionar Data Lake Storage Gen2 como su **tipo de almacenamiento principal**, no puede seleccionar una cuenta de Data Lake Storage Gen1 como almacenamiento adicional.

## <a name="create-an-hdinsight-cluster-with-data-lake-storage-gen2"></a>Crear un clúster de HDInsight con Data Lake Storage Gen2

## <a name="use-the-azure-portal"></a>Uso de Azure Portal

Para crear un clúster de HDInsight que usa Data Lake almacenamiento Gen2 para el almacenamiento, siga estos pasos para configurar una cuenta de Data Lake Storage Gen2.

1. Cree una identidad administrada asignada por el usuario si todavía no tiene una. Consulte [Creación, enumeración, eliminación o asignación de un rol a una identidad administrada asignada por el usuario mediante Azure Portal](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md#create-a-user-assigned-managed-identity).

    ![Crear una identidad administrada asignada por el usuario](./media/hdinsight-hadoop-data-lake-storage-gen2/create-user-assigned-managed-identity-portal.png)

1. Cree una cuenta de almacenamiento de Azure Data Lake Storage Gen2. Asegúrese de que el **espacio de nombres jerárquico** está habilitada. Para más información, consulte [Inicio rápido: Creación de una cuenta de almacenamiento de Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-quickstart-create-account.md).

    ![Captura de pantalla en la que se muestra la creación de una cuenta de almacenamiento en Azure Portal](./media/hdinsight-hadoop-data-lake-storage-gen2/azure-data-lake-storage-account-create-advanced.png)
 
1. Asigne la identidad administrada al rol **Propietario de datos de blobs de almacenamiento (versión preliminar)** de la cuenta de almacenamiento. Para más información, consulte [Administración de los derechos de acceso a los datos de la cola y el blob de Azure con RBAC (versión preliminar)](../storage/common/storage-auth-aad-rbac.md).

    1. En [Azure Portal](https://portal.azure.com), vaya a la cuenta de almacenamiento.
    1. Seleccione la cuenta de almacenamiento y luego seleccione **control de acceso (IAM)** para mostrar la configuración de control de acceso para la cuenta. Seleccione la pestaña **Asignaciones de roles** para ver la lista de asignaciones de roles.
    
        ![Captura de pantalla que muestra la configuración de control de acceso del almacenamiento](./media/hdinsight-hadoop-data-lake-storage-gen2/portal-access-control.png)
    
    1. Seleccione el **+ Agregar asignación de roles** para agregar un nuevo rol.
    1. En la ventana **Agregar asignación de roles**, seleccione el rol **Propietario de datos de blobs de almacenamiento (versión preliminar)**. Luego, seleccione la suscripción que tiene la identidad administrada y la cuenta de almacenamiento. A continuación, busque la identidad administrada asignada por el usuario que creó anteriormente. Por último, seleccione la identidad administrada y se mostrará en **miembros seleccionados**.
    
        ![Captura de pantalla que muestra cómo asignar un rol de RBAC](./media/hdinsight-hadoop-data-lake-storage-gen2/add-rbac-role3.png)
    
    1. Seleccione **Guardar**. La identidad asignada por el usuario que seleccionó ahora aparece en el rol **Colaborador**.

    1. Una vez completada la configuración inicial, puede crear un clúster a través del portal. El clúster debe estar en la misma región de Azure que la cuenta de almacenamiento. En la sección **Almacenamiento** del menú de creación del clúster, seleccione estas opciones:
        
        * Para **tipo de almacenamiento principal**, seleccione **Azure Data Lake Storage Gen2**.
        * En **seleccione una cuenta de almacenamiento**, busque y seleccione la cuenta de almacenamiento de Data Lake Storage Gen2 recién creada.
        
            ![Configuración de almacenamiento para usar Data Lake Storage Gen2 con Azure HDInsight](./media/hdinsight-hadoop-data-lake-storage-gen2/primary-storage-type-adls-gen2.png)
        
        * En **identidad**, seleccione la suscripción correcta y recién creado asignada por el usuario identidad administrada.
        
            ![Configuración de identidad para usar Data Lake Storage Gen2 con Azure HDInsight](./media/hdinsight-hadoop-data-lake-storage-gen2/managed-identity-cluster-creation.png)

### <a name="use-an-azure-resource-manager-template-deployed-with-the-azure-cli"></a>Usar una plantilla de Azure Resource Manager implementada con la CLI de Azure

También puede [descargar un archivo de plantilla de ejemplo](https://github.com/Azure-Samples/hdinsight-data-lake-storage-gen2-templates/blob/master/hdinsight-adls-gen2-template.json) y [descargar un archivo de parámetros de ejemplo](https://github.com/Azure-Samples/hdinsight-data-lake-storage-gen2-templates/blob/master/parameters.json). Antes de usar la plantilla, reemplace la cadena `<SUBSCRIPTION_ID>` por su identificador de suscripción de Azure reales. Además, reemplace la cadena `<PASSWORD>` con la contraseña elegida para establecer tanto la contraseña que usará para iniciar sesión en el clúster y la contraseña de SSH.

El siguiente fragmento de código realiza los pasos iniciales siguientes:

1. Inicia sesión en su cuenta de Azure.
1. Establece la suscripción activa donde se realizará las operaciones de creación.
1. Crea un nuevo grupo de recursos para las actividades de implementación nuevo denominado `hdinsight-deployment-rg`.
1. Crea una identidad administrada asignada por el usuario denominada `test-hdinsight-msi`.
1. Agrega una extensión a la CLI de Azure para utilizar las características de Data Lake Storage Gen2.
1. Crea una nueva cuenta de Data Lake Storage Gen2 denominada `hdinsightadlsgen2`, utilizando el `--hierarchical-namespace true` marca.

```azurecli
az login
az account set --subscription <subscription_id>

# Create resource group
az group create --name hdinsight-deployment-rg --location eastus

# Create managed identity
az identity create -g hdinsight-deployment-rg -n test-hdinsight-msi

az extension add --name storage-preview

az storage account create --name hdinsightadlsgen2 \
    --resource-group hdinsight-deployment-rg \
    --location eastus --sku Standard_LRS \
    --kind StorageV2 --hierarchical-namespace true
```

A continuación, inicie sesión en el portal. Agregar la nueva identidad administrada asignada por el usuario para el **colaborador de datos de Blob de almacenamiento (versión preliminar)** rol en la cuenta de almacenamiento, como se describe en el paso 3 en [mediante Azure portal](hdinsight-hadoop-use-data-lake-storage-gen2.md).

Después de haber asignado el rol para la identidad administrada asignada por el usuario, implemente la plantilla mediante el siguiente fragmento de código.

```azurecli
az group deployment create --name HDInsightADLSGen2Deployment \
    --resource-group hdinsight-deployment-rg \
    --template-file hdinsight-adls-gen2-template.json \
    --parameters parameters.json
```

## <a name="access-control-for-data-lake-storage-gen2-in-hdinsight"></a>Control de acceso de Data Lake Storage Gen2 en HDInsight

### <a name="what-kinds-of-permissions-does-data-lake-storage-gen2-support"></a>¿Qué tipos de permisos admite Data Lake Storage Gen2?

Data Lake Storage Gen2 usa un modelo de control de acceso que admite el control de acceso basado en roles (RBAC) y listas de control de acceso POSIX como (ACL). Admite de Data Lake Storage Gen1 control de acceso sólo para controlar el acceso a datos.

RBAC utiliza las asignaciones de roles para aplicar de forma eficaz los conjuntos de permisos a los usuarios, grupos y entidades de servicio para los recursos de Azure. Normalmente, los recursos de Azure están limitados a recursos de nivel superior (por ejemplo, cuentas de almacenamiento de Azure). Para el almacenamiento de Azure así como Data Lake Storage Gen2, este mecanismo se ha ampliado para el recurso de sistema de archivos.

 Para obtener más información acerca de los permisos de archivo con RBAC, consulte [control de acceso de Azure basada en roles (RBAC)](../storage/blobs/data-lake-storage-access-control.md#azure-role-based-access-control-rbac).

Para obtener más información acerca de los permisos de archivo con las ACL, consulte [listas de control de acceso de archivos y directorios](../storage/blobs/data-lake-storage-access-control.md#access-control-lists-on-files-and-directories).

### <a name="how-do-i-control-access-to-my-data-in-data-lake-storage-gen2"></a>¿Cómo se puede controlar acceso a Mis datos en Data Lake Storage Gen2?

Capacidad de su clúster HDInsight para obtener acceso a archivos en Data Lake Storage Gen2 se controla a través de las identidades administradas. Una identidad administrada es una identidad registrada en Azure Active Directory (Azure AD) cuyas credenciales se administran mediante Azure. Con las identidades administradas, no es necesario registrar a las entidades de servicio en Azure AD o mantener credenciales, como certificados.

Servicios de Azure tienen dos tipos de identidades administradas: asignado por el sistema y asignada por el usuario. HDInsight usa identidades administradas asignada por el usuario para tener acceso a Data Lake Storage Gen2. Las identidades administrada asignadas por el usuario se crean como recursos de Azure independientes. Mediante un proceso de creación, Azure crea una identidad en el inquilino de Azure AD de confianza para la suscripción que se utiliza. Una vez creada la identidad, esta puede asignarse a una o varias instancias de servicio de Azure.

El ciclo de vida de una identidad asignada por el usuario no se administra junto con el ciclo de vida de las instancias de servicio de Azure a las que se asigna. Para obtener más información acerca de las identidades administradas, vea [cómo las identidades administradas por el trabajo de los recursos de Azure?](../active-directory/managed-identities-azure-resources/overview.md#how-does-the-managed-identities-for-azure-resources-work).

### <a name="how-do-i-set-permissions-for-azure-ad-users-to-query-data-in-data-lake-storage-gen2-by-using-hive-or-other-services"></a>¿Cómo configuro permisos para usuarios de Azure AD para consultar los datos en Data Lake Storage Gen2 mediante el uso de Hive u otros servicios?

Para establecer permisos para los usuarios para consultar los datos, utilice grupos de seguridad de Azure AD como la entidad de seguridad asignado en las ACL. No asignar directamente los permisos de acceso de archivo a usuarios individuales o entidades de servicio. Cuando use grupos de seguridad de Azure AD para controlar el flujo de permisos, puede agregar y quitar usuarios o entidades de servicio sin volver a aplicar las ACL a una estructura de todo el directorio. Solo tiene que agregar o quitar los usuarios del grupo de seguridad de Azure AD adecuado. No se heredan las ACL, para volver a aplicar las ACL es necesario actualizar la ACL en todos los archivos y subdirectorios.

## <a name="next-steps"></a>Pasos siguientes

* [Uso de Azure Data Lake Storage Gen2 (versión preliminar) con clústeres de Azure HDInsight](../storage/blobs/data-lake-storage-use-hdi-cluster.md)
* [Azure HDInsight integration with Data Lake Storage Gen2 preview - ACL and security update](https://azure.microsoft.com/blog/azure-hdinsight-integration-with-data-lake-storage-gen-2-preview-acl-and-security-update/) (Integración de Azure HDInsight con Data Lake Storage Gen2 [versión preliminar]: ACL y actualización de seguridad)
* [Introducción a Azure Data Lake Storage Gen2 (versión preliminar)](../storage/blobs/data-lake-storage-introduction.md)
