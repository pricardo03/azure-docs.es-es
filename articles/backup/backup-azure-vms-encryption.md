---
title: Copia de seguridad y restauración de máquinas virtuales de Azure cifradas con Azure Backup
description: Describe cómo realizar copias de seguridad y restauración de máquinas virtuales de Azure cifrada con el servicio de copia de seguridad de Azure.
services: backup
author: geetha
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 4/3/2019
ms.author: geetha
ms.openlocfilehash: 99117c96f79dd7d0da388a0e793908f6ffb8ed27
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59266451"
---
# <a name="back-up-and-restore-encrypted-azure-vm"></a>Copia de seguridad y restauración de máquinas virtuales de Azure cifradas

Este artículo describe cómo realizar copias de seguridad y restauración de Windows o Linux en Azure máquinas virtuales (VM) con discos cifrados mediante la [Azure Backup](backup-overview.md) service.

Si desea obtener más información acerca de cómo interactúa la copia de seguridad de Azure con máquinas virtuales de Azure antes de comenzar, revise estos recursos:

- [Revisión](backup-architecture.md#architecture-direct-backup-of-azure-vms) la arquitectura de copia de seguridad de máquina virtual de Azure.
- [Obtenga información sobre](backup-azure-vms-introduction.md) copia de seguridad de máquina virtual de Azure y la extensión de copia de seguridad de Azure.

## <a name="encryption-support"></a>Compatibilidad con el cifrado

Azure Backup admite la copia de seguridad de máquinas virtuales de Azure que tengan sus discos de SO/datos cifrados con Azure Disk Encryption (ADE). ADE usa BitLocker para el cifrado de máquinas virtuales de Windows y la característica dm-crypt para máquinas virtuales Linux. ADE se integra con Azure Key Vault para administrar los secretos y claves de cifrado de disco. Claves de cifrado de clave (KEK) de Key Vault puede utilizarse para agregar una capa adicional de seguridad, cifrado de secretos de cifrado antes de escribirlos en el almacén de claves.

Azure Backup puede realizar copias de seguridad y restauración de máquinas virtuales de Azure con ADE con y sin la aplicación de Azure AD, como se resume en la tabla siguiente.

**Tipo de disco de máquina virtual** | **ADE (BEK/dm-crypt)** | **ADE y KEK**
--- | --- | --- 
**No administrado** | Sí | Sí
**Administrado**  | Sí | Sí

- Obtenga más información sobre [ADE](../security/azure-security-disk-encryption-overview.md), [Key Vault](../key-vault/key-vault-overview.md), y [KEK](https://blogs.msdn.microsoft.com/cclayton/2017/01/03/creating-a-key-encrypting-key-kek/).
- Leer el [preguntas más frecuentes sobre](../security/azure-security-disk-encryption-faq.md) para el cifrado de disco de máquina virtual de Azure.



### <a name="limitations"></a>Limitaciones

- Puede realizar copias de seguridad y restauración de máquinas virtuales cifradas dentro de la misma suscripción y región.
- Azure Backup admite máquinas virtuales cifradas mediante claves independiente. Cualquier tecla que forma parte de un certificado usado para cifrar una máquina virtual no se admite actualmente.
- Puede realizar copias de seguridad y restauración de máquinas virtuales cifradas dentro de la misma suscripción y región que el almacén de Recovery Services Backup.
- Las máquinas virtuales cifradas no se pueden recuperar a nivel de archivo o carpeta. Necesita recuperar toda la máquina virtual para restaurar archivos y carpetas.
- Al restaurar una máquina virtual, no puede usar el [reemplazar la máquina virtual existente](backup-azure-arm-restore-vms.md#restore-options) opción para las máquinas virtuales cifradas. Esta opción solo se admite para discos administrados sin cifrar.




## <a name="before-you-start"></a>Antes de comenzar

Antes de empezar, haga lo siguiente:

1. Asegúrese de que tiene uno o más [Windows](../security/azure-security-disk-encryption-windows.md) o [Linux](../security/azure-security-disk-encryption-linux.md) habilitado las máquinas virtuales con ADE.
2. [Revise la matriz de compatibilidad](backup-support-matrix-iaas.md) para copia de seguridad de máquina virtual de Azure
3. [Crear](backup-azure-arm-vms-prepare.md#create-a-vault) un almacén de Recovery Services Backup si no tiene uno.
4. Si habilita el cifrado para las máquinas virtuales que ya están habilitadas para la copia de seguridad, basta con proporcionar copias de seguridad con permisos para acceder al almacén de claves para que las copias de seguridad puedan continuar sin interrupciones. [Obtenga más información](#provide-permissions) acerca de cómo asignar estos permisos.

Además, hay un par de cosas que debe hacer en algunas circunstancias:

- **Instale el agente de máquina virtual en la máquina virtual**: Azure Backup realiza una copia de seguridad de máquinas virtuales de Azure instalando una extensión en el agente de máquina virtual de Azure que se ejecuta en la máquina. Si se creó la máquina virtual desde una imagen de marketplace de Azure, el agente está instalado y ejecutándose. Si crea una máquina virtual personalizada, o migrar una máquina local, es posible que deba [instalar el agente manualmente](backup-azure-arm-vms-prepare.md#install-the-vm-agent).
- **Permitir explícitamente el acceso de salida**: Por lo general, no es necesario permitir explícitamente el acceso de red saliente para que una máquina virtual de Azure para poder comunicarse con Azure Backup. Sin embargo, algunas máquinas virtuales podrían experimentar problemas de conexión, que muestra la **ExtensionSnapshotFailedNoNetwork** error al intentar conectarse. Si esto sucede, debería [permitir explícitamente el acceso de salida](backup-azure-arm-vms-prepare.md#explicitly-allow-outbound-access), por lo que la extensión de copia de seguridad de Azure puede comunicarse con Azure direcciones IP públicas para el tráfico de copia de seguridad.



## <a name="configure-a-backup-policy"></a>Configuración de una directiva de copia de seguridad

1. Si todavía no ha creado un almacén de backup Recovery Services, siga [estas instrucciones](backup-azure-arm-vms-prepare.md#create-a-vault)
2. Abra el almacén en el portal y seleccione **copia de seguridad** en el **Introducción** sección.

    ![Hoja Copia de seguridad](./media/backup-azure-vms-encryption/select-backup.png)

3. En **objetivo de Backup** > **donde se ejecuta la carga de trabajo?** seleccione **Azure**.
4. En **qué desea realizar copias de seguridad?** seleccione **Máquina Virtual** > **Aceptar**.

      ![Hoja Escenario](./media/backup-azure-vms-encryption/select-backup-goal-one.png)

5. En **directiva de copia de seguridad** > **elegir directiva de copia de seguridad**, seleccione la directiva que desea asociar con el almacén. A continuación, haga clic en **Aceptar**.
    - Una directiva de copia de seguridad especifica cuando se realizan copias de seguridad y cuánto tiempo se almacenan.
    - Los detalles de la directiva predeterminada se muestran en el menú desplegable.

    ![Hoja Escenario abierta](./media/backup-azure-vms-encryption/select-backup-goal-two.png)

6. Si no desea usar la directiva predeterminada, seleccione **crear nuevo**, y [crear una directiva personalizada](backup-azure-arm-vms-prepare.md#create-a-custom-policy).


7. Elija las máquinas virtuales cifradas que desea realizar copias de seguridad mediante la directiva de selección y seleccione **Aceptar**.

      ![Selección de las máquinas virtuales cifradas](./media/backup-azure-vms-encryption/selected-encrypted-vms.png)

8. Si usa Azure Key Vault, en la página del almacén, verá un mensaje que la copia de seguridad de Azure necesita acceso de solo lectura a las claves y secretos en Key Vault.

    - Si recibe este mensaje, se requiere ninguna acción.
    
        ![Acceso Aceptar](./media/backup-azure-vms-encryption/access-ok.png)
        
    - Si recibe este mensaje, deberá establecer los permisos tal como se describe en el [siguiente procedimiento](#provide-permissions).
    
        ![Advertencia de acceso](./media/backup-azure-vms-encryption/access-warning.png)

9. Haga clic en **habilitar copia de seguridad** para implementar la directiva de copia de seguridad en el almacén y habilitar la copia de seguridad para las máquinas virtuales seleccionadas. 


## <a name="trigger-a-backup-job"></a>Desencadenamiento de un trabajo de copia de seguridad

La copia de seguridad inicial se ejecutará según la programación, pero puede ejecutarlo inmediatamente la manera siguiente:

1. En el menú Almacén, haga clic en **Elementos de copia de seguridad**.
2. En **Elementos de copia de seguridad**, haga clic en **Máquina virtual de Azure**.
3. En el **elementos de copia de seguridad** lista, haga clic en el botón de puntos suspensivos (...).
4. Haga clic en **Realizar copia de seguridad ahora**.
5. En **copia de seguridad ahora**, use el control de calendario para seleccionar el último día que se debe conservar el punto de recuperación. A continuación, haga clic en **Aceptar**.
6. Supervise las notificaciones del portal. Puede supervisar el progreso del trabajo en el panel del almacén > **Trabajos de copia de seguridad** > **En curso**. Según el tamaño de la máquina virtual, la creación de la copia de seguridad inicial puede tardar un tiempo.


## <a name="provide-permissions"></a>Proporcionar permisos

Máquina virtual de Azure necesita acceso de solo lectura para realizar copias de seguridad de las claves y secretos, junto con las máquinas virtuales asociadas.

- El almacén de claves está asociado con el inquilino de Azure AD de la suscripción de Azure. Si eres un **usuario miembro**, Azure Backup adquiere el acceso al almacén de claves sin realizar más acciones.
- Si eres un **usuario invitado**, debe proporcionar permisos para Azure Backup acceder al almacén de claves.

Para establecer permisos:

1. En el portal de Azure, seleccione **todos los servicios**y busque **los almacenes de claves**.
2. Seleccione el almacén de claves asociado a la máquina virtual cifrada que se hace copia de seguridad.
3. Seleccione **las directivas de acceso** > **Agregar nuevo**.
4. Seleccione **seleccione entidad**y, a continuación, escriba **administración de copia de seguridad**. 
5. Seleccione **servicio de administración de copia de seguridad** > **seleccione**.

    ![Selección del servicio Backup](./media/backup-azure-vms-encryption/select-backup-service.png)

6. En **Agregar directiva de acceso** > **configurar a partir de plantilla (opcional)**, seleccione **Azure Backup**.
    - Los permisos necesarios se rellenan previamente en **Permisos clave** y **Permisos de secretos**.
    - Si la máquina virtual está cifrada con **solo BEK**, eliminar la selección de **permisos de clave** puesto que sólo tiene permisos para los secretos. 

    ![Selección de Azure Backup](./media/backup-azure-vms-encryption/select-backup-template.png)

6. Haga clic en **OK**. **Servicio de administración de copia de seguridad** se agrega a **las directivas de acceso**. 

    ![Directivas de acceso](./media/backup-azure-vms-encryption/backup-service-access-policy.png)

7. Haga clic en **guardar** para proporcionar copias de seguridad de Azure con los permisos.

## <a name="restore-an-encrypted-vm"></a>Restauración de una máquina virtual cifrada

Restaurar las máquinas virtuales cifradas como sigue:

1. [Restaurar el disco de máquina virtual](backup-azure-arm-restore-vms.md#restore-disks).
2. A continuación, realice uno de los siguientes:
    - Use la plantilla que se genera durante la operación de restauración para personalizar la configuración de máquina virtual y desencadenar la implementación de máquinas virtuales. [Más información](backup-azure-arm-restore-vms.md#use-templates-to-customize-a-restored-vm).
    - Crear una nueva máquina virtual a partir de los discos restaurados mediante Powershell. [Más información](backup-azure-vms-automation.md#create-a-vm-from-restored-disks).

## <a name="next-steps"></a>Pasos siguientes

Si experimenta algún problema, consulte

- [Errores comunes](backup-azure-vms-troubleshoot.md#troubleshoot-backup-of-encrypted-vms) cuando copia de seguridad y restauración de máquinas virtuales de Azure cifran.
- [General](backup-azure-vms-troubleshoot.md) problemas de la máquina virtual de Azure.
- [Extensión de copia de seguridad o agente de máquina virtual Azure](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md) problemas.
