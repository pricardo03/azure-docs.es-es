---
title: 'Tutorial: Configuración de recursos para la validación como servicio | Microsoft Docs'
description: En este tutorial, aprenderá a configurar recursos para la validación como servicio.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/26/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 11/26/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 460edc7f092764cce3584c81371db4801fbd69bc
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2019
ms.locfileid: "55240023"
---
# <a name="tutorial-set-up-resources-for-validation-as-a-service"></a>Tutorial: Configuración de recursos para la validación como servicio

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Deberá crear una solución. Una solución de validación como servicio (VaaS) representa una solución de Azure Stack con una lista de materiales de hardware en particular. Usará la solución para comprobar si el hardware puede admitir la ejecución de Azure Stack. Siga este tutorial y prepárese para el uso del servicio con la solución.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Prepararse para usar VaaS mediante la configuración de la instancia de Azure AD.
> * Cree una cuenta de almacenamiento.

## <a name="configure-an-azure-ad-tenant"></a>Configurar un inquilino de Azure AD

Se necesita un inquilino de Azure AD para autenticarse y registrarse con VaaS. El asociado usará las características de control de acceso basado en rol (RBAC) para administrar qué usuarios de la organización del asociado pueden usar VaaS.

Registre el directorio de inquilino de la organización de Azure AD (en lugar del directorio de inquilino de Azure AD usado para Azure Stack) y establezca una directiva para administrar las cuentas de usuario que contiene. Para más información, consulte [Administración del directorio de Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-administer).

### <a name="create-a-tenant"></a>Creación de un inquilino

Cree un inquilino para usarlo específicamente con VaaS con un nombre descriptivo, como por ejemplo, `ContosoVaaS@onmicrosoft.com`.

1. Cree un inquilino de Azure AD en [Azure Portal](https://portal.azure.com) o use un inquilino existente. <!-- For instructions on creating new Azure AD tenants, see [Get started with Azure AD](https://docs.microsoft.com/azure/active-directory/get-started-azure-ad). -->

2. Agregue miembros de la organización al inquilino. Estos usuarios serán responsables de usar el servicio para ver o programar las pruebas. Una vez que termine el registro, definirá los niveles de acceso de los usuarios.
 
    Autorice a los usuarios de su inquilino para ejecutar acciones en VaaS mediante la asignación de uno de los roles siguientes:

    | Nombre de rol | DESCRIPCIÓN |
    |---------------------|------------------------------------------|
    | Propietario | Tiene acceso total a todos los recursos. |
    | Lector | Puede ver todos los recursos, pero no crearlos ni administrarlos. |
    | Colaborador en las pruebas | Puede crear y administrar recursos de prueba. |

    Para asignar roles en la aplicación **Azure Stack Validation Service**:

    1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
    2. Seleccione **Todos los servicios** > **Azure Active Directory** en la sección **Identidad**.
    3. Seleccione la aplicación **Aplicaciones empresariales** > **Azure Stack Validation Service**.
    4. Seleccione **Usuarios y grupos**. En la hoja **Azure Stack Validation Service: Usuarios y grupos** se enumeran los usuarios con permiso para usar la aplicación.
    5. Seleccione **+ Agregar usuario** para agregar un usuario del inquilino y asignar un rol.
   
    Si quiere aislar recursos y acciones de VaaS entre diferentes grupos dentro de una organización, puede crear varios directorios de inquilino de Azure AD.

### <a name="register-your-tenant"></a>Registro del inquilino

Este proceso le otorga al inquilino autorización para la aplicación **Azure Stack Validation Service** de Azure AD.

1. Envíe la siguiente información acerca del inquilino a Microsoft, a la dirección de correo electrónico [vaashelp@microsoft.com](mailto:vaashelp@microsoft.com).

    | Datos | DESCRIPCIÓN |
    |--------------------------------|---------------------------------------------------------------------------------------------|
    | Nombre de la organización | Nombre oficial de la organización. |
    | Nombre del directorio del inquilino de Azure AD | Nombre del directorio de inquilino de Azure AD que se registra. |
    | Identificador del directorio de inquilino de Azure AD | GUID del directorio de inquilino de Azure AD asociado con el directorio. Para obtener información acerca de cómo encontrar el identificador del directorio de inquilino de Azure AD, consulte [Get tenant ID](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#get-tenant-id) (Obtención del identificador de inquilino). |

2. Espere la confirmación del equipo de Azure Stack Validation para comprobar que el inquilino puede usar el portal de VaaS.

### <a name="consent-to-the-vaas-application"></a>Consentimiento a la aplicación VaaS

Como administrador de Azure AD, proporcione a la aplicación VaaS de Azure AD los permisos necesarios en nombre de su inquilino:

1. Use las credenciales de administrador global del inquilino para iniciar sesión en el [portal de VaaS](https://azurestackvalidation.com/). 

2. Seleccione **Mi cuenta**.

Cuando se le pida que conceda a VaaS los permisos de Azure AD enumerados, acepte los términos.

## <a name="create-an-azure-storage-account"></a>Creación de una cuenta de Azure Storage

Durante la ejecución de prueba, VaaS genera registros de diagnóstico en una cuenta de almacenamiento de Azure. Además de los registros de prueba, la cuenta de almacenamiento también puede usarse para cargar los paquetes de extensión OEM para el flujo de trabajo de validación de soluciones.

La cuenta de almacenamiento de Azure se hospeda en la nube pública de Azure, no en el entorno de Azure Stack.

1. En Azure Portal, seleccione **Todos los servicios** > **Almacenamiento** > **Cuentas de almacenamiento**. En la hoja **Cuentas de almacenamiento**, seleccione **Agregar**.

2. Seleccione la suscripción en la que se va a crear la cuenta de almacenamiento.

3. En **Grupo de recursos**, seleccione **Crear nuevo**. Escriba un nombre para el nuevo grupo de recursos.

4. Escriba un nombre para la cuenta de almacenamiento. El nombre que elija debe cumplir estos requisitos:
    - Ser único en Azure
    - Tener entre 3 y 24 caracteres
    - Contener solo números y letras minúsculas

5. Seleccione la región **Oeste de EE. UU.** para la cuenta de almacenamiento.

    Para asegurarse de no incurrir en cargos de red por el almacenamiento de registros, la cuenta de Azure Storage se puede configurar para usar únicamente la región **Oeste de EE. UU**. Las características de replicación de datos y nivel de acceso frecuente no son necesarias para estos datos. La habilitación de cualquiera de estas características aumentará los costos considerablemente.

6. Deje la configuración en los valores predeterminados, excepto **Tipo de cuenta**:

    - De manera predeterminada, el campo **Modelo de implementación** se establece en **Resource Manager**.
    - El campo **Rendimiento** se establece en **Estándar** de forma predeterminada.
    - En el campo **Tipo de cuenta**, seleccione **Almacenamiento de blobs**.
    - De manera predeterminada, el campo **Replicación** se establece en **Almacenamiento con redundancia local (LRS)**.
    - El **nivel de acceso** se establece en **Frecuente** de forma predeterminada.

7. Haga clic en **Revisar y crear** para revisar la configuración de la cuenta de almacenamiento y crear la cuenta.

## <a name="next-steps"></a>Pasos siguientes

Si su entorno no permite conexiones entrantes, siga el tutorial de implementación del agente local para ejecutar una prueba en su hardware.

> [!div class="nextstepaction"]
> [Implementación del agente local](azure-stack-vaas-local-agent.md)
