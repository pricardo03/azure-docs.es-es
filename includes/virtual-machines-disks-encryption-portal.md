---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 01/13/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 0912316d1c41f46e5dba74b58017f4fd5e8ed529
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2020
ms.locfileid: "76909140"
---
### <a name="portal"></a>Portal

La configuración de claves administradas por el cliente para los discos requerirá la creación de recursos en un orden determinado, si lo hace por primera vez. En primer lugar, tendrá que crear y configurar una instancia de Azure Key Vault.

#### <a name="setting-up-your-azure-key-vault"></a>Configuración de Azure Key Vault

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) y busque Key Vault.
1. Busque y seleccione **Key Vaults**.

    [ ![sse-key-vault-portal-search.png](media/virtual-machines-disk-encryption-portal/sse-key-vault-portal-search.png)](media/virtual-machines-disk-encryption-portal/sse-key-vault-portal-search-expanded.png#lightbox)

    > [!IMPORTANT]
    > La instancia de Azure Key Vault, el conjunto de cifrado de disco, la VM, los discos y las instantáneas deben estar en la misma región y suscripción para que la implementación se realice correctamente.

1. Seleccione **+Agregar** para crear una nueva instancia de Key Vault.
1. Creación de un nuevo grupo de recursos
1. Escriba un nombre de almacén de claves, seleccione una región y seleccione un plan de tarifa.
1. Seleccione **Revisar y crear**, compruebe las opciones y, a continuación, seleccione **Crear**.

    ![sse-create-a-key-vault.png](media/virtual-machines-disk-encryption-portal/sse-create-a-key-vault.png)

1. Una vez que el almacén de claves termine de implementarse, selecciónelo.
1. En **Configuración**, seleccione **Claves**.
1. Seleccione **Generar o importar**.

    ![sse-key-vault-generate-settings.png](media/virtual-machines-disk-encryption-portal/sse-key-vault-generate-settings.png)

1. Deje **Tipo de clave** establecido en **RSA** y **Tamaño de la clave RSA** establecido en **2080**.
1. Rellene las selecciones restantes como desee y, a continuación, seleccione **Crear**.

    ![sse-create-a-key-generate.png](media/virtual-machines-disk-encryption-portal/sse-create-a-key-generate.png)

#### <a name="setting-up-your-disk-encryption-set"></a>Configuración del conjunto de cifrado de disco

Para crear y configurar conjuntos de cifrado de disco, debe usar el siguiente vínculo: https://aka.ms/diskencryptionsets. La creación del conjunto de cifrado de disco todavía no está disponible en Azure Portal global.

1. Abra el [vínculo de conjuntos de cifrado de disco](https://aka.ms/diskencryptionsets).
1. Seleccione **+Agregar**.

    ![sse-create-disk-encryption-set.png](media/virtual-machines-disk-encryption-portal/sse-create-disk-encryption-set.png)

1. Seleccione el grupo de recursos, asigne un nombre al conjunto de cifrado y seleccione la misma región que el almacén de claves.
1. Seleccione **Almacén de claves y clave**.
1. Seleccione el almacén de claves y la clave que creó anteriormente, así como la versión.
1. Haga clic en **Seleccionar**.
1. Seleccione **Revisar y crear** y, a continuación, **Crear**.

    ![sse-disk-enc-set-blade-key.png](media/virtual-machines-disk-encryption-portal/sse-disk-enc-set-blade-key.png)

1. Abra el conjunto de cifrado de disco una vez que termine de crear y seleccione la alerta que aparece.

    ![sse-disk-enc-alert-fix.png](media/virtual-machines-disk-encryption-portal/sse-disk-enc-alert-fix.png)

Dos notificaciones deberían aparecer y completarse correctamente. Esto le permitirá usar el conjunto de cifrado de disco con el almacén de claves.

![disk-enc-notification-success.png](media/virtual-machines-disk-encryption-portal/disk-enc-notification-success.png)

#### <a name="deploy-a-vm"></a>Implementación de una máquina virtual

Ahora que ha creado y configurado el almacén de claves y el conjunto de cifrado de disco, puede implementar una VM mediante el cifrado.
El proceso de implementación de VM es similar al proceso de implementación estándar, las únicas diferencias son que debe implementar la VM en la misma región que los demás recursos y optar por usar una clave administrada por el cliente.

1. Abra el [vínculo de conjuntos de cifrado de disco](https://aka.ms/diskencryptionsets).
1. Busque **Máquinas virtuales** y seleccione **+Agregar** para crear una VM.
1. En la pestaña **Básico**, seleccione la misma región que el conjunto de cifrado de disco y Azure Key Vault.
1. Rellene los demás valores en la pestaña **Básico** como desee.

    ![sse-create-a-vm-region.png](media/virtual-machines-disk-encryption-portal/sse-create-a-vm-region.png)

1. En la pestaña **Discos**, seleccione **Cifrado en reposo con una clave administrada por el cliente**.
1. Seleccione el conjunto de cifrado de disco en el menú desplegable **Conjunto de cifrado de disco**.
1. Realice las selecciones restantes como desee.

    ![sse-create-vm-select-cmk-encryption-set.png](media/virtual-machines-disk-encryption-portal/sse-create-vm-select-cmk-encryption-set.png)

#### <a name="enable-on-an-existing-disk"></a>Habilitación en un disco existente

Para administrar y configurar el cifrado de disco en los discos existentes, debe usar el siguiente vínculo: https://aka.ms/diskencryptionsets. La habilitación de claves administradas por el cliente en discos existentes todavía no está disponible en Azure Portal global.

> [!CAUTION]
> Para habilitar el cifrado de disco en todos los discos conectados a una VM, será necesario detener la VM.

1. Abra el [vínculo de conjuntos de cifrado de disco](https://aka.ms/diskencryptionsets).
1. Vaya a una VM que esté en la misma región que uno de los conjuntos de cifrado de disco.
1. Abra la VM y seleccione **Detener**.

    ![sse-stop-VM-to-encrypt-disk.png](media/virtual-machines-disk-encryption-portal/sse-stop-VM-to-encrypt-disk.png)

1. Una vez finalizada la detención de la VM, seleccione **Discos** y, después, seleccione el disco que desea cifrar.

    ![sse-existing-disk-select.png](media/virtual-machines-disk-encryption-portal/sse-existing-disk-select.png)

1. Seleccione **Cifrado** y seleccione **Cifrado en reposo con una clave administrada por el cliente** y, a continuación, seleccione el conjunto de cifrado de disco en la lista desplegable.
1. Seleccione **Guardar**.

    ![sse-encrypt-existing-disk-customer-managed-key.png](media/virtual-machines-disk-encryption-portal/sse-encrypt-existing-disk-customer-managed-key.png)

1. Repita este proceso para todos los demás discos conectados a la VM que desea cifrar.
1. Cuando los discos terminen de cambiar a las claves administradas por el cliente, si no hay ningún otro disco conectado que quiera cifrar, puede iniciar la VM.
