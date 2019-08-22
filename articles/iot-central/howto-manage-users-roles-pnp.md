---
title: Administración de usuarios y roles en una aplicación de Azure IoT Central | Microsoft Docs
description: Como administrador, aquí se indica la forma de administrar usuarios y roles en su aplicación de Azure IoT Central
author: v-krghan
ms.author: v-krghan
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 0c1a6b7370a4d1f8e01b22bfd52caa6cb6973947
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2019
ms.locfileid: "69879260"
---
# <a name="manage-users-and-roles-in-your-iot-central-application"></a>Administrar usuarios y roles en la aplicación de IoT Central

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

En este artículo describe cómo puede agregar, editar y eliminar usuarios como administrador y cómo administrar roles en su aplicación de Azure IoT Central.

Para tener acceso a la sección **Administración** y poder usarla, debe disponer del rol **Administrador** para la aplicación Azure IoT Central. Si crea una aplicación Azure IoT Central, se le asigna automáticamente el rol **Administrator** para esa aplicación.


## <a name="add-users"></a>Agregar usuarios

Cada usuario debe tener una cuenta de usuario antes de poder iniciar sesión y acceder a la aplicación Azure IoT Central. Se admiten las cuentas de Microsoft (MSA) y las de Azure Active Directory (Azure AD) en Azure IoT Central. Actualmente no se admiten los grupos de Azure Active Directory en Azure IoT Central.

Para más información, consulte la [ayuda de la cuenta de Microsoft](https://support.microsoft.com/products/microsoft-account?category=manage-account) y la [Guía de inicio rápido: Adición de nuevos usuarios a Azure Active Directory](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory).

1. Para agregar un usuario a una aplicación de IoT Central, vaya a la página **Usuarios** de la sección **Administración**.

    ![Lista de usuarios](media/howto-manage-users-roles-pnp/image1.png)

1. Para agregar un usuario, en la página **Usuarios**, seleccione **+ Agregar usuario**.

1. Seleccione un rol para el usuario en el menú desplegable **Rol**. Más información sobre los roles en la sección [Administración de roles](#manage-roles) de este artículo.

    ![Selección de roles](media/howto-manage-users-roles-pnp/image3.png)

    > [!NOTE]
    >  Para agregar usuarios de forma masiva, indique los identificadores de usuario de todos los usuarios que quiera agregar, separados por punto y coma. Seleccione un rol en el menú desplegable **Rol**. Después, seleccione **Guardar**.

### <a name="edit-the-roles-that-are-assigned-to-users"></a>Modificar los roles asignados a usuarios

Los roles no se pueden cambiar una vez asignados. Para cambiar el rol asignado a un usuario, elimine el usuario y agréguelo de nuevo con un rol diferente.

> [!NOTE]
> Los roles asignados son específicos de la aplicación de IoT Central y no se pueden administrar desde Azure Portal.

## <a name="delete-users"></a>Eliminación de usuarios

Para eliminar usuarios, seleccione una o más casillas en la página **Usuarios**. A continuación, seleccione **Eliminar**.

## <a name="manage-roles"></a>Administrar roles

Los roles le permiten controlar qué usuarios de la organización pueden realizar varias tareas en IoT Central. Hay tres roles que se pueden asignar a los usuarios de la aplicación.

### <a name="administrator"></a>Administrador

Los usuarios con el rol **Administrador** tienen acceso a toda la funcionalidad de la aplicación.

El usuario que crea una aplicación se asigna automáticamente al rol **Administrador**. Siempre debe haber al menos un usuario en el rol **Administrador**.

### <a name="application-builder"></a>Generador de aplicaciones

Los usuarios con el rol **Application Builder** (Generador de aplicaciones) pueden hacerlo todo en una aplicación, excepto administrarla. Esto significa que los generadores de pueden crear, editar y eliminar plantillas de dispositivo y dispositivos, administrar conjuntos de dispositivos y ejecutar análisis y trabajos. Los generadores no tienen acceso a la sección **Administración** de la aplicación.

### <a name="application-operator"></a>Operador de aplicaciones

Los usuarios con el rol **Application Operator** (Operador de la aplicación) no puede realizar cambios en las plantillas de dispositivo ni administrar la aplicación. Esto significa que los operadores pueden agregar y eliminar dispositivos, administrar conjuntos de dispositivos y ejecutar análisis y trabajos. Los operadores no tienen acceso a las páginas **Application Builder** (Generador de aplicaciones) ni **Administración**.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a administrar usuarios y roles en Azure IoT Central, el siguiente paso sugerido es aprender a [ver su factura](howto-view-bill-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) en Azure IoT Central.
