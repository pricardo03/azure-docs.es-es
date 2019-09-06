---
title: 'Administración del servicio de certificados de OPC Vault: Azure | Microsoft Docs'
description: Administre los certificados de entidad de certificación raíz y los permisos de usuario de OPC Vault.
author: mregen
ms.author: mregen
ms.date: 8/16/2019
ms.topic: conceptual
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 4420e5b0d895f8ea30dbd39fc50dd7480d57d086
ms.sourcegitcommit: 4b8a69b920ade815d095236c16175124a6a34996
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/23/2019
ms.locfileid: "69995990"
---
# <a name="how-to-manage-the-opc-vault-certificate-service"></a>Administración del servicio de certificados de OPC Vault

En este artículo se explican las tareas administrativas del servicio de administración de certificados de OPC Vault en Azure, cómo renovar los certificados de entidad de certificación del emisor, cómo renovar la lista de revocación de certificados (CRL) y cómo conceder y revocar el acceso de usuario.

## <a name="create-or-renew-the-root-ca-certificate"></a>Creación o renovación del certificado de entidad de certificación raíz

Crear el certificado de entidad de certificación raíz es un paso obligatorio después de la implementación. Sin un certificado de entidad de certificación del emisor válido, no se puede firmar ni emitir ningún certificado de aplicación.<br>Consulte el capítulo sobre la [duración de los certificados](howto-opc-vault-secure-ca.md#certificates) para administrar los certificados con duraciones seguras razonables. Un certificado de entidad de certificación del emisor se debe renovar después de la mitad de su duración, pero a más tardar antes que la duración configurada de un certificado de aplicación recién firmado supere la duración del certificado del emisor.<br>
> [!IMPORTANT]
> El rol "Administrador" es necesario para crear o renovar el certificado de entidad de certificación del emisor.

1. Abra el servicio de certificados en `https://myResourceGroup-app.azurewebsites.net` e inicie sesión.
2. Vaya a la página `Certificate Groups`.
3. Aparece una grupo de certificados `Default`. Haga clic en `Edit`.
4. En `Edit Certificate Group Details`, puede modificar el nombre del firmante y la duración de los certificados de entidad de certificación y de la aplicación.<br>El firmante y la duración solo se deben establecer una vez antes de que se emita el primer certificado de entidad de certificación. Los cambios de duración durante las operaciones pueden generar duraciones incoherente de las CRL y los certificados emitidos.
5. Escriba un firmante válido, por ejemplo, `CN=My CA Root, O=MyCompany, OU=MyDepartment`.<br>
   > [!IMPORTANT]
   > Para cambiar el firmante, es necesario renovar el certificado de emisor o el servicio no podrá firmar los certificados de la aplicación. El firmante de la configuración se comprueba con respecto al firmante del certificado de emisor activo. Si los firmantes no coinciden, se rechaza la firma del certificado.
6. Haga clic en el botón `Save`.
7. Si en este momento se encuentra con un error "prohibido", las credenciales de usuario no tienen permiso de administrador para modificar o crear un nuevo certificado raíz. De manera predeterminada, el usuario que implementó el servicio tiene roles de administrador y de firma con el servicio, es necesario agregar otros usuarios a los roles "Aprobador", "Escritor" o "Administrador", según corresponda, en el registro de la aplicación AzureAD.
8. Haga clic en el botón `Details`. `View Certificate Group Details` debe mostrar la información actualizada.
9. Haga clic en el botón `Renew CA Certificate` para emitir el primer certificado de entidad de certificación del emisor o para renovar el certificado de emisor. Presione `Ok` para continuar.
10. Los `Certificate Details` aparecen después de unos segundos. Presione `Issuer` o `Crl` para descargar el certificado de entidad de certificación y la CRL más recientes para distribuirlas a las aplicaciones OPC UA.
11. Ahora que el servicio de administración de certificados de OPC UA está listo para emitir certificados para las aplicaciones OPC UA.

## <a name="renew-the-crl"></a>Renovación de la CRL

La renovación de la lista de revocación de certificados (CRL) es una actualización que se debe distribuir a las aplicaciones a intervalos regulares. Los dispositivos OPC UA, que admiten la extensión X509 del punto de distribución de CRL, pueden actualizar directamente la CRL desde el punto de conexión del microservicio. Otros dispositivos de OPC UA pueden requerir actualizaciones manuales o, en el mejor de los casos, se pueden actualizar mediante las extensiones de inserción de servidor GDS (*) para actualizar las listas de confianza con los certificados y las CRL.

En el flujo de trabajo siguiente, todas las solicitudes de certificado de los estados eliminados se revocan en las CRL, que corresponden al certificado de entidad de certificación del emisor para el que se emitieron. El número de versión de la CRL se incrementa en 1. <br>
> [!NOTE]
> Todas las CRL emitidas son válidas hasta la expiración del certificado de entidad de certificación del emisor, ya que la especificación de OPC UA no requiere un modelo de distribución obligatorio y determinista para la CRL.

> [!IMPORTANT]
> El rol "Administrador" es necesario para renovar la CRL del emisor.

1. Abra el servicio de certificados en `https://myResourceGroup.azurewebsites.net` e inicie sesión.
2. Vaya a la página `Certificate Groups`.
3. Haga clic en el botón `Details`. `View Certificate Group Details` debe mostrar la información del certificado y la CRL actuales.
4. Haga clic en el botón `Update CRL Revocation List(CRL)` para emitir una CRL actualizada para todos los certificados de emisor activos en el almacenamiento de OPC Vault.
5. Los `Certificate Details` aparecen después de unos segundos. Presione `Issuer` o `Crl` para descargar el certificado de entidad de certificación y la CRL más recientes para distribuirlas a las aplicaciones OPC UA.

## <a name="manage-user-roles"></a>Administrar los roles de usuario

Los roles de usuario para el microservicio OPC Vault se administran en la aplicación empresarial de Azure Active Directory.

Para una descripción detallada de las definiciones de roles, consulte la sección [Roles](howto-opc-vault-secure-ca.md#roles).

De manera predeterminada, un usuario autenticado en el inquilino puede iniciar sesión en el servicio como un "Lector". Los roles con privilegios elevados requieren la administración manual en Azure Portal o mediante PowerShell.

### <a name="add-user"></a>Agregar usuario

1. Abra Azure Portal en `portal.azure.com`.
2. Vaya a `Azure Active Directory`/`Enterprise applications`.
3. Elija de manera predeterminada `resourceGroupName-service` como el registro del microservicio OPC Vault.
4. Vaya a `Users and Groups`.
5. Haga clic en `Add User`.
6. Seleccione o invite al usuario para la asignación a un rol específico.
7. Seleccione el rol de los usuarios.
8. Presione el botón `Assign`.
9. Para los usuarios en los roles `Administrator` o `Approver`, siga agregando las directivas de acceso de Azure Key Vault.

### <a name="remove-user"></a>Quitar usuario

1. Abra Azure Portal en `portal.azure.com`.
2. Vaya a `Azure Active Directory`/`Enterprise applications`.
3. Elija de manera predeterminada `resourceGroupName-service` como el registro del microservicio OPC Vault.
4. Vaya a `Users and Groups`.
5. Seleccione un usuario con un rol para quitarlo.
6. Presione el botón `Remove`.
7. Quite los Administradores y Aprobadores quitados también de las directivas de Azure Key Vault.

### <a name="add-user-access-policy-to-azure-key-vault"></a>Agregar una directiva de acceso de usuario a Azure Key Vault

Se requieren directivas de acceso adicionales para los **Aprobadores** y los **Administradores**.

De manera predeterminada, la identidad del servicio solo tiene permisos limitados para acceder a Key Vault y evitar que las operaciones o los cambios se realicen sin la suplantación del usuario. Los permisos de servicios básicos son `Get` y `List` tanto para secretos como para certificados. En el caso de los secretos, solo hay una excepción: el servicio puede `Delete` una clave privada del almacén secreto después de su aceptación por parte de un usuario. Todas las demás operaciones requieren permisos suplantados del usuario.<br>

#### <a name="for-an-approver-role-the-following-permissions-must-be-added-to-key-vault"></a>Para un **rol Aprobador**, se deben agregar los permisos siguientes a Key Vault:

1. Abra Azure Portal en `portal.azure.com`.
2. Vaya al `resourceGroupName` de OPC Vault usado durante la implementación.
3. Vaya al `resourceGroupName-xxxxx` de Key Vault.
4. Vaya a las `Access Policies`.
5. Haga clic en `Add new`.
6. Omita la plantilla, no hay plantilla que coincida con los requisitos.
7. Haga clic en `Select Principal` y seleccione el usuario que se va a agregar o invite a un usuario nuevo al inquilino.
8. Compruebe `Key permissions`: `Get`, `List` y, lo que es más importante, `Sign`.
9. Compruebe `Secret permissions`: `Get`, `List`, `Set` y `Delete`.
10. Compruebe `Certificate permissions`: `Get` y `List`.
11. Haga clic en `Ok`.
12. `Save` los cambios.

#### <a name="for-an-administrator-role-the-following-permissions-must-be-added-to-key-vault"></a>Para un **rol Administrador**, se deben agregar los permisos siguientes a Key Vault:

1. Abra Azure Portal en `portal.azure.com`.
2. Vaya al `resourceGroupName` de OPC Vault usado durante la implementación.
3. Vaya al `resourceGroupName-xxxxx` de Key Vault.
4. Vaya a las `Access Policies`.
5. Haga clic en `Add new`.
6. Omita la plantilla, no hay plantilla que coincida con los requisitos.
7. Haga clic en `Select Principal` y seleccione el usuario que se va a agregar o invite a un usuario nuevo al inquilino.
8. Compruebe `Key permissions`: `Get`, `List` y, lo que es más importante, `Sign`.
9. Compruebe `Secret permissions`: `Get`, `List`, `Set` y `Delete`.
10. Compruebe `Certificate permissions`: `Get`, `List`, `Update`, `Create` y `Import`.
11. Haga clic en `Ok`.
12. `Save` los cambios.

### <a name="remove-user-access-policy-from-azure-key-vault"></a>Quitar la directiva de acceso de usuario de Azure Key Vault

1. Abra Azure Portal en `portal.azure.com`.
2. Vaya al `resourceGroupName` de OPC Vault usado durante la implementación.
3. Vaya al `resourceGroupName-xxxxx` de Key Vault.
4. Vaya a las `Access Policies`.
5. Busque el usuario que se va a quitar y haga clic en `... / Delete` para eliminar el acceso de usuario.

## <a name="next-steps"></a>Pasos siguientes

Ahora que aprendió a administrar los usuarios y certificados de OPC Vault, este es el siguiente paso sugerido:

> [!div class="nextstepaction"]
> [Comunicación segura de los dispositivos OPC](howto-opc-vault-secure.md)
