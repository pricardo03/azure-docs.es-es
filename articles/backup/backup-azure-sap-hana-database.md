---
title: Copia de seguridad de una base de datos de SAP HANA en Azure con Azure Backup | Microsoft Docs
description: Este tutorial explica cómo realizar copias de seguridad de una base de datos de SAP HANA en Azure con el servicio de copia de seguridad de Azure.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: raynew
ms.openlocfilehash: 5ed41013535e4591d88bff5c017c1fcf4c4053cc
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2019
ms.locfileid: "65237813"
---
# <a name="back-up-an-sap-hana-database"></a>Realizar copias de seguridad de una base de datos de SAP HANA

[Copia de seguridad de Azure](backup-overview.md) admite la copia de seguridad de bases de datos de SAP HANA en Azure.

> [!NOTE]
> Esta característica actualmente está en su versión preliminar pública. Actualmente no está lista para producción y no tiene un SLA garantizado. 


## <a name="scenario-support"></a>Compatibilidad con los escenarios

**Soporte técnico** | **Detalles**
--- | ---
**Zonas geográficas admitidas** | Sudeste de Australia, este de Australia <br> Sur de Brasil <br> Canadá Central, Canadá oriental <br> Sudeste asiático, Asia oriental <br> East US, East US 2, centro occidental de EE.UU, oeste de Estados Unidos, oeste de EE.UU. 2, North Central US, EE. UU., centro y sur Central de EE.<br> India Central, India del sur <br> Este de Japón, Oeste de Japón<br> Corea Central, Corea del Sur <br> Norte de Europa y Oeste de Europa <br> Sur de Reino Unido, oeste de Reino Unido
**Sistemas operativos VM** | SLES 12 con SP2 o SP3.
**Versiones compatibles de HANA** | SSDC en HANA 1.x, MDC en HANA 2.x < = SPS03


### <a name="current-limitations"></a>Limitaciones actuales

- Solo puede realizar una copia de bases de datos de SAP HANA que se ejecutan en máquinas virtuales de Azure.
- Solo puede configurar copia de seguridad de SAP HANA en Azure portal. La característica no se puede configurar con PowerShell, CLI o la API de REST.
- Solo puede realizar una copia de bases de datos en modo de escalado vertical.
- Hacer copias de seguridad de registros de base de datos cada 15 minutos. Las copias de seguridad del registro solo comienzan a flujo una vez finalizada una copia de seguridad completa correcta para la base de datos.
- Puede realizar copias de seguridad completas y diferenciales. Actualmente no se admite la copia de seguridad incremental.
- No se puede modificar la directiva de copia de seguridad después de aplicar las copias de seguridad de SAP HANA. Si desea realizar copias de seguridad con una configuración diferente, cree una nueva directiva o asignar una directiva diferente. 
    - Para crear una nueva directiva, en el almacén, haga clic en **directivas** > **las directivas de copia de seguridad** > **+ agregar** > **SAP HANA en Máquina virtual de Azure**y especifique la configuración de directiva.
    - Para asignar una directiva diferente, en las propiedades de la máquina virtual que ejecuta la base de datos, haga clic en el nombre de la directiva actual. A continuación, en el **directiva de copia de seguridad** página puede seleccionar una directiva diferente que se usará para la copia de seguridad.




## <a name="prerequisites"></a>Requisitos previos

Asegúrese de que haga lo siguiente antes de configurar las copias de seguridad:

1. En la máquina virtual que se ejecutan la base de datos de SAP HANA, instale Microsoft oficial [en tiempo de ejecución de .NET Core 2.1](https://dotnet.microsoft.com/download/linux-package-manager/sles/runtime-current) paquete. Observe lo siguiente:
    - Solo necesita el **dotnet-runtime-2.1** paquete. No es necesario **aspnetcore-runtime-2.1**.
    - Si la máquina virtual no tiene internet acceder, reflejado o proporcionar una caché sin conexión para dotnet-runtime-2.1 (y todos los paquetes RPM dependientes) desde la fuente del paquete de Microsoft especificado en la página.
    - Durante la instalación de paquetes, puede que se le pida para especificar una opción. Si es así, especifique **solución 2**.

        ![Opción de instalación del paquete](./media/backup-azure-sap-hana-database/hana-package.png)

2.  En la máquina virtual, instalar y habilitar paquetes de controladores ODBC desde el oficial paquete SLES/medio con zypper, como sigue:

    ``` 
    sudo zypper update
    sudo zypper install unixODBC
    ```
4.  Permitir la conectividad de la máquina virtual a internet, por lo que puede llegar a Azure, como se describe en el siguiente procedimiento.


### <a name="set-up-network-connectivity"></a>Configurar la conectividad de red

Todas las operaciones, la VM de SAP HANA necesita conectividad con direcciones IP públicas de Azure. Operaciones de máquina virtual (detección de base de datos, configurar copias de seguridad, programar copias de seguridad, restaurar puntos de recuperación y así sucesivamente) no pueden funcionar sin conexión. Establecer la conectividad al permitir el acceso a los intervalos IP de centro de datos de Azure: 

- Puede descargar el [intervalos de direcciones IP](https://www.microsoft.com/download/details.aspx?id=41653) centros de datos de Azure y, a continuación, permitir el acceso a estas direcciones IP.
- Si usa grupos de seguridad de red (NSG), puede usar el AzureCloud [etiqueta de servicio](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags) para permitir IP pública de Azure en todas las direcciones. Puede usar el [cmdlet Set-AzureNetworkSecurityRule](https://docs.microsoft.com/powershell/module/servicemanagement/azure/set-azurenetworksecurityrule?view=azuresmps-4.0.0) para modificar las reglas de NSG.

## <a name="onboard-to-the-public-preview"></a>Incorporación a la versión preliminar pública

Incorporación a la versión preliminar pública como sigue:

- En el portal, registre el identificador de suscripción para el proveedor de servicio de Recovery Services por [seguir este artículo](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-register-provider-errors#solution-3---azure-portal). 
- Para PowerShell, ejecute este cmdlet. Debería completarse como "Registrado".

    ```
    PS C:>  Register-AzProviderFeature -FeatureName "HanaBackup" –ProviderNamespace Microsoft.RecoveryServices
    ```



[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="discover-the-databases"></a>Detectar las bases de datos


1. En el almacén, en **Introducción**, haga clic en **copia de seguridad**. En **donde se ejecuta la carga de trabajo?**, seleccione **SAP HANA en máquinas virtuales de Azure**.
2. Haga clic en **Iniciar detección**. Esto inicia la detección de máquinas virtuales de Linux no protegidas en la región del almacén.

   - Después de la detección, sin protección de máquinas virtuales aparecen en el portal, enumeradas por nombre y grupo de recursos.
   - Si no aparece una máquina virtual según lo previsto, compruebe si lo es ya una copia de seguridad en un almacén.
   - Varias máquinas virtuales pueden tener el mismo nombre pero pertenecen a distintos grupos de recursos.

3. En **seleccionar máquinas virtuales**, haga clic en el vínculo para descargar el script que proporciona permisos para el servicio de copia de seguridad de Azure tener acceso a las máquinas virtuales de SAP HANA para la detección de base de datos
4. Ejecute el script en cada máquina virtual que hospeda las bases de datos de SAP HANA que se desean realizar copias de seguridad.
5. Después de ejecutar el script en las máquinas virtuales, en **seleccionar máquinas virtuales**, seleccione las máquinas virtuales. A continuación, haga clic en **detectar bases de datos**.
6. Copia de seguridad de Azure detecta todas las bases de datos de SAP HANA en la máquina virtual. Durante la detección, copia de seguridad de Azure registra la máquina virtual con el almacén y se instala una extensión en la máquina virtual. No hay ningún agente está instalado en la base de datos.

    ![Detectar bases de datos de SAP HANA](./media/backup-azure-sap-hana-database/hana-discover.png)
    
## <a name="configure-backup"></a>Configuración de la copia de seguridad  

Ahora habilite la copia de seguridad.

1. En el paso 2, haga clic en **Configurar copia de seguridad**.
2. En **seleccionar elementos para copia de seguridad**, seleccione todas las bases de datos que desea proteger > **Aceptar**.
3. En **directiva de copia de seguridad** > **elegir directiva de copia de seguridad**, cree una nueva directiva de copia de seguridad para las bases de datos, según las instrucciones siguientes.
4. Después de crear la directiva, en el **copia de seguridad** menú, haga clic en **habilitar copia de seguridad**.
5. Realizar un seguimiento del progreso de la configuración de copia de seguridad en el **notificaciones** área del portal.

### <a name="create-a-backup-policy"></a>Creación de una directiva de copia de seguridad
Una directiva de copia de seguridad define cuándo se realizan copias de seguridad y cuánto tiempo sí se conservan.

- Una directiva se crea en el nivel de almacén.
- Varios almacenes pueden usar la misma directiva de copia de seguridad, pero debe aplicar la directiva de copia de seguridad a cada almacén.

Especifique la configuración de directiva como sigue:

1. En **Nombre de la directiva**, escriba un nombre para la nueva directiva.
2. En el menú **Full Backup policy** (Directiva de copia de seguridad completa), seleccione un valor para **Backup Frequency** (Frecuencia de copia de seguridad) entre **Daily** (Diaria) o **Weekly** (Semanal).
   - **Diaria**: Seleccione la hora y zona horaria en el que comienza el trabajo de copia de seguridad.
   
       - Debe ejecutar una copia de seguridad completa. No se puede desactivar esta opción.
       - Haga clic en **Full Backup** (Copia de seguridad completa) para ver la directiva.
       - No puede crear copias de seguridad diferenciales para copias de seguridad completas diarias.
       
   - **Semanal**: Seleccione el día de la semana, hora y zona horaria en la que se ejecuta el trabajo de copia de seguridad.
3. En **duración de retención**, configurar opciones de retención para la copia de seguridad completa.
    - De forma predeterminada se seleccionan todas las opciones. Desactive los límites de intervalo de retención que no desea usar y establecerlos que realizar.
    - El período de retención mínimo para cualquier tipo de copia de seguridad (completa, diferencial o registro) es siete días.
    - Los puntos de recuperación se etiquetan para la retención en función de su duración de retención. Por ejemplo, si selecciona la frecuencia diaria, solo se desencadena una copia de seguridad completa cada día.
    - La copia de seguridad de un día concreto es etiquetado y se mantienen se basa en la configuración y la duración de retención semanal.
    - La duración de retención mensual y anual se comporta de forma similar.

4. En el **directiva de copia de seguridad completa** menú, haga clic en **Aceptar** para aceptar la configuración.
5. Seleccione **copia de seguridad diferencial** para agregar una directiva diferencial.
6. En **Differential Backup policy** (Directiva de copia de seguridad diferencial), seleccione **Enable** (Habilitar) para abrir los controles de retención y frecuencia.
    - A lo sumo, puede desencadenar una copia de seguridad diferencial al día.
    - Como máximo, las copias de seguridad diferenciales se pueden retener durante 180 días. Si necesita más tiempo de retención, debe usar copias de seguridad completas.

    > [!NOTE]
    > Actualmente no se admiten copias de seguridad incrementales. 

7. Haga clic en **Aceptar** para guardar la directiva y volver a la principal **directiva de copia de seguridad** menú.
8. Seleccione **copia de seguridad de registro** para agregar una directiva de copia de seguridad del registro transaccional,
    - En **copia de seguridad del registro**, seleccione **habilitar**.
    - Establecer los controles de frecuencia y retención.

    > [!NOTE]
    > Las copias de seguridad del registro solo comienzan a flujo una vez completada una copia de seguridad completa correcta.

9. Haga clic en **Aceptar** para guardar la directiva y volver a la principal **directiva de copia de seguridad** menú.
10. Cuando termine de definir la directiva de copia de seguridad, haga clic en **Aceptar**.


## <a name="run-an-on-demand-backup"></a>Ejecución de una copia de seguridad a petición

Ejecutan copias de seguridad según la programación de la directiva. Puede ejecutar una copia de seguridad y a petición como sigue:


1. En el menú Almacén, haga clic en **Elementos de copia de seguridad**.
2. En **elementos de copia de seguridad**, seleccione la máquina virtual que ejecuta la base de datos de SAP HANA y, a continuación, haga clic en **de copia de seguridad ahora**.
3. En **copia de seguridad ahora**, use el control de calendario para seleccionar el último día que se debe conservar el punto de recuperación. A continuación, haga clic en **Aceptar**.
4. Supervise las notificaciones del portal. Puede supervisar el progreso del trabajo en el panel del almacén > **trabajos de copia de seguridad** > en curso. Según el tamaño de la base de datos, crear la copia de seguridad inicial puede tardar un rato.

## <a name="run-sap-hana-studio-backup-on-a-database-with-azure-backup-enabled"></a>Ejecutar copia de seguridad de SAP HANA Studio en una base de datos con Azure Backup habilitada

Si desea realizar una copia de seguridad local (mediante HANA Studio) de una base de datos que se copia con Azure Backup, haga lo siguiente:

1. Espere a que cualquier completo o las copias de seguridad del registro para la base de datos finalice. Compruebe el estado en SAP HANA Studio.
2. Deshabilitar las copias de seguridad del registro y establece el catálogo de copia de seguridad en el sistema de archivos de base de datos pertinente.
3. Para ello, haga doble clic en **systemdb** > **configuración** > **Seleccionar base de datos** > **filtro (registro)** .
4. Establecer **enable_auto_log_backup** a **No**.
5. Establecer **log_backup_using_backint** a **False**.
6. Realizar copia de seguridad de la base de datos un completo ad hoc.
7. Espere a que la copia de seguridad completa y copia de seguridad de catálogos en Finalizar.
8. Revertir la configuración anterior a las de Azure:
    - Establecer **enable_auto_log_backup** a **Sí**.
    - Establecer **log_backup_using_backint** a **True**.



## <a name="next-steps"></a>Pasos siguientes

[Obtenga información sobre](backup-azure-arm-vms-prepare.md) la copia de seguridad de máquinas virtuales de Azure.


