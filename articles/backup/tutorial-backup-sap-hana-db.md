---
title: 'Tutorial: Copia de seguridad de bases de datos de SAP HANA en máquinas virtuales de Azure'
description: En este tutorial, aprenderá a hacer una copia de seguridad de una base de datos de SAP HANA que se ejecuta en una máquina virtual de Azure en un almacén de Azure Backup Recovery Services.
ms.topic: tutorial
ms.date: 11/12/2019
ms.openlocfilehash: a622370fca3144aeb6a5d7c071c227b3c21cf135
ms.sourcegitcommit: e50a39eb97a0b52ce35fd7b1cf16c7a9091d5a2a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2019
ms.locfileid: "74287192"
---
# <a name="tutorial-back-up-sap-hana-databases-in-an-azure-vm"></a>Tutorial: Copia de seguridad de bases de datos de SAP HANA en una máquina virtual de Azure

Este tutorial muestra cómo realizar una copia de seguridad de una base de datos de SAP HANA que se ejecuta en máquinas virtuales de Azure en un almacén de Azure Backup Recovery Services. En este artículo, aprenderá a:

> [!div class="checklist"]
>
> * Crear y configurar un almacén
> * Detectar bases de datos
> * Configuración de copias de seguridad

[Aquí](sap-hana-backup-support-matrix.md#scenario-support) están todos los escenarios que se admiten actualmente.

## <a name="onboard-to-the-public-preview"></a>Incorporación a la versión preliminar pública

Incorpórese a la versión preliminar pública de la siguiente manera:

* En el portal, registre el identificador de suscripción en el proveedor de servicios de Recovery Services [siguiendo este artículo](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-register-provider-errors#solution-3---azure-portal).

* Para PowerShell, ejecute este cmdlet. Debería completarse como "Registrado".

    ```powershell
    Register-AzProviderFeature -FeatureName "HanaBackup" –ProviderNamespace Microsoft.RecoveryServices
    ```

## <a name="prerequisites"></a>Requisitos previos

Asegúrese de seguir estos pasos antes de configurar copias de seguridad:

1. En la máquina virtual que ejecuta la base de datos de SAP HANA, instale y habilite los paquetes de controlador ODBC desde el paquete o soporte físico SLES oficial mediante Zypper, de la manera siguiente:

```bash
sudo zypper update
sudo zypper install unixODBC
```

>[!NOTE]
> Si no actualiza los repositorios, asegúrese de que la versión de unixODBC es, como mínimo, la 2.3.4. Para conocer la versión de uniXODBC, ejecute `odbcinst -j` como raíz
>

2. Permita la conectividad desde la máquina virtual a Internet para que pueda acceder a Azure, tal y como se describe en el [procedimiento siguiente](#set-up-network-connectivity).

3. Ejecute el script de registro previo en la máquina virtual donde está instalado HANA como usuario raíz. [Este script](https://aka.ms/scriptforpermsonhana) establecerá los [permisos adecuados](#setting-up-permissions).

## <a name="set-up-network-connectivity"></a>Configurar la conectividad de red

Para todas las operaciones, la máquina virtual SAP HANA necesita conectividad a las direcciones IP públicas de Azure. Si falta dicha conectividad, las operaciones de máquina virtual como detectar bases de datos, configurar y programar copias de seguridad o restaurar puntos de recuperación producirán errores. Establezca la conectividad permitiendo el acceso a los intervalos IP del centro de datos de Azure:

* Puede descargar los [intervalos de direcciones IP](https://www.microsoft.com/download/details.aspx?id=41653) para los centros de datos de Azure y, a continuación, permitir el acceso a estas direcciones IP.
* Si usa grupos de seguridad de red (NSG), puede usar la [etiqueta de servicio](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags) AzureCloud para permitir todas las direcciones IP públicas de Azure. Para modificar las reglas de NSG, puede usar el [cmdlet Set-AzureNetworkSecurityRule](https://docs.microsoft.com/powershell/module/servicemanagement/azure/set-azurenetworksecurityrule?view=azuresmps-4.0.0).
* El puerto 443 debe agregarse a la lista de permitidos, ya que el transporte se realiza mediante HTTPS.

## <a name="setting-up-permissions"></a>Configuración de permisos

El script de registro previo realiza las acciones siguientes:

1. Crea AZUREWLBACKUPHANAUSER en el sistema HANA y agrega estos roles y permisos requeridos:
   * DATABASE ADMIN: para crear nuevas bases de datos durante la restauración.
   * CATALOG READ: para leer el catálogo de copia de seguridad.
   * SAP_INTERNAL_HANA_SUPPORT: para acceder a algunas tablas privadas.
2. Agrega una clave a Hdbuserstore para que el complemento de HANA controle todas las operaciones (consultas de base de datos, operaciones de restauración, configuración y ejecución de copias de seguridad).

Para confirmar la creación de la clave, ejecute el comando HDBSQL en la máquina HANA con credenciales SIDADM:

```hdbsql
hdbuserstore list
```

La salida de comandos debe mostrar la tecla {SID}{DBNAME}, con el usuario como AZUREWLBACKUPHANAUSER.

>[!NOTE]
> Asegúrese de que tiene un conjunto único de archivos SSFS en /usr/sap/{SID}/home/.hdb/. Solo debería haber una carpeta en esta ruta de acceso.
>

## <a name="create-a-recovery-service-vault"></a>Creación de un almacén de Recovery Services

Un almacén de Recovery Services es una entidad que almacena las copias de seguridad y los puntos de recuperación creados a lo largo del tiempo. También contiene las directivas de copia de seguridad asociadas con las máquinas virtuales protegidas.

Para crear un almacén de Recovery Services:

1. Inicie sesión en su suscripción en [Azure Portal](https://portal.azure.com/).

2. En el menú izquierdo, seleccione **Todos los servicios**.

![Seleccionar Todos los servicios](./media/tutorial-backup-sap-hana-db/all-services.png)

3. En el cuadro de diálogo **Todos los servicios**, escriba **Recovery Services**. La lista de recursos se filtra en función de lo que especifique. En la lista de recursos, seleccione **Almacenes de Recovery Services**.

![Selección de almacenes de Recovery Services](./media/tutorial-backup-sap-hana-db/recovery-services-vaults.png)

4. En el panel **Almacenes de Recovery Services**, seleccione **Agregar**.

![Adición de un almacén de Recovery Services](./media/tutorial-backup-sap-hana-db/add-vault.png)

Se abre el cuadro de diálogo **Almacén de Recovery Services**. Proporcione valores para **Nombre, Suscripción, Grupo de recursos** y **Ubicación**.

![Crear almacén de Recovery Services](./media/tutorial-backup-sap-hana-db/create-vault.png)

* **Nombre**: El nombre se usa para identificar el almacén de Recovery Services y debe ser único en la suscripción de Azure. Especifique un nombre que tenga entre 2 y 50 caracteres. El nombre debe comenzar por una letra y consta solo de letras, números y guiones. En este tutorial, hemos usado el nombre **SAPHanaVault**.
* **Suscripción**: elija la suscripción que desee usar. Si es miembro de una sola suscripción, verá solo ese nombre. Si no está seguro de qué suscripción va a utilizar, use la predeterminada (sugerida). Solo hay varias opciones si la cuenta profesional o educativa está asociada a más de una suscripción de Azure. Aquí hemos usado la suscripción **SAP HANA solution lab subscription**.
* **Grupo de recursos**: Use un grupo de recursos existente o cree uno. Aquí hemos usado **SAPHANADemo**.<br>
Para ver la lista de grupos de recursos disponibles en una suscripción, seleccione **Usar existente** y, después, seleccione un recurso en el cuadro de lista desplegable. Para crear un grupo de recursos, seleccione **Crear nuevo** y escriba el nombre. Para más información acerca de los grupos de recursos, consulte [Introducción a Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).
* **Ubicación**: seleccione la región geográfica del almacén. El almacén debe estar en la misma región que las máquinas virtuales que ejecutan SAP HANA. Hemos usado **Este de EE. UU. 2**.

5. Seleccione **Revisar + crear**.

   ![Seleccionar Revisar + crear](./media/tutorial-backup-sap-hana-db/review-create.png)

Ahora se ha creado el almacén de Recovery Services.

## <a name="discover-the-databases"></a>Detección de bases de datos

1. En el almacén, en **Introducción**, haga clic en **Copia de seguridad**. En **¿Dónde se ejecuta su carga de trabajo?** , seleccione **SAP HANA en máquina virtual de Azure**.
2. Haga clic en **Iniciar detección**. Se iniciará la detección de máquinas virtuales de Linux no protegidas en la región del almacén. Verá la máquina virtual de Azure que desea proteger.
3. En **Seleccionar máquinas virtuales**, haga clic en el vínculo para descargar el script que proporciona permisos para el servicio Azure Backup para acceder a las máquinas virtuales de SAP HANA para la detección de bases de datos.
4. Ejecute el script en la máquina virtual que hospeda las bases de datos de SAP HANA de las que desea hacer una copia de seguridad.
5. Tras ejecutar el script en la máquina virtual, seleccione la máquina virtual en **Seleccionar máquinas virtuales**. A continuación, haga clic en **Detectar bases de datos**.
6. Azure Backup detecta todas las bases de datos de SAP HANA en la máquina virtual. Durante la detección, Azure Backup registra la máquina virtual con el almacén e instala una extensión en la máquina virtual. No se instala ningún agente en la base de datos.

   ![Detección de bases de datos](./media/tutorial-backup-sap-hana-db/database-discovery.png)

## <a name="configure-backup"></a>Configuración de la copia de seguridad

Ahora que se han detectado las bases de datos de las que queremos realizar una copia de seguridad, vamos a habilitar la copia de seguridad.

1. Haga clic en **Configurar copia de seguridad**.

![Configuración de la copia de seguridad](./media/tutorial-backup-sap-hana-db/configure-backup.png)

2. En **Seleccione los elementos de los que desea hacer una copia de seguridad**, seleccione una o varias bases de datos que desee proteger y, a continuación, haga clic en **Aceptar**.

![Seleccionar elementos de los que se va a hacer una copia de seguridad](./media/tutorial-backup-sap-hana-db/select-items-to-backup.png)

3. En **Directiva de copia de seguridad > Elegir directiva de copia de seguridad**, cree una nueva directiva de copia de seguridad para las bases de datos, según las instrucciones de la sección siguiente.

![Elegir directiva de copia de seguridad](./media/tutorial-backup-sap-hana-db/backup-policy.png)

4. Tras crear la directiva, en el menú **Copia de seguridad**, haga clic en **Habilitar copia de seguridad**.

   ![Clic en Habilitar copia de seguridad](./media/tutorial-backup-sap-hana-db/enable-backup.png)

5. Realice el seguimiento del progreso de la configuración de copia de seguridad en el área **Notificaciones** del portal.

## <a name="creating-a-backup-policy"></a>Creación de una directiva de copia de seguridad

Una directiva de copia de seguridad define cuándo se realizan las copias de seguridad y cuánto tiempo se conservan.

* Una directiva se crea en el nivel de almacén.
* Varios almacenes pueden usar la misma directiva de copia de seguridad, pero debe aplicar la directiva de copia de seguridad a cada almacén.

Especifique la configuración de la directiva como se muestra a continuación:

1. En **Nombre de la directiva**, escriba un nombre para la nueva directiva. En este caso, escriba **SAPHANA**.

![Escribir el nombre de la nueva directiva](./media/tutorial-backup-sap-hana-db/new-policy.png)

2. En el menú **Full Backup policy** (Directiva de copia de seguridad completa), seleccione un valor para **Frecuencia de copia de seguridad**. Puede elegir **Diaria** o **Semanal**. En este tutorial, elegimos la copia de seguridad **Diaria**.

![Seleccionar una frecuencia de copia de seguridad](./media/tutorial-backup-sap-hana-db/backup-frequency.png)

3. En **Duración de retención**, configure las opciones de retención de la copia de seguridad completa.
   * De forma predeterminada, se seleccionan todas las opciones. Borre los límites del intervalo de retención que no desee usar y establezca aquellos que sí quiera utilizar.
   * El período de retención mínimo para cualquier tipo de copia de seguridad (completa, diferencial o de registro) es de siete días.
   * Los puntos de recuperación se etiquetan para la retención en función de su duración de retención. Por ejemplo, si selecciona la frecuencia diaria, solo se desencadena una copia de seguridad completa cada día.
   * La copia de seguridad de un día específico se etiqueta y se retiene según la duración de retención semanal y la configuración.
   * La duración de retención mensual y anual se comporta de forma similar.
4. En el menú **Directiva de copia de seguridad completa**, haga clic en **Aceptar** para aceptar la configuración.
5. A continuación, seleccione **Copia de seguridad diferencial** para agregar una directiva diferencial.
6. En **Differential Backup policy** (Directiva de copia de seguridad diferencial), seleccione **Enable** (Habilitar) para abrir los controles de retención y frecuencia. Hemos habilitado una copia de seguridad diferencial cada **Domingo** a las **2:00 AM**, que se conserva durante **30 días**.

   ![Directiva de copia de seguridad diferencial](./media/tutorial-backup-sap-hana-db/differential-backup-policy.png)

>[!NOTE]
>Las copias de seguridad incrementales no se admiten en la actualidad.
>

7. Haga clic en **Aceptar** para guardar la directiva y volver al menú principal **Directiva de copia de seguridad**.
8. Para agregar una directiva de copia de seguridad del registro de transacciones, seleccione **Copia de seguridad de registros**.
   * La **Copia de seguridad de registros** está establecida de forma predeterminada en **Habilitar**. No se puede deshabilitar, ya que SAP HANA administra todas las copias de seguridad de registros.
   * Hemos establecido **2 horas** como programación de copia de seguridad y **15 días** de período de retención.

    ![Directiva de copia de seguridad de registros](./media/tutorial-backup-sap-hana-db/log-backup-policy.png)

>[!NOTE]
> Las copias de seguridad de registros comienzan el flujo solo después de que se haya completado correctamente una copia de seguridad completa.
>

9. Haga clic en **Aceptar** para guardar la directiva y volver al menú principal **Directiva de copia de seguridad**.
10. Cuando haya terminado de definir la directiva de copia de seguridad, haga clic en **Aceptar**.

Ahora ha configurado correctamente las copias de seguridad de las bases de datos de SAP HANA.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre cómo [ejecutar copias de seguridad a petición en bases de datos de SAP HANA que se ejecutan en máquinas virtuales de Azure](backup-azure-sap-hana-database.md#run-an-on-demand-backup).
* Obtenga información sobre cómo [restaurar bases de datos de SAP HANA que se ejecutan en máquinas virtuales de Azure](sap-hana-db-restore.md).
* Obtenga información acerca de cómo [administrar bases de datos de SAP HANA de las que se realizan copias de seguridad mediante Azure Backup](sap-hana-db-manage.md).
* Obtenga información acerca de cómo [solucionar problemas comunes al realizar copias de seguridad de bases de datos de SAP HANA](backup-azure-sap-hana-database-troubleshoot.md).
