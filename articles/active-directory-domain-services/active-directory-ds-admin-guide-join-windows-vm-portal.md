---
title: 'Azure Active Directory Domain Services: Unir una máquina virtual Windows Server a un dominio administrado | Microsoft Docs'
description: Unión de una máquina virtual de Windows Server a Azure AD DS
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: 29316313-c76c-4fb9-8954-5fa5ec82609e
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/21/2018
ms.author: ergreenl
ms.openlocfilehash: 41e7f0ea8616b25eac43ecff7802e0f9e1c08050
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55180711"
---
# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain"></a>Unión de una máquina virtual de Windows Server a un dominio administrado
Este artículo muestra cómo implementar una máquina virtual de Windows Server mediante Azure Portal. A continuación, muestra cómo unir la máquina virtual a un dominio administrado de Azure Active Directory Domain Services (Azure AD DS).

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="step-1-create-a-windows-server-virtual-machine"></a>Paso 1: Creación de una máquina virtual con Windows Server
Para crear una máquina virtual de Windows que esté unida a la red virtual en la que ha habilitado Azure AD DS, haga lo siguiente:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. En la parte superior del panel izquierdo, seleccione **Nuevo**.
3. Seleccione **Compute** y, después, seleccione **Windows Server 2016 Datacenter**.

    ![Vínculo a Windows Server 2016 Datacenter](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-image.png)

4. En la página **Datos básicos** del asistente, configure las opciones básicas para la máquina virtual.

    ![Panel Datos básicos](./media/active-directory-domain-services-admin-guide/create-windows-vm-basics.png)

    > [!TIP]
    > El nombre de usuario y la contraseña que escriba aquí son para una cuenta de administrador local que se utiliza para iniciar sesión en la máquina virtual. Elija una contraseña segura para proteger la máquina virtual frente a ataques de fuerza bruta de contraseñas. No escriba las credenciales de la cuenta de usuario del dominio aquí.
    >

5. Seleccione un **Tamaño** para la máquina virtual. Para ver más tamaños, seleccione **Ver todo** o cambie el filtro **Tipo de disco admitido**.

    ![Panel Elegir un tamaño](./media/active-directory-domain-services-admin-guide/create-windows-vm-size.png)

6. En el panel **Configuración**, seleccione la red virtual en la que se está implementado el dominio administrado de Azure AD DS. Elija una subred diferente a aquella en la que está implementado el dominio administrado. Para las demás opciones, conserve los valores predeterminados y, a continuación, seleccione **Aceptar**.

    ![Configuración de la red virtual para la máquina virtual](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-vnet.png)

    > [!TIP]
    > **Elija la red y subred virtual correctas.**
    >
    > Seleccione la red virtual en la que está implementado el dominio administrado o una red virtual que esté conectada a ella mediante el emparejamiento de redes virtuales. Si selecciona una red virtual no conectada, no se puede unir la máquina virtual al dominio administrado.
    >
    > Le recomendamos que implemente el dominio administrado en una subred dedicada. Por lo tanto, no seleccione la subred en la que se ha habilitado el dominio administrado.

7. Para las demás opciones, conserve los valores predeterminados y, a continuación, seleccione **Aceptar**.
8. En la página **Compra**, revise la configuración y, a continuación, seleccione **Aceptar** para implementar la máquina virtual.
9. La implementación de la máquina virtual se ancla al panel de Azure Portal.

    ![¡Listo!](./media/active-directory-domain-services-admin-guide/create-windows-vm-done.png)
10. Una vez finalizada la implementación, puede ver la información acerca de la máquina virtual en la página **Información general**.


## <a name="step-2-connect-to-the-windows-server-virtual-machine-by-using-the-local-administrator-account"></a>Paso 2: Conéctese a la máquina virtual de Windows Server con la cuenta de administrador local
A continuación, conéctese a la máquina virtual de Windows Server recién creada para unirla al dominio. Use las credenciales de administrador local que especificó cuando creó la máquina virtual.

Realice los pasos siguientes para conectarse a la máquina virtual:

1. En el panel **Información general**, seleccione **Conectar**.  
    Se crea y se descarga un archivo de Protocolo de Escritorio remoto (.rdp).

    ![Conexión a máquina virtual de Windows](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)

2. Para conectarse a la máquina virtual, abra el archivo RDP descargado. Cuando se le pida, seleccione **Conectar**.
3. Escriba sus **credenciales de administrador local**, que especificó cuando creó la máquina virtual (por ejemplo, *localhost\mahesh*).
4. Si ve una advertencia de certificado durante el proceso de inicio de sesión, seleccione **Sí**o **Continuar** para conectarse.

En este punto, debe tener la sesión iniciada en la máquina virtual de Windows recién creada con las credenciales de administrador local. El siguiente paso es unir la máquina virtual al dominio.


## <a name="step-3-join-the-windows-server-virtual-machine-to-the-azure-ad-ds-managed-domain"></a>Paso 3: Unión de la máquina virtual de Windows Server al dominio administrado con Azure AD DS
Para unir la máquina virtual de Windows Server al dominio administrado con Azure AD DS, haga lo siguiente:

1. Conéctese a la máquina virtual de Windows Server tal y como se muestra en el Paso 2. En la pantalla **Inicio**, abra **Administrador del servidor**.
2. En el panel izquierdo de la ventana **Administrador del servidor**, seleccione **Servidor local**.

    ![Ventana Administrador del servidor en la máquina virtual](./media/active-directory-domain-services-admin-guide/join-domain-server-manager.png)

3. En **Propiedades**, seleccione **Grupo de trabajo**.
4. En la ventana **Propiedades del sistema**, seleccione **Cambiar** para unirse al dominio.

    ![Ventana Propiedades del sistema](./media/active-directory-domain-services-admin-guide/join-domain-system-properties.png)

5. En el cuadro **Dominio**, especifique el nombre del dominio administrado con Azure AD DS y, a continuación, seleccione **Aceptar**.

    ![Especificar el dominio al que se unirá](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-domain.png)

6. Se le solicitará que especifique sus credenciales para unirse al dominio. Use las credenciales de un *usuario que pertenezca al grupo de administradores de Azure AD DC*. Solo los miembros de este grupo tienen privilegios para unir máquinas al dominio administrado.

    ![Ventana de seguridad de Windows para especificar credenciales](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-credentials.png)

7. Puede especificar credenciales de cualquiera de las maneras siguientes:

   * **Formato UPN**: (recomendado) especifique el sufijo del nombre principal de usuario (UPN) de la cuenta de usuario según cómo esté configurado en Azure AD. En este ejemplo, el sufijo UPN del usuario *bob* es *bob@domainservicespreview.onmicrosoft.com*.

   * **Formato SAMAccountName**: puede especificar el nombre de la cuenta en formato SAMAccountName. En este ejemplo, el usuario *bob* tendría que escribir *CONTOSO100\bob*.

     > [!TIP]
     > **Se recomienda usar el formato de UPN para especificar las credenciales.**
     >
     > Si el prefijo UPN de un usuario es demasiado largo (por ejemplo, *joehasareallylongname*), SAMAccountName puede generarse automáticamente. Si varios usuarios tienen el mismo prefijo UPN (por ejemplo, *bob*) en su inquilino de Azure AD, el servicio puede generar automáticamente su formato SAMAccountName. En estos casos, el formato UPN puede utilizarse de forma fiable para iniciar sesión en el dominio.
     >

8. Una vez se haya unido a un dominio correctamente, se muestra el siguiente mensaje de bienvenida al dominio.

    ![Bienvenido al dominio](./media/active-directory-domain-services-admin-guide/join-domain-done.png)

9. Para completar el proceso de unión al dominio, reinicie la máquina virtual.

## <a name="troubleshoot-joining-a-domain"></a>Solución de problemas de unión a un dominio
### <a name="connectivity-issues"></a>Problemas de conectividad
Si la máquina virtual no puede encontrar el dominio, pruebe los siguientes pasos de solución de problemas:

* Compruebe que la máquina virtual está conectada a la misma red virtual en la que habilitó Azure AD DS. De lo contrario, la máquina virtual no podrá conectarse ni unirse al dominio.

* Compruebe que la máquina virtual está en una red virtual que a su vez está conectada a la red virtual en la que se habilitó Azure AD DS.

* Trate de hacer ping al nombre de dominio DNS administrado (por ejemplo, *ping contoso100.com*). Si no puede hacerlo, trate de hacer ping a las direcciones IP del dominio que aparece en la página en la que habilitó Azure AD DS (por ejemplo, *ping 10.0.0.4*). Si puede hacer ping a la dirección IP pero no al dominio, DNS puede estar configurado incorrectamente. Compruebe si las direcciones IP del dominio están configuradas como servidores DNS de la red virtual.

* Intente vaciar la memoria caché de resolución DNS en la máquina virtual (*ipconfig /flushdns*).

Si se muestra una ventana que solicita credenciales para unirse al dominio, significa que no tiene problemas de conectividad.

### <a name="credentials-related-issues"></a>Problemas relacionados con las credenciales
Si tiene problemas con las credenciales y no puede unirse al dominio, realice uno de los siguientes pasos para solucionar problemas:

* Pruebe a usar el formato UPN para especificar las credenciales. El atributo SAMAccountName de su cuenta se puede generar automáticamente si hay varios usuarios con el mismo prefijo UPN en el inquilino o si el prefijo UPN es demasiado largo. En estos casos, el formato del atributo SAMAccountName de su cuenta puede que no sea el mismo que espera o que usa en su dominio local.

* Intente usar las credenciales de una cuenta de usuario que pertenezca al grupo *Administradores de AAD DC*.

* Compruebe que [habilitó la sincronización de contraseñas](active-directory-ds-getting-started-password-sync.md) en el dominio administrado.

* Compruebe que usó el UPN del usuario según está configurado en Azure AD (por ejemplo, *bob@domainservicespreview.onmicrosoft.com*) para iniciar sesión.

* Espere suficiente tiempo para que la sincronización de contraseña se complete, como se especifica en la guía de introducción.

## <a name="related-content"></a>Contenido relacionado
* [Guía de introducción a Azure AD DS](active-directory-ds-getting-started.md)
* [Administración de un dominio administrado con Azure AD DS](active-directory-ds-admin-guide-administer-domain.md)
