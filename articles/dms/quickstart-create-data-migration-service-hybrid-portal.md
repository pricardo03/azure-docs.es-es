---
title: 'Inicio rápido: Creación de una instancia de Azure Database Migration Service en modo híbrido mediante Azure Portal | Microsoft Docs'
description: Utilice Azure Portal para crear una instancia de Azure Database Migration Service en modo híbrido
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: quickstart
ms.date: 12/06/2019
ms.openlocfilehash: a124c33f15318f1b9b22a750a1de15601823afa3
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/06/2019
ms.locfileid: "74890698"
---
# <a name="quickstart-create-an-instance-of-azure-database-migration-service-in-hybrid-mode-using-the-azure-portal-preview"></a>Inicio rápido: Creación de una instancia de Azure Database Migration Service en modo híbrido mediante Azure Portal (versión preliminar)

Azure Database Migration Service en modo híbrido administra las migraciones de base de datos mediante un trabajo de migración que se hospeda de forma local junto con una instancia de Azure Database Migration Service que se ejecuta en la nube. El modo híbrido es especialmente útil en escenarios en los que hay una falta de conectividad de sitio a sitio entre la red local y Azure, o si hay un ancho de banda limitado de conectividad de sitio a sitio.

En este inicio rápido, puede usar Azure Portal para crear una instancia de Azure Database Migration Service en modo híbrido. Después, descargue, instale y configure Hybrid Worker en la red local. Durante la versión preliminar, puede usar Azure Database Migration Service en modo híbrido para migrar los datos de una instancia de SQL Server local a Azure SQL Database.

> [!IMPORTANT]
> El instalador híbrido de Azure Database Migration Service requiere .NET 4.7.2, o cualquier versión posterior. Para encontrar las versiones más recientes de .NET, consulte la página de [descarga de .NET Framework](https://dotnet.microsoft.com/download/dotnet-framework).

Si no tiene una suscripción a Azure, cree una cuenta [gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Abra el explorador web, vaya a [Microsoft Azure Portal](https://portal.azure.com/) y, a continuación, escriba sus credenciales para iniciar sesión en el portal.

La vista predeterminada es el panel del servicio.

## <a name="register-the-resource-provider"></a>Registrar el proveedor de recursos

Registre el proveedor de recursos Microsoft.DataMigration antes de crear su primera instancia de Azure Database Migration Service.

1. En Azure Portal, seleccione **Suscripciones**, seleccione la suscripción en la que quiere crear la instancia de Azure Database Migration Service y después seleccione **Proveedores de recursos**.

    ![Búsqueda del proveedor de recursos](media/quickstart-create-data-migration-service-hybrid-portal/dms-portal-search-resource-provider.png)

2. Busque la migración y después, a la derecha de **Microsoft.DataMigration**, seleccione **Registrar**.

    ![Registro del proveedor de recursos](media/quickstart-create-data-migration-service-hybrid-portal/dms-portal-register-resource-provider.png)

## <a name="create-an-instance-of-the-service"></a>Creación de una instancia del servicio

1. Seleccione +**Crear un recurso** para crear una instancia de Azure Database Migration Service.

2. Busque "migration" en Marketplace, seleccione **Azure Database Migration Service** y, en la pantalla **Azure Database Migration Service**, seleccione **Crear**.

3. En la pantalla **Crear el servicio de migración**:

    - Elija un **nombre del servicio** que sea fácil de recordar y único para identificar la instancia de Azure Database Migration Service.
    - Seleccione la **suscripción** de Azure donde desea crear la instancia.
    - Seleccione un **Grupo de recursos** existente o cree uno.
    - Elija la **ubicación** más cercana a su servidor de origen o de destino.

    > [!IMPORTANT]
    > Durante la versión preliminar, el modo híbrido solo se admite en la región Este de EE. UU. Puesto que Hybrid Worker está instalado en la red local, el rendimiento no se ve afectado, ni siquiera si se migra a un destino en otra región.

    - Para el **Modo de servicio**, seleccione **Hybrid (versión preliminar)** .

      ![Creación del servicio de migración: aspectos básicos](media/quickstart-create-data-migration-service-hybrid-portal/dms-create-service-basics.png)

4. Seleccione **Revisar + crear**.

5. En la pestaña **Revisar + crear**, revise los términos, compruebe la información que se proporciona y, a continuación, seleccione **Crear**.

    ![Creación del servicio de migración: Revisar + crear](media/quickstart-create-data-migration-service-hybrid-portal/dms-create-service-review-and-create.png)

    Transcurridos unos instantes, se creará la instancia de Azure Database Migration Service en modo híbrido y estará lista para usarse. La instancia de Azure Database Migration Service se muestra tal como aparece en la imagen siguiente:

    ![Instancia del modo híbrido de Azure Database Migration Service](media/quickstart-create-data-migration-service-hybrid-portal/dms-instance-hybrid-mode.png)

6. Después de crear el servicio, seleccione **Propiedades** y, a continuación, copie el valor que se muestra en el cuadro **Id. de recurso**, que usará para instalar el Hybrid Worker de Azure Database Migration Service.

    ![Propiedades del modo híbrido de Azure Database Migration Service](media/quickstart-create-data-migration-service-hybrid-portal/dms-copy-resource-id.png)

## <a name="create-azure-app-registration-id"></a>Creación de un Id. de registro de aplicación de Azure

Debe crear un Id. de registro de aplicación de Azure que Hybrid Worker local pueda usar para comunicarse con Azure Database Migration Service en la nube.

1. En Azure Portal, seleccione **Azure Active Directory**, **Registros de aplicaciones** y **Nuevo registro**.
2. Especifique un nombre para la aplicación y, a continuación, en **Tipos de cuenta admitidos**, seleccione el tipo de cuentas que quiere admitir para especificar quién puede usar la aplicación.

    ![Registro de aplicación del modo híbrido de Azure Database Migration Service](media/quickstart-create-data-migration-service-hybrid-portal/dms-register-application.png)

3. Use los valores predeterminados para los campos de **URI de redireccionamiento (opcional)** y, a continuación, seleccione **Registrar**.

4. Una vez completado el registro del Id. de la aplicación, tome nota del **Id. de la aplicación (cliente)** , que usará al instalar Hybrid Worker.

5. En Azure Portal, vaya a Azure Database Migration Service, seleccione **Control de acceso (IAM)** y, a continuación, seleccione **Agregar asignación de roles** para asignar el acceso de colaborador al Id. de la aplicación.

    ![Asignación de rol de colaborador en el modo híbrido de Azure Database Migration Service](media/quickstart-create-data-migration-service-hybrid-portal/dms-app-assign-contributor.png)

6. Seleccione **Colaborador** como rol, asigne el acceso a **Usuario de Azure AD o entidad de servicio** y, a continuación, seleccione el nombre del Id. de aplicación.

    ![Detalles de la asignación de rol de colaborador en el modo híbrido de Azure Database Migration Service](media/quickstart-create-data-migration-service-hybrid-portal/dms-add-role-assignment.png)

7. Seleccione **Guardar** para guardar la asignación de roles para el Id. de aplicación en el recurso de Azure Database Migration Service.

## <a name="download-and-install-the-hybrid-worker"></a>Descarga e instalación de Hybrid Worker

1. En Azure Portal, vaya a su instancia de Azure Database Migration Service

2. En **Configuración**, seleccione **Hybrid** y, a continuación, seleccione **Descarga del instalador** para descargar e instalar Hybrid Worker.

    ![Descarga de Hybrid Worker de Azure Database Migration Service](media/quickstart-create-data-migration-service-hybrid-portal/dms-installer-download.png)

3. Extraiga el archivo ZIP en el servidor que hospedará al Hybrid Worker de Azure Database Migration Service.

    > [!IMPORTANT]
    > El instalador híbrido de Azure Database Migration Service requiere .NET 4.7.2, o cualquier versión posterior. Para encontrar las versiones más recientes de .NET, consulte la página de [descarga de .NET Framework](https://dotnet.microsoft.com/download/dotnet-framework).

4. En la carpeta de instalación, busque y abra el archivo **dmsSettings.json**, especifique los valores de **ApplicationId** y **resourceId**, y guarde el archivo.

    ![Configuración de Hybrid Worker de Azure Database Migration Service](media/quickstart-create-data-migration-service-hybrid-portal/dms-settings.png)
 
5. Genere un certificado que Azure Database Migration Service pueda utilizar para autenticar la comunicación desde Hybrid Worker mediante el siguiente comando.

    ```
    <drive>:\<folder>\Install>DMSWorkerBootstrap.exe -a GenerateCert
    ```

    Se genera un certificado en la carpeta Install.

    ![Certificado de Hybrid Worker de Azure Database Migration Service](media/quickstart-create-data-migration-service-hybrid-portal/dms-certificate.png)

6. En Azure Portal, vaya al Id. de la aplicación; en **Administrar**, seleccione **Certificados y secretos** y, después, seleccione **Cargar certificado** para seleccionar el certificado público que ha generado.

    ![Carga de certificado de Hybrid Worker de Azure Database Migration Service](media/quickstart-create-data-migration-service-hybrid-portal/dms-app-upload-certificate.png)

7. Instale Hybrid Worker de Azure Database Migration Service en el servidor local mediante el siguiente comando:

    ```
    <drive>:\<folder>\Install>DMSWorkerBootstrap.exe -a Install -IAcceptDMSLicenseTerms
    ```

8. Si el instalador se ejecuta sin errores, el servicio mostrará un estado en línea en Azure Database Migration Service y estará listo para migrar las bases de datos.

    ![Instancia de Azure Database Migration Service en línea](media/quickstart-create-data-migration-service-hybrid-portal/dms-instance-hybrid-mode-online.png)

## <a name="uninstall-azure-database-migration-service-hybrid-mode"></a>Desinstalación del modo híbrido de Azure Database Migration Service

Actualmente, la desinstalación de Azure Database Migration Service en modo híbrido solo se admite a través del instalador de Hybrid Worker de Azure Database Migration Service en el servidor local mediante el comando siguiente:

```
<drive>:\<folder>\Install>DMSWorkerBootstrap.exe -a uninstall
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Migración de SQL Server a una instancia administrada de Azure SQL Database en línea](tutorial-sql-server-managed-instance-online.md)
> [Migración de SQL Server a una base de datos única o agrupada en Azure SQL Database sin conexión](tutorial-sql-server-to-azure-sql.md)
