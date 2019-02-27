---
title: Configuración de la replicación en máquinas virtuales habilitadas para Azure Disk Encryption (ADE) en Azure Site Recovery | Microsoft Docs
description: En este artículo se describe cómo configurar la replicación de máquinas virtuales de ADE de una región de Azure a otra mediante Site Recovery.
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 11/27/2018
ms.author: sutalasi
ms.openlocfilehash: f9abc6d79bd821ef612e9e7648b1b5af98bb5cf6
ms.sourcegitcommit: 75fef8147209a1dcdc7573c4a6a90f0151a12e17
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/20/2019
ms.locfileid: "56456238"
---
# <a name="replicate-azure-disk-encryption-ade-enabled-virtual-machines-to-another-azure-region"></a>Replicación de máquinas virtuales habilitadas para Azure Disk Encryption (ADE) en otra región de Azure

En este artículo se describe cómo habilitar la replicación de máquinas virtuales habilitadas para Azure Disk Encryption (ADE) de una región de Azure a otra.

>[!NOTE]
>Actualmente, Azure Site Recovery solo admite máquinas virtuales Azure con el sistema operativo Windows y que estén [habilitadas para el cifrado con la aplicación de Azure AD](https://aka.ms/ade-aad-app).
>

## <a name="required-user-permissions"></a>Permisos de usuario necesarios
Azure Site Recovery requiere que el usuario tenga permiso para crear el almacén de claves en la región de destino y que copie las claves a dicha región.

Para habilitar la replicación de máquinas virtuales de ADE desde el portal, el usuario debe tener los permisos siguientes.
- Permisos del almacén de claves
    - list
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

Para administrar los permisos, puede ir al recurso de almacén de claves en el portal y agregar los permisos necesarios para el usuario. Por ejemplo: la siguiente guía muestra cómo se habilita para el almacén de claves "ContosoWeb2Keyvault", que se encuentra en la región de origen.


-  Vaya a "Inicio > Almacenes de claves > ContosoWeb2KeyVault > Directivas de acceso"

![permisos de almacén de claves](./media/azure-to-azure-how-to-enable-replication-ade-vms/key-vault-permission-1.png)



- Puede ver que no hay permisos del usuario, así que agregue el permiso indicado anteriormente, para lo que debe hacer clic en "Agregar nuevo" y en el usuario y los permisos

![permisos de almacén de claves](./media/azure-to-azure-how-to-enable-replication-ade-vms/key-vault-permission-2.png)

Si el usuario que habilita la recuperación ante desastres (DR) no tiene los permisos necesarios para copiar las claves, se puede dar el siguiente script al administrador de seguridad con los permisos adecuados para copiar las claves y los secretos de cifrado en la región de destino.

Consulte [en este artículo](#trusted-root-certificates-error-code-151066) para solucionar los problemas de permisos.
>[!NOTE]
>Para habilitar la replicación de máquinas virtuales de ADE desde el portal, necesita al menos permisos de enumeración en los almacenes de claves, los secretos y las claves
>

## <a name="copy-ade-keys-to-dr-region-using-powershell-script"></a>Copia de las claves de ADE en la región de recuperación ante desastres mediante un script de PowerShell

1. Abra el código de script sin formato de "CopyKeys" en una ventana del explorador mediante un clic en [este vínculo](https://aka.ms/ade-asr-copy-keys-code).
2. Copie el script en un archivo y asígnele el nombre "Copy-keys.ps1".
2. Abra la aplicación Windows PowerShell y vaya a la ubicación de carpeta donde reside el archivo.
3. Inicio de "Copy-keys.ps1"
4. Proporcione las credenciales de inicio de sesión de Azure.
5. Seleccione la **suscripción de Azure** de las máquinas virtuales.
6. Espere a que se caguen los grupos de recursos y, luego, seleccione el **grupo de recursos** de sus máquinas virtuales.
7. Seleccione las máquinas virtuales en la lista de máquinas virtuales que se muestra. Solo se muestran en la lista las máquinas virtuales habilitadas para Azure Disk Encryption.
8. Seleccione la **ubicación de destino**.
9. **Almacenes de claves de cifrado de disco**: de forma predeterminada, Azure Site Recovery crea un almacén de claves en la región de destino cuyo nombre tiene el sufijo "asr" en función de las claves de cifrado de disco de la máquina virtual de origen. Si el almacén de claves que ha creado Azure Site Recovery ya existe, se vuelve a usar. Si es necesario, puede seleccionar un almacén de claves diferente de la lista.
10. **Almacenes de claves de cifrado de claves**: de forma predeterminada, Azure Site Recovery crea un almacén de claves en la región de destino cuyo nombre tiene el sufijo "asr" en función de las claves de cifrado de claves de la máquina virtual de origen. Si el almacén de claves que ha creado Azure Site Recovery ya existe, se vuelve a usar. Si es necesario, puede seleccionar un almacén de claves diferente de la lista.

## <a name="enable-replication"></a>Habilitar replicación

En este procedimiento se supone que la región principal de Azure es Asia Pacífico, y la secundaria es Asia Suroriental.

1. En el almacén, haga clic en **+ Replicar**.
2. Especifique los siguientes campos:
    - **Origen**: el punto de origen de las máquinas virtuales, que en este caso es **Azure**.
    - **Ubicación de origen**: la región de Azure desde la que se quieren proteger las máquinas virtuales. En este ejemplo, la ubicación de origen es "Asia Oriental".
    - **Modelo de implementación**: el modelo de implementación de Azure de las máquinas de origen.
    - **Suscripción de origen**: suscripción a la que pertenecen sus máquinas virtuales de origen. Puede tratarse de cualquier suscripción dentro del mismo inquilino de Azure Active Directory donde exista el almacén de Recovery Services.
    - **Grupos de recursos**: el grupo de recursos al que pertenecen las máquinas virtuales de origen. Todas las máquinas virtuales del grupo de recursos seleccionado se enumeran para su protección en el paso siguiente.

3. En **Máquinas virtuales > Seleccionar máquinas virtuales**, haga clic en cada máquina virtual que quiera replicar y selecciónela. Solo puede seleccionar aquellas máquinas en las que se pueda habilitar la replicación. A continuación, haga clic en **Aceptar**.

4. En **Configuración**, tiene la opción de configurar el sitio de destino:

    - **Ubicación de destino**: ubicación donde se replicarán los datos de la máquina virtual de origen. Según la ubicación de las máquinas seleccionadas, Site Recovery proporcionará la lista de regiones de destino adecuadas. Se recomienda mantener como la ubicación de destino la misma ubicación del almacén de Recovery Services.
    - **Suscripción de destino**: la suscripción de destino utilizada para la recuperación ante desastres. De forma predeterminada, la suscripción de destino será la misma que la suscripción de origen.
    - **Grupo de recursos de destino**: el grupo de recursos al que pertenecen todas las máquinas virtuales replicadas. De forma predeterminada, Azure Site Recovery crea un nuevo grupo de recursos en la región de destino con un nombre con el sufijo "asr". En caso de que el grupo de recursos que cree Azure Site Recovery ya exista, se vuelve a usar. También puede elegir personalizarlo tal como se muestra en la sección siguiente. La ubicación del grupo de recursos de destino puede ser cualquier región de Azure excepto la región en la que se hospedan las máquinas virtuales de origen.
    - **Red virtual de destino**: de manera predeterminada, Site Recovery crea una red virtual en la región de destino con un nombre que tiene el sufijo "asr". Esta se asigna a la red de origen y se usa para todas las protecciones futuras. [más información](site-recovery-network-mapping-azure-to-azure.md) sobre la asignación de red.
    - **Cuentas de almacenamiento de destino (si la máquina virtual de origen no usa discos administrados)**: de forma predeterminada, Site Recovery crea una cuenta de almacenamiento de destino mediante la imitación de la configuración de almacenamiento de la máquina virtual de origen. En caso de que ya exista una cuenta de almacenamiento, esta se vuelve a usar.
    - **Discos administrados de réplica (si la máquina virtual de origen utiliza discos administrados)**: Site Recovery crea discos administrados de réplica en la región de destino para reflejar los discos administrados de la máquina virtual de origen con el mismo tipo de almacenamiento (Standard o Premium) que el disco administrado de la máquina virtual de origen.
    - **Cuentas de almacenamiento en caché**: Site Recovery necesita una cuenta de almacenamiento adicional denominada almacenamiento en caché en la región de origen. Todos los cambios que se producen en las máquinas virtuales de origen se siguen y se envían a la cuenta de almacenamiento en caché antes de su replicación en la ubicación de destino.
    - **Conjunto de disponibilidad**: de forma predeterminada, Azure Site Recovery crea un conjunto de disponibilidad en la región de destino con un nombre con el sufijo "asr". En caso de que el conjunto de disponibilidad que cree Azure Site Recovery ya exista, se vuelve a usar.
    - **Almacenes de claves de cifrado de disco**: de forma predeterminada, Azure Site Recovery crea un almacén de claves en la región de destino cuyo nombre tiene el sufijo "asr" en función de las claves de cifrado de disco de la máquina virtual de origen. Si el almacén de claves que ha creado Azure Site Recovery ya existe, se vuelve a usar.
    - **Almacenes de claves de cifrado de claves**: de forma predeterminada, Azure Site Recovery crea un almacén de claves en la región de destino cuyo nombre tiene el sufijo "asr" en función de las claves de cifrado de claves de la máquina virtual de origen. Si el almacén de claves que ha creado Azure Site Recovery ya existe, se vuelve a usar.
    - **Directiva de replicación**: define la configuración del historial de retención del punto de recuperación y la frecuencia de instantánea coherente con la aplicación. De manera predeterminada, Azure Site Recovery crea una directiva de replicación nueva con la configuración predeterminada de 24 horas para la retención del punto de recuperación y 60 minutos para la frecuencia de instantánea coherente con la aplicación.



## <a name="customize-target-resources"></a>Personalizar los recursos de destino

Puede modificar la configuración de destino predeterminada utilizada por Site Recovery.


1. Haga clic en la opción **Personalizar:** que se encuentra al lado de "Suscripción de destino" para modificar la suscripción de destino predeterminada. Seleccione la suscripción en la lista de todas las suscripciones disponibles en el mismo inquilino de Azure Active Directory (AAD).

2. Haga clic en **Personalizar:** junto a Grupo de recursos, Red, Almacenamiento y Conjuntos de disponibilidad para modificar los siguientes valores predeterminados:
    - En **Grupo de recursos de destino**, seleccione el grupo de recursos en la lista de todos los grupos de recursos que existen en la ubicación de destino de la suscripción.
    - En **Red virtual de destino**, seleccione la red en una lista de todas las redes virtuales en la ubicación de destino.
    - En **Conjunto de disponibilidad**, puede agregar la configuración del conjunto de disponibilidad a la máquina virtual en caso de que forme parte de un conjunto de disponibilidad de la región de origen.
    - En **Cuentas de almacenamiento de destino**, seleccione la cuenta que desea usar.


2. Haga clic en **Personalizar:** junto a "Configuración de cifrado" para modificar los siguientes valores predeterminados:
    - En **Almacén de claves de cifrado de discos de destino**, seleccione el almacén de claves de cifrado de disco de destino en la lista de todos los almacenes de claves de la ubicación de destino de la suscripción.
  - En **Almacén de claves de cifrado de claves de destino**, seleccione el almacén de claves de cifrado de claves de destino en la lista de todos los almacenes de claves de la ubicación de destino de la suscripción.

3. Haga clic en **Crear recurso de destino** > **Habilitar replicación**.
4. Después de que las máquinas virtuales están habilitadas para la replicación, puede comprobar el estado de mantenimiento de las máquinas virtuales en **Elementos replicados**.

>[!NOTE]
>Durante la replicación inicial, el estado de la máquina virtual puede tardar un tiempo en actualizarse, sin ningún progreso. Haga clic en el botón **Actualizar** para ver el estado más reciente.
>

## <a name="update-target-vm-encryption-settings"></a>Actualización de la configuración de cifrado de la máquina virtual de destino
En los escenarios siguientes, se le pedirá que actualice la configuración de cifrado de la máquina virtual de destino.
  - Ha habilitado la replicación de Site Recovery en la máquina virtual y Azure Disk Encryption (ADE) en la máquina virtual de origen en una fecha posterior
  - Ha habilitado la replicación de Site Recovery en la máquina virtual y ha cambiado la clave de cifrado de disco o la clave de cifrado de claves en la máquina virtual de origen en una fecha posterior

Puede usar [el script](#copy-ade-keys-to-dr-region-using-powershell-script) para copiar las claves de cifrado en la región de destino y, luego, actualizar la configuración de cifrado de destino en **Almacén de Recovery Services > elemento replicado -> Propiedades -> Proceso y red.**

![update-ade-settings](./media/azure-to-azure-how-to-enable-replication-ade-vms/update-ade-settings.png)

## <a name="trusted-root-certificates-error-code-151066"></a>Solución de problemas de permisos del almacén de claves durante la replicación de máquinas virtuales en Azure a Azure

**Causa 1:** Es posible que haya seleccionado un almacén de claves que ya se ha creado en la región de destino que no tiene los permisos necesarios.
Si va a seleccionar un almacén de claves ya creado en la región de destino, en lugar de dejar que Azure Site Recovery lo cree. Asegúrese de que el almacén de claves tienen los permisos que se han indicado anteriormente.</br>
*Por ejemplo*: Un usuario intenta replicar una máquina virtual, que tiene un almacén de claves en la región de origen, por ejemplo "ContososourceKeyvault".
El usuario tiene todos los permisos todo en el almacén de claves de la región de origen, pero durante la protección selecciona un almacén de claves que ya estaba creado, "ContosotargetKeyvault", y que no tiene permisos. En ese caso la protección provocará un error.</br>
**Solución:** Vaya a "Inicio> Almacenes de claves> ContososourceKeyvault > Directivas de acceso" y agregue los permisos como se ha indicado. 

**Causa 2:** Es posible que haya seleccionado un almacén de claves que ya se ha creado en la región de destino que no tiene los permisos de cifrado y descifrado.
Si va a seleccionar un almacén de claves ya creado en la región de destino, en lugar de dejar que Azure Site Recovery lo cree. Asegúrese de que el usuario tiene los permisos de cifrado y descifrado, por si va a cifrar la clave también en la región de origen.</br>
*Por ejemplo*: Un usuario intenta replicar una máquina virtual, que tiene un almacén de claves en la región de origen, por ejemplo "ContososourceKeyvault".
El usuario tiene todos los permisos todo en el almacén de claves de la región de origen, pero durante la protección selecciona un almacén de claves que ya estaba creado, "ContosotargetKeyvault", y que no tiene permiso para cifrar y descifrar.</br>
**Solución:** Vaya a "Inicio> Almacenes de claves> ContososourceKeyvault > Directivas de acceso" y agregue los permisos en Permisos de clave> Operaciones criptográficas.

## <a name="next-steps"></a>Pasos siguientes

[Más información](site-recovery-test-failover-to-azure.md) sobre la ejecución de una conmutación por error de prueba.
