---
title: Hacer una copia de seguridad de una base de datos de SAP HANA a Azure con Azure Backup | Microsoft Docs
description: En este tutorial se explica cómo hacer una copia de seguridad de una base de datos de SAP HANA en Azure con el servicio Azure Backup.
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 08/27/2019
ms.author: dacurwin
ms.openlocfilehash: 8d99ff6f2d8a21a501631a3a062be6b05130c05b
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2019
ms.locfileid: "72931814"
---
# <a name="back-up-an-sap-hana-database-to-azure"></a>Hacer una copia de seguridad de una base de datos de SAP HANA en Azure

[Azure Backup](backup-overview.md) admite la realización de copias de seguridad de bases de datos de SAP HANA en Azure.

> [!NOTE]
> Esta característica actualmente está en su versión preliminar pública. Actualmente no está lista para producción y no tiene un contrato de nivel de servicio garantizado.

## <a name="scenario-support"></a>Compatibilidad con los escenarios

**Soporte técnico** | **Detalles**
--- | ---
**Zonas geográficas admitidas** | Sudeste de Australia y Australia Oriental <br> Sur de Brasil <br> Centro de Canadá y Este de Canadá <br> Sudeste Asiático y Asia Oriental <br> Este de EE. UU., Este de EE. UU. 2, Centro-oeste de EE. UU., Oeste de EE. UU., Oeste de EE. UU. 2, Centro-norte de EE. UU., Centro de EE. UU. y Centro-sur de EE. UU.<br> India central e India meridional <br> Este de Japón, Oeste de Japón<br> Corea Central, Corea del Sur <br> Norte de Europa y Oeste de Europa <br> Sur de Reino Unido y Oeste de Reino Unido
**Sistemas operativos de máquina virtual admitidos** | SLES 12 con SP2, SP3 o SP4.
**Versiones de HANA admitidas** | SDC en HANA 1.x, MDC en HANA 2.x <= SPS04 Rev 43

### <a name="current-limitations"></a>Limitaciones actuales

- Solo puede hacer copias de seguridad de bases de datos de SAP HANA que se ejecuten en máquinas virtuales de Azure.
- Solo puede realizar copias de seguridad de instancias de SAP HANA que se ejecuten en una sola máquina virtual de Azure. Actualmente no se admiten varias instancias de HANA en la misma máquina virtual de Azure.
- Solo puede hacer copias de seguridad de bases de datos en modo de escalado vertical. El escalado horizontal, es decir, una instancia de HANA en varias máquinas virtuales de Azure, no es compatible actualmente con la copia de seguridad.
- No se puede realizar una copia de seguridad de la instancia de SAP HANA con capas dinámicas en un servidor extendido, es decir, en un nivel dinámico presente en otro nodo. Esta es básicamente la escalabilidad horizontal, que no se admite.
- No se puede realizar una copia de seguridad de la instancia de SAP HANA con capas dinámicas habilitadas en el mismo servidor. La organización en niveles dinámicos no se admite actualmente.
- Solo puede configurar las copias de seguridad de SAP HANA en Azure Portal. Esta característica no se puede configurar con la CLI de PowerShell.
- Puede hacer una copia de seguridad de los registros de la base de datos cada 15 minutos. Las copias de seguridad de los registros comienzan el flujo solo después de que se haya realizado correctamente una copia de seguridad completa de la base de datos.
- Puede hacer copias de seguridad completas y diferenciales. Las copias de seguridad incrementales no se admiten en la actualidad.
- No puede modificar la directiva de copia de seguridad una vez que la ha aplicado para las copias de seguridad de SAP HANA. Si desea hacer copias de seguridad con una configuración diferente, cree una nueva directiva o asigne una directiva diferente.
  - Para crear una nueva directiva, en el almacén, haga clic en **Directivas** > **Directivas de copia de seguridad** >  **+Agregar** > **SAP HANA en Máquina virtual de Azure** y especifique la configuración de la directiva.
  - Para asignar una directiva diferente, en las propiedades de la máquina virtual que ejecuta la base de datos, haga clic en el nombre de la directiva actual. A continuación, en la página **Directiva de copia de seguridad**, puede seleccionar una directiva diferente que se usará para la copia de seguridad.

## <a name="prerequisites"></a>Requisitos previos

Asegúrese de seguir estos pasos antes de configurar copias de seguridad:

1. En la máquina virtual que ejecuta la base de datos de SAP HANA, instale y habilite los paquetes de controlador ODBC desde el paquete o soporte físico SLES oficial mediante Zypper, de la manera siguiente:

    ```unix
    sudo zypper update
    sudo zypper install unixODBC
    ```

    > [!NOTE]
    > Si no actualiza los repositorios, asegúrese de que la versión de unixODBC es como mínimo 2.3.4. Para conocer la versión de uniXODBC, ejecute ```odbcinst -j``` como raíz

2. Permita la conectividad desde la máquina virtual a Internet para que pueda acceder a Azure, tal y como se describe en el procedimiento [siguiente](#set-up-network-connectivity).

3. Ejecute el script de registro previo en la máquina virtual donde está instalado HANA como usuario raíz. El script se proporciona [en el portal](#discover-the-databases) del flujo y es necesario para configurar los [permisos correctos](backup-azure-sap-hana-database-troubleshoot.md#setting-up-permissions).

### <a name="set-up-network-connectivity"></a>Configurar la conectividad de red

Para todas las operaciones, la máquina virtual SAP HANA necesita conectividad a las direcciones IP públicas de Azure. Si falta dicha conectividad, las operaciones de máquina virtual como detectar bases de datos, configurar y programar copias de seguridad o restaurar puntos de recuperación producirán errores. Establezca la conectividad permitiendo el acceso a los intervalos IP del centro de datos de Azure: 

- Puede descargar los [intervalos de direcciones IP](https://www.microsoft.com/download/details.aspx?id=41653) para los centros de datos de Azure y, a continuación, permitir el acceso a estas direcciones IP.
- Si usa grupos de seguridad de red (NSG), puede usar la [etiqueta de servicio](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags) AzureCloud para permitir todas las direcciones IP públicas de Azure. Para modificar las reglas de NSG, puede usar el [cmdlet Set-AzureNetworkSecurityRule](https://docs.microsoft.com/powershell/module/servicemanagement/azure/set-azurenetworksecurityrule?view=azuresmps-4.0.0).
- El puerto 443 debe estar en la lista de permitidos, ya que el transporte es a través de HTTPS.

## <a name="onboard-to-the-public-preview"></a>Incorporación a la versión preliminar pública

Incorpórese a la versión preliminar pública de la siguiente manera:

- En el portal, registre el identificador de suscripción en el proveedor de servicios de Recovery Services [siguiendo este artículo](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-register-provider-errors#solution-3---azure-portal). 
- Para PowerShell, ejecute este cmdlet. Debería completarse como "Registrado".

    ```powershell
    PS C:>  Register-AzProviderFeature -FeatureName "HanaBackup" –ProviderNamespace Microsoft.RecoveryServices
    ```

[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="discover-the-databases"></a>Detección de bases de datos

1. En el almacén, en **Introducción**, haga clic en **Copia de seguridad**. En **¿Dónde se ejecuta su carga de trabajo?** , seleccione **SAP HANA en máquina virtual de Azure**.
2. Haga clic en **Iniciar detección**. Se iniciará la detección de máquinas virtuales de Linux no protegidas en la región del almacén.

   - Tras la detección, las máquinas virtuales no protegidas aparecen en el portal, enumeradas por nombre y grupo de recursos.
   - Si una máquina virtual no aparece en la lista como cabría esperar, compruebe que no se haya copiado ya en un almacén.
   - Varias máquinas virtuales pueden tener el mismo nombre pero pertenecer a distintos grupos de recursos.

3. En **Seleccionar máquinas virtuales**, haga clic en el vínculo para descargar el script que proporciona permisos para el servicio Azure Backup para obtener acceso a las máquinas virtuales de SAP HANA para la detección de bases de datos
4. Ejecute el script en cada máquina virtual que hospede bases de datos de SAP HANA de las que desee hacer una copia de seguridad.
5. Tras ejecutar el script en las máquinas virtuales, seleccione las máquinas virtuales en **Seleccionar máquinas virtuales**. A continuación, haga clic en **Detectar bases de datos**.
6. Azure Backup detecta todas las bases de datos de SAP HANA en la máquina virtual. Durante la detección, Azure Backup registra la máquina virtual con el almacén e instala una extensión en la máquina virtual. No se instala ningún agente en la base de datos.

    ![Detectar bases de datos de SAP HANA](./media/backup-azure-sap-hana-database/hana-discover.png)

## <a name="configure-backup"></a>Configuración de la copia de seguridad  

Ahora habilite la copia de seguridad.

1. En el paso 2, haga clic en **Configurar copia de seguridad**.
2. En **Seleccione los elementos de los que desea hacer una copia de seguridad**, seleccione todas las bases de datos que desee proteger y haga clic en **Aceptar**.
3. En **Directiva de copia de seguridad** > **Elegir directiva de copia de seguridad**, cree una directiva de copia de seguridad para las bases de datos, según las instrucciones siguientes.
4. Tras crear la directiva, en el menú **Copia de seguridad**, haga clic en **Habilitar la copia de seguridad**.
5. Realice el seguimiento del progreso de la configuración de copia de seguridad en el área **Notificaciones** del portal.

### <a name="create-a-backup-policy"></a>Creación de una directiva de copia de seguridad

Una directiva de copia de seguridad define cuándo se realizan las copias de seguridad y cuánto tiempo se conservan.

- Una directiva se crea en el nivel de almacén.
- Varios almacenes pueden usar la misma directiva de copia de seguridad, pero debe aplicar la directiva de copia de seguridad a cada almacén.

Especifique la configuración de la directiva como se muestra a continuación:

1. En **Nombre de la directiva**, escriba un nombre para la nueva directiva.
2. En el menú **Full Backup policy** (Directiva de copia de seguridad completa), seleccione un valor para **Backup Frequency** (Frecuencia de copia de seguridad) entre **Daily** (Diaria) o **Weekly** (Semanal).
   - **Diaria**: Seleccione la zona horaria y la hora a la que comienza el trabajo de copia de seguridad.
   
       - Debe ejecutar una copia de seguridad completa. No se puede desactivar esta opción.
       - Haga clic en **Full Backup** (Copia de seguridad completa) para ver la directiva.
       - No puede crear copias de seguridad diferenciales para copias de seguridad completas diarias.
       
   - **Semanal**: seleccione el día de la semana, la hora y la zona horaria en la que se ejecuta el trabajo de copia de seguridad.
3. En **Duración de retención**, configure las opciones de retención de la copia de seguridad completa.
    - De forma predeterminada, se seleccionan todas las opciones. Borre los límites de duración de retención que no desee usar y establezca los que sí quiera utilizar.
    - El período de retención mínimo para cualquier tipo de copia de seguridad (completa, diferencial o de registro) es de siete días.
    - Los puntos de recuperación se etiquetan para la retención en función de su duración de retención. Por ejemplo, si selecciona la frecuencia diaria, solo se desencadena una copia de seguridad completa cada día.
    - La copia de seguridad de un día específico se etiqueta y se retiene según la duración de retención semanal y la configuración.
    - La duración de retención mensual y anual se comporta de forma similar.

4. En el menú **Directiva de copia de seguridad completa**, haga clic en **Aceptar** para aceptar la configuración.
5. Para agregar una directiva de copia de seguridad diferencial, seleccione **Copia de seguridad diferencial**.
6. En **Differential Backup policy** (Directiva de copia de seguridad diferencial), seleccione **Enable** (Habilitar) para abrir los controles de retención y frecuencia.
    - A lo sumo, puede desencadenar una copia de seguridad diferencial al día.
    - Como máximo, las copias de seguridad diferenciales se pueden retener durante 180 días. Si necesita más tiempo de retención, debe usar copias de seguridad completas.

    > [!NOTE]
    > Las copias de seguridad incrementales no se admiten en la actualidad. 

7. Haga clic en **Aceptar** para guardar la directiva y volver al menú principal **Directiva de copia de seguridad**.
8. Para agregar una directiva de copia de seguridad del registro de transacciones, seleccione **Copia de seguridad de registros**.
    - En **Copia de seguridad de registros**, seleccione **Habilitar**.
    - Establezca los controles de frecuencia y retención.

    > [!NOTE]
    > Las copias de seguridad de registros comienzan el flujo solo después de que se haya completado correctamente una copia de seguridad completa.

9. Haga clic en **Aceptar** para guardar la directiva y volver al menú principal **Directiva de copia de seguridad**.
10. Cuando haya terminado de definir la directiva de copia de seguridad, haga clic en **Aceptar**.


## <a name="run-an-on-demand-backup"></a>Ejecución de una copia de seguridad a petición

Las copias de seguridad se ejecutan según la programación de la directiva. Puede ejecutar una copia de seguridad a petición siguiendo estos pasos:


1. En el menú Almacén, haga clic en **Elementos de copia de seguridad**.
2. En **Elementos de copia de seguridad**, seleccione la máquina virtual que ejecuta la base de datos de SAP HANA y, a continuación, haga clic en **Hacer copia de seguridad ahora**.
3. En **Hacer copia de seguridad ahora**, use el control del calendario para seleccionar el último día que debería retenerse el punto de recuperación. A continuación, haga clic en **Aceptar**.
4. Supervise las notificaciones del portal. Puede supervisar el progreso del trabajo en el panel del almacén > **Trabajos de copia de seguridad** > En curso. Según el tamaño de la base de datos, la creación de la copia de seguridad inicial puede tardar un tiempo.

## <a name="run-sap-hana-studio-backup-on-a-database-with-azure-backup-enabled"></a>Ejecutar una copia de seguridad de SAP HANA Studio en una base de datos con Azure Backup habilitado

Si desea hacer una copia de seguridad local (mediante HANA Studio) de una base de datos de la que se está haciendo una copia de seguridad con Azure Backup, haga lo siguiente:

1. Espere a que finalicen las copias de seguridad completas o de registro de la base de datos. Compruebe el estado en SAP HANA Studio.
2. Inhabilite las copias de seguridad de registros y establezca el catálogo de copias de seguridad al sistema de archivos de la base de datos pertinente.
3. Para ello, haga doble clic en **systemdb** > **Configuración** > **Seleccionar base de datos** > **Filtro (registro)** .
4. Establezca **enable_auto_log_backup** en **No**.
5. Establezca **log_backup_using_backint** en **False**.
6. A continuación, haga una copia de seguridad completa ad hoc de la base de datos.
7. Espere a que finalicen la copia de seguridad completa y la copia de seguridad de catálogos.
8. Revierta la configuración anterior a las opciones de Azure:
    - Establezca **enable_auto_log_backup** en **Sí**.
    - Establezca **log_backup_using_backint** en **True**.


## <a name="upgrading-protected-10-dbs-to-20"></a>Actualización de bases de datos protegidas de la versión 1.0 a la versión 2.0

Si va a proteger bases de datos SAP HANA 1.0 y quiere actualizarlas a la versión 2.0, siga los pasos que se describen a continuación.

- Detenga la protección con Retain data (Conservar datos) de la base de datos de SDC anterior.
- Vuelva a ejecutar el script anterior al registro con los detalles correctos de (sid y mdc). 
- Vuelva a registrar la extensión (Backup [Copia de seguridad] -> View details [Ver detalles] -> Select the relevant Azure VM [Seleccionar la máquina virtual de Azure pertinente] -> Re-register [Volver a registrar]). 
- Haga clic en Re-discover DBs (Volver a detectar bases de datos) para la misma máquina virtual. Se deberían mostrar las nuevas bases de datos en el paso 2 con los detalles correctos (SYSTEMDB y base de datos de inquilino, no SDC). 
- Proteja estas nuevas bases de datos.

## <a name="next-steps"></a>Pasos siguientes

[Más información](backup-azure-sap-hana-database-troubleshoot.md) sobre cómo solucionar los errores habituales al usar la copia de seguridad de SAP HANA en máquinas virtuales de Azure.
[Más información sobre](backup-azure-arm-vms-prepare.md) cómo hacer copias de seguridad de máquinas virtuales de Azure.
