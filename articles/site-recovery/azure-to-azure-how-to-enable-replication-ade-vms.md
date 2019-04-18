---
title: Configurar la replicación de máquinas virtuales habilitadas para Azure Disk Encryption en Azure Site Recovery | Microsoft Docs
description: En este artículo se describe cómo configurar la replicación de Azure Disk Encryption máquinas virtuales habilitadas desde una región de Azure a otra mediante Site Recovery.
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 04/08/2019
ms.author: sutalasi
ms.openlocfilehash: 4943b730bb46ee00200d84faf95a7ccb069d3aa8
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59678962"
---
# <a name="replicate-azure-disk-encryption-enabled-virtual-machines-to-another-azure-region"></a>Replicación de máquinas virtuales habilitadas para Azure Disk Encryption en otra región de Azure

En este artículo se describe cómo replicar máquinas virtuales habilitadas para Azure Disk Encryption en una región de Azure a otra.

>[!NOTE]
>Actualmente, Azure Site Recovery admite solo máquinas virtuales de Azure que ejecutan un sistema operativo de Windows y que están [habilitado para el cifrado con Azure Active Directory (Azure AD)](https://aka.ms/ade-aad-app).

## <a name="required-user-permissions"></a>Permisos de usuario necesarios
Recuperación de sitio requiere que el usuario tenga permisos para crear el almacén de claves en las claves de región y copia de destino a la región.

Para habilitar la replicación de máquinas virtuales basadas en disco cifrado desde el portal de Azure, el usuario necesita los siguientes permisos:

- Permisos del almacén de claves
    - Enumerar
    - Crear
    - Obtener

-   Permisos de secretos del almacén de claves
    - Enumerar
    - Crear
    - Obtener

- Permisos de clave de almacén de claves (necesarios solo si las máquinas virtuales usan la clave de cifrado para cifrar las claves de cifrado de disco)
    - Enumerar
    - Obtener
    - Crear
    - Cifrado
    - Descifrado

Para administrar permisos, vaya al recurso de almacén de claves en el portal. Agregue los permisos necesarios para el usuario. El ejemplo siguiente muestra cómo habilitar los permisos en el almacén de claves *ContosoWeb2Keyvault*, que se encuentra en la región de origen.

1. Vaya a **inicio** > **Keyvaults** > **ContosoWeb2KeyVault > directivas de acceso**.

   ![Ventana de permisos del almacén de claves](./media/azure-to-azure-how-to-enable-replication-ade-vms/key-vault-permission-1.png)

2. Puede ver que no hay ningún permiso de usuario. Seleccione **Agregar nuevo**. Escriba la información de usuario y permisos.

   ![permisos del almacén de claves](./media/azure-to-azure-how-to-enable-replication-ade-vms/key-vault-permission-2.png)

Si el usuario que es lo que permite la recuperación ante desastres (DR) no tiene permisos para copiar las claves, un administrador de seguridad que tiene los permisos adecuados puede usar el siguiente script para copiar las claves y secretos de cifrado en la región de destino.

Para solucionar problemas de permisos, consulte [problemas de permisos de almacén de clave](#trusted-root-certificates-error-code-151066) más adelante en este artículo.

>[!NOTE]
>Para habilitar la replicación de máquinas virtuales basadas en disco cifrado desde el portal, se necesitan al menos "lista" permisos en los almacenes de claves, secretos y claves.

## <a name="copy-disk-encryption-keys-to-the-dr-region-by-using-the-powershell-script"></a>Copiar las claves de cifrado de disco a la región de recuperación ante desastres mediante el script de PowerShell

1. [Abra el código de script sin formato "CopyKeys"](https://aka.ms/ade-asr-copy-keys-code).
2. Copie el script en un archivo y asígnele el nombre **copia keys.ps1**.
3. Abra la aplicación de Windows PowerShell y vaya a la carpeta donde guardó el archivo.
4. Execute Copy-keys.ps1.
5. Proporcione las credenciales de Azure para iniciar sesión.
6. Seleccione la **suscripción de Azure** de las máquinas virtuales.
7. Espere a que los grupos de recursos cargar y, a continuación, seleccione el **grupo de recursos** de las máquinas virtuales.
8. Seleccione las máquinas virtuales en la lista que se muestra. Solo las máquinas virtuales que están habilitadas para el cifrado de disco están en la lista.
9. Seleccione el **ubicación de destino**.

    - **Almacenes de claves de cifrado de disco**
    - **Almacenes de claves de cifrado de claves**

   De forma predeterminada, Site Recovery crea un nuevo almacén de claves en la región de destino. El nombre del almacén tiene un sufijo "asr" que se basa en las claves de cifrado de disco de máquina virtual de origen. Si ya existe un almacén de claves creado por Site Recovery, se vuelve a usar. Seleccione un almacén de claves diferente de la lista si es necesario.

## <a name="enable-replication"></a>Habilitar replicación

En este ejemplo, la región principal de Azure es Asia oriental y la región secundaria es Asia Suroriental.

1. En el almacén, seleccione **+ replicar**.
2. Tenga en cuenta los siguientes campos.
    - **Origen**: el punto de origen de las máquinas virtuales, que en este caso es **Azure**.
    - **Ubicación de origen**: La región de Azure donde desea proteger las máquinas virtuales. En este ejemplo, la ubicación de origen es "Asia oriental".
    - **Modelo de implementación**: El modelo de implementación de Azure de las máquinas de origen.
    - **Suscripción de origen**: suscripción a la que pertenecen sus máquinas virtuales de origen. Puede ser cualquier suscripción que esté en el mismo inquilino de Azure Active Directory como su almacén de recovery services.
    - **Grupo de recursos**: el grupo de recursos al que pertenecen las máquinas virtuales de origen. Se muestran todas las máquinas virtuales en el grupo de recursos seleccionado para la protección en el paso siguiente.

3. En **máquinas virtuales** > **seleccionar máquinas virtuales**, seleccione cada máquina virtual que desea replicar. Solo puede seleccionar aquellas máquinas en las que se pueda habilitar la replicación. Después, seleccione **Aceptar**.

4. En **configuración**, puede configurar las siguientes opciones de sitio de destino.

    - **Ubicación de destino**: ubicación donde se replicarán los datos de la máquina virtual de origen. Site Recovery proporciona una lista de regiones de destino adecuadas según la ubicación de la máquina seleccionada. Se recomienda que use la misma ubicación como ubicación del almacén de Recovery Services.
    - **Suscripción de destino**: La suscripción de destino que se usa para la recuperación ante desastres. De forma predeterminada, la suscripción de destino será la misma que la de origen.
    - **Grupo de recursos de destino**: el grupo de recursos al que pertenecen todas las máquinas virtuales replicadas. De forma predeterminada, Site Recovery crea un nuevo grupo de recursos en la región de destino. El nombre Obtiene el sufijo "asr". Si ya existe un grupo de recursos que se creó con Azure Site Recovery, se vuelve a usar. También puede elegir personalizarlo, tal como se muestra en la sección siguiente. La ubicación del grupo de recursos de destino puede ser cualquier región de Azure, excepto la región donde se hospedan las máquinas virtuales de origen.
    - **Red virtual de destino**: De forma predeterminada, Site Recovery crea una nueva red virtual en la región de destino. El nombre Obtiene el sufijo "asr". Se ha asignado a la red de origen y utilizar para todas las protecciones futuras. [más información](site-recovery-network-mapping-azure-to-azure.md) sobre la asignación de red.
    - **Las cuentas de almacenamiento de destino (si el origen de máquina virtual no usa discos administrados)**: De forma predeterminada, Site Recovery crea una nueva cuenta de almacenamiento de destino mediante la imitación de la configuración de almacenamiento de máquina virtual de origen. Si ya existe una cuenta de almacenamiento, se vuelve a usar.
    - **Discos administrados de réplica (si la máquina virtual de origen utiliza discos administrados)**: Site Recovery crea nuevos discos administrados de réplica en la región de destino para reflejar los discos administrados de la máquina virtual de origen del mismo tipo de almacenamiento (estándar o premium) como discos administrados de la máquina virtual de origen.
    - **Cuentas de almacenamiento en caché**: Site Recovery necesita una cuenta de almacenamiento adicional denominada *caché almacenamiento* en la región de origen. Se realiza un seguimiento de todos los cambios en las máquinas virtuales de origen y se envían a la cuenta de almacenamiento en caché. Que están replicado, a continuación, en la ubicación de destino.
    - **Conjunto de disponibilidad**: De forma predeterminada, Site Recovery crea un nuevo conjunto de disponibilidad en la región de destino. El nombre tiene el sufijo "asr". Si un conjunto de disponibilidad que se creó por Site Recovery ya existe, se vuelve a usar.
    - **Almacenes de claves de cifrado de disco**: De forma predeterminada, Site Recovery crea un nuevo almacén de claves en la región de destino. Tiene un sufijo "asr" que se basa en las claves de cifrado de disco de máquina virtual de origen. Si un almacén de claves que se creó con Azure Site Recovery ya existe, se vuelve a usar.
    - **Almacenes de claves de cifrado de claves**: De forma predeterminada, Site Recovery crea un nuevo almacén de claves en la región de destino. El nombre tiene un sufijo "asr" que se basa en las claves de cifrado de claves de máquina virtual de origen. Si un almacén de claves creado por Azure Site Recovery ya existe, se vuelve a usar.
    - **Directiva de replicación**: Define la configuración de historial de retención de punto de recuperación y la frecuencia de instantánea coherente con la aplicación. De forma predeterminada, Site Recovery crea una nueva directiva de replicación con la configuración predeterminada de *24 horas* retención de punto de recuperación y *60 minutos* de frecuencia de instantánea coherente con la aplicación.

## <a name="customize-target-resources"></a>Personalizar los recursos de destino

Siga estos pasos para modificar la configuración de destino de recuperación del sitio predeterminado.

1. Seleccione **personalizar** junto a "Suscripción de destino" para modificar la suscripción de destino predeterminado. Seleccione la suscripción en la lista de suscripciones que están disponibles en el inquilino de Azure AD.

2. Seleccione **personalizar** junto a "grupo de recursos, red, almacenamiento y conjuntos de disponibilidad" para modificar la configuración predeterminada siguiente:
    - Para **grupo de recursos de destino**, seleccione el grupo de recursos de la lista de grupos de recursos en la ubicación de destino de la suscripción.
    - Para **red virtual de destino**, seleccione la red de una lista de redes virtuales en la ubicación de destino.
    - Para **conjunto de disponibilidad**, puede agregar configuración de conjunto de disponibilidad a la máquina virtual, si fuesen parte de un conjunto de disponibilidad en la región de origen.
    - Para **cuentas de almacenamiento de destino**, seleccione la cuenta para usarla.

2. Seleccione **personalizar** junto a "Configuración de cifrado" para modificar la configuración predeterminada siguiente:
   - Para **almacén de claves de cifrado de disco de destino**, seleccione el almacén de claves de cifrado de disco de destino en la lista de almacenes de claves en la ubicación de destino de la suscripción.
   - Para **almacén de claves de cifrado de claves de destino**, seleccione el almacén de claves de cifrado de claves de destino en la lista de almacenes de claves en la ubicación de destino de la suscripción.

3. Seleccione **crear recurso de destino** > **Habilitar replicación**.
4. Después de que las máquinas virtuales habilitadas para la replicación, puede comprobar el estado de mantenimiento de las máquinas virtuales en **elementos replicados**.

>[!NOTE]
>Durante la replicación inicial, el estado puede tardar algún tiempo en actualizarse, sin ningún progreso aparente. Haga clic en **actualizar** para obtener el estado más reciente.

## <a name="update-target-vm-encryption-settings"></a>Actualización de la configuración de cifrado de la máquina virtual de destino
En los escenarios siguientes, se le pide para actualizar la configuración de cifrado de la máquina virtual de destino:
  - Se habilitó la replicación de Site Recovery en la máquina virtual. Más adelante, habilitó el cifrado de disco en la máquina virtual de origen.
  - Se habilitó la replicación de Site Recovery en la máquina virtual. Más adelante, se puede cambiar la clave de cifrado de disco o la clave de cifrado de claves en la máquina virtual de origen.

Puede usar [una secuencia de comandos](#copy-disk-encryption-keys-to-the-dr-region-by-using-the-powershell-script) para copiar las claves de cifrado en la región de destino y, a continuación, actualice la configuración de cifrado de destino en **almacén de Recovery services** > *elementoreplicado*  >  **Propiedades** > **proceso y red**.

![Actualizar la ventana de cuadro de diálogo de configuración de ADE](./media/azure-to-azure-how-to-enable-replication-ade-vms/update-ade-settings.png)

## <a id="trusted-root-certificates-error-code-151066"></a>Solucionar problemas de permisos de almacén de claves durante la replicación de máquinas virtuales en Azure

**Causa 1:** Puede que haya seleccionado desde la región de destino un almacén de claves creado ya que no tiene los permisos necesarios en lugar de permitir que Site Recovery cree uno. Asegúrese de que el almacén de claves tiene el requieren permisos, como se describió anteriormente.

*Por ejemplo*: Vuelva a replicar una máquina virtual con el almacén de claves *ContososourceKeyvault* en una región de origen.
Tiene todos los permisos en el almacén de claves de región de origen. Pero durante la protección, seleccionar el almacén clave ya creado ContosotargetKeyvault, que no tiene permisos. Se produce un error.

**Solución:** Vaya a **inicio** > **Keyvaults** > **ContososourceKeyvault** > **las directivas de acceso** y agregue los permisos adecuados.

**Causa 2:** Es posible que ha seleccionado de la región de destino un almacén de claves ya ha creado no tiene permisos en lugar de permitir que Site Recovery cree uno descifrar y cifrar. Asegúrese de que tiene descifrar y cifrar los permisos si también está cifrando la clave en la región de origen.</br>

*Por ejemplo*: Vuelva a replicar una máquina virtual que tiene un almacén de claves *ContososourceKeyvault* en la región de origen. Tiene todos los permisos necesarios en el almacén de claves de región de origen. Pero durante la protección, seleccionar el almacén clave ya creado ContosotargetKeyvault, que no tiene permisos para descifrar y cifrar. Se produce un error.</br>

**Solución:** Vaya a **inicio** > **Keyvaults** > **ContososourceKeyvault** > **las directivas de acceso**. Agregar permisos en **permisos de clave** > **operaciones criptográficas**.

## <a name="next-steps"></a>Pasos siguientes

[Más información](site-recovery-test-failover-to-azure.md) sobre la ejecución de una conmutación por error de prueba.
