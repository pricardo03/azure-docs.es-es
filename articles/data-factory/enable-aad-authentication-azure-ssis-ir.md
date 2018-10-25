---
title: Habilitación de la autenticación de Azure Active Directory para el entorno de ejecución para la integración de SSIS en Azure | Microsoft Docs
description: En este artículo se describe cómo configurar el entorno de ejecución para la integración de SSIS en Azure para habilitar conexiones que usan la autenticación de Azure Active Directory.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 06/21/2018
ms.author: douglasl
ms.openlocfilehash: 234fb5af55565602d283539c63076adebad1ed25
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/03/2018
ms.locfileid: "48248982"
---
# <a name="enable-azure-active-directory-authentication-for-the-azure-ssis-integration-runtime"></a>Habilitación de la autenticación de Azure Active Directory para el entorno de ejecución para la integración de SSIS en Azure

En este artículo se muestra cómo crear el entorno de ejecución para la integración de SSIS en Azure con la identidad de servicio Azure Data Factory. La autenticación de Azure Active Directory (Azure AD) con la identidad administrada para los recursos de Azure para el entorno de ejecución para la integración de SSIS en Azure le permite usar MSI de Data Factory en lugar de la autenticación SQL para crear un entorno de ejecución de integración de SSIS en Azure.

Para más información sobre MSI de Data Factory, consulte [Identidad de servicio de Azure Data Factory](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity).

> [!NOTE]
> Si ya ha creado un entorno de ejecución para la integración de SSIS en Azure con autenticación SQL, no puede volver a configurar el entorno de ejecución de integración para utilizar la autenticación Azure AD con PowerShell en este momento.

## <a name="create-a-group-in-azure-ad-and-make-the-data-factory-msi-a-member-of-the-group"></a>Creación de un grupo en Azure AD y convertir en miembro del grupo a MSI de Data Factory

Puede utilizar un grupo de Azure AD existente o crear uno nuevo con PowerShell de Azure AD.

1.  Instalación del módulo de [PowerShell de Azure AD](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2).

2.  Inicie sesión con `Connect-AzureAD` y ejecute el siguiente comando para crear el grupo y guardarlo en una variable:

    ```powershell
    $Group = New-AzureADGroup -DisplayName "SSISIrGroup" `
                              -MailEnabled $false `
                              -SecurityEnabled $true `
                              -MailNickName "NotSet"
    ```

    La salida es similar a la del siguiente ejemplo, que también examina el valor de la variable:

    ```powershell
    $Group

    ObjectId DisplayName Description
    -------- ----------- -----------
    6de75f3c-8b2f-4bf4-b9f8-78cc60a18050 SSISIrGroup
    ```

3.  Agregue MSI de Data Factory al grupo. Puede seguir la [identidad de servicio de Azure Data Factory](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity) para obtener la ID. DE LA IDENTIDAD DE SERVICIO principal (por ejemplo, 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc, pero no utilizar la ID. DE LA APLICACIÓN DE IDENTIDAD DE SERVICIO para este propósito).

    ```powershell
    Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc
    ```

    También puede examinar la pertenencia al grupo más adelante.

    ```powershell
    Get-AzureAdGroupMember -ObjectId $Group.ObjectId
    ```

## <a name="enable-azure-ad-on-azure-sql-database"></a>Habilitación de Azure AD en Azure SQL Database

Azure SQL Database admite la creación de una base de datos con un usuario de Azure AD. Como resultado, puede configurar un usuario de Azure AD como administrador de Active Directory y, a continuación, iniciar sesión en SSMS con el usuario de Azure AD. Después puede crear un usuario contenido para el grupo de Azure AD para permitir que el entorno de ejecución para la integración cree el catálogo de SQL Server Integration Services (SSIS) en el servidor.

### <a name="enable-azure-ad-authentication-for-the-azure-sql-database"></a>Habilitación de la autenticación de Azure AD para Azure SQL Database

Puede [configurar la autenticación de Azure AD para la base de datos SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure) siguiendo estos pasos:

1.  En Azure Portal, seleccione **Todos los servicios** -> **Servidores SQL** en el panel de navegación izquierdo.

2.  Seleccione servidor SQL Database para habilitarlo para la autenticación de Azure AD.

3.  En la sección **Configuración** de la hoja, haga clic en **Administrador de Active Directory**.

4.  En la barra de comandos, seleccione **Establecer administrador**.

5.  Seleccione una cuenta de usuario de Azure AD para que se convierta en administrador del servidor y, después, seleccione **Seleccionar**.

6.  En la barra de comandos, seleccione **Guardar**.

### <a name="create-a-contained-user-in-the-database-that-represents-the-azure-ad-group"></a>Cree un usuario contenido en la base de datos que representa el grupo de Azure AD

En el paso siguiente, necesita [Microsoft SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS).

1.  Inicie SQL Server Management Studio.

2.  En el cuadro de diálogo **Conectar al servidor**, escriba el nombre del servidor SQL en el campo **Nombre del servidor**.

3.  En el campo **Autenticación**, seleccione **Active Directory - Universal compatible con MFA**. (También puede usar otros dos tipos de autenticación de Active Directory. Consulte [Configuración y administración de la autenticación de Azure Active Directory con SQL Database, Instancia administrada](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure)).

4.  En el campo **Nombre de usuario**, escriba el nombre de la cuenta de Azure AD que estableció como administradora del servidor, por ejemplo, testuser@xxxonline.com.

5.  Seleccione **Conectar**. Complete el proceso de inicio de sesión.

6.  En el **Explorador de objetos**, expanda la carpeta **Bases de datos** -> Bases de datos del sistema.

7.  Seleccione con el botón derecho la base de datos **maestra** y seleccione **Nueva consulta**.

8.  En la ventana de consulta, escriba la línea siguiente y seleccione **Ejecutar** en la barra de herramientas:

    ```sql
    CREATE USER [SSISIrGroup] FROM EXTERNAL PROVIDER
    ```

    El comando debería completarse correctamente y se creará el usuario contenido para el grupo.

9.  Limpie la ventana de consulta, escriba la línea siguiente y haga clic en **Ejecutar** en la barra de herramientas:

    ```sql
    ALTER ROLE dbmanager ADD MEMBER [SSISIrGroup]
    ```

    El comando debería completarse correctamente, concediendo al usuario contenido la capacidad de crear la base de datos.

## <a name="enable-azure-ad-on-azure-sql-database-managed-instance"></a>Habilitación de Azure AD en Instancia administrada de Azure SQL Database

Azure SQL Database Managed Instance no admite la creación de una base de datos con ningún otro usuario de Azure AD que no sea administrador de AD. Como resultado, tendrá que establecer el grupo de Azure AD como administrador de Active Directory. No es necesario crear el usuario contenido.

También puede [configurar autenticación de Azure AD para el servidor de Instancia administrada de SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure) siguiendo estos pasos:

7.  En Azure Portal, seleccione **Todos los servicios** -> **Servidores SQL** en el panel de navegación izquierdo.

8.  Haga clic en el servidor SQL para habilitarlo para la autenticación de Azure AD.

9.  En la sección **Configuración** de la hoja, haga clic en **Administrador de Active Directory**.

10. En la barra de comandos, seleccione **Establecer administrador**.

11. Busque y seleccione el grupo de Azure AD (por ejemplo, SSISIrGroup) y seleccione **Seleccionar**.

12. En la barra de comandos, seleccione **Guardar**.

## <a name="provision-the-azure-ssis-ir-in-the-portal"></a>Aprovisionamiento del entorno de ejecución de integración de SSIS en Azure en el Portal

Al aprovisionar el entorno de ejecución para la integración de SSIS en Azure con Azure Portal, en la página **Configuración de SQL**, active la opción "Usar autenticación de AAD con MSI de ADF". (La captura de pantalla siguiente muestra la configuración para el entorno de ejecución de integración con Azure SQL Database. Para el entorno de ejecución de integración con Instancia administrada, la propiedad "Catalog Database Service Tier" [Nivel de servicio de base de datos de catálogo] no está disponible; los demás valores de configuración son iguales).

Para más información sobre cómo crear una instancia de Integration Runtime para la integración de SSIS de Azure, consulte [Creación de una instancia de Integration Runtime de SSIS de Azure en Azure Data Factory](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).

![Configuración de la instancia de Integration Runtime para la integración de SSIS en Azure](media/enable-aad-authentication-azure-ssis-ir/enable-aad-authentication.png)

## <a name="provision-the-azure-ssis-ir-with-powershell"></a>Aprovisionamiento del entorno de ejecución para la integración de SSIS en Azure con PowerShell

Para aprovisionar el entorno de ejecución para la integración de SSIS en Azure con PowerShell, haga lo siguiente:

1.  Instale el módulo de [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/tag/v5.5.0-March2018) .

2.  En el script, no establezca el parámetro *CatalogAdminCredential*. Por ejemplo: 

    ```powershell
    Set-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -Type Managed `
                                               -CatalogServerEndpoint $SSISDBServerEndpoint `
                                               -CatalogPricingTier $SSISDBPricingTier `
                                               -Description $AzureSSISDescription `
                                               -Edition $AzureSSISEdition `
                                               -Location $AzureSSISLocation `
                                               -NodeSize $AzureSSISNodeSize `
                                               -NodeCount $AzureSSISNodeNumber `
                                               -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                               -SetupScriptContainerSasUri $SetupScriptContainerSasUri

    Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                                 -DataFactoryName $DataFactoryName `
                                                 -Name $AzureSSISName
   ```
