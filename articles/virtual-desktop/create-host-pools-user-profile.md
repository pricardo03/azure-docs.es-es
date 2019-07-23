---
title: 'Configuración de un recurso compartido de perfiles de usuario para un grupo host en versión preliminar de Windows Virtual Desktop: Azure'
description: Cómo configurar un contenedor de perfiles de FSLogix para un grupo host de versión preliminar de Windows Virtual Desktop.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 04/05/2019
ms.author: helohr
ms.openlocfilehash: f6516e37107a16d80c4d9eb9514782bdbcc44184
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "64925213"
---
# <a name="set-up-a-user-profile-share-for-a-host-pool"></a>Configuración de un recurso compartido de archivos de perfil de usuario para un grupo host

El servicio de versión preliminar de Windows Virtual Desktop ofrece contenedores de perfiles de FSLogix como solución recomendada para los perfiles de usuario. No se recomienda usar la solución Disco de perfil de usuario (UPD), que estará en desuso en futuras versiones de Windows Virtual Desktop.

En esta sección verá cómo configurar un recurso compartido de contenedor de perfiles de FSLogix para un grupo host. Para obtener documentación general sobre FSLogix, consulte el [sitio de FSLogix](https://docs.fslogix.com/).

## <a name="create-a-new-virtual-machine-that-will-act-as-a-file-share"></a>Crear una nueva máquina virtual que actúe como recurso compartido de archivos

Al crear la máquina virtual, asegúrese de colocarla en la misma red virtual que las máquinas virtuales del grupo host o en una red virtual que tenga conectividad a las máquinas virtuales del grupo host. Puede crear una máquina virtual de varias maneras:

- [Crear una máquina virtual desde una imagen de la galería de Azure](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#create-virtual-machine)
- [Crear una máquina virtual desde una imagen administrada](https://docs.microsoft.com/azure/virtual-machines/windows/create-vm-generalized-managed)
- [Crear una máquina virtual desde una imagen no administrada](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image)

Después de crear la máquina virtual, únala al dominio haciendo lo siguiente:

1. [Conéctese a la máquina virtual](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#connect-to-virtual-machine) con las credenciales que proporcionó al crear la máquina virtual.
2. En la máquina virtual, inicie el **Panel de control** y seleccione **Sistema**.
3. Seleccione **Nombre del equipo**, seleccione **Cambiar configuración** y, luego, seleccione **Cambiar…**
4. Seleccione **Dominio** y, luego, escriba el dominio de Active Directory en la red virtual.
5. Autentíquese con una cuenta de dominio que tenga privilegios en máquinas unidas a dominio.

## <a name="prepare-the-virtual-machine-to-act-as-a-file-share-for-user-profiles"></a>Preparar la máquina virtual para que actúe como recurso compartido de archivos para los perfiles de usuario

Las siguientes son instrucciones generales sobre cómo preparar una máquina virtual para que actúe como recurso compartido de archivos para perfiles de usuario:

1. Agregue los usuarios de Active Directory de Windows Virtual Desktop a un [grupo de seguridad de Active Directory](https://docs.microsoft.com/windows/security/identity-protection/access-control/active-directory-security-groups). Este grupo de seguridad se usará para autenticar a los usuarios de Windows Virtual Desktop en la máquina de virtual del recurso compartido de archivos que acaba de crear.
2. [Conéctese a la máquina virtual del recurso compartido de archivos](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#connect-to-virtual-machine).
3. En la máquina virtual del recurso compartido de archivos, cree una carpeta en la **unidad C**, que se usará como recurso compartido de perfiles.
4. Haga clic con el botón derecho en la nueva carpeta, seleccione **Propiedades**, seleccione **Uso compartido**, luego, seleccione **Uso compartido avanzado…**
5. Seleccione **Compartir esta carpeta**, seleccione **Permisos…** , luego, seleccione **Agregar…**
6. Busque el grupo de seguridad al que agregó los usuarios de Windows Virtual Desktop, luego, asegúrese de que ese grupo tenga **Control total**.
7. Después de agregar el grupo de seguridad, haga clic con el botón derecho en la carpeta, seleccione **Propiedades**, seleccione **Uso compartido**, luego, copie la **Ruta de acceso de red** para consultar más adelante.

Para más información acerca de los permisos, consulte la [documentación de FSLogix](https://docs.fslogix.com/display/20170529/Requirements%2B-%2BProfile%2BContainers).

## <a name="configure-the-fslogix-profile-container"></a>Configurar el contenedor de perfiles de FSLogix

Para configurar las máquinas virtuales con el software de FSLogix, siga este procedimiento en cada máquina registrada en el grupo host:

1. [Conéctese a la máquina virtual](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#connect-to-virtual-machine) con las credenciales que proporcionó al crear la máquina virtual.
2. Inicie un explorador de Internet y vaya a [este vínculo](https://go.microsoft.com/fwlink/?linkid=2084562) para descargar el agente de FSLogix. Como parte de la versión preliminar pública de Windows Virtual Desktop, obtendrá una clave de licencia para activar el software de FSLogix. La clave es el archivo LicenseKey.txt incluido en el archivo .zip del agente de FSLogix.
3. Vaya a \\\\Win32\\Release o \\\\X64\\Release en el archivo .zip y ejecute **FSLogixAppsSetup** para instalar el agente de FSLogix.
4. Vaya a **Archivos de programa** > **FSLogix** > **Apps** para confirmar que el agente esté instalado.
5. Desde el menú de Inicio, ejecute **RegEdit** como administrador. Vaya a **Equipo\\HKEY_LOCAL_MACHINE\\software\\FSLogix**.
6. Cree una clave denominada **Profiles**.
7. Cree los siguientes valores para la clave Profiles:

| NOMBRE                | Type               | Datos/valor                        |
|---------------------|--------------------|-----------------------------------|
| habilitado             | DWORD              | 1                                 |
| VHDLocations        | Valor de varias cadenas | "Network path for file share"     |

>[!IMPORTANT]
>Para ayudar a proteger su entorno de Windows Virtual Desktop en Azure, se recomienda no abrir el puerto de entrada 3389 en las máquinas virtuales. Windows Virtual Desktop no requiere un puerto de entrada abierto 3389 para que los usuarios accedan a máquinas virtuales del grupo host. Si debe abrir el puerto 3389 para solucionar problemas, se recomienda usar [acceso de máquina virtual Just-in-Time](https://docs.microsoft.com/azure/security-center/security-center-just-in-time).