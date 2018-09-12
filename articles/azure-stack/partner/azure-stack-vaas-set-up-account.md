---
title: Configuración de una cuenta de validación como servicio de Azure Stack | Microsoft Docs
description: Aprenda a configurar una cuenta de validación como servicio.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 07/24/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 3f87721dcf7485e15a473e82597f6bf4baeca659
ms.sourcegitcommit: 8ebcecb837bbfb989728e4667d74e42f7a3a9352
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2018
ms.locfileid: "43775053"
---
# <a name="set-up-your-validation-as-a-service-account"></a>Configuración de una cuenta de validación como servicio

[!INCLUDE[Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Validación como servicio (VaaS) es un servicio de Azure que pueden usar los asociados de Microsoft Azure Stack que tengan un contrato de ingeniería conjunta con Microsoft para diseñar, desarrollar, validar, vender, implementar y dar soporte técnico a las soluciones de Azure Stack que hay en el mercado.

Aprenda a preparar un sistema para validación como servicio. Configure la instancia de Azure Active Directory y realice otras tareas necesarias para preparar un sistema para usar VaaS. 

Dichas tareas incluyen:

- Creación de un blob de Azure Storage para almacenar registros
- Implementación del agente local
- Descarga de las máquinas virtuales de imagen de prueba en la instancia de Azure Stack que se va a probar

## <a name="create-an-azure-active-directory-tenant-id"></a>Creación del identificador de un inquilino de Azure Active Directory

1. Creación de un inquilino de Azure Active Directory en [Azure Portal](https://portal.azure.com) o uso de un inquilino existente.

    Se recomienda crear un inquilino específicamente para usarlo con VaaS con un nombre descriptivo, como por ejemplo, ContosoVaaS@onmicrosoft.com. El asociado usará las características de control de acceso basado en rol (RBAC) para administrar qué usuarios de la organización del asociado pueden usar VaaS.  
    
    Para más información, consulte [Administración del directorio de Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-administer).

    > [!Note]  
    > Para más información acerca de la creación de nuevos inquilinos de Azure Active Directory, consulte [Get started with Azure AD](https://docs.microsoft.com/azure/active-directory/get-started-azure-ad) (Introducción a Azure AD).

2. Agregue los miembros de su organización responsables del uso del servicio en el inquilino. Asigne a cada usuario del inquilino uno de los siguientes roles para controlar su nivel de acceso a VaaS:

    | Nombre de rol | DESCRIPCIÓN |
    |---------------------|------------------------------------------|
    | Propietario | Tiene acceso total a todos los recursos. |
    | Lector | Puede ver todos los recursos, pero no los puede editar. |
    | Colaborador en las pruebas | Puede administrar los recursos de las pruebas. |
    | Colaborador del catálogo | Puede administrar los recursos de publicación de soluciones. |

## <a name="set-up-your-tenant"></a>Configuración del inquilino

Configure el inquilino en la aplicación **Azure Stack Validation Service**. 

1. Envíe la información siguiente acerca del inquilino a Microsoft, a la dirección de correo electrónico vaashelp@microsoft.com.

    | Datos | DESCRIPCIÓN |
    |--------------------------------|---------------------------------------------------------------------------------------------|
    | Nombre de la organización | Nombre oficial de la organización. |
    | Nombre del directorio del inquilino de Azure AD | El nombre del directorio del inquilino de Azure AD que se registra. |
    | Identificador del directorio del inquilino de Azure AD | El GUID del directorio del inquilino de Azure AD asociado con el directorio.<br> Para obtener información acerca de cómo buscar el identificador del directorio del inquilino de Azure AD, consulte [Obtención del identificador de inquilino](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#get-tenant-id). |

    

2. El equipo de Azure Stack confirma que el inquilino puede usar el portal de VaaS.

3. Use las credenciales de administrador global del inquilino para iniciar sesión en el [portal de VaaS](https://azurestackvalidation.com/
). Seleccione **Mi cuenta**.

    ![Iniciar sesión en el portal de VaaS](media/vaas_portalsignin.png)

3. El sitio le pedirá que conceda acceso a VaaS. Acepte los términos para continuar.

## <a name="assign-user-roles"></a>Asignación de roles de usuario

Para asignar un rol de usuario:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Seleccione **Todos los servicios** > **Azure Active Directory** en el grupo **Identidad**.
3. Seleccione la aplicación **Aplicaciones empresariales** > **Azure Stack Validation Service**.
4. Seleccione **Usuarios y grupos**. En la hoja **Azure Stack Validation Service: Usuarios y grupos** se enumeran los usuarios con permiso para usar la aplicación.
5. Seleccione **+ Agregar usuario** para agregar una asignación.

## <a name="create-an-azure-storage-blob-to-store-logs"></a>Creación de un blob de Azure Storage para almacenar registros

VaaS crea registros de diagnóstico al ejecutar las pruebas de validación. Necesita una ubicación, una dirección URL de SAS de un servicio de blob de Azure para almacenar los registros. La cuenta de almacenamiento también se puede utilizar para almacenar los paquetes de personalización de OEM.

En estos pasos se describe cómo configurar y generar un identificador URI de almacenamiento como servicio (SAS) para una cuenta de almacenamiento de Azure y dónde especificar la cuenta de almacenamiento en el portal de VaaS al iniciar las pruebas.

### <a name="create-an-azure-storage-account"></a>Creación de una cuenta de Azure Storage

1. Para crear una cuenta de almacenamiento, siga las instrucciones de [Creación de una cuenta de almacenamiento](../../storage/common/storage-quickstart-create-account.md).

2. Al seleccionar el tipo de cuenta de almacenamiento, seleccione **Blob Storage**.

### <a name="generate-a-sas-url-for-the-storage-account"></a>Generación de una dirección URL de SAS para la cuenta de almacenamiento

1. Vaya a la cuenta de almacenamiento que acaba de crear.

2. En la hoja de **Configuración**, seleccione **Firma de acceso compartido**.

3. Seleccione solo **Blob** en **Allowed Services options** (Opciones de servicios que se permiten), desactive el resto.

4. Seleccione **Servicio**, **Contenedor y **Objeto** en **Tipos de recursos permitidos**.

5. Seleccione **Lectura**, **Escritura**, **Lista**, **Adición** y **Creación** en **Permisos permitidos**  (desactive el resto).

6. En **Hora de inicio** especifique la hora actual y en **Hora de finalización** tres meses después de la hora actual.

7. Seleccione **Generar la cadena de conexión y SAS** y guarde la cadena **URL de SAS de Blob service**.

> [!Note]  
> La dirección URL de SAS expira a la hora de finalización establecida ak generar la dirección URL. Asegúrese de que la dirección URL tiene un periodo de validez suficiente antes de compartirla con el equipo de producto para la depuración, o que la dirección URL es válida durante más de 30 días al programar las pruebas.

## <a name="next-steps"></a>Pasos siguientes

- Utilice al agente local de VaaS para comprobar el hardware. Para obtener instrucciones, consulte [Implementación del agente local y prueba de las máquinas virtuales](azure-stack-vaas-test-vm.md).
- Para más información, consulte [Validación como servicio de Azure Stack](https://docs.microsoft.com/azure/azure-stack/partner).