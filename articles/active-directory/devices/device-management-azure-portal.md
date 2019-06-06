---
title: Administración de dispositivos con Azure Portal | Microsoft Docs
description: Aprenda a usar Azure Portal para administrar dispositivos.
services: active-directory
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
editor: ''
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.subservice: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/04/2019
ms.author: joflore
ms.reviewer: jairoc
ms.collection: M365-identity-device-management
ms.openlocfilehash: 48f162e5e6eb29e4a658000826ccf25389086342
ms.sourcegitcommit: 1aefdf876c95bf6c07b12eb8c5fab98e92948000
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66730435"
---
# <a name="manage-device-identity-using-the-azure-portal"></a>Administrar la identidad de dispositivo mediante el portal de Azure

Con la administración de identidades de dispositivo en Azure Active Directory (Azure AD), puede asegurarse de que los usuarios acceso a los recursos desde dispositivos que cumplen los estándares de seguridad y cumplimiento.

Este artículo:

- Se da por supuesto que está familiarizado con la [Introducción a la administración de identidades de dispositivo en Azure Active Directory](overview.md)
- Proporciona información acerca de cómo administrar las identidades de dispositivo mediante el portal de Azure AD

## <a name="manage-device-identities"></a>Administración de identidades de dispositivo

El portal de Azure AD le proporciona un lugar central para administrar las identidades de dispositivo. Para obtener esta ubicación, use un [vínculo directo](https://portal.azure.com/#blade/Microsoft_AAD_IAM/DevicesMenuBlade/Devices) o siga los pasos que se detallan en el manual:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador.
2. En la barra de navegación izquierda, haga clic en **Active Directory**.

   ![Configuración de dispositivo](./media/device-management-azure-portal/01.png)

3. En la sección **Administrar**, haga clic en **Dispositivos**.

   ![Configuración de dispositivo](./media/device-management-azure-portal/74.png)

La página **Dispositivos** le permitirá:

- Configuración de las opciones del dispositivo
- Búsqueda de dispositivos
- Realizar tareas de administración de identidades de dispositivo
- Revise los registros de auditoría relacionados con el dispositivo  
  
## <a name="configure-device-settings"></a>Configuración de dispositivo

Para administrar las identidades de dispositivo mediante el portal de Azure AD, estos deben ser [registrados o Unidos](overview.md) a Azure AD. Como administrador, puede ajustar el proceso de registro y unión de dispositivos al configurar los valores del dispositivo.

![Configuración de dispositivo](./media/device-management-azure-portal/22.png)

La página de configuración del dispositivo le permite configurar:

![Administración de un dispositivo de Intune](./media/device-management-azure-portal/21.png)

- **Los usuarios pueden inscribir dispositivos en Azure AD** -esta opción le permite seleccionar los usuarios que pueden registrar sus dispositivos como [dispositivos de Azure AD join](overview.md#azure-ad-joined-devices). El valor predeterminado es **Todos**.

>[!NOTE]
> **Los usuarios pueden inscribir dispositivos en Azure AD** configuración sólo es aplicable a Azure AD join en Windows 10.

- **Administradores locales adicionales en dispositivos unidos a Azure AD**: puede seleccionar a qué usuarios se conceden derechos de administrador local en un dispositivo. Los usuarios agregados aquí se agregan al rol *Administradores de dispositivos* de Azure AD. De forma predeterminada, a los administradores globales de Azure AD y a los propietarios de dispositivos se les conceden derechos de administrador local. Esta opción es una capacidad de la edición Premium disponible en productos como Azure AD Premium o Enterprise Mobility Suite (EMS).
- **Los usuarios pueden registrar sus dispositivos con Azure AD** -deberá configurar esta opción para permitir que los dispositivos de Windows 10 personal, iOS, Android y macOs se [registrado](overview.md#azure-ad-registered-devices) con Azure AD. Si selecciona **ninguno**, los dispositivos no pueden registrar con Azure AD. La inscripción en Microsoft Intune o Administración de dispositivos móviles (MDM) para Office 365 exige registrarse. Si ha configurado alguno de estos servicios, se selecciona **TODOS** y **NINGUNO** no está disponible.
- **Requerir Multi-Factor Authentication para unir dispositivos**: puede decidir si se exige a los usuarios proporcionar una segunda fase de autenticación para [unir](overview.md#azure-ad-joined-devices) su dispositivo a Azure AD. El valor predeterminado es **No**. Se recomienda exigir Multi-Factor Authentication al registrar un dispositivo. Antes de habilitar Multi-Factor Authentication para este servicio, debe asegurarse de que está configurado para los usuarios que registran sus dispositivos. Para más información sobre los distintos servicios de Azure Multi-Factor Authentication, vea [Introducción a Azure Multi-Factor Authentication](../authentication/concept-mfa-whichversion.md). 

>[!NOTE]
> **Requerir Multi-factor Auth para conectar dispositivos** configuración no es aplicable a los dispositivos Unidos a Azure AD híbrido.

- **Número máximo de dispositivos**: esta opción permite seleccionar el número máximo de dispositivos que puede tener un usuario en Azure AD. Si un usuario alcanza esta cuota, no puede agregar dispositivos adicionales hasta que se quitan uno o varios de los dispositivos existentes. La cuota de dispositivo se cuenta para todos los dispositivos que están registrados actualmente Unidos a Azure AD o Azure AD. El valor predeterminado es **20**.

>[!NOTE]
> **Número máximo de dispositivos** configuración no es aplicable a los dispositivos Unidos a Azure AD híbrido.

- **Los usuarios pueden sincronizar la configuración y los datos de aplicaciones en distintos dispositivos**: de forma predeterminada, esta opción se establece en **NINGUNO**. La selección de usuarios o grupos concretos o de TODOS permite sincronizar la configuración y los datos de aplicación del usuario en sus dispositivos Windows 10. Más información sobre cómo funciona la sincronización en Windows 10.
Esta opción es una capacidad Premium disponible en productos como Azure AD Premium o Enterprise Mobility Suite (EMS).

## <a name="locate-devices"></a>Búsqueda de dispositivos

Tiene dos opciones para buscar dispositivos registrados y unidos:

- **Todos los dispositivos** en la sección **Administrar** de la página **Dispositivos**.  

   ![Todos los dispositivos](./media/device-management-azure-portal/41.png)

- **Dispositivos** en la sección **Administrar** de la página **Usuarios**.

   ![Todos los dispositivos](./media/device-management-azure-portal/43.png)

Con ambas opciones, puede ir a una vista que:

- Permite buscar dispositivos si se usa el nombre para mostrar como filtro.
- Proporciona información general detallada de los dispositivos registrados y unidos.
- Permite realizar tareas comunes de administración de dispositivos.

![Todos los dispositivos](./media/device-management-azure-portal/51.png)

Para algunos dispositivos iOS, los nombres de dispositivos que contienen apóstrofos podrían usar caracteres diferentes similares a los apóstrofos. Por lo que buscar esos dispositivos es un poco complicado: si no ve los resultados de búsqueda correctamente, asegúrese de que la cadena de búsqueda contiene el carácter de apóstrofo coincidente.

## <a name="device-identity-management-tasks"></a>Tareas de administración de identidades de dispositivo

Como administrador global o administrador de dispositivos en la nube, puede administrar los dispositivos registrados o Unidos. Los administradores de servicios de Intune pueden:

- Actualizar dispositivos:  las operaciones diarias, como habilitar o deshabilitar dispositivos, son un ejemplo de ello.
- Eliminar dispositivos: cuando un dispositivo se retira y debe eliminarse en Azure AD.

En esta sección se proporciona información sobre tareas comunes de administración de identidades de dispositivo.

### <a name="manage-an-intune-device"></a>Administración de un dispositivo de Intune

Si es administrador de Intune, puede administrar los dispositivos marcados como **Microsoft Intune**.

![Administración de un dispositivo de Intune](./media/device-management-azure-portal/31.png)

### <a name="enable--disable-an-azure-ad-device"></a>Habilitar o deshabilitar un dispositivo de Azure AD

Para habilitar o deshabilitar un dispositivo, tiene dos opciones:

- En el menú de tareas ("...") en la página **Todos los dispositivos**.

   ![Administración de un dispositivo de Intune](./media/device-management-azure-portal/71.png)

- La barra de herramientas en la página **Dispositivos**.

   ![Administración de un dispositivo de Intune](./media/device-management-azure-portal/32.png)

**Comentarios:**

- Debe ser un administrador global o administrador de dispositivos de nube en Azure AD para habilitar o deshabilitar un dispositivo. 
- Al deshabilitar un dispositivo, impide que un dispositivo de autenticar correctamente con Azure AD, impidiendo así el dispositivo de acceso a los recursos de Azure AD que están protegidos por entidad emisora de certificados de dispositivo o con sus credenciales de WH4B.

### <a name="delete-an-azure-ad-device"></a>Eliminar un dispositivo de Azure AD

Para eliminar un dispositivo, tiene dos opciones:

- En el menú de tareas ("...") en la página **Todos los dispositivos**.

   ![Administración de un dispositivo de Intune](./media/device-management-azure-portal/72.png)

- La barra de herramientas en la página **Dispositivos**.

   ![Eliminar un dispositivo](./media/device-management-azure-portal/34.png)

**Comentarios:**

- Para eliminar un dispositivo, debe ser administrador global o administrador de Intune en Azure AD.
- La eliminación de un dispositivo:
   - Evita que un dispositivo acceda a los recursos de Azure AD.
   - Quita todos los detalles asociados al dispositivo como, por ejemplo, las claves de BitLocker de los dispositivos Windows.  
   - Representa una actividad no recuperable y no se recomienda a menos que sea necesaria.

Si un dispositivo está administrado por otra entidad de administración (por ejemplo, Microsoft Intune), asegúrese de que el dispositivo se ha borrado o retirado antes de eliminar el dispositivo en Azure AD.

### <a name="view-or-copy-device-id"></a>Ver o copiar el id. del dispositivo

Puede usar un id. de dispositivo para verificar los detalles del id.de dispositivo en ese dispositivo o usar PowerShell durante la solución de problemas. Para acceder a la opción de copia, haga clic en el dispositivo.

![Visualización de identificador de dispositivo](./media/device-management-azure-portal/35.png)
  
### <a name="view-or-copy-bitlocker-keys"></a>Ver o copiar las claves de BitLocker

Puede ver y copiar las claves de BitLocker para ayudar a otros usuarios a recuperar su unidad cifrada. Estas claves solo están disponibles para dispositivos Windows cifrados y con las claves almacenadas en Azure AD. Puede copiar estas claves al acceder a los detalles del dispositivo.

![Visualización de claves de BitLocker](./media/device-management-azure-portal/36.png)

Para ver o copiar las claves de BitLocker, debe ser el propietario del dispositivo, o bien un usuario que tenga al menos uno de los siguientes roles asignados:

- Administrador global
- Administrador del departamento de soporte técnico
- Administrador de seguridad
- Lector de seguridad
- Administrador de servicios de Intune

> [!NOTE]
> Los dispositivos Windows 10 híbridos unidos a Azure AD no tienen un propietario. Por lo tanto, si está buscando un dispositivo por propietario y no lo encontró, busque por el identificador de dispositivo.

## <a name="audit-logs"></a>Registros de auditoría

Las actividades de un dispositivo están disponibles a través de los registros de actividad. Estos registros incluyen actividades que desencadena el servicio de registro de dispositivos y los usuarios:

- Creación de dispositivos y adición de propietarios o usuarios al dispositivo.
- Cambios en la configuración de un dispositivo
- Operaciones de dispositivo como eliminar o actualizar un dispositivo

El punto de entrada a los datos de auditoría está en **Registros de auditoría**, que se encuentra en la sección **Actividad** de la página **Dispositivos**.

![Registros de auditoría](./media/device-management-azure-portal/61.png)

Un registro de auditoría tiene una vista de lista predeterminada que muestra:

- La fecha y hora de la repetición
- Los destinos
- El iniciador o actor (quién) de una actividad
- La actividad (qué)

![Registros de auditoría](./media/device-management-azure-portal/63.png)

Puede personalizar la vista de lista, haga clic en **Columnas** en la barra de herramientas.

![Registros de auditoría](./media/device-management-azure-portal/64.png)

Para restringir los datos del informe a un nivel que se adapte a sus necesidades, puede filtrar los datos de auditoría con los siguientes campos:

- Categoría
- Tipo de recurso de actividad
- Actividad
- Intervalo de fechas
- Destino
- Iniciado por (actor)

Además de los filtros, puede buscar entradas concretas.

![Registros de auditoría](./media/device-management-azure-portal/65.png)

## <a name="next-steps"></a>Pasos siguientes

[Cómo administrar dispositivos obsoletos en Azure AD](manage-stale-devices.md)
