---
title: Creación y configuración de clústeres de Enterprise Security Package en Azure HDInsight
description: Aprenda a crear clústeres de Enterprise Security Package en Azure HDInsight.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 05/09/2019
ms.openlocfilehash: 8da50757182609402ecb035b6f3e92959758ef46
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2019
ms.locfileid: "68442304"
---
# <a name="create-and-configure-enterprise-security-package-clusters-in-azure-hdinsight"></a>Creación y configuración de clústeres de Enterprise Security Package en Azure HDInsight

Enterprise Security Package para Azure HDInsight le proporciona acceso a la autenticación basada en Active Directory, compatibilidad con varios usuarios y control de acceso basado en rol para los clústeres de Apache Hadoop en Azure. Los clústeres de Enterprise Security Package (ESP) para HDInsight permiten a las organizaciones, normalmente adheridas a estrictas directivas de seguridad corporativa, procesar información confidencial de manera segura.

El objetivo de esta guía es configurar correctamente los recursos necesarios para que los usuarios locales puedan iniciar sesión en un ESP habilitado para el clúster de HDInsight. En este artículo se describen los pasos necesarios para crear un clúster de Azure HDInsight con Enterprise Security Package habilitado. Estos pasos incluyen la creación de una máquina virtual de IaaS de Windows con Active Directory y Servicio de nombres de dominio (DNS) habilitado. Este servidor actuará como un sustituto del entorno local **real** y le permitirá continuar con los pasos de instalación y configuración para que los pueda repetir más adelante en su propio entorno. Esta guía también le ayudará a crear un entorno de identidad híbrida mediante la sincronización de hash de contraseñas con Azure Active Directory.

Esta guía está pensada para ser complementaria de [Uso de Enterprise Security Package en HDInsight](apache-domain-joined-architecture.md).

Antes de usar este proceso en su propio entorno, configure Active Directory y Servicio de nombres de dominio (DNS). Además, habilite Azure Active Directory y sincronice cuentas de usuario locales en Azure Active Directory.

![diagrama de arquitectura](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image002.png)

## <a name="create-on-premises-environment"></a>Creación de entorno local

Información general: En esta sección, usará una plantilla de implementación rápida de Azure para crear nuevas máquinas virtuales, configurar instancias de Servicio de nombres de dominio y un nuevo bosque de AD.

1. Vaya a [Create an Azure VM with a new AD Forest](https://azure.microsoft.com/resources/templates/active-directory-new-domain/) (Creación de una máquina virtual de Azure con un nuevo bosque de AD), para ver la plantilla de implementación rápida.

1. Haga clic en **Implementar en Azure**.
1. Inicie sesión en la suscripción de Azure.
1. En la pantalla **Create an Azure VM with a new AD Forest** (Creación de una máquina virtual de Azure con un nuevo bosque de AD), complete los pasos siguientes:
    1. Seleccione la suscripción en la que desea implementar los recursos en el menú desplegable **Suscripción**.
    1. Seleccione **Crear nuevo** situado junto al **grupo de recursos** y escriba el nombre **OnPremADVRG**
    1. Escriba la siguiente información en el resto de los campos de la plantilla:

        * **Ubicación**: Centro de EE. UU.
        * **Nombre de usuario administrador**: HDIFabrikamAdmin
        * **Contraseña de administrador**: <SU_CONTRASEÑA>
        * **Dominio**: HDIFabrikam.com
        * **Prefijo de DNS**: hdifabrikam

        ![Plantilla de creación de una máquina virtual de Azure y un bosque de AD](./media/apache-domain-joined-create-configure-enterprise-security-cluster/create-azure-vm-ad-forest.png)

    1. Haga clic en **Comprar**.
    1. Supervise la implementación y espere a que se complete.
    1. Confirme que los recursos se crean en el grupo de recursos correcto `OnPremADVRG`.

## <a name="configure-users-and-groups-for-cluster-access"></a>Configuración de usuarios y grupos para el acceso al clúster

Información general: En esta sección, va a crear usuarios que tendrán acceso al clúster de HDInsight al final de esta guía.

1. Conéctese al controlador de dominio mediante Escritorio remoto.
    1. Si ha usado la plantilla que se ha mencionado al principio, el controlador de dominio será una máquina virtual denominada **adVM** del grupo de recursos `OnPremADVRG`.
    1. Vaya a Azure Portal > **Grupos de recursos** > **OnPremADVRG** > **adVM** > **Conectar**.
    1. Haga clic en la pestaña **RDP** y, a continuación, haga clic en **Descargar archivo RDP**.
    1. Guarde el archivo en el equipo y ábralo.
    1. Cuando se le soliciten las credenciales, use `HDIFabrikam\HDIFabrikamAdmin` como nombre de usuario y, a continuación, especifique la contraseña que eligió para la cuenta de administrador.

1. Una vez que se abra la sesión de Escritorio remoto en la máquina virtual del controlador de dominio, inicie **Usuarios y equipos de Active Directory** en el panel **Administrador del servidor**. Haga clic en **Herramientas** en la esquina superior derecha y, a continuación, en **Usuarios y equipos de Active Directory** en la lista desplegable.

    ![Abrir administración de Active Directory en Administrador del servidor](./media/apache-domain-joined-create-configure-enterprise-security-cluster/server-manager-active-directory-screen.png)

1. Cree dos nuevos usuarios, **HDIAdmin** y **HDIUser**. Estos dos usuarios se usarán para iniciar sesión en los clústeres de HDInsight.

    1. En la pantalla **Usuarios y equipos de Active Directory**, haga clic en **Acción** > **Nuevo** > **Usuario**.

        ![Crear nuevo usuario de Active Directory](./media/apache-domain-joined-create-configure-enterprise-security-cluster/create-new-user.png)

    1. En la pantalla **New Object - User** (Nuevo objeto - Usuario), escriba `HDIUser` como **Nombre de inicio de sesión del usuario** y haga clic en **Siguiente**.

        ![Crear el primer usuario administrador](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image020.png)

    1. En el menú emergente que aparece, escriba la contraseña deseada para la nueva cuenta. Seleccione la casilla que dice **La contraseña no expira nunca**. Haga clic en **Aceptar**.
    1. Haga clic en **Finalizar** para crear la nueva cuenta.
    1. Cree otro usuario `HDIAdmin`.

        ![Crear el segundo usuario administrador](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image024.png)

1. En la pantalla **Usuarios y equipos de Active Directory**, haga clic en **Acción** > **Nuevo** > **Grupo**. Cree `HDIUserGroup` como nuevo grupo.

    ![Crear nuevo grupo de Active Directory](./media/apache-domain-joined-create-configure-enterprise-security-cluster/create-new-group.png)

    ![crear nuevo grupo2](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image028.png)

1. Agregue el usuario **HDIUser** creado en el paso anterior al grupo **HDIUserGroup** como miembro.

    1. Haga clic con el botón derecho en **HDIUserGroup** y, después, en **Propiedades**.
    1. Vaya a la pestaña **Miembros** y haga clic en **Agregar**.
    1. Escriba `HDIUser` en el cuadro etiquetado **Escriba los nombres de objeto que desea seleccionar** y haga clic en **Aceptar**.
    1. Repita el paso anterior para la otra cuenta `HDIAdmin`

        ![agregar miembros a grupo](./media/apache-domain-joined-create-configure-enterprise-security-cluster/active-directory-add-users-to-group.png)

Ahora ya ha creado el entorno de Active Directory, junto con dos usuarios y un grupo de usuarios para acceder al clúster de HDInsight.

Estos usuarios se sincronizarán con Azure AD.

### <a name="create-a-new-azure-active-directory"></a>Creación de un directorio de Azure Active Directory

1. Inicie sesión en el Portal de Azure.
1. Haga clic en **Crear un recurso** y escriba **directorio**. Seleccione **Azure Active Directory** > **Crear**.
1. Escriba **HDIFabrikam** en **Nombre de la organización**.
1. Escriba **HDIFabrikamoutlook** en **Nombre de dominio inicial**.
1. Haga clic en **Create**(Crear).
1. En el panel izquierdo de Azure Portal, haga clic en **Azure Active Directory**.
1. Si es necesario, haga clic en **Cambiar directorio** para cambiar al directorio nuevo que ha creado: **HDIFabrikamoutlook**.
1. En **Administrar** haga clic en **Nombres de dominio personalizados** > **Agregar dominio personalizado**.
1. Escriba **HDIFabrikam.com** en **Nombre de dominio personalizado** y haga clic en **Agregar dominio**.

![creación de un directorio de Azure Active Directory](./media/apache-domain-joined-create-configure-enterprise-security-cluster/create-new-directory.png)

![crear un dominio personalizado](./media/apache-domain-joined-create-configure-enterprise-security-cluster/create-custom-domain.png)

## <a name="configure-your-azure-ad-tenant"></a>Configuración del inquilino de Azure AD

Información general: Ahora va a configurar el inquilino de Azure AD de forma que pueda sincronizar usuarios y grupos desde AD en el entorno local a la nube.

1. Cree un administrador de inquilinos de AD.
    1. Inicie sesión en Azure Portal y seleccione el inquilino **HDIFabrikam** de Azure AD.
    1. Seleccione **Usuarios** en **Administrar** y, a continuación, **Nuevo usuario**.
    1. Escriba la siguiente información del nuevo usuario:

        * Nombre: fabrikamazureadmin
        * Nombre de usuario: fabrikamazureadmin@hdifabrikam.com
        * Contraseña: una contraseña segura de su elección

    1. Haga clic en la sección **Grupos**, busque **Administradores de controladores de dominio de AAD** y haga clic en **Seleccionar**.

        ![Grupos](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image038.png)

    1. Haga clic en la sección **Rol del directorio** y seleccione **Administrador global** en el lado derecho. Haga clic en **Aceptar**.

        ![Rol del directorio](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image040.png)

    1. Escriba una contraseña para el usuario. Haga clic en **Create**(Crear).

1. Si desea cambiar la contraseña para el usuario recién creado <fabrikamazureadmin@hdifabrikam.com>, inicie sesión en Azure Portal mediante la identidad y, a continuación, se le pedirá que cambie la contraseña.

## <a name="sync-on-premises-users-to-azure-ad"></a>Sincronización de usuarios locales en Azure AD

### <a name="download-and-install-microsoft-azure-active-directory-connect"></a>Descarga e instalación de Microsoft Azure Active Directory Connect

1. [Descarga de Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594).

1. Instale Microsoft Azure Active Directory Connect en el controlador de dominio.
    1. Abra el archivo ejecutable que descargó en el paso anterior y acepte los términos de licencia. Haga clic en **Continue**.

        ![Azure AD Connect](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image052.png)

    1. Haga clic en **Usar la configuración rápida** y complete la instalación.

        ![Usar la configuración rápida](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image054.png)

### <a name="configure-sync-with-on-premises-domain-controller"></a>Configuración de la sincronización con el controlador de dominio local

1. En la pantalla **Conectar a Azure AD**, escriba el nombre de usuario y la contraseña del administrador global de Azure AD. Haga clic en  **Siguiente**. Este es el nombre de usuario `fabrikamazureadmin@hdifabrikam.com` que creó al configurar el inquilino de AD.
    ![Conectarse a Azure AD](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image058.png)
1. En la pantalla **Conectarse a Active Directory Domain Services**, escriba el nombre de usuario y la contraseña de una cuenta de administrador de empresa. Haga clic en  **Siguiente**. Este es el nombre de usuario `HDIFabrikam\HDIFabrikamAdmin` y la contraseña correspondiente que creó anteriormente.

   ![Conectarse a Active Directory Domain Services](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image060.png)
1. En la página **Configuración de inicio de sesión de Azure AD**, haga clic en **Siguiente**.
    ![Configuración de inicio de sesión de Azure AD](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image062.png)
1. En la pantalla Listo para configurar, haga clic en  **Instalar**.
    ![install](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image064.png)
1. Cuando aparezca **Configuración completa** en la pantalla, haga clic en **Salir**.
    ![Configuración completa](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image078.png)

1. Una vez completada la sincronización confirme si los usuarios que ha creado en la instancia de Active Directory de IAAS están sincronizados con Azure Active Directory.
    1. Inicie sesión en el Portal de Azure.
    1. Seleccione **Azure Active Directory** > **HDIFabrikam** > **Usuarios**.

### <a name="create-an-user-assigned-managed-identity"></a>Creación de una identidad administrada asignada por el usuario

Cree una identidad administrada asignada por el usuario que se usará para configurar Azure Active Directory Domain Services (Azure AD-DS). Para más información sobre la creación y el uso de identidades asignadas por el usuario, consulte [Creación, enumeración, eliminación o asignación de un rol a una identidad administrada asignada por el usuario mediante Azure Portal](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md).

1. Inicie sesión en el Portal de Azure.
1. Haga clic en **Crear un recurso** y escriba **identidad administrada**. Seleccione **Identidad administrada asignada por el usuario** > **Crear**.
1. Escriba **HDIFabrikamManagedIdentity** como **Nombre de recurso**.
1. Seleccione su suscripción.
1. En **Grupo de recursos** haga clic en **Crear nuevo** y escriba **HDIFabrikam-CentralUS**.
1. Seleccione **Centro de EE. UU.** en **Ubicación**.
1. Haga clic en **Create**(Crear).

![creación de una identidad administrada asignada por el usuario](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image082.png)

### <a name="enable-azure-active-directory-domain-services"></a>Habilitación de Azure Active Directory Domain Services

Para obtener más información, consulte [Habilitación de Azure Active Directory Domain Services mediante Azure Portal](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started).

1. Cree la red virtual para hospedar a Azure AD-DS. Ejecute el siguiente código de PowerShell.

    ```powershell
    Connect-AzAccount
    Get-AzSubscription
    Set-AzContext -Subscription 'SUBSCRIPTION_ID'
    $virtualNetwork = New-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-CentralUS' -Location 'Central US' -Name 'HDIFabrikam-AADDSVNET' -AddressPrefix 10.1.0.0/16
    $subnetConfig = Add-AzVirtualNetworkSubnetConfig -Name 'AADDS-subnet' -AddressPrefix 10.1.0.0/24 -VirtualNetwork $virtualNetwork
    $virtualNetwork | Set-AzVirtualNetwork
    ```

1. Inicie sesión en el Portal de Azure.
1. Haga clic en **Crear recurso**, escriba **Servicios de dominio** y seleccione **Azure AD Domain Services**.
1. En la pantalla **Datos básicos**, complete los siguientes pasos:
    1. En **Nombre de directorio** seleccione la instancia de Azure Active Directory que creó para este artículo, **HDIFabrikam**.
    1. En **Nombre de dominio DNS** escriba **HDIFabrikam.com**.
    1. Seleccione su suscripción.
    1. Especifique el grupo de recursos **HDIFabrikam-CentralUS** y en **Ubicación** seleccione **Centro de EE. UU.** .

        ![datos básicos de Azure AD DS](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image084.png)

1. En la pantalla **Red**, seleccione la red (**HDIFabrikam-VNET**) y la subred (**AADDS-subnet**) que creó con el script de PowerShell anterior. O bien puede usar la opción **Crear nueva** para crear una red virtual ahora.

    ![seleccionar red](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image086.png)

1. En la pantalla **Grupo de administradores**, aparecerá una notificación que le indica que un grupo denominado **Administradores de DC de AAD** ya se ha creado para administrar este grupo. Opcionalmente, puede modificar la pertenencia a este grupo, pero no es necesario para los pasos de este artículo. Haga clic en **OK**.

    ![ver grupo de administradores](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image088.png)

1. En la pantalla **Sincronización**, habilite la sincronización completa seleccionando **Todos** y, a continuación, haga clic en **Aceptar**.

    ![habilitar sincronización](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image090.png)

1. En la pantalla **Resumen**, compruebe los detalles de Azure AD-DS y haga clic en **Aceptar**.

    ![comprobar detalles](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image092.png)

1. Después de habilitar Azure AD DS, se ejecuta un servidor local del Servicio de nombres de dominio (DNS) en las máquinas virtuales (VM) de AD.

### <a name="configure-your-azure-ad-ds-virtual-network"></a>Configuración de la red virtual de Azure AD-DS

Los pasos descritos en esta sección le ayudarán a configurar la red virtual de Azure AD-DS (**HDIFabrikam-AADDSVNET**) para que use los servidores DNS personalizados.

1. Busque las direcciones IP de los servidores DNS personalizados. Haga clic en el recurso de AD-DS **HDIFabrikam.com**, haga clic en **Propiedades** en **Administrar**  y observe las direcciones IP que aparecen en **Dirección IP en la red virtual**.

    ![Buscar direcciones IP de DNS personalizados para Azure AD-DS](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image096.png)

1. Configure **HDIFabrikam-AADDSVNET** en las direcciones IP personalizadas `10.0.0.4` y `10.0.0.5`.

    1. Seleccione **Servidores DNS** en la categoría **Configuración** . A continuación, haga clic en el botón de radio al lado de la opción **Personalizado**, escriba la primera dirección IP (10.0.0.4) en el cuadro de texto y, a continuación, haga clic en **Guardar**.
    1. Agregue direcciones IP adicionales (10.0.0.5) siguiendo los mismos pasos.

1. En nuestro escenario, Azure AD-DS se configuró para usar las direcciones IP 10.0.0.4 y 10.0.0.5, configurando la misma dirección IP en la red virtual AADDS como se muestra en la imagen siguiente.

    ![ver los servidores dns personalizados](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image098.png)

## <a name="securing-ldap-traffic"></a>Protección del tráfico de LDAP

El protocolo ligero de acceso a directorios (LDAP) se usa para leer de Active Directory y escribir en él. Puede hacer que el tráfico de LDAP sea confidencial y seguro mediante la tecnología de Capa de sockets seguros (SSL) o Seguridad de la capa de transporte (TLS). Puede habilitar LDAP a través de SSL (LDAPS) mediante la instalación de un certificado con el formato correcto.

Para más información sobre LDAP seguro, consulte [Configuración de LDAP seguro (LDAPS) para un dominio administrado con Azure AD Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap).

En esta sección, cree un certificado autofirmado, descargue el certificado y configure LDAP seguro (LDAPS) para el dominio administrado de Azure AD DS **hdifabrikam**.

El script siguiente crea un certificado para hdifabrikam. El certificado se guarda en la ruta de acceso "LocalMachine".

> [!Note] 
> Cualquier utilidad o aplicación que crea una solicitud PKCS \#10 válida puede usarse para formar la solicitud de certificado SSL.

```powershell
$lifetime = Get-Date
New-SelfSignedCertificate -Subject hdifabrikam.com `
-NotAfter $lifetime.AddDays(365) -KeyUsage DigitalSignature, KeyEncipherment `
-Type SSLServerAuthentication -DnsName *.hdifabrikam.com, hdifabrikam.com
```

Compruebe que el certificado está instalado en el almacén Personal del equipo. Complete los siguientes pasos:

1. Inicie Microsoft Management Console (MMC).
1. Agregue el complemento Certificados que administra certificados en el equipo local.
1. Expanda **Certificados (equipo local)** , expanda **Personal** y, a continuación, expanda **Certificados**. Debe existir un nuevo certificado en el almacén Personal. Este certificado se emite para el nombre de host completo.

    ![comprobar creación de certificados](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image102.png)

1. En el panel derecho, haga clic con el botón derecho en el certificado que creó en el paso anterior, elija **Todas las tareas** y, a continuación, haga clic en **Exportar**.

1. En la página  **Exportar la clave privada** , haga clic en **Sí, exportar la clave privada**. La clave privada es necesaria para los mensajes cifrados que se leerán desde el equipo donde se importará la clave.

    ![exportar clave privada](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image103.png)

1. En la página **Exportar formato de archivo** , deje la configuración predeterminada y, a continuación, haga clic en **Siguiente**. 
1. En la página **Contraseña** , escriba una contraseña para la clave privada, seleccione **TripleDES-SHA1** para **Cifrado** y haga clic en **Siguiente**.
1. En la página **Archivo que se va a exportar** , escriba la ruta y el nombre del archivo de certificado exportado y haga clic en **Siguiente**.
1. El nombre del archivo tiene que tener la extensión .pfx, este archivo se configura en Azure Portal para establecer una conexión segura.
1. Habilite LDAP seguro (LDAPS) para un dominio administrado de Azure AD Domain Services.
    1. Seleccione el dominio **HDIFabrikam.com** desde Azure Portal.
    1. Haga clic en **LDAP seguro** en **Administrar**.
    1. En la pantalla **LDAP seguro**, haga clic en **Habilitar** que aparece debajo de **LDAP seguro**.
    1. Busque el archivo de certificado .pfx que exportó en el equipo.
    1. Escriba la contraseña del certificado.

    ![habilitar ldap seguro](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image113.png)

1. Ahora que ha habilitado el protocolo LDAP seguro, asegúrese de que sea accesible habilitando el puerto 636.
    1. Haga clic en el grupo de seguridad de red **AADDS-HDIFabrikam.com-NSG** en el grupo de recursos **HDIFabrikam-CentralUS**.
    1. En **Configuración**, haga clic en **Reglas de seguridad de entrada** > **Agregar**.
    1. En la pantalla **Agregar regla de seguridad de entrada**, especifique las siguientes propiedades y haga clic en **Agregar**:

        | Propiedad | Valor |
        |---|---|
        | Origen | Cualquiera |
        | Intervalos de puertos de origen | * |
        | Destino | Cualquiera |
        | Intervalo de puertos de destino | 636 |
        | Protocolo | Cualquiera |
        | . | PERMITIR |
        | Prioridad | \<Número deseado\> |
        | NOMBRE | Port_LDAP_636 |

    ![regla de seguridad de entrada](./media/apache-domain-joined-create-configure-enterprise-security-cluster/add-inbound-security-rule.png)

1. `HDIFabrikamManagedIdentity` es la identidad administrada asignada por el usuario, el rol de colaborador de Domain Services para HDInsight se habilita para la identidad administrada que habilitará esta identidad para leer, crear, modificar y eliminar las operaciones de Domain Services.

    ![crear una identidad administrada asignada por el usuario](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image117.png)

## <a name="creating-enterprise-security-package-enabled-hdinsight-cluster"></a>Creación de un clúster de HDInsight con Enterprise Security Package habilitado

Este paso requiere los siguientes requisitos previos:

1. Cree un grupo de recursos `HDIFabrikam-WestUS` en la ubicación `West US`.
1. Cree una red virtual que hospedará el clúster de HDInsight habilitado para ESP.

    ```powershell
    $virtualNetwork = New-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-WestUS' -Location 'West US' -Name 'HDIFabrikam-HDIVNet' -AddressPrefix 10.1.0.0/16
    $subnetConfig = Add-AzVirtualNetworkSubnetConfig -Name 'SparkSubnet' -AddressPrefix 10.1.0.0/24 -VirtualNetwork $virtualNetwork
    $virtualNetwork | Set-AzVirtualNetwork
    ```

1. Cree una relación del mismo nivel entre la red virtual que hospeda AADDS (`HDIFabrikam-AADDSVNET`) y la red virtual que hospedará el clúster de HDInsight habilitado para ESP (`HDIFabrikam-HDIVNet`). Use el siguiente código de PowerShell para emparejar estas dos redes virtuales.

    ```powershell
    Add-AzVirtualNetworkPeering -Name 'HDIVNet-AADDSVNet' -RemoteVirtualNetworkId (Get-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-CentralUS').Id -VirtualNetwork (Get-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-WestUS')

    Add-AzVirtualNetworkPeering -Name 'AADDSVNet-HDIVNet' -RemoteVirtualNetworkId (Get-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-WestUS').Id -VirtualNetwork (Get-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-CentralUS')
    ```

1. Cree una nueva cuenta de Azure Data Lake Storage Gen2, **Hdigen2store**, que esté configurada con la identidad administrada del usuario **HDIFabrikamManagedIdentity**. Para más información sobre la creación de cuentas de Data Lake Storage Gen2 habilitadas con identidades administradas de usuarios, consulte [Uso de Azure Data Lake Storage Gen2 con clústeres de Azure HDInsight](../hdinsight-hadoop-use-data-lake-storage-gen2.md).

1. Configure DNS personalizado en la red virtual **HDIFabrikam AADDSVNET**.
    1. Vaya a Azure Portal > **Grupos de recursos** > **OnPremADVRG** > **HDIFabrikam-AADDSVNET** > **Servidores DNS**.
    1. Seleccione **Personalizado** y escriba `10.0.0.4` y `10.0.0.5`.
    1. Haga clic en **Save**(Guardar).

        ![guardar configuración de DNS personalizado](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image123.png)

1. Cree un nuevo clúster de Spark en HDInsight habilitado para ESP.
    1. Haga clic en **Personalizado (tamaño, configuración, aplicaciones)** .
    2. Escriba los detalles que desee en la sección 1 **Aspectos básicos**. Asegúrese de que el **tipo de clúster** es **Spark 2.3 (HDI 3.6)** y el **grupo de recursos** es **HDIFabrikam-CentralUS**

    1. En la sección 2 **Seguridad y redes**, complete los pasos siguientes:
        1. Haga clic en **Habilitado** en **Enterprise Security Package**.
        1. Haga clic en **Usuario administrador de clúster** y seleccione la cuenta **HDIAdmin** que creó anteriormente como el usuario administrador local. Haga clic en **Seleccionar**.

        1. Haga clic en **Grupo de acceso de clúster** y, a continuación, seleccione **HDIUserGroup**. Cualquier usuario que agregue a este grupo en el futuro podrá acceder a los clústeres de HDInsight.

            ![seleccionar grupo de acceso de clúster](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image129.jpg)

    1. Complete los demás pasos de la configuración del clúster y compruebe los detalles en **Resumen del clúster**. Haga clic en **Create**(Crear).

1. Inicie sesión en la interfaz de usuario de Ambari para el clúster recién creado en `https://CLUSTERNAME.azurehdinsight.net` mediante su nombre de usuario administrador `hdiadmin@hdifabrikam.com` y la contraseña.

    ![iniciar sesión en Ambari](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image135.jpg)

1. Haga clic en **Roles** en el panel del clúster.
1. En la página **Roles**, escriba el grupo **hdiusergroup** para asignarlo al rol **Administrador de clústeres** en **Assign roles to these** (Asignar roles a estos).

    ![asignar rol Administrador de clústeres a hdiusergroup](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image137.jpg)

1. Abra el cliente SSH e inicie sesión en el clúster con el usuario **hdiuser** que creó anteriormente en la instancia de Active Directory local.

    ![iniciar sesión en el clúster con SSH](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image139.jpg)

Si puede iniciar sesión con esta cuenta, ha configurado el clúster de ESP correctamente para la sincronización con la instancia de Active Directory local.

## <a name="next-steps"></a>Pasos siguientes

* [Introducción a la seguridad de Apache Hadoop con Enterprise Security Package](hdinsight-security-overview.md)
