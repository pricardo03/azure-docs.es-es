---
title: Configuración de la replicación en máquinas virtuales habilitadas para Azure Disk Encryption en Azure Site Recovery | Microsoft Docs
description: En este artículo se describe cómo configurar la replicación de máquinas virtuales habilitadas para Azure Disk Encryption de una región de Azure a otra mediante Site Recovery.
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 04/08/2019
ms.author: sutalasi
ms.openlocfilehash: 4943b730bb46ee00200d84faf95a7ccb069d3aa8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60791020"
---
# <a name="replicate-azure-disk-encryption-enabled-virtual-machines-to-another-azure-region"></a>Replicación de máquinas virtuales habilitadas para Azure Disk Encryption en otra región de Azure

En este artículo se describe cómo replicar máquinas virtuales habilitadas para Azure Disk Encryption de una región de Azure a otra.

>[!NOTE]
>Actualmente, Azure Site Recovery solo admite máquinas virtuales de Azure que se ejecutan con un sistema operativo Windows y que están [habilitadas para el cifrado con Azure Active Directory (Azure AD)](https://aka.ms/ade-aad-app).

## <a name="required-user-permissions"></a>Permisos de usuario necesarios
Site Recovery requiere que el usuario tenga permisos para crear el almacén de claves en la región de destino y que copie las claves a esta región.

Para habilitar la replicación de máquinas virtuales habilitadas para Azure Disk Encryption desde Azure Portal, el usuario necesita los permisos siguientes:

- Permisos del almacén de claves
    - Enumerar
    - Crear
    - Obtener

-   Permisos de secretos del almacén de claves
    - Enumerar
    - Crear
    - Obtener

- Permisos de clave del almacén de claves (necesarios solo si las máquinas virtuales usan la clave de cifrado de claves para cifrar las claves de cifrado de disco)
    - Enumerar
    - Obtener
    - Crear
    - Cifrado
    - Descifrado

Para administrar permisos, vaya al recurso de almacén de claves en el portal. Agregue los permisos requeridos del usuario. El ejemplo siguiente muestra cómo se habilitan permisos para el almacén de claves *ContosoWeb2Keyvault*, que se encuentra en la región de origen.

1. Vaya a **Inicio** > **Keyvaults** > **ContosoWeb2KeyVault > Directivas de acceso**.

   ![Ventana de permisos del almacén de claves](./media/azure-to-azure-how-to-enable-replication-ade-vms/key-vault-permission-1.png)

2. Puede ver que no hay ningún permiso de usuario. Seleccione **Agregar nuevo**. Escriba la información del usuario y de los permisos.

   ![Permisos del almacén de claves](./media/azure-to-azure-how-to-enable-replication-ade-vms/key-vault-permission-2.png)

Si el usuario que habilita la recuperación ante desastres (DR) no tiene los permisos necesarios para copiar las claves, un administrador de seguridad que cuente con los permisos adecuados podrá usar el script siguiente para copiar las claves y los secretos de cifrado en la región de destino.

Para solucionar problemas de permisos, consulte [incidencias de permisos del almacén de claves](#trusted-root-certificates-error-code-151066) más adelante en este artículo.

>[!NOTE]
>Para habilitar la replicación de máquinas virtuales habilitadas para Azure Disk Encryption desde el portal, necesita al menos permisos de "lista" en los almacenes de claves, los secretos y las claves.

## <a name="copy-disk-encryption-keys-to-the-dr-region-by-using-the-powershell-script"></a>Copia de las claves de Disk Encryption a la región de recuperación ante desastres con el script de PowerShell

1. [Abra el código de script sin formato "CopyKeys"](https://aka.ms/ade-asr-copy-keys-code).
2. Copie el script en un archivo y asígnele el nombre **Copy-keys.ps1**.
3. Abra la aplicación Windows PowerShell y vaya a la carpeta donde se ha guardado el archivo.
4. Ejecute Copy-keys.ps1.
5. Proporcione las credenciales de Azure para iniciar sesión.
6. Seleccione la **suscripción de Azure** de las máquinas virtuales.
7. Espere a que se carguen los grupos de recursos y luego seleccione el **grupo de recursos** de las máquinas virtuales.
8. Seleccione las máquinas virtuales en la lista que se muestra. Solo las máquinas virtuales que están habilitadas para el cifrado de disco aparecen en la lista.
9. Seleccione la **ubicación de destino**.

    - **Almacenes de claves de cifrado de disco**
    - **Almacenes de claves de cifrado de claves**

   De forma predeterminada, Site Recovery crea un nuevo almacén de claves en la región de destino. El nombre del almacén tiene un sufijo "asr" y se basa en las claves de cifrado de disco de la máquina virtual de origen. Si un almacén de claves que ha creado Site Recovery ya existe, se vuelve a usar. Si es necesario, seleccione un almacén de claves diferente de la lista.

## <a name="enable-replication"></a>Habilitar replicación

En este ejemplo la región principal de Azure es Asia Oriental y la secundaria es Asia Suroriental.

1. En el almacén, seleccione **+ Replicar**.
2. Especifique los campos siguientes.
    - **Origen**: el punto de origen de las máquinas virtuales, que en este caso es **Azure**.
    - **Ubicación de origen**: la región de Azure donde quiere proteger las máquinas virtuales. En este ejemplo, la ubicación de origen es "Asia Oriental".
    - **Modelo de implementación**: el modelo de implementación de Azure de las máquinas de origen.
    - **Suscripción de origen**: suscripción a la que pertenecen sus máquinas virtuales de origen. Puede tratarse de cualquier suscripción que se encuentre en el mismo inquilino de Azure Active Directory donde esté el almacén de servicios de recuperación.
    - **Grupo de recursos**: el grupo de recursos al que pertenecen las máquinas virtuales de origen. Todas las máquinas virtuales del grupo de recursos seleccionado se enumeran para su protección en el paso siguiente.

3. En **Máquinas virtuales** > **Seleccionar máquinas virtuales**, seleccione las máquinas virtuales que quiera replicar. Solo puede seleccionar aquellas máquinas en las que se pueda habilitar la replicación. Después, seleccione **Aceptar**.

4. En **Configuración**, puede configurar las opciones siguientes de sitio de destino.

    - **Ubicación de destino**: ubicación donde se replicarán los datos de la máquina virtual de origen. Site Recovery proporciona una lista de regiones de destino adecuadas en función de la ubicación de la máquina seleccionada. Se recomienda usar la misma ubicación que la del almacén de Recovery Services.
    - **Suscripción de destino**: la suscripción de destino que se usa para la recuperación ante desastres. De forma predeterminada, la suscripción de destino será la misma que la de origen.
    - **Grupo de recursos de destino**: el grupo de recursos al que pertenecen todas las máquinas virtuales replicadas. De forma predeterminada, Site Recovery crea un nuevo grupo de recursos en la región de destino. El nombre toma el sufijo "asr". Si un grupo de recursos que ha creado Azure Site Recovery ya existe, se vuelve a usar. También se puede elegir personalizarlo, tal como se muestra en la sección siguiente. La ubicación del grupo de recursos de destino puede ser cualquier región de Azure excepto la región en la que se hospedan las máquinas virtuales de origen.
    - **Red virtual de destino**: De manera predeterminada, Site Recovery crea una red virtual en la región de destino. El nombre toma el sufijo "asr". Se asigna a la red de origen y se usa para todas las protecciones futuras. [más información](site-recovery-network-mapping-azure-to-azure.md) sobre la asignación de red.
    - **Cuentas de almacenamiento de destino (si la máquina virtual de origen no usa discos administrados)** : Site Recovery crea, de forma predeterminada, una cuenta de almacenamiento de destino mediante la imitación de la configuración de almacenamiento de la máquina virtual de origen. Si ya existe una cuenta de almacenamiento, esta se vuelve a usar.
    - **Discos administrados de réplica (si la máquina virtual de origen utiliza discos administrados)** : Site Recovery crea discos administrados de réplica en la región de destino para reflejar los discos administrados de la máquina virtual de origen del mismo tipo de almacenamiento (Standard o Premium) que los discos administrados de la máquina virtual de origen.
    - **Cuentas de almacenamiento en caché**: Site Recovery necesita una cuenta de almacenamiento adicional llamada *almacenamiento en caché* en la región de origen. Todos los cambios que se realicen en las máquinas virtuales de origen se controlan y se envían a la cuenta de almacenamiento en caché. Luego se replican en la ubicación de destino.
    - **Conjunto de disponibilidad**: Site Recovery, de manera predeterminada, crea un conjunto de disponibilidad nuevo en la región de destino. El nombre tiene el sufijo "asr". Si un conjunto de disponibilidad que ha creado Azure Site Recovery ya existe, se vuelve a usar.
    - **Almacenes de claves de cifrado de disco**: De forma predeterminada, Site Recovery crea un nuevo almacén de claves en la región de destino. El nombre tiene un sufijo "asr" y se basa en las claves de cifrado de disco de la máquina virtual de origen. Si un almacén de claves que ha creado Azure Site Recovery ya existe, se vuelve a usar.
    - **Almacenes de claves de cifrado de claves**: De forma predeterminada, Site Recovery crea un nuevo almacén de claves en la región de destino. El nombre tiene un sufijo "asr" y se basa en las claves de cifrado de claves de la máquina virtual de origen. Si un almacén de claves que ha creado Azure Site Recovery ya existe, se vuelve a usar.
    - **Directiva de replicación**: define la configuración del historial de retención del punto de recuperación y la frecuencia de instantánea coherente con la aplicación. De manera predeterminada, Site Recovery crea una directiva de replicación nueva con la configuración predeterminada de *24 horas* para la retención del punto de recuperación y *60 minutos* para la frecuencia de instantánea coherente con la aplicación.

## <a name="customize-target-resources"></a>Personalizar los recursos de destino

Siga estos pasos para modificar los valores de destino predeterminados de Site Recovery.

1. Seleccione **Personalizar**, junto a "Suscripción de destino", para modificar la suscripción de destino predeterminada. Seleccione la suscripción en la lista de suscripciones disponibles en el inquilino de Azure AD.

2. Seleccione **Personalizar**, junto a "Grupo de recursos, red, almacenamiento y conjuntos de disponibilidad", para modificar los siguientes valores predeterminados:
    - Para **Grupo de recursos de destino**, seleccione el grupo de recursos de la lista de grupos de recursos que existen en la ubicación de destino de la suscripción.
    - Para **Red virtual de destino**, seleccione la red en una lista de las redes virtuales en la ubicación de destino.
    - Para **Conjunto de disponibilidad**, puede agregar la configuración del conjunto de disponibilidad a la máquina virtual en caso de que forme parte de un conjunto de disponibilidad de la región de origen.
    - Para **Cuentas de almacenamiento de destino**, seleccione la cuenta que quiere usar.

2. Seleccione **Personalizar**, junto a "Configuración de cifrado", para modificar los siguientes valores predeterminados:
   - Para **Almacén de claves de cifrado de disco de destino**, seleccione el almacén de claves de cifrado de disco de destino en la lista de almacenes de claves de la ubicación de destino de la suscripción.
   - Para **Almacén de claves de cifrado de clave de destino**, seleccione el almacén de claves de cifrado de clave de destino en la lista de almacenes de claves de la ubicación de destino de la suscripción.

3. Seleccione **Crear recurso de destino** > **Habilitar replicación**.
4. Después de que las máquinas virtuales estén habilitadas para la replicación, puede comprobar el estado de mantenimiento de las máquinas virtuales en **Elementos replicados**.

>[!NOTE]
>Durante la replicación inicial, el estado de la máquina virtual puede tardar un tiempo en actualizarse, sin ningún progreso aparente. Haga clic en **Actualizar** para obtener el estado más reciente.

## <a name="update-target-vm-encryption-settings"></a>Actualización de la configuración de cifrado de la máquina virtual de destino
En los escenarios siguientes, se le pedirá que actualice la configuración de cifrado de la máquina virtual de destino:
  - Se ha habilitado la replicación de Site Recovery en la máquina virtual. Después, se ha habilitado el cifrado de disco en la máquina virtual de origen.
  - Se ha habilitado la replicación de Site Recovery en la máquina virtual. Luego, se ha cambiado la clave de cifrado de disco o la clave de cifrado de clave en la máquina virtual de origen.

Se puede usar [un script](#copy-disk-encryption-keys-to-the-dr-region-by-using-the-powershell-script) para copiar las claves de cifrado en la región de destino y, luego, actualizar la configuración de cifrado de destino en **Almacén de Recovery Services** > *elemento replicado* > **Propiedades** > **Proceso y red**.

![Actualización de la ventana del cuadro de diálogo de configuración de ADE](./media/azure-to-azure-how-to-enable-replication-ade-vms/update-ade-settings.png)

## <a id="trusted-root-certificates-error-code-151066"></a>Solución de problemas de permisos del almacén de claves durante la replicación de máquinas virtuales de Azure a Azure

**Causa 1:** puede que se haya seleccionado en la región de destino un almacén de claves ya creado que no tiene los permisos necesarios en lugar de permitir que Site Recovery cree uno. Asegúrese de que el almacén de claves tiene los permisos necesarios, tal como se ha descrito anteriormente.

*Por ejemplo*: intenta replicar una máquina virtual, que tiene un almacén de claves *ContososourceKeyvault* en una región de origen.
Dispone de todos los permisos en el almacén de claves de la región de origen. Pero, durante la protección, selecciona el almacén de claves ContosotargetKeyvault ya creado, que no tiene permisos. Se produce un error.

**Solución:** vaya a **Inicio** > **Keyvaults** > **ContososourceKeyvault** > **Directivas de acceso** y agregue los permisos adecuados.

**Causa 2:** puede que se haya seleccionado en la región de destino un almacén de claves ya creado que no tiene los permisos de cifrado y descifrado en lugar de permitir que Site Recovery cree uno. Asegúrese de que tiene los permisos de cifrado y descifrado en caso de que también vaya a cifrar la clave en la región de origen.</br>

*Por ejemplo*: intenta replicar una máquina virtual, que tiene un almacén de claves *ContososourceKeyvault* en la región de origen. Dispone de todos los permisos necesarios en el almacén de claves de la región de origen. Pero, durante la protección, selecciona el almacén de claves ContosotargetKeyvault ya creado, que no tiene permisos para cifrar y descifrar. Se produce un error.</br>

**Solución:** vaya a **Inicio** > **Keyvaults** > **ContososourceKeyvault** > **Directivas de acceso**. Agregue permisos en **Permisos de clave** > **Operaciones criptográficas**.

## <a name="next-steps"></a>Pasos siguientes

[Más información](site-recovery-test-failover-to-azure.md) sobre la ejecución de una conmutación por error de prueba.
