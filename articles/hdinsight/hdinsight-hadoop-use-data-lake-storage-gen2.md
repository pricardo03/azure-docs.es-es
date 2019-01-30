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
ms.openlocfilehash: 1d43c7b6dd1bdec0a2507d8ce1a3883f5ce31a39
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2019
ms.locfileid: "54479443"
---
# <a name="use-azure-data-lake-storage-gen2-with-azure-hdinsight-clusters"></a>Uso de Azure Data Lake Storage Gen2 con clústeres de Azure HDInsight

Azure Data Lake Storage (Data Lake Storage) Gen2 es un conjunto de funcionalidades dedicado al análisis de macrodatos, basado en Azure Blob Storage. Data Lake Storage Gen2 es el resultado de la combinación de las funcionalidades de Azure Blob Storage y Azure Data Lake Storage Gen1. El resultado es un servicio que ofrece características de Azure Data Lake Storage Gen1, como la semántica del sistema de archivos, la seguridad de nivel de directorio y archivo y la escala junto con las funcionalidades de recuperación ante desastres o alta disponibilidad, y de almacenamiento por niveles de bajo costo de Azure Blob Storage.

## <a name="data-lake-storage-gen2-availability"></a>Disponibilidad de Data Lake Storage Gen2

Azure Data Lake Storage Gen2 está disponible como opción de almacenamiento para casi todos los tipos de clúster de Azure HDInsight como una cuenta de almacenamiento predeterminada o adicional. Sin embargo, HBase solo puede tener una cuenta de Data Lake Storage Gen2.

> [!Note] 
> Una vez que selecciona Data Lake Storage Gen2 como el **tipo de almacenamiento principal**, no puede seleccionar una cuenta de Data Lake Storage Gen1 como almacenamiento adicional.

## <a name="creating-an-hdinsight-cluster-with-data-lake-storage-gen2"></a>Creación de un clúster de HDInsight con Data Lake Storage Gen2

Para crear un clúster de HDInsight, que usa Data Lake Storage Gen2 para almacenamiento, use estos pasos para crear una cuenta de Data Lake Storage Gen2 correctamente configurada.

1. Cree una identidad administrada asignada por el usuario si todavía no tiene una. Consulte [Creación, enumeración, eliminación o asignación de un rol a una identidad administrada asignada por el usuario mediante Azure Portal](/../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal#create-a-user-assigned-managed-identity.md).

    ![Crear una identidad administrada asignada por el usuario](./media/hdinsight-hadoop-data-lake-storage-gen2/create-user-assigned-managed-identity-portal.png)

1. Cree una cuenta de almacenamiento de Azure Data Lake Storage Gen2. Asegúrese de que esté habilitada la opción **Sistema de archivos jerárquico**. Consulte [Quickstart: Creación de una cuenta de almacenamiento de Azure Data Lake Storage Gen2](/../storage/blobs/data-lake-storage-quickstart-create-account.md) para más detalles.

    ![Captura de pantalla en la que se muestra la creación de una cuenta de almacenamiento en Azure Portal](./media/hdinsight-hadoop-data-lake-storage-gen2/azure-data-lake-storage-account-create-advanced.png)
 
1. Asigne la identidad administrada al rol **Colaborador de dato de blobs de almacenamiento (versión preliminar)** de la cuenta de almacenamiento. Consulte [Administración de los derechos de acceso a los datos de la cola y el blob de Azure con RBAC (versión preliminar)](/../storage/common/storage-auth-aad-rbac#assign-a-role-scoped-to-the-storage-account-in-the-azure-portal.md)

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

## <a name="access-control-for-data-lake-storage-gen2-in-hdinsight"></a>Control de acceso de Data Lake Storage Gen2 en HDInsight

### <a name="what-kinds-of-permissions-does-data-lake-storage-gen2-support"></a>¿Qué tipos de permisos admite Data Lake Storage Gen2?

Azure Data Lake Storage Gen2 implementa un modelo de control de acceso compatible con el control de acceso basado en rol (RBAC) de Azure y las listas de control de acceso (ACL) tipo POSIX. Data Lake Storage Gen1 solo admitía las listas de control de acceso para controlar el acceso a los datos.

El control de acceso basado en rol (RBAC) de Azure usa las asignaciones de roles para aplicar de forma eficaz conjuntos de permisos a usuarios, grupos y entidades de servicio para los recursos de Azure. Normalmente, los recursos de Azure están limitados a los recursos de nivel superior (por ejemplo, cuentas de Azure Storage). En el caso de Azure Storage, y también Azure Data Lake Storage Gen2, este mecanismo se ha ampliado al recurso de sistema de archivos.

 Para más información sobre los permisos de archivo con RBAC, consulte [Control de acceso basado en rol (RBAC) de Azure](/../storage/blobs/data-lake-storage-access-control#azure-role-based-access-control-rbac.md).

Para más información sobre los permisos de archivo con las listas de control de acceso, consulte [Listas de control de acceso en archivos y directorios](/../storage/blobs/data-lake-storage-access-control#access-control-lists-on-files-and-directories.md).


### <a name="how-do-i-control-access-to-my-data-in-gen2"></a>¿Cómo puedo controlar el acceso a mis datos en Gen2?

La capacidad que el clúster de HDInsight tiene de acceder a los archivos en Data Lake Storage Gen2 se controla mediante las identidades administradas. Una identidad administrada es una identidad registrada en Azure AD con credenciales administradas por Azure. No es necesario registrar entidades de servicio en Azure AD ni mantener credenciales, tales como los certificados.

Existen dos tipos de identidades administradas para los servicios de Azure: asignadas por el sistema y asignadas por el usuario. Azure HDInsight usa identidades administradas asignadas por el usuario para acceder a Azure Data Lake Storage Gen2. Las identidades administrada asignadas por el usuario se crean como recursos de Azure independientes. Mediante un proceso de creación, Azure crea una identidad en el inquilino de Azure AD de confianza para la suscripción que se utiliza. Una vez creada la identidad, esta puede asignarse a una o varias instancias de servicio de Azure. El ciclo de vida de una identidad asignada por el usuario no se administra junto con el ciclo de vida de las instancias de servicio de Azure a las que se asigna. Para más información sobre las identidades administradas, consulte [¿Cómo funcionan las identidades administradas para los recursos de Azure?](/../active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-worka-namehow-does-it-worka.md).

### <a name="how-do-i-set-permissions-for-azure-ad-users-to-query-data-in-data-lake-storage-gen2-using-hive-or-other-services"></a>¿Cómo puedo establecer permisos para que los usuarios de Azure AD consulten datos en Data Lake Storage Gen2 mediante Hive u otros servicios?

Use grupos de seguridad de Azure AD como la entidad de seguridad asignada en las listas de control de acceso. No asigne directamente permisos de acceso a archivos a usuarios individuales ni a entidades de servicio. Cuando usa los grupos de seguridad de AD para controlar el flujo de los permisos, puede agregar y quitar usuarios o entidades de servicio sin volver a aplicar las ACL a la estructura de todo un directorio. Solo tiene que agregar o quitar los usuarios del grupo de seguridad de Azure AD adecuado. Las ACL no se heredan y, por ello, para volver a aplicar las ACL es necesario actualizarlas en cada archivo y subdirectorio.

## <a name="next-steps"></a>Pasos siguientes

* [Uso de Azure Data Lake Storage Gen2 (versión preliminar) con clústeres de Azure HDInsight](/../storage/blobs/data-lake-storage-use-hdi-cluster.md)
* [Azure HDInsight integration with Data Lake Storage Gen2 preview - ACL and security update](https://azure.microsoft.com/blog/azure-hdinsight-integration-with-data-lake-storage-gen-2-preview-acl-and-security-update/) (Integración de Azure HDInsight con Data Lake Storage Gen2 [versión preliminar]: ACL y actualización de seguridad)
* [Introducción a Azure Data Lake Storage Gen2 (versión preliminar)](/../storage/blobs/data-lake-storage-introduction.md)