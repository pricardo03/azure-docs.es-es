---
title: Habilitación de LDAP seguro (LDAPS) en Azure AD Domain Services | Microsoft Docs
description: Habilitación de LDAP seguro (LDAPS) para un dominio administrado de Azure AD Domain Services
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: c6da94b6-4328-4230-801a-4b646055d4d7
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/01/2018
ms.author: ergreenl
ms.openlocfilehash: df189e405dcd5277c1ccbd94e9d5d302660be79b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60359863"
---
# <a name="enable-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a>Habilitación de LDAP seguro (LDAPS) para un dominio administrado de Azure AD Domain Services

## <a name="before-you-begin"></a>Antes de empezar
Realice la [Tarea 2: exportación del certificado de LDAP seguro a un archivo .PFX](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md).


## <a name="task-3-enable-secure-ldap-for-the-managed-domain-using-the-azure-portal"></a>Tarea 3: Habilitación de LDAP seguro para el dominio administrado mediante Azure Portal
Realice los siguientes pasos de configuración para habilitar LDAP seguro:

1. Vaya a **[Azure Portal](https://portal.azure.com)**.

2. Busque 'servicios de dominio de' en el cuadro de búsqueda **Buscar recursos**. En el resultado de la búsqueda, seleccione **Azure AD Domain Services**. La página **Azure AD Domain Services** muestra el dominio administrado.

    ![Búsqueda del dominio administrado que se va aprovisionar](./media/getting-started/domain-services-provisioning-state-find-resource.png)

2. Haga clic en el nombre del dominio administrado (por ejemplo, "contoso100.com") para ver más detalles sobre el dominio.

    ![Domain Services: estado de aprovisionamiento](./media/getting-started/domain-services-provisioning-state.png)

3. Haga clic en **LDAP seguro** en el panel de navegación.

    ![Servicios de dominio: página LDAP seguro](./media/active-directory-domain-services-admin-guide/secure-ldap-blade.png)

4. De forma predeterminada, se deshabilita el acceso de LDAP seguro a su dominio administrado. Cambie **LDAP seguro** a **Habilitar**.

    ![Habilitación de LDAP seguro](./media/active-directory-domain-services-admin-guide/secure-ldap-blade-configure.png)
5. De forma predeterminada, el acceso de LDAP seguro al dominio administrado sobre Internet está deshabilitado. Cambie **Permitir el acceso mediante LDAP seguro a través de Internet** a **Habilitado**, si lo necesita.

    > [!WARNING]
    > Cuando se habilita el acceso LDAP seguro a través de Internet, el dominio es susceptible de recibir ataque por búsqueda de clave para la contraseña por Internet. Por lo tanto, se recomienda configurar un NSG para bloquear el acceso a los intervalos de direcciones IP de origen necesarios. Consulte las instrucciones para [bloquear el acceso LDAPS al dominio administrado a través de Internet](active-directory-ds-ldaps-bind-lockdown.md#task-6-lock-down-secure-ldap-access-to-your-managed-domain-over-the-internet).
    >

6. Haga clic en el icono de la carpeta que sigue al archivo **.PFX con el certificado LDAP seguro**. Especifique la ruta de acceso al archivo PFX con el certificado para el acceso de LDAP seguro al dominio administrado.

7. Especifique la **contraseña para descifrar el archivo .PFX**. Proporcione la misma contraseña que usó al exportar el certificado al archivo PFX.

8. Cuando haya terminado, haga clic en el botón **Guardar**.

9. Verá una notificación que le informará de que el LDAP seguro se ha configurado para el dominio administrado. Hasta que se complete la operación, no podrá modificar otras configuraciones para el dominio.

    ![Configuración del LDAP seguro para el dominio administrado](./media/active-directory-domain-services-admin-guide/secure-ldap-blade-configuring.png)

> [!NOTE]
> Se tardará entre unos 10 y 15 minutos en habilitar LDAP seguro para el dominio administrado. Si el certificado LDAP seguro proporcionado no coincide con los criterios necesarios, no se habilitará LDAP seguro para el directorio y aparecerá un mensaje de error. Por ejemplo, se indicará que el nombre de dominio es incorrecto, que el certificado ha expirado o que lo hará pronto. En este caso, vuelva a intentarlo con un certificado válido.
>
>

## <a name="next-step"></a>Paso siguiente
[Tarea 4: Configuración de DNS para acceder al dominio administrado desde Internet](active-directory-ds-ldaps-configure-dns.md)
