---
title: Validación controlada de unión a Azure AD híbrido - Azure AD
description: Obtenga información sobre cómo realizar una validación controlada de combinación de Azure AD híbrido antes de habilitarlo en toda la organización a la vez
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: article
ms.date: 05/30/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: cd5b388f92a875fb2635037a6eae3ff3b6a95793
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66513283"
---
# <a name="controlled-validation-of-hybrid-azure-ad-join"></a>Validación controlada de la unión a Azure AD híbrido

Cuando todos los requisitos previos están en su lugar, se registrarán automáticamente los dispositivos de Windows como dispositivos en su inquilino de Azure AD. El estado de estas identidades de dispositivo en Azure AD se conoce como combinación de Azure AD híbrido. Puede encontrar más información sobre los conceptos tratados en este artículo en los artículos [Introducción a la administración de dispositivos en Azure Active Directory](overview.md) y [planear su implementación híbrida de la unión de Azure Active Directory ](hybrid-azuread-join-plan.md).

Las organizaciones que desee realizar una validación controlada de combinación de Azure AD híbrido antes de habilitarlo en toda su organización a la vez. En este artículo se explica cómo realizar una validación controlada de combinación de Azure AD híbrido.

## <a name="controlled-validation-of-hybrid-azure-ad-join-on-windows-current-devices"></a>Validación controlada de combinación de Azure AD híbrido en dispositivos de Windows actuales

Para dispositivos que ejecutan el sistema operativo de escritorio de Windows, la versión admitida es la Actualización de aniversario de Windows 10 (versión 1607), o una versión posterior. Como procedimiento recomendado, actualice a la versión más reciente de Windows 10.

Para realizar una validación controlada de combinación de Azure AD híbrido en dispositivos de Windows actuales, es preciso:

1. Borrar la entrada de punto de conexión de servicio (SCP) de Active Directory (AD), si existe
1. Configurar la configuración del registro de cliente de SCP en los equipos unidos a un dominio con un objeto de directiva de grupo (GPO)
1. Si utilizas AD FS, también debe configurar la configuración del registro del lado cliente de SCP en el servidor de AD FS mediante un GPO  



### <a name="clear-the-scp-from-ad"></a>Desactive el SCP de AD

Utilice el Editor de Interfaces de servicios de Active Directory (ADSI Edit) para modificar los objetos de SCP de AD.

1. Iniciar el **ADSI Edit** desde la aplicación de escritorio y estación de trabajo administrativa o un controlador de dominio como administrador de organización.
1. Conectarse a la **el contexto de nomenclatura de configuración** del dominio.
1. Vaya a **CN = Configuration, DC = contoso, DC = com** > **CN = Services** > **CN = Device Registration Configuration**
1. Haga clic con el botón derecho en el objeto de hoja bajo **CN = Device Registration Configuration** y seleccione **propiedades**
   1. Seleccione **palabras clave** desde el **Editor de atributos** ventana y haga clic en **editar**
   1. Seleccione los valores de **azureADId** y **azureADName** (uno en uno) y haga clic en **quitar**
1. Cerrar **Editor ADSI**


### <a name="configure-client-side-registry-setting-for-scp"></a>Configuración del registro del lado cliente de SCP

Use el ejemplo siguiente para crear un objeto de directiva de grupo (GPO) para implementar una configuración del registro, configurar una entrada de SCP en el registro de los dispositivos.

1. Abra una consola de administración de directivas de grupo y crear un nuevo objeto de directiva de grupo en el dominio.
   1. Proporcione el GPO recién creado en un nombre (por ejemplo, ClientSideSCP).
1. Edite el GPO y busque la ruta de acceso siguiente: **Configuración del equipo** > **preferencias** > **Windows configuración** > **registro**
1. Haga doble clic en el registro y seleccione **New** > **elemento del registro**
   1. En el **General** pestaña, configure lo siguiente
      1. Acción: **Actualizar**
      1. Hive: **HKEY_LOCAL_MACHINE**
      1. Ruta de acceso de clave: **SOFTWARE\Microsoft\Windows\CurrentVersion\CDJ\AAD**
      1. Nombre del valor: **TenantId**
      1. Tipo de valor: **REG_SZ**
      1. Datos del valor: El GUID o **Id. de directorio** de su instancia de Azure AD (este valor puede encontrarse en el **portal Azure** > **Azure Active Directory**  >   **Propiedades** > **Id. de directorio**)
   1. Haga clic en **Aceptar**
1. Haga doble clic en el registro y seleccione **New** > **elemento del registro**
   1. En el **General** pestaña, configure lo siguiente
      1. Acción: **Actualizar**
      1. Hive: **HKEY_LOCAL_MACHINE**
      1. Ruta de acceso de clave: **SOFTWARE\Microsoft\Windows\CurrentVersion\CDJ\AAD**
      1. Nombre del valor: **TenantName**
      1. Tipo de valor: **REG_SZ**
      1. Datos del valor: Su comprobado **nombre de dominio** en Azure AD (por ejemplo, `contoso.onmicrosoft.com` o cualquier otro nombre de dominio comprobado en el directorio)
   1. Haga clic en **Aceptar**
1. Cierre el editor para el GPO recién creado
1. Vincule el GPO recién creado a la OU deseada que contiene equipos unidos a un dominio que pertenecen a la población de lanzamiento controlado

### <a name="configure-ad-fs-settings"></a>Configuración de AD FS

Si utilizas AD FS, primero deberá configurar el SCP del lado cliente mediante las instrucciones mencionadas anteriormente pero vincular el GPO a los servidores de AD FS. Esta configuración es necesaria para que AD FS para establecer el origen de identidades de dispositivo como Azure AD.

## <a name="controlled-validation-of-hybrid-azure-ad-join-on-windows-down-level-devices"></a>Validación controlada de combinación de Azure AD híbrido en dispositivos de nivel inferior de Windows

Para registrar los dispositivos de nivel inferior de Windows, deben instalar las organizaciones [Microsoft Workplace Join para equipos sin Windows 10](https://www.microsoft.com/download/details.aspx?id=53554) disponible en Microsoft Download Center.

Puede implementar el paquete mediante el uso de un sistema de distribución de software como [System Center Configuration Manager](https://www.microsoft.com/cloud-platform/system-center-configuration-manager). El paquete admite las opciones de instalación silenciosa estándar mediante el parámetro quiet. La rama actual de Configuration Manager ofrece ventajas adicionales sobre las versiones anteriores, como la posibilidad de realizar el seguimiento de los registros completados.

El instalador crea una tarea programada en el sistema que se ejecuta en el contexto de usuario. La tarea se desencadena cuando el usuario inicia sesión en Windows. La tarea une de forma silenciosa el dispositivo con Azure AD con las credenciales de usuario después de realizar la autenticación con Azure AD.

Para controlar el registro de dispositivos, debe implementar el paquete de Windows Installer para el grupo de dispositivos de nivel inferior de Windows seleccionado.

> [!NOTE]
> Si no se configura un SCP en Active Directory, debe seguir el mismo enfoque tal como se describe a [configuración del registro del lado cliente de SCP](#configure-client-side-registry-setting-for-scp)) en los equipos unidos a un dominio con un objeto de directiva de grupo (GPO).


Después de comprobar que todo funciona según lo previsto, puede registrar automáticamente el resto de los dispositivos de Windows actuales y de nivel inferior con Azure AD por [configurar SCP con Azure AD Connect](hybrid-azuread-join-managed-domains.md#configure-hybrid-azure-ad-join).

## <a name="next-steps"></a>Pasos siguientes

[Planeamiento de la implementación de la unión a Azure Active Directory híbrido](hybrid-azuread-join-plan.md)
