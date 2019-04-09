---
title: Configure un recurso compartido de perfil de usuario para un grupo de host de vista previa de Escritorio Virtual de Windows - Azure
description: Cómo configurar un contenedor de perfil FSLogix para un grupo de host de vista previa de Escritorio Virtual de Windows.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 04/04/2019
ms.author: helohr
ms.openlocfilehash: a7e2f3c95819c6ab6d2e63e5c7a2f62649ebd15c
ms.sourcegitcommit: b4ad15a9ffcfd07351836ffedf9692a3b5d0ac86
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/05/2019
ms.locfileid: "59056102"
---
# <a name="set-up-a-user-profile-share-for-a-host-pool"></a>Configuración de un recurso compartido de archivos de perfil de usuario para un grupo host

El servicio de Windows Vista previa del escritorio Virtual ofrece contenedores de perfil FSLogix como la solución de perfil de usuario recomendada. No se recomienda con la solución de disco de perfil de usuario (UPD), que estará en desuso en futuras versiones de Escritorio Virtual de Windows.

En esta sección le indicará cómo configurar un recurso compartido de contenedor de FSLogix perfil para un grupo host. Para obtener documentación general sobre FSLogix, consulte el [FSLogix sitio](https://docs.fslogix.com/).

## <a name="create-a-new-virtual-machine-that-will-act-as-a-file-share"></a>Crear una nueva máquina virtual que actúe como un recurso compartido de archivos

Al crear la máquina virtual, asegúrese de colocarlo en cualquier la misma red virtual que las máquinas virtuales de grupo de host o en una red virtual que tenga conectividad a las máquinas virtuales de grupo de host. Puede crear una máquina virtual de varias maneras:

- [Crear una máquina virtual desde una imagen de galería de Azure](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#create-virtual-machine)
- [Crear una máquina virtual desde una imagen administrada](https://docs.microsoft.com/azure/virtual-machines/windows/create-vm-generalized-managed)
- [Crear una máquina virtual desde una imagen no administrada](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image)

Después de crear la máquina virtual, unirse al dominio haciendo lo siguiente:

1. [Conectarse a la máquina virtual](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#connect-to-virtual-machine) con las credenciales que proporcionó al crear la máquina virtual.
2. En la máquina virtual, inicie **Panel de Control** y seleccione **sistema**.
3. Seleccione **nombre_equipo**, seleccione **cambiar la configuración de**y, a continuación, seleccione **cambios...**
4. Seleccione **dominio** y, a continuación, escriba el dominio de Active Directory en la red virtual.
5. Autenticar con una cuenta de dominio que tenga privilegios para unir máquinas mediante dominio.

## <a name="prepare-the-virtual-machine-to-act-as-a-file-share-for-user-profiles"></a>Prepare la máquina virtual para que actúe como un recurso compartido de archivos para perfiles de usuario

Los siguientes son instrucciones generales sobre cómo preparar una máquina virtual para que actúe como un recurso compartido de archivos para perfiles de usuario:

1. Agregue los usuarios de Escritorio Virtual Active Directory de Windows a un [grupo de seguridad de Active Directory](https://docs.microsoft.com/windows/security/identity-protection/access-control/active-directory-security-groups). Este grupo de seguridad se utilizará para autenticar a los usuarios de Escritorio Virtual de Windows a la máquina de virtual del recurso compartido de archivo que acaba de crear.
2. [Conectarse a la máquina virtual de recurso compartido de archivo](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#connect-to-virtual-machine).
3. En la máquina virtual del recurso compartido de archivo, cree una carpeta en el **unidad C** que se usará como el recurso compartido de perfil.
4. Haga clic en la nueva carpeta, seleccione **propiedades**, seleccione **compartir**, a continuación, seleccione **de uso compartido avanzado...** .
5. Seleccione **compartir esta carpeta**, seleccione **permisos...** , a continuación, seleccione **agregar...** .
6. Busque el grupo de seguridad al que agregó a los usuarios de Escritorio Virtual de Windows, a continuación, asegúrese de que ese grupo tiene **Control total**.
7. Después de agregar el grupo de seguridad, haga clic en la carpeta, seleccione **propiedades**, seleccione **compartir**, a continuación, copie el **ruta de acceso de red** para consultar más adelante.

Para obtener más información acerca de los permisos, consulte el [FSLogix documentación](https://docs.fslogix.com/display/20170529/Requirements%2B-%2BProfile%2BContainers).

## <a name="configure-the-fslogix-profile-container"></a>Configurar el contenedor de perfil FSLogix

Para configurar las máquinas virtuales con el software FSLogix, siga este procedimiento en cada máquina registrada en el grupo host:

1. [Conectarse a la máquina virtual](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#connect-to-virtual-machine) con las credenciales que proporcionó al crear la máquina virtual.
2. Inicie un explorador de internet y vaya a [este vínculo](https://go.microsoft.com/fwlink/?linkid=2084562) para descargar el agente FSLogix. Como parte de la versión preliminar pública de Escritorio Virtual de Windows, obtendrá una clave de licencia para activar el software FSLogix. La clave es el archivo de LicenseKey.txt incluido en el archivo .zip de agente de FSLogix.
3. Vaya a cualquiera \\ \\Win32\\versión o \\ \\X64\\versión en el archivo .zip y ejecute **FSLogixAppsSetup** para instalar el agente FSLogix.
4. Vaya a **archivos de programa** > **FSLogix** > **aplicaciones** para confirmar el agente instalado.
5. En el menú Inicio, ejecutar **RegEdit** como administrador. Vaya a **equipo\\HKEY_LOCAL_MACHINE\\software\\FSLogix**.
6. Cree una clave denominada **perfiles**.
7. Cree los siguientes valores para la clave de perfiles:

| NOMBRE                | Type               | Datos y valor                        |
|---------------------|--------------------|-----------------------------------|
| habilitado             | DWORD              | 1                                 |
| VHDLocations        | Valor de cadena múltiple | "Ruta de acceso de red para el recurso compartido de archivos"     |
