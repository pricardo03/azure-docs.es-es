---
title: 'Tutorial: Configuración de Workday para el aprovisionamiento automático de usuarios con Azure Active Directory | Microsoft Docs'
description: Obtenga información sobre cómo configurar Azure Active Directory para aprovisionar y cancelar automáticamente el aprovisionamiento de cuentas de usuario de Workday.
services: active-directory
author: cmmdesai
documentationcenter: na
manager: mtillman
ms.assetid: 1a2c375a-1bb1-4a61-8115-5a69972c6ad6
ms.service: active-directory
ms.component: saas-app-tutorial
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/18/2018
ms.author: chmutali
ms.openlocfilehash: 30354ddb010c22dabe5cd69373ae59daaf4a8b46
ms.sourcegitcommit: 96527c150e33a1d630836e72561a5f7d529521b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2018
ms.locfileid: "51346752"
---
# <a name="tutorial-configure-workday-for-automatic-user-provisioning-preview"></a>Tutorial: Configuración de Workday para el aprovisionamiento automático de usuarios (versión preliminar)

El objetivo de este tutorial consiste en mostrarle los pasos que debe llevar a cabo para importar perfiles de trabajo de Workday a Active Directory y Azure Active Directory, con la reescritura opcional de la dirección de correo electrónico en Workday.

## <a name="overview"></a>Información general

El [servicio de aprovisionamiento de usuarios de Azure Active Directory](../manage-apps/user-provisioning.md) se integra con la [API de recursos humanos de Workday](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html) para poder aprovisionar cuentas de usuario. Azure AD usa esta conexión para habilitar los siguientes flujos de trabajo de aprovisionamiento de usuarios:

* **Aprovisionamiento de usuarios en Active Directory**: sincronice los conjuntos de usuarios seleccionados de Workday con uno o varios dominios de Active Directory.

* **Aprovisionamiento de usuarios solo de nube a Azure Active Directory**: en escenarios en los que no se utiliza Active Directory local, los usuarios pueden aprovisionarse directamente desde Workday a Azure Active Directory mediante el servicio de aprovisionamiento de usuarios de Azure AD. 

* **Reescritura de las direcciones de correo electrónico en Workday**: el servicio de aprovisionamiento de usuarios de Azure AD puede reescribir las direcciones de correo electrónico de los usuarios de Azure AD seleccionados en Workday. 

### <a name="what-human-resources-scenarios-does-it-cover"></a>¿Qué escenarios de recursos humanos se tratan?

El flujo de trabajo de aprovisionamiento de usuarios de Workday compatible con el servicio de aprovisionamiento de usuarios de Azure AD permite la automatización de los siguientes escenarios de recursos humanos y de administración del ciclo de vida de identidades:

* **Contratación de nuevos empleados**: cuando se agrega un nuevo empleado a Workday, se crea automáticamente una cuenta de usuario en Active Directory, Azure Active Directory y, opcionalmente, en Office 365 y en [otras aplicaciones SaaS compatibles con Azure AD](../manage-apps/user-provisioning.md), con reescritura de la dirección de correo electrónico en Workday.

* **Actualizaciones de atributos y de perfil de los empleados**: si se actualiza un registro de empleado en Workday (por ejemplo, el nombre, el cargo o el administrador), su cuenta de usuario se actualizará automáticamente en Active Directory, Azure Active Directory y, opcionalmente, en Office 365 y en [otras aplicaciones SaaS compatibles con Azure AD](../manage-apps/user-provisioning.md).

* **Ceses de empleados**: cuando se prescinde de un empleado en Workday, su cuenta de usuario se deshabilita automáticamente de Active Directory, Azure Active Directory y, opcionalmente, de Office 365 y de [otras aplicaciones SaaS compatibles con Azure AD](../manage-apps/user-provisioning.md).

* **Recontrataciones de empleados**: cuando se vuelve a contratar a un empleado en Workday, se puede reactivar o volver a aprovisionar automáticamente su cuenta antigua (en función de sus preferencias) en Active Directory, Azure Active Directory y, opcionalmente, en Office 365 y en [otras aplicaciones SaaS compatibles con Azure AD](../manage-apps/user-provisioning.md).

### <a name="who-is-this-user-provisioning-solution-best-suited-for"></a>¿Para quién es más adecuada esta solución de aprovisionamiento de usuarios?

Esta solución de aprovisionamiento de usuarios de Workday se encuentra actualmente en versión preliminar pública y resulta adecuada para:

* Organizaciones que desean una solución basada en la nube precompilada para el aprovisionamiento de usuarios de Workday

* Organizaciones que necesitan aprovisionamiento de usuarios directo de Workday a Active Directory, o a Azure Active Directory

* Organizaciones que necesitan aprovisionamiento de usuarios mediante los datos obtenidos del módulo HCM de Workday (consulte [Get_Workers](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html))

* Organizaciones que necesitan combinar y mover usuarios y dejarles que se sincronicen con uno o varios bosques, dominios o unidades organizativas de Active Directory en función de únicamente la información de cambio detectada en el módulo HCM de Workday (consulte [Get_Workers](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html))

* Organizaciones que usan Office 365 para el correo electrónico

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-hybrid-note.md)]

## <a name="solution-architecture"></a>Arquitectura de la solución

En esta sección se describe la arquitectura de la solución de aprovisionamiento de usuarios de un extremo a otro para entornos híbridos comunes. Hay dos flujos relacionados:

* **Flujo de datos de RR.HH. relevante, desde Workday a Active Directory local:** en este flujo de trabajo (por ejemplo, nuevas contrataciones, transferencias o finalizaciones), los eventos se producen en primer lugar en el inquilino de la nube de RR.HH. de Workday y, a continuación, los datos de eventos fluyen hacia Active Directory local mediante Azure AD y el agente de aprovisionamiento. En función del evento, puede dar lugar a las operaciones de creación, actualización, habilitación o deshabilitación en AD.
* **Flujo de escritura diferida de correo electrónico desde un entorno local de Active Directory a Workday:** una vez completada la creación de cuentas en Active Directory, se sincronizan con Azure AD mediante Azure AD Connect y el atributo de correo electrónico con origen en Active Directory puede escribirse de forma diferida en Workday.

![Información general](./media/workday-inbound-tutorial/wd_overview.png)

### <a name="end-to-end-user-data-flow"></a>Flujo de datos de usuarios de un extremo a otro

1. El equipo de RR.HH. realiza las transacciones de trabajo (empleados que se incorporan a la empresa, se trasladan o la se van, o nuevas contrataciones, transferencias o terminaciones de contratos) en Workday HCM
2. El servicio de aprovisionamiento de Azure AD ejecuta las sincronizaciones programadas de las identidades de RR.HH. de Workday e identifica los cambios que deben procesarse para la sincronización con Active Directory local.
3. El servicio de aprovisionamiento de Azure AD invoca al agente de aprovisionamiento de AAD Connect local con una carga de solicitud que contiene las operaciones de creación, actualización, habilitación o deshabilitación de las cuentas de AD.
4. El agente de aprovisionamiento de Azure AD Connect usa una cuenta de servicio para agregar o actualizar los datos de la cuenta de AD.
5. El motor de Azure AD Connect / AD Sync ejecuta una sincronización diferencial para extraer las actualizaciones de AD.
6. Las actualizaciones de Active Directory se sincronizan con Azure Active Directory.
7. Si se configura el conector de escritura en diferido de Workday, escribe en diferido el atributo de correo electrónico en Workday, según el atributo coincidente utilizado.


## <a name="planning-your-deployment"></a>Planeamiento de la implementación

Antes de iniciar la integración de Workday, compruebe los requisitos previos siguientes y lea las siguientes instrucciones sobre cómo ajustar su arquitectura actual de Active Directory y los requisitos de aprovisionamiento de usuarios con las soluciones proporcionadas por Azure Active Directory.

### <a name="prerequisites"></a>Requisitos previos

En la situación descrita en este tutorial se supone que ya cuenta con los elementos siguientes:

* Una suscripción válida a Azure AD Premium P1 con acceso de administrador global
* Un inquilino de implementación de Workday para fines de pruebas y de integración
* Permisos de administrador en Workday para crear un usuario de integración del sistema y efectuar cambios para probar los datos de los empleados con fines de pruebas
* Para el aprovisionamiento de usuarios en Active Directory, se necesita un servidor que ejecute Windows Server 2012 o una versión posterior con el entorno de ejecución .NET 4.7+ a fin de hospedar el [agente de aprovisionamiento local](https://go.microsoft.com/fwlink/?linkid=847801)
* [Azure AD Connect](../hybrid/whatis-hybrid-identity.md) para la sincronización entre Active Directory y Azure AD.

### <a name="planning-considerations"></a>Consideraciones de planeación

Azure AD ofrece una amplia variedad de conectores de aprovisionamiento para que pueda resolver el aprovisionamiento y la administración del ciclo de vida de identidades de Workday a Active Directory, Azure AD, aplicaciones SaaS, etc. Las características que va a usar y la configuración que aplicará a la solución variarán según el entorno y los requisitos de su organización. Como primer paso, valore cuántos de los siguientes elementos están presentes e implementados en su organización:

* ¿Cuántos bosques de Active Directory están en uso?
* ¿Cuántos dominios de Active Directory están en uso?
* ¿Cuántas unidades organizativas (UO) de Active Directory están en uso?
* ¿Cuántos inquilinos de Azure Active Directory están en uso?
* ¿Hay usuarios que necesiten aprovisionarse en Active Directory y en Azure Active Directory (por ejemplo, los usuarios "híbridos")?
* ¿Hay usuarios que necesiten aprovisionarse en Azure Active Directory pero no en Active Directory (por ejemplo, los usuarios que solo están en la nube)?
* ¿Las direcciones de correo electrónico de los usuarios se tienen que reescribir en Workday?

Una vez que tenga respuesta a estas preguntas, podrá planear la implementación de aprovisionamiento de Workday siguiendo las instrucciones que se detallan a continuación.

#### <a name="planning-deployment-of-aad-connect-provisioning-agent"></a>Planeamiento de la implementación del agente de aprovisionamiento de AAD Connect

La solución de aprovisionamiento de usuarios para AD de Workday requiere la implementación de uno o varios agentes de aprovisionamiento en servidores que ejecutan Windows 2012 R2 o posterior con, al menos, 4 GB de RAM y el motor de ejecución .NET 4.7+. Las siguientes consideraciones deben tenerse en cuenta antes de instalar el agente de aprovisionamiento:

* Asegúrese de que el servidor host que ejecuta el agente de aprovisionamiento tiene acceso de red para el dominio de AD de destino.
* El Asistente para configuración del agente de aprovisionamiento registra el agente con el inquilino de Azure AD y el proceso de registro requiere acceso a *.msappproxy.net en el puerto 8082. Asegúrese de que las reglas de firewall de salida que permiten esta comunicación están en vigor.
* El agente de aprovisionamiento usa una cuenta de servicio para comunicarse con los dominios de AD de la red local. Antes de la instalación del agente, se recomienda que cree una cuenta de servicio con permisos de lectura/escritura de propiedades de usuario y una contraseña que no expire.  
* Durante la configuración del agente de aprovisionamiento, puede seleccionar los controladores de dominio que deben controlar las solicitudes de aprovisionamiento. Si tiene varios controladores de dominio distribuidos geográficamente, instale al agente de aprovisionamiento en el mismo sitio que los controladores de dominio preferidos para mejorar la confiabilidad y el rendimiento de la solución de un extremo a otro.
* Para lograr una alta disponibilidad, puede implementar más de un agente de aprovisionamiento y registrarlos para controlar el mismo conjunto de dominios de Active Directory locales.

> [!IMPORTANT]
> En entornos de producción, Microsoft recomienda que tenga un mínimo de tres agentes de aprovisionamiento configurados con el inquilino de Azure AD para lograr una alta disponibilidad.

#### <a name="selecting-provisioning-connector-apps-to-deploy"></a>Selección de aplicaciones de conectores de aprovisionamiento que implementar

Al integrar Workday y Active Directory, hay varios sistemas de origen y de destino que se deben tener en cuenta:

| Sistema de origen | Sistema de destino | Notas |
| ---------- | ---------- | ---------- |
| Workday | Dominio de Active Directory | Cada dominio se trata como un sistema de destino distinto |
| Workday | Inquilino de Azure AD | Según sea necesario para los usuarios que solo están en la nube |
| Bosque de Active Directory | Inquilino de Azure AD | Actualmente, este flujo está controlado por AAD Connect |
| Inquilino de Azure AD | Workday | Para la escritura diferida de direcciones de correo electrónico |

Para facilitar el aprovisionamiento de flujos de trabajo entre Workday y Active Directory, Azure AD proporciona varias aplicaciones de conectores de aprovisionamiento que puede agregar desde la galería de aplicaciones de Azure AD:

![Galería de aplicaciones de AAD](./media/workday-inbound-tutorial/wd_gallery.png)

* **Aprovisionamiento de Workday en Active Directory**: esta aplicación facilita el aprovisionamiento de cuentas de usuario desde Workday a un dominio de Active Directory. Si tiene varios dominios, puede agregar una instancia de esta aplicación desde la galería de aplicaciones de Azure AD para cada dominio de Active Directory en el que deba efectuar un aprovisionamiento.

* **Aprovisionamiento de Workday en Azure AD**: mientras AAD Connect es la herramienta que se debe usar para sincronizar los usuarios de Active Directory con Azure Active Directory, esta aplicación se puede usar para facilitar el aprovisionamiento de usuarios que solo están en la nube desde Workday a un inquilino de Azure Active Directory.

* **Escritura diferida de Workday**: esta aplicación facilita la escritura diferida de direcciones de correo electrónico del usuario de Azure Active Directory a Workday.

> [!TIP]
> La aplicación "Workday" normal se usa para configurar el inicio de sesión único entre Workday y Azure Active Directory. 

#### <a name="determine-workday-to-ad-user-attribute-mapping-and-transformations"></a>Determinación de la asignación de atributos de usuario de Workday para AD y las transformaciones

Antes de configurar el aprovisionamiento de usuarios en un dominio de Active Directory, tenga en cuenta las siguientes preguntas. Las respuestas a estas preguntas determinarán cómo deben establecerse los filtros de ámbito y las asignaciones de atributos.

* **¿Qué usuarios de Workday se deben aprovisionar en este bosque de Active Directory?**

   * *Ejemplo: usuarios donde el atributo "Company" de Workday contenga el valor "Contoso" y el atributo "Worker_Type" contenga "Regular"*

* **¿Cómo se enrutan los usuarios a diferentes unidades organizativas (OU)?**

   * *Ejemplo: los usuarios se enrutan a las unidades organizativas que corresponden a una ubicación de oficina, como se define en los atributos "Municipality" y "Country_Region_Reference" de Workday*

* **¿Cómo se deben rellenar los atributos siguientes en Active Directory?**

   * Nombre común (cn)
      * *Ejemplo: use el valor User_ID de Workday establecido por Recursos Humanos*
      
   * Identificador de empleado (employeeId)
      * *Ejemplo: use el valor Worker_ID de Workday*
      
   * Nombre de cuenta SAM (sAMAccountName)
      * *Ejemplo: use el valor User_ID de Workday, que se filtra mediante una expresión de aprovisionamiento de Azure AD para quitar caracteres incorrectos*
      
   * Nombre principal de usuario (userPrincipalName)
      * *Ejemplo: use el valor User_ID de Workday con una expresión de aprovisionamiento de Azure AD para anexar un nombre de dominio*

* **¿Cómo se hacen coincidir los usuarios de Workday y Active Directory?**

  * *Ejemplo: los usuarios con un valor "Worker_ID" específico de Workday se hacen coincidir con los usuarios de Active Directory donde "employeeID" tenga el mismo valor. Si el valor Worker_ID no se encuentra en Active Directory, cree un nuevo usuario.*
  
* **¿El bosque de Active Directory ya contiene los identificadores de usuario necesarios para que funcione la lógica de coincidencia?**

  * *Ejemplo: si es una nueva implementación de Workday, se recomienda firmemente rellenar previamente Active Directory con los valores Worker_ID correctos de Workday (o un valor de identificación único de su elección) para mantener la lógica de coincidencia lo más simple posible.*



El modo de instalación y configuración de estas aplicaciones de conectores de aprovisionamiento especiales es el tema de las secciones restantes de este tutorial. Las aplicaciones que elija para la configuración dependerán de los sistemas que deba aprovisionar y de la cantidad de dominios de Active Directory y de inquilinos de Azure AD que haya en su entorno.



## <a name="configure-integration-system-user-in-workday"></a>Configuración de un usuario de integración en Workday

Un requisito común de todos los conectores de aprovisionamiento de Workday es que requieren credenciales para una cuenta de integración de sistemas en Workday para poder conectarse a la API de recursos humanos de Workday. En esta sección se describe cómo crear un usuario de integración de sistemas en Workday.

> [!NOTE]
> Se puede omitir este procedimiento y usar una cuenta de administrador global de Workday como cuenta de integración de sistemas. Puede funcionar correctamente en las demostraciones, pero no se recomienda en las implementaciones de producción.

### <a name="create-an-integration-system-user"></a>Creación de un usuario del sistema de integración

**Para crear un usuario del sistema de integración:**

1. Inicie sesión en su inquilino de Workday usando una cuenta de administrador. En **Workday Aplication** (Aplicación de Workday), escriba create user (crear usuario) en el cuadro de búsqueda y haga clic en **Create Integration System User** (Crear usuario del sistema de integración).

    ![Creación de usuarios](./media/workday-inbound-tutorial/wd_isu_01.png "Creación de usuarios")
2. Para completar la tarea **Create Integration System User** (Crear usuario del sistema de integración), especifique el nombre de usuario y la contraseña del nuevo usuario del sistema de integración.  
 * Deje la opción **Require New Password at Next Sign In** (Solicitar una nueva contraseña en el siguiente inicio de sesión) sin activar, ya que este usuario iniciará sesión mediante programación.
 * Deje **Session Timeout Minutes** (Minutos de tiempo de espera de la sesión) en 0 (su valor predeterminado), con el fin de evitar que las sesiones del usuario agoten el tiempo de espera de manera prematura.
 * Seleccione la opción **Do Not Allow UI Sessions** (No permitir sesiones de la interfaz de usuario) ya que proporciona una capa adicional de seguridad que impide que un usuario con la contraseña del sistema de integración inicie sesión en Workday. 

    ![Crear usuario de sistema de integración](./media/workday-inbound-tutorial/wd_isu_02.png "Crear usuario de sistema de integración")

### <a name="create-a-security-group"></a>Creación de un grupo de seguridad
En este paso, creará un grupo de seguridad del sistema de integración sin restricciones en Workday y asignará a este grupo el usuario del sistema de integración creado en el paso anterior.

**Para crear un grupo de seguridad:**

1. Escriba create security group (crear grupo de seguridad) en el cuadro de búsqueda y luego haga clic en **Create Security Group**(Crear grupo de seguridad).

    ![Crear grupo de seguridad](./media/workday-inbound-tutorial/wd_isu_03.png "Crear grupo de seguridad")
2. Complete la tarea **Create Security Group** (Crear grupo de seguridad).  
   * En la lista desplegable **Type of Tenanted Security Group** (Tipo de grupo de seguridad con inquilinos), seleccione **Integration System Security Group—Unconstrained** (Grupo de seguridad del sistema de integración: sin restringir).

    ![Crear grupo de seguridad](./media/workday-inbound-tutorial/wd_isu_04.png "Crear grupo de seguridad")

3. Una vez que se realice correctamente la creación del grupo de seguridad, verá una página donde puede asignar miembros al grupo de seguridad. Agregue el nuevo usuario del sistema de integración a este grupo de seguridad y seleccione el ámbito de organización apropiado.
![Editar grupo de seguridad](./media/workday-inbound-tutorial/wd_isu_05.png "Editar grupo de seguridad")
 
### <a name="configure-domain-security-policy-permissions"></a>Configuración de permisos de directiva de seguridad de dominio
En ese paso, concederá al grupo de seguridad los permisos de directiva "seguridad de dominio" para los datos de los empleados.

**Para configurar permisos de directiva de seguridad de dominio:**

1. Escriba **Domain Security Configuration** (Configuración de seguridad de dominio) en el cuadro de búsqueda y, a continuación, haga clic en el vínculo **Domain Security Configuration Report** (Informe de configuración de seguridad de dominio).  

    ![Directivas de seguridad de dominio](./media/workday-inbound-tutorial/wd_isu_06.png "Directivas de seguridad de dominio")  
2. En el cuadro de texto **Dominio**, busque los siguientes dominios y agréguelos al filtro de uno en uno.  
   * *External Account Provisioning* (Aprovisionamiento de cuentas externas)
   * *Worker Data: Public Worker Reports* (Datos de empleado: informes de trabajadores públicos)
   * *Person Data: Work Contact Information* (Datos personales: información de contacto de trabajo)
   * *Worker Data: All Positions* (Datos de empleado: todos los cargos)
   * *Worker Data: Current Staffing Information* (Datos de empleado: información de plantilla actual)
   * *Worker Data: Business Title on Worker Profile* (Datos de empleado: cargo empresarial en el perfil del trabajador)
 
    ![Directivas de seguridad de dominio](./media/workday-inbound-tutorial/wd_isu_07.png "Directivas de seguridad de dominio")  

    ![Directivas de seguridad de dominio](./media/workday-inbound-tutorial/wd_isu_08.png "Directivas de seguridad de dominio") 

    Haga clic en **OK**.

3. En el informe que se muestra, seleccione el botón de puntos suspensivos (...) que aparece junto a **External Account Provisioning** (Aprovisionamiento de cuentas externas) y haga clic en la opción de menú **Domain -> Edit Security Policy Permissions**  (Dominio -> Editar permisos de directiva de seguridad)

    ![Directivas de seguridad de dominio](./media/workday-inbound-tutorial/wd_isu_09.png "Directivas de seguridad de dominio")  

4. En la página **Edit Domain Security Policy Permissions** (Editar permisos de directiva de seguridad de dominio), desplácese hacia abajo hasta la sección **Integration Permissions** (Permisos de integración). Haga clic en el signo "+" para agregar el grupo de sistema de integración a la lista de grupos de seguridad con los permisos de integración **Get** (obtención) y **Put** (colocación).

    ![Editar permiso](./media/workday-inbound-tutorial/wd_isu_10.png "Editar permiso")  

5. Haga clic en el signo "+" para agregar el grupo de sistema de integración a la lista de grupos de seguridad con los permisos de integración **Get** (Obtención) y **Put** (Colocación).

    ![Editar permiso](./media/workday-inbound-tutorial/wd_isu_11.png "Editar permiso")  

6. Repita los pasos del 3 al 5 anteriores con cada una de las directivas de seguridad restantes:

   | Operación | Directiva de seguridad de dominio |
   | ---------- | ---------- | 
   | Obtener y poner | Worker Data: Public Worker Reports |
   | Obtener y poner | Person Data: Work Contact Information (Datos personales: información de contacto de trabajo) |
   | Obtener | Worker Data: All Positions |
   | Obtener | Worker Data: Current Staffing Information |
   | Obtener | Worker Data: Business Title on Worker Profile |

### <a name="configure-business-process-security-policy-permissions"></a>Configuración de los permisos de directiva de seguridad del proceso de negocio
En ese paso, concederá permisos de directiva "seguridad de proceso de negocio" al grupo de seguridad para los datos de trabajo. Esto es necesario para configurar el conector de aplicaciones de escritura diferida de Workday. 

**Para configurar los permisos de directiva de seguridad del proceso de negocio:**

1. Escriba **Business Process Policy** (Directiva de proceso de negocio) en el cuadro de búsqueda y, a continuación, haga clic en la tarea **Edit Business Process Security Policy** (Editar directiva de seguridad del proceso de negocio) del vínculo.  

    ![Directivas de seguridad del proceso de negocio](./media/workday-inbound-tutorial/wd_isu_12.png "Business Process Security Policies")  

2. En el cuadro de texto **Business Process Type** (Tipo de proceso de negocio), busque *póngase en contacto con* y seleccione el proceso de negocio **Contact Change** (Cambio de contacto) y haga clic en **OK** (Aceptar).

    ![Directivas de seguridad del proceso de negocio](./media/workday-inbound-tutorial/wd_isu_13.png "Business Process Security Policies")  

3. En la página **Edit Business Process Security Policy** (Editar directiva de seguridad del proceso de negocio), desplácese a la sección **Maintain Contact Information (Web Service)** [Mantener información de contacto (servicio web)].

    ![Directivas de seguridad del proceso de negocio](./media/workday-inbound-tutorial/wd_isu_14.png "Business Process Security Policies")  

4. Seleccione y agregue el nuevo grupo de seguridad del sistema de integración a la lista de grupos de seguridad que pueden iniciar la solicitud de servicios web. Haga clic en **Done** (Acabado). 

    ![Directivas de seguridad del proceso de negocio](./media/workday-inbound-tutorial/wd_isu_15.png "Business Process Security Policies")  

 
### <a name="activate-security-policy-changes"></a>Activación de cambios en directivas de seguridad

**Para activar cambios en directiva de seguridad:**

1. Escriba activate en el cuadro de búsqueda y, a continuación, haga clic en el vínculo **Activate Pending Security Policy Changes** (Activar cambios en la directiva de seguridad pendientes).

    ![Activar](./media/workday-inbound-tutorial/wd_isu_16.png "Activar") 
2. Inicie la tarea Activate Pending Security Policy Changes (Activar cambios en la directiva de seguridad pendientes), para ello tiene que escribir un comentario para fines de auditoría y luego hacer clic en el botón **Aceptar**. 

    ![Activar seguridad pendiente](./media/workday-inbound-tutorial/wd_isu_17.png "Activar seguridad pendiente")  
1. Complete la tarea que aparece en la pantalla siguiente; para ello, active la casilla etiquetada como **Confirm** (Confirmar) y luego haga clic en **OK** (Aceptar).

    ![Activar seguridad pendiente](./media/workday-inbound-tutorial/wd_isu_18.png "Activar seguridad pendiente")  

## <a name="configuring-user-provisioning-from-workday-to-active-directory"></a>Configuración del aprovisionamiento de usuarios de Workday a Active Directory

Siga estas instrucciones para configurar el aprovisionamiento de cuentas de usuario de Workday para cada dominio de Active Directory dentro del ámbito de su integración.

### <a name="part-1-install-and-configure-on-premises-provisioning-agents"></a>Parte 1: Instalación y configuración de agentes de aprovisionamiento en el entorno local

Para aprovisionar en Active Directory local, debe haber un agente instalado en un servidor que tenga .NET 4.7+ Framework y acceso de red al dominio o dominios de Active Directory deseados.

> [!TIP]
> Puede comprobar la versión de la plataforma de .NET en el servidor mediante las instrucciones proporcionadas [aquí](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed).
> Si el servidor no tiene instalado .NET 4.7 o posterior, puede descargarlo [aquí](https://support.microsoft.com/help/3186497/the-net-framework-4-7-offline-installer-for-windows).  

Una vez que ha implementado .NET 4.7+, puede descargar el **[agente de aprovisionamiento local aquí](https://go.microsoft.com/fwlink/?linkid=847801)**  y seguir los pasos indicados a continuación para llevar a cabo la configuración del agente.

1. Inicie sesión en el servidor Windows Server donde desee instalar el nuevo agente.
2. Inicie el instalador del agente de aprovisionamiento, acepte los términos y haga clic en el botón **Instalar**.
![Pantalla Instalar](./media/workday-inbound-tutorial/pa_install_screen_1.png "Install Screen")

3. Una vez completada la instalación, se iniciará el asistente y verá la pantalla **Conectar a Azure AD**. Haga clic en el botón **Autenticar** para conectarse a su instancia de Azure AD.
![Conectar a Azure AD](./media/workday-inbound-tutorial/pa_install_screen_2.png "Connect Azure AD")

4. Autentíquese en su instancia de Azure AD con las credenciales de administrador global. 
![Autenticación de administrador](./media/workday-inbound-tutorial/pa_install_screen_3.png "Admin Auth")

5. Tras una autenticación correcta con Azure AD, verá la pantalla **Conectar a Active Directory**. En este paso, escriba su nombre de dominio de AD y haga clic en el botón **Agregar directorio**.
![Agregar directorio](./media/workday-inbound-tutorial/pa_install_screen_4.png "Add Directory")

6. Ahora se pedirá que escriba las credenciales necesarias para conectarse al dominio de AD. En la misma pantalla, puede usar la opción **Select domain controller priority** (Seleccionar prioridad del controlador de dominio) para especificar los controladores de dominio que el agente debe utilizar para enviar las solicitudes de aprovisionamiento.
![Credenciales de dominio](./media/workday-inbound-tutorial/pa_install_screen_5.png "Domain Creds")

7. Después de configurar el dominio, el instalador muestra una lista de los dominios configurados. En esta pantalla, puede repetir los pasos 5 y 6 para agregar más dominios, o hacer clic en **Siguiente** para continuar con el registro del agente. 
![Dominios configurados](./media/workday-inbound-tutorial/pa_install_screen_6.png "Configured Domains")

   > [!NOTE]
   > Si tiene varios dominios de AD (por ejemplo, na.contoso.com o emea.contoso.com), agregue cada dominio individualmente a la lista. No basta con agregar el dominio primario (por ejemplo, contoso.com) y se recomienda que registre cada dominio secundario con el agente. 

8. Revise los detalles de configuración y haga clic en **Confirmar** para registrar el agente. 
![Pantalla de confirmación](./media/workday-inbound-tutorial/pa_install_screen_7.png "Confirm Screen")

9. El Asistente para configuración muestra el progreso del registro del agente.
![Registro del agente](./media/workday-inbound-tutorial/pa_install_screen_8.png "Agent Registration")

10. Una vez que se realiza el registro del agente, puede hacer clic en **Salir** para salir del asistente. 
![Pantalla Salir](./media/workday-inbound-tutorial/pa_install_screen_9.png "Exit Screen")

11. Compruebe la instalación del agente y asegúrese de que se está ejecutando; para ello, abra el complemento "Servicios" y busque el servicio denominado "Microsoft Azure AD Connect Provisioning Agent" ![Servicios](./media/workday-inbound-tutorial/services.png)  


**Solución de problemas del agente**

El [registro de eventos de Windows](https://technet.microsoft.com/library/cc722404(v=ws.11).aspx) en la máquina Windows Server que hospeda el agente contiene eventos de todas las operaciones realizadas por el agente. Para ver estos eventos:
    
1. Abra **Eventvwr.msc**.
2. Seleccione **Registros de Windows > Aplicación**.
3. Vea todos los eventos registrados en el origen **AAD.Connect.ProvisioningAgent**. 
4. Compruebe los errores y las advertencias.

    
### <a name="part-2-adding-the-provisioning-connector-app-and-creating-the-connection-to-workday"></a>Parte 2: agregación de la aplicación del conector de aprovisionamiento y creación de la conexión con Workday

**Para configurar Workday en el aprovisionamiento de Active Directory:**

1.  Vaya a <https://portal.azure.com>.

2.  En la barra de navegación de la izquierda, seleccione **Azure Active Directory**.

3.  Seleccione **Enterprise Applications** (Aplicaciones empresariales) y **All Applications** (Todas las aplicaciones).

4.  Seleccione **Add an application** (Agregar una aplicación) y seleccione la categoría **All** (Todos).

5.  Busque **Workday Provisioning to Active Directory** (Aprovisionamiento de Workday en Active Directory) y agregue esa aplicación desde la galería.

6.  Una vez que se haya agregado la aplicación y se muestre la pantalla de detalles de la aplicación, seleccione **Aprovisionamiento**.

7.  Cambie el **modo** **de aprovisionamiento** a **Automático**.

8.  Cumplimente la sección **Credenciales de administrador** del siguiente modo:

   * **Nombre de usuario de administrador**: escriba el nombre de usuario de la cuenta del sistema de integración de Workday, anexando el nombre de dominio del inquilino. **Debe tener un aspecto similar al siguiente: username@contoso4**

   * **Contraseña de administrador**: escriba la contraseña de la cuenta del sistema de integración de Workday.

   * **URL de inquilino**: escriba la dirección URL al punto de conexión de servicios web de Workday de su inquilino. Debería ser similar a https://wd3-impl-services1.workday.com/ccx/service/contoso4/Human_Resources, donde contoso4 se reemplaza por el nombre correcto del inquilino y wd3-impl se reemplaza por la cadena de entorno correcta.

   * **Bosque de Active Directory:** el nombre de su dominio de Active Directory, según se ha registrado con el agente. Suele ser una cadena tipo *contoso.com*.

   * **Contenedor de Active Directory:** escriba el DN del contenedor donde el agente debe crear cuentas de usuario de forma predeterminada. 
        Ejemplo: *OU=Standard Users,OU=Users,DC=contoso,DC=test*
> [!NOTE]
> Esta configuración solo entra en juego para creaciones de cuentas de usuario si el atributo *parentDistinguishedName* no está configurado en las asignaciones de atributos. Esta configuración no se usa para la búsqueda de usuarios o las operaciones de actualización. El subárbol de todo el dominio se encuentra en el ámbito de la operación de búsqueda.
   * **Correo electrónico de notificación**: escriba su dirección de correo electrónico y marque la casilla "Enviar una notificación por correo electrónico cuando se produzca un error".
> [!NOTE]
> El servicio de aprovisionamiento de Azure AD envía la notificación por correo electrónico si el trabajo de aprovisionamiento entra en un estado de[cuarentena](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning#quarantine).

   * Haga clic en el botón **Probar conexión**. Si la prueba de conexión se lleva a cabo correctamente, haga clic en el botón **Guardar** situado en la parte superior. Si se produce un error, compruebe que las credenciales de Workday y las credenciales de AD configuradas en la instalación del agente sean válidas.

![Azure Portal](./media/workday-inbound-tutorial/wd_1.png)

### <a name="part-2-configure-attribute-mappings"></a>Parte 2: configuración de las asignaciones de atributos 

En esta sección configurará cómo fluyen los datos de los usuarios de Workday a Active Directory.

1.  En la pestaña Aprovisionamiento, en **Asignaciones**, haga clic en **Synchronize Workday Workers to OnPremises** (Sincronizar trabajadores de Workday con Local).

2.  En el campo **Ámbito de objeto de origen** puede seleccionar los conjuntos de usuarios de Workday que deben estar en el ámbito para el aprovisionamiento en AD; para ello debe definir un conjunto de filtros basados en atributos. El ámbito predeterminado es "Todos los usuarios de Workday". Filtros de ejemplo:

   * Ejemplo: ámbito de los usuarios que tengan los id. de trabajador comprendidos entre 1 000 000 y 2 000 000

      * Atributo: WorkerID

      * Operador: REGEX.COINCIDIR

      * Valor: (1[0-9][0-9][0-9][0-9][0-9][0-9])

   * Ejemplo: solo los empleados, no los trabajadores temporales 

      * Atributo: EmployeeID

      * Operador: NO ES NULO

3.  En el campo **Acciones del objeto de destino**, puede filtrar de forma global las acciones permitidas en Active Directory. **Crear** y **Actualizar** son las más habituales.

4.  En la sección **Asignaciones de atributos** puede definir cómo se asignan los distintos atributos de Workday a los atributos de Active Directory.

5. Haga clic en una asignación de atributos existente para actualizarla o haga clic en **Agregar nueva asignación** en la parte inferior de la pantalla para agregar asignaciones nuevas. Las asignaciones de atributos admiten estas propiedades:

      * **Tipo de asignación**

         * **Directo**: escribe el valor del atributo de Workday en el atributo de AD sin cambios

         * **Constante**: escribe un valor de cadena estático y constante en el atributo de AD

         * **Expresión**: le permite escribir un valor personalizado en el atributo de AD, en función de uno o varios atributos de Workday. [Para obtener más información, consulte este artículo sobre las expresiones](../manage-apps/functions-for-customizing-application-data.md).

      * **Atributo de origen**: el atributo de usuario de Workday. Si el atributo que está buscando no está presente, consulte [Personalización de la lista de atributos de usuario de Workday](#customizing-the-list-of-workday-user-attributes).

      * **Valor predeterminado**: opcional. Si el atributo de origen tiene un valor vacío, la asignación escribirá este valor.
            La configuración más habitual consiste en dejarlo en blanco.

      * **Atributo de destino**: atributo de usuario de Active Directory.

      * **Hacer coincidir objetos con este atributo**: especifica si se debe usar o no esta asignación para identificar de forma unívoca a los usuarios entre Workday y Active Directory. Se suele establecer en el campo Id. del trabajador de Workday, que se suele asignar a uno de los atributos de id. de empleado de Active Directory.

      * **Precedencia de coincidencia**: se pueden establecer varios atributos coincidentes. Si hay varios, se evalúan en el orden definido por este campo. En el momento en que se encuentre una coincidencia, no se evaluarán más atributos coincidentes.

      * **Aplicar esta asignación**
       
         * **Siempre**: esta asignación se aplica a las acciones de creación y actualización de usuarios

         * **Solo durante la creación**: esta asignación se aplica solo a las acciones de creación de usuarios

6. Para guardar las asignaciones, haga clic en **Guardar** en la parte superior de la sección Asignación de atributos.

![Azure Portal](./media/workday-inbound-tutorial/WD_2.PNG)

**A continuación se muestran algunos ejemplos de asignaciones de atributos entre Workday y Active Directory, con algunas expresiones comunes**

-   La expresión que se asigna al atributo parentDistinguishedName se usa para aprovisionar a los usuarios en distintas unidades organizativas en función de uno o varios atributos de origen de Workday. En este ejemplo se coloca a los usuarios en distintas unidades organizativas en función de la ciudad en la que se encuentran.

-   El atributo userPrincipalName de Active Directory se genera mediante la concatenación de un identificador de usuario de Workday con un sufijo de dominio

-   [Aquí encontrará documentación sobre cómo escribir expresiones](../manage-apps/functions-for-customizing-application-data.md). Se incluyen ejemplos sobre cómo quitar caracteres especiales.

  
| ATRIBUTO DE WORKDAY | ATRIBUTO DE ACTIVE DIRECTORY |  ¿IDENTIFICADOR COINCIDENTE? | CREAR / ACTUALIZAR |
| ---------- | ---------- | ---------- | ---------- |
| **WorkerID**  |  EmployeeID | **Sí** | Escrito únicamente en Crear |
| **UserID**    |  cn    |   |   Escrito únicamente en Crear |
| **Join("@", [UserID], "contoso.com")**   | userPrincipalName     |     | Escrito únicamente en Crear 
| **Replace(Mid(Replace(\[UserID\], , "(\[\\\\/\\\\\\\\\\\\\[\\\\\]\\\\:\\\\;\\\\|\\\\=\\\\,\\\\+\\\\\*\\\\?\\\\&lt;\\\\&gt;\])", , "", , ), 1, 20), , "([\\\\.)\*\$](file:///\\.)*$)", , "", , )**      |    sAMAccountName            |     |         Escrito únicamente en Crear |
| **Switch(\[Active\], , "0", "True", "1", "False")** |  accountDisabled      |     | Crear y Actualizar |
| **Nombre**   | givenName       |     |    Crear y Actualizar |
| **Apellidos**   |   sn   |     |  Crear y Actualizar |
| **PreferredNameData**  |  DisplayName |     |   Crear y Actualizar |
| **Company**         | company   |     |  Crear y Actualizar |
| **SupervisoryOrganization**  | department  |     |  Crear y Actualizar |
| **ManagerReference**   | manager  |     |  Crear y Actualizar |
| **BusinessTitle**   |  título     |     |  Crear y Actualizar | 
| **AddressLineData**    |  streetAddress  |     |   Crear y Actualizar |
| **Municipality**   |   l   |     | Crear y Actualizar |
| **CountryReferenceTwoLetter**      |   co |     |   Crear y Actualizar |
| **CountryReferenceTwoLetter**    |  c  |     |         Crear y Actualizar |
| **CountryRegionReference** |  st     |     | Crear y Actualizar |
| **WorkSpaceReference** | physicalDeliveryOfficeName    |     |  Crear y Actualizar |
| **PostalCode**  |   postalCode  |     | Crear y Actualizar |
| **PrimaryWorkTelephone**  |  telephoneNumber   |     | Crear y Actualizar |
| **Fax**      | facsimileTelephoneNumber     |     |    Crear y Actualizar |
| **Mobile**  |    mobile       |     |       Crear y Actualizar |
| **LocalReference** |  preferredLanguage  |     |  Crear y Actualizar |                                               
| **Switch(\[Municipality\], "OU=Standard Users,OU=Users,OU=Default,OU=Locations,DC=contoso,DC=com", "Dallas", "OU=Standard Users,OU=Users,OU=Dallas,OU=Locations,DC=contoso,DC=com", "Austin", "OU=Standard Users,OU=Users,OU=Austin,OU=Locations,DC=contoso,DC=com", "Seattle", "OU=Standard Users,OU=Users,OU=Seattle,OU=Locations,DC=contoso,DC=com", “London", "OU=Standard Users,OU=Users,OU=London,OU=Locations,DC=contoso,DC=com")**  | parentDistinguishedName     |     |  Crear y Actualizar |
  


### <a name="part-4-start-the-service"></a>Parte 4: inicio del servicio
Una vez que las partes 1-3 se han completado, puede volver a iniciar el servicio de aprovisionamiento en Azure Portal.

1.  En la pestaña **Aprovisionamiento**, establezca **Estado de aprovisionamiento** en **Activado**.

2. Haga clic en **Save**(Guardar).

3. Se iniciará la sincronización inicial, que puede tardar una cantidad de horas variable, según el número de usuarios que haya en Workday.

4. En cualquier momento, compruebe la pestaña **Registros de auditoría** en Azure Portal para ver las acciones que ha realizado el servicio de aprovisionamiento. Los registros de auditoría muestran todos los eventos de sincronización individuales realizados por el servicio de aprovisionamiento, por ejemplo, los usuarios que se leen fuera de Workday y que luego se agregan o actualizan en Active Directory. **[Consulte la guía de creación de informes de aprovisionamiento para obtener instrucciones detalladas sobre cómo leer los registros de auditoría](../manage-apps/check-status-user-account-provisioning.md)**.

1.  Compruebe el [registro de eventos de Windows](https://technet.microsoft.com/library/cc722404(v=ws.11).aspx) en la máquina Windows Server que hospeda el agente para ver si hay nuevos errores o advertencias. Estos eventos se pueden ver si se inicia **Eventvwr.msc** en el servidor y se selecciona **Registros de Windows > Aplicación**. Todos los mensajes relacionados con el aprovisionamiento se registran en el origen **AADSyncAgent**.

6. Una vez finalizada la operación, se escribe un informe resumido de auditoría en la pestaña **Aprovisionamiento**, tal y como se muestra a continuación.

![Azure Portal](./media/workday-inbound-tutorial/WD_3.PNG)


## <a name="configuring-user-provisioning-to-azure-active-directory"></a>Configuración del aprovisionamiento de usuarios en Azure Active Directory
El modo de configuración del aprovisionamiento en Azure Active Directory dependerá de sus requisitos de aprovisionamiento, tal como se detalla en la tabla siguiente.

| Escenario | Solución |
| -------- | -------- |
| **Los usuarios se deben aprovisionar en Active Directory y en Azure AD** | Use **[AAD Connect](../hybrid/whatis-hybrid-identity.md)** |
| **Los usuarios solo se deben aprovisionar en Active Directory** | Use **[AAD Connect](../hybrid/whatis-hybrid-identity.md)** |
| **Los usuarios solo se deben aprovisionar en Azure AD (solo los usuarios que están en la nube)** | Use la aplicación de **aprovisionamiento de Workday en Azure Active Directory** en la galería de aplicaciones |

Para obtener instrucciones sobre cómo configurar Azure AD Connect, consulte la [documentación de Azure AD Connect](../hybrid/whatis-hybrid-identity.md).

En las siguientes secciones se describe cómo configurar una conexión entre Workday y Azure AD para aprovisionar usuarios que solo están en la nube.

> [!IMPORTANT]
> Siga el siguiente procedimiento únicamente si tiene usuarios que solo están en la nube que se deban aprovisionar en Azure AD, y no en Active Directory local.

### <a name="part-1-adding-the-azure-ad-provisioning-connector-app-and-creating-the-connection-to-workday"></a>Parte 1: agregación de la aplicación del conector de aprovisionamiento de Azure AD y creación de la conexión con Workday

**Para configurar el aprovisionamiento de Workday en Azure Active Directory para los usuarios que solo están en la nube:**

1.  Vaya a <https://portal.azure.com>.

2.  En la barra de navegación de la izquierda, seleccione **Azure Active Directory**.

3.  Seleccione **Enterprise Applications** (Aplicaciones empresariales) y **All Applications** (Todas las aplicaciones).

4.  Seleccione **Agregar una aplicación** y, luego, seleccione la categoría **Todos**.

5.  Busque **Workday to Azure AD provisioning** (Aprovisionamiento de Workday en Azure AD) y agregue esa aplicación desde la galería.

6.  Una vez que se haya agregado la aplicación y se muestre la pantalla de detalles de la aplicación, seleccione **Aprovisionamiento**.

7.  Cambie el **modo** **de aprovisionamiento** a **Automático**.

8.  Cumplimente la sección **Credenciales de administrador** del siguiente modo:

   * **Nombre de usuario de administrador**: escriba el nombre de usuario de la cuenta del sistema de integración de Workday, anexando el nombre de dominio del inquilino. Debe tener un aspecto similar al siguiente: username@contoso4

   * **Contraseña de administrador**: escriba la contraseña de la cuenta del sistema de integración de Workday.

   * **URL de inquilino**: escriba la dirección URL al punto de conexión de servicios web de Workday de su inquilino. Debería ser similar a https://wd3-impl-services1.workday.com/ccx/service/contoso4/Human_Resources, donde contoso4 se reemplaza por el nombre correcto del inquilino y wd3-impl se reemplaza por la cadena de entorno correcta. Si no se conoce esta dirección URL, trabaje con su asociado de integración o representante de soporte técnico de Workday para determinar la dirección URL correcta que se debe usar.

   * **Correo electrónico de notificación**: escriba su dirección de correo electrónico y marque la casilla "Enviar una notificación por correo electrónico cuando se produzca un error".

   * Haga clic en el botón **Probar conexión**.

   * Si la prueba de conexión se lleva a cabo correctamente, haga clic en el botón **Guardar** situado en la parte superior. Si se produce un error, compruebe que la dirección URL y las credenciales de Workday sean válidas en Workday.

### <a name="part-2-configure-attribute-mappings"></a>Parte 2: configuración de las asignaciones de atributos 

En esta sección configurará cómo fluyen los datos de los usuarios de Workday a Azure Active Directory para los usuarios que solo están en la nube.

1. En la pestaña Aprovisionamiento, en **Asignaciones**, haga clic en **Synchronize Workers to Azure AD** (Sincronizar trabajadores con Azure AD).

2. En el campo **Ámbito de objeto de origen** puede seleccionar los conjuntos de usuarios de Workday que deben estar en el ámbito para el aprovisionamiento en Azure AD; para ello debe definir un conjunto de filtros basados en atributos. El ámbito predeterminado es "Todos los usuarios de Workday". Filtros de ejemplo:

   * Ejemplo: ámbito de los usuarios que tengan los id. de trabajador comprendidos entre 1 000 000 y 2 000 000

      * Atributo: WorkerID

      * Operador: REGEX.COINCIDIR

      * Valor: (1[0-9][0-9][0-9][0-9][0-9][0-9])

   * Ejemplo: solo los trabajadores temporales, no los empleados habituales

      * Atributo: ContingentID

      * Operador: NO ES NULO

3. En el campo **Acciones del objeto de destino**, puede filtrar de forma global las acciones permitidas en Azure AD. **Crear** y **Actualizar** son las más habituales.

4. En la sección **Asignaciones de atributos** puede definir cómo se asignan los distintos atributos de Workday a los atributos de Active Directory.

5. Haga clic en una asignación de atributos existente para actualizarla o haga clic en **Agregar nueva asignación** en la parte inferior de la pantalla para agregar asignaciones nuevas. Las asignaciones de atributos admiten estas propiedades:

   * **Tipo de asignación**

      * **Directo**: escribe el valor del atributo de Workday en el atributo de AD sin cambios.

      * **Constante**: escribe un valor de cadena estático y constante en el atributo de AD.

      * **Expresión**: le permite escribir un valor personalizado en el atributo de AD, en función de uno o varios atributos de Workday. [Para obtener más información, consulte este artículo sobre las expresiones](../manage-apps/functions-for-customizing-application-data.md).

   * **Atributo de origen**: el atributo de usuario de Workday. Si el atributo que está buscando no está presente, consulte [Personalización de la lista de atributos de usuario de Workday](#customizing-the-list-of-workday-user-attributes).

   * **Valor predeterminado**: opcional. Si el atributo de origen tiene un valor vacío, la asignación escribirá este valor.
            La configuración más habitual consiste en dejarlo en blanco.

   * **Atributo de destino**: atributo de usuario de Azure AD.

   * **Hacer coincidir objetos con este atributo**: especifica si se debe usar o no esta asignación para identificar de forma unívoca a los usuarios entre Workday y Azure AD. Se suele establecer en el campo Id. del trabajador de Workday, que se suele asignar al atributo de id. de empleado (nuevo) o a un atributo de extensión de Azure AD.

   * **Precedencia de coincidencia**: se pueden establecer varios atributos coincidentes. Si hay varios, se evalúan en el orden definido por este campo. En el momento en que se encuentre una coincidencia, no se evaluarán más atributos coincidentes.

   * **Aplicar esta asignación**

     * **Siempre**: esta asignación se aplica a las acciones de creación y actualización de usuarios

     * **Solo durante la creación**: esta asignación se aplica solo a las acciones de creación de usuarios

6. Para guardar las asignaciones, haga clic en **Guardar** en la parte superior de la sección Asignación de atributos.

### <a name="part-3-start-the-service"></a>Parte 3: inicio del servicio
Después de las partes 1 y 2, puede iniciar el servicio de aprovisionamiento.

1. En la pestaña **Aprovisionamiento**, establezca **Estado de aprovisionamiento** en **Activado**.

2. Haga clic en **Save**(Guardar).

3. Se iniciará la sincronización inicial, que puede tardar una cantidad de horas variable, según el número de usuarios que haya en Workday.

4. Los eventos de sincronización se pueden ver en la pestaña **Registros de auditoría**. **[Consulte la guía de creación de informes de aprovisionamiento para obtener instrucciones detalladas sobre cómo leer los registros de auditoría](../manage-apps/check-status-user-account-provisioning.md)**.

5. Una vez finalizada la operación, se escribe un informe resumido de auditoría en la pestaña **Aprovisionamiento**, tal y como se muestra a continuación.

## <a name="configuring-writeback-of-email-addresses-to-workday"></a>Configuración de la reescritura de direcciones de correo electrónico en Workday
Siga estas instrucciones para configurar la reescritura de direcciones de correo electrónico de los usuarios de Azure Active Directory a Workday.

### <a name="part-1-adding-the-provisioning-connector-app-and-creating-the-connection-to-workday"></a>Parte 1: agregación de la aplicación del conector de aprovisionamiento y creación de la conexión con Workday

**Para configurar el conector de escritura diferida de Workday:**

1. Vaya a <https://portal.azure.com>.

2. En la barra de navegación de la izquierda, seleccione **Azure Active Directory**.

3. Seleccione **Enterprise Applications** (Aplicaciones empresariales) y **All Applications** (Todas las aplicaciones).

4. Seleccione **Agregar una aplicación** y, luego, seleccione la categoría **Todos**.

5. Busque **Workday Writeback** (Reescritura de Workday) y agregue esa aplicación desde la galería.

6. Una vez que se haya agregado la aplicación y se muestre la pantalla de detalles de la aplicación, seleccione **Aprovisionamiento**.

7. Cambie el **modo** **de aprovisionamiento** a **Automático**.

8. Cumplimente la sección **Credenciales de administrador** del siguiente modo:

   * **Nombre de usuario de administrador**: escriba el nombre de usuario de la cuenta del sistema de integración de Workday, anexando el nombre de dominio del inquilino. Debe tener un aspecto similar al siguiente: username@contoso4

   * **Contraseña de administrador**: escriba la contraseña de la cuenta del sistema de integración de Workday.

   * **URL de inquilino**: escriba la dirección URL al punto de conexión de servicios web de Workday de su inquilino. Debería ser similar a https://wd3-impl-services1.workday.com/ccx/service/contoso4/Human_Resources, donde contoso4 se reemplaza por el nombre correcto del inquilino y wd3-impl se reemplaza por la cadena de entorno correcta (si es necesario).

   * **Correo electrónico de notificación**: escriba su dirección de correo electrónico y marque la casilla "Enviar una notificación por correo electrónico cuando se produzca un error".

   * Haga clic en el botón **Probar conexión**. Si la prueba de conexión se lleva a cabo correctamente, haga clic en el botón **Guardar** situado en la parte superior. Si se produce un error, compruebe que la dirección URL y las credenciales de Workday sean válidas en Workday.

### <a name="part-2-configure-attribute-mappings"></a>Parte 2: configuración de las asignaciones de atributos 

En esta sección configurará cómo fluyen los datos de los usuarios de Workday a Active Directory.

1. En la pestaña Aprovisionamiento, en **Asignaciones**, haga clic en **Synchronize Azure AD Users to Workday** (Sincronizar usuarios de Azure AD con Workday).

2. En el campo **Ámbito de objeto de origen** tiene la opción de filtrar los conjuntos de usuarios de Azure Active Directory que deben reescribir sus direcciones de correo electrónico en Workday. El ámbito predeterminado es "Todos los usuarios de Azure AD". 

3. En la sección **Asignaciones de atributos**, actualice el identificador coincidente para indicar el atributo en Azure Active Directory donde se almacena el identificador de trabajo o el identificador de empleado de Workday. Un método de búsqueda de coincidencias conocido consiste en sincronizar el Id. de trabajador de Workday o el Id. de empleado con el atributo extensionAttribute1-15 en Azure AD y, luego, usar dicho atributo en Azure AD para hacer coincidir los usuarios en Workday. 

4. Para guardar las asignaciones, haga clic en **Guardar** en la parte superior de la sección Asignación de atributos.

### <a name="part-3-start-the-service"></a>Parte 3: inicio del servicio
Después de las partes 1 y 2, puede iniciar el servicio de aprovisionamiento.

1. En la pestaña **Aprovisionamiento**, establezca **Estado de aprovisionamiento** en **Activado**.

2. Haga clic en **Save**(Guardar).

3. Se iniciará la sincronización inicial, que puede tardar una cantidad de horas variable, según el número de usuarios que haya en Workday.

4. Los eventos de sincronización se pueden ver en la pestaña **Registros de auditoría**. **[Consulte la guía de creación de informes de aprovisionamiento para obtener instrucciones detalladas sobre cómo leer los registros de auditoría](../manage-apps/check-status-user-account-provisioning.md)**.

5. Una vez finalizada la operación, se escribe un informe resumido de auditoría en la pestaña **Aprovisionamiento**, tal y como se muestra a continuación.

## <a name="customizing-the-list-of-workday-user-attributes"></a>Personalización de la lista de atributos de usuario de Workday
Las aplicaciones de aprovisionamiento de Workday para Active Directory y Azure AD incluyen ambas una lista predeterminada de atributos de usuario de Workday que puede seleccionar. Sin embargo, estas listas no son exhaustivas. Workday admite muchos cientos de posibles atributos de usuario, que pueden ser estándar o exclusivos de su inquilino de Workday. 

El servicio de aprovisionamiento de Azure AD admite la posibilidad de personalizar la lista o los atributos de Workday para incluir cualquiera de los atributos expuestos en la operación [Get_Workers](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html) de la API de Recursos Humanos.

Para ello, debe usar [Workday Studio](https://community.workday.com/studio-download) para extraer las expresiones XPath que representan los atributos que quiere usar y, luego, agregarlas a la configuración de aprovisionamiento mediante el editor de atributos avanzados de Azure Portal.

**Para recuperar una expresión XPath para un atributo de usuario de Workday, siga estos pasos:**

1. Descargue e instale [Workday Studio](https://community.workday.com/studio-download). Necesitará una cuenta de la comunidad de Workday para acceder al instalador.

2. Descargue el archivo WDSL Human_Resources de Workday desde esta dirección URL: https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Human_Resources.wsdl

3. Inicie Workday Studio.

4. En la barra de comandos, seleccione la opción **Workday > Test Web Service in Tester** (Probar servicio web en Evaluador).

5. Seleccione **External** (Externo) y seleccione el archivo WSDL Human_Resources que descargó en el paso 2.

    ![Workday Studio](./media/workday-inbound-tutorial/wdstudio1.png)

6. Establezca el campo **Location** (Ubicación) en `https://IMPL-CC.workday.com/ccx/service/TENANT/Human_Resources`, pero reemplace "IMPL-CC" por el tipo de instancia real y "TENANT" por el nombre real del inquilino.

7. Establezca **Operation** (Operación) en **Get_Workers**.

8.  Haga clic en el vínculo pequeño de **configuración** bajo los paneles de solicitud/respuesta para establecer las credenciales de Workday. Marque **Authentication** (Autenticación) y escriba el nombre de usuario y la contraseña de la cuenta del sistema de integración de Workday. Asegúrese de que el formato del nombre de usuario sea name@tenant y deje seleccionada la opción **WS-Security UsernameToken**.

    ![Workday Studio](./media/workday-inbound-tutorial/wdstudio2.png)

9. Seleccione **Aceptar**.

10. En el panel **Request** (Solicitud), pegue el archivo XML debajo y establezca **Employee_ID** en el identificador de empleado de un usuario real en el inquilino de Workday. Seleccione un usuario que tenga rellenado el atributo que desea extraer.

    ```
    <?xml version="1.0" encoding="UTF-8"?>
    <env:Envelope xmlns:env="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
      <env:Body>
        <wd:Get_Workers_Request xmlns:wd="urn:com.workday/bsvc" wd:version="v21.1">
          <wd:Request_References wd:Skip_Non_Existing_Instances="true">
            <wd:Worker_Reference>
              <wd:ID wd:type="Employee_ID">21008</wd:ID>
            </wd:Worker_Reference>
          </wd:Request_References>
          <wd:Response_Group>
            <wd:Include_Reference>true</wd:Include_Reference>
            <wd:Include_Personal_Information>true</wd:Include_Personal_Information>
            <wd:Include_Employment_Information>true</wd:Include_Employment_Information>
            <wd:Include_Management_Chain_Data>true</wd:Include_Management_Chain_Data>
            <wd:Include_Organizations>true</wd:Include_Organizations>
            <wd:Include_Reference>true</wd:Include_Reference>
            <wd:Include_Transaction_Log_Data>true</wd:Include_Transaction_Log_Data>
            <wd:Include_Photo>true</wd:Include_Photo>
            <wd:Include_User_Account>true</wd:Include_User_Account>
            <wd:Include_Roles>true</wd:Include_Roles>
          </wd:Response_Group>
        </wd:Get_Workers_Request>
      </env:Body>
    </env:Envelope>
    ```
 
11. Haga clic en **Send Request** (Enviar solicitud) (flecha verde) para ejecutar el comando. Si se ejecuta correctamente, la respuesta debe aparecer en el panel **Response** (Respuesta). Compruebe la respuesta para asegurarse de que tiene los datos del identificador de usuario especificado y no un error.

12. Si es correcta, copie el código XML del panel **Response** (Respuesta) y guárdelo como un archivo XML.

13. En la barra de comandos de Workday Studio, seleccione **File > Open File...** (Archivo > Abrir archivo...) y abra el archivo XML que ha guardado. El archivo se abre en el editor XML de Workday Studio.

    ![Workday Studio](./media/workday-inbound-tutorial/wdstudio3.png)

14. En el árbol de archivos, desplácese por **/env:Envelope > env:Body > wd:Get_Workers_Response > wd:Response_Data > wd:Worker** para encontrar los datos del usuario. 

15. En **wd:Worker**, busque el atributo que quiere agregar y selecciónelo.

16. Copie la expresión XPath del atributo seleccionado del campo **Document Path** (Ruta de acceso a documento).

1. Quite el prefijo **/env:Envelope/env:Body/wd:Get_Workers_Response/wd:Response_Data/** de la expresión copiada.

18. Si el último elemento de la expresión copiada es un nodo (ejemplo: "/wd:Birth_Date"), anexe **/text()** al final de la expresión. Esto no es necesario si el último elemento es un atributo (ejemplo: "/@wd: tipo").

19. El resultado debe ser similar a `wd:Worker/wd:Worker_Data/wd:Personal_Data/wd:Birth_Date/text()`. Esto es lo que copiará en Azure Portal.


**Para agregar el atributo de usuario personalizado de Workday a la configuración de aprovisionamiento, siga estos pasos:**

1. Inicie [Azure Portal](https://portal.azure.com) y vaya a la sección Aprovisionamiento de la aplicación de aprovisionamiento de Workday, como se ha descrito anteriormente en este tutorial.

2. Establezca **Estado de aprovisionamiento** en **Desactivado** y seleccione **Guardar**. Esto le ayudará a garantizar que los cambios se apliquen solo cuando esté listo.

3. En **Asignaciones**, seleccione **Synchronize Workers to OnPremises** (Sincronizar trabajos con el entorno local) (o **Synchronize Workers to Azure AD** [Sincronizar trabajos con Azure AD]).

4. Desplácese a la parte inferior de la pantalla siguiente y seleccione **Mostrar opciones avanzadas**.

5. Seleccione **Edit attribute list for Workday** (Editar lista de atributos para Workday).

    ![Workday Studio](./media/workday-inbound-tutorial/wdstudio_aad1.png)

6. Desplácese a la parte inferior de la lista de atributos, al lugar donde están los campos de entrada.

7. En **Name** (Name), escriba un nombre para mostrar para el atributo.

8. En **Type** (Tipo), seleccione el tipo que corresponde adecuadamente a su atributo (el más común es **String** [Cadena]).

9. En **API Expression** (Expresión de API), escriba la expresión XPath que copió de Workday Studio. Ejemplo: `wd:Worker/wd:Worker_Data/wd:Personal_Data/wd:Birth_Date/text()`

10. Seleccione **Add Attribute** (Agregar atributo).

    ![Workday Studio](./media/workday-inbound-tutorial/wdstudio_aad2.png)

11. Seleccione **Save** (Guardar) arriba y, luego, **Yes** (Sí) en el cuadro de diálogo. Cierre la pantalla de asignación de atributos si aún está abierta.

12. De nuevo en la pestaña principal **Aprovisionamiento**, seleccione otra vez **Synchronize Workers to OnPremises** (Sincronizar trabajos con el entorno local) (o **Synchronize Workers to Azure AD** [Sincronizar trabajos con Azure AD]).

13. Seleccione **Add new mapping** (Agregar nueva asignación).

14. El nuevo atributo debe aparecer ahora en la lista **Atributo de origen**.

15. Agregue una asignación para el nuevo atributo según sea necesario.

16. Cuando termine, recuerde establecer **Estado de aprovisionamiento** de nuevo en **Activado** y haga clic en Guardar.

## <a name="known-issues"></a>Problemas conocidos

* Actualmente no se admite la escritura de datos en el atributo de usuario thumbnailPhoto en Active Directory local.

* El conector "Workday a Azure AD" no se admite actualmente en los inquilinos de Azure AD en los que la conexión AAD está habilitada.  

## <a name="managing-personal-data"></a>Administración de datos personales

La solución de aprovisionamiento Workday para Active Directory requiere que se instale un agente de sincronización en un servidor unido a un dominio, y este agente crea registros en el registro de eventos de Windows, que puede contener información de identificación personal.

## <a name="next-steps"></a>Pasos siguientes

* [Aprenda a revisar los registros y a obtener informes sobre la actividad de aprovisionamiento](../manage-apps/check-status-user-account-provisioning.md)
* [Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Workday](workday-tutorial.md)
* [Aprenda a integrar otras aplicaciones SaaS con Azure Active Directory](tutorial-list.md)

