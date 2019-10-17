---
title: Aprovisionamiento automatizado de usuarios para aplicaciones SaaS en Azure AD | Microsoft Docs
description: Una introducción sobre cómo puede usar Azure AD para el aprovisionamiento, el desaprovisionamiento y la actualización continua de cuentas de usuario de manera automática en varias aplicaciones SaaS de terceros.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/12/2019
ms.author: mimart
ms.reviewer: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3ef652b05f62218ee1d0e72543bfa546f0c14abe
ms.sourcegitcommit: be344deef6b37661e2c496f75a6cf14f805d7381
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/07/2019
ms.locfileid: "72001699"
---
# <a name="automate-user-provisioning-and-deprovisioning-to-saas-applications-with-azure-active-directory"></a>Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory

Azure Active Directory (Azure AD) permite automatizar la creación, el mantenimiento y la eliminación de identidades de usuario en aplicaciones de nube ([SaaS](https://azure.microsoft.com/overview/what-is-saas/)), como Dropbox, Salesforce, ServiceNow y muchas más. Esto se conoce como aprovisionamiento automático de usuarios para aplicaciones SaaS.

> [!VIDEO https://www.youtube.com/embed/_ZjARPpI6NI]

Esta característica le permite hacer lo siguiente:

- Crear cuentas automáticamente en los sistemas adecuados para los usuarios nuevos cuando se unen a su equipo u organización.
- Desactivar las cuentas automáticamente en los sistemas adecuados cuando los usuarios dejan el equipo o la organización.
- Asegurarse de que las identidades de las aplicaciones y sistemas se mantienen actualizadas con los cambios del directorio o el sistema de recursos humanos.
- Aprovisionar los objetos que no sean de los usuarios, como los grupos, para las aplicaciones que los admitan.

El aprovisionamiento automático de usuarios también incluye esta funcionalidad:

- Capacidad de hacer coincidir las identidades existentes entre los sistemas de origen y de destino.
- Asignaciones de atributos personalizables que definen qué datos del usuario deben fluir del sistema de origen al sistema de destino.
- Alertas de correo electrónico opcionales para errores de aprovisionamiento.
- Informes y registros de actividades para facilitar la supervisión y solución de problemas.

## <a name="why-use-automated-provisioning"></a>¿Por qué usar el aprovisionamiento automático?

Las razones habituales por las que se debe usar esta característica son:

- Evitar los costos, las ineficiencias y los errores humanos asociados con los procesos de aprovisionamiento manuales.
- Evitar los costos asociados al hospedaje y el mantenimiento de scripts y soluciones de aprovisionamiento desarrollados de forma personalizada.
- Proteger su organización mediante la eliminación instantánea de las identidades de los usuarios de aplicaciones SaaS claves cuando estos abandonan la organización.
- Importar fácilmente una gran cantidad de usuarios a una aplicación o sistema SaaS concretos.
- Tener un único conjunto de directivas para determinar quién se aprovisiona y quién puede iniciar sesión en una aplicación.

## <a name="how-does-automatic-provisioning-work"></a>¿Cómo funciona el aprovisionamiento automático?

El **servicio de aprovisionamiento de Azure AD** aprovisiona usuarios para las aplicaciones SaaS y otros sistemas mediante la conexión de los puntos de conexión de la API de administración de usuarios que proporciona el proveedor de cada aplicación. Estos puntos de conexión de la API de administración de usuarios permiten a Azure AD crear, actualizar y quitar usuarios mediante programación. Para las aplicaciones seleccionadas, el servicio de aprovisionamiento también puede crear, actualizar y quitar objetos adicionales relacionados con la identidad, como los grupos y los roles.

![Servicio de aprovisionamiento de Azure AD](./media/user-provisioning/provisioning0.PNG)
*Figura 1: Servicio de aprovisionamiento de Azure AD*

![Flujo de trabajo de aprovisionamiento de usuarios salientes](./media/user-provisioning/provisioning1.PNG)
*Figura 2: Flujo de trabajo "saliente" del aprovisionamiento de usuarios desde Azure AD a aplicaciones SaaS populares*

![Flujo de trabajo de aprovisionamiento de usuarios salientes](./media/user-provisioning/provisioning2.PNG)
*Figura 3: Flujo de trabajo "entrante" del aprovisionamiento de usuarios desde aplicaciones populares de administración del capital humano (HCM) a Azure Active Directory y Windows Server Active Directory*

## <a name="what-applications-and-systems-can-i-use-with-azure-ad-automatic-user-provisioning"></a>¿Qué aplicaciones y sistemas puedo usar con el aprovisionamiento automático de usuarios de Azure AD?

Azure AD ofrece compatibilidad preintegrada con muchas aplicaciones SaaS y sistemas de recursos humanos populares, así como compatibilidad genérica con aplicaciones que implementan determinadas partes del [estándar SCIM 2.0](https://techcommunity.microsoft.com/t5/Identity-Standards-Blog/Provisioning-with-SCIM-getting-started/ba-p/880010).

### <a name="pre-integrated-applications"></a>Aplicaciones preintegradas

Para ver una lista de todas las aplicaciones en las que Azure AD admite un conector de aprovisionamiento previamente integrado, consulte la [lista de tutoriales de aplicaciones de aprovisionamiento de usuarios](../saas-apps/tutorial-list.md).

Si desea ponerse en contacto con el equipo de ingeniería de Azure AD para solicitar soporte técnico para el aprovisionamiento de aplicaciones adicionales, envíe un mensaje a través del [foro de comentarios de Azure Active Directory](https://feedback.azure.com/forums/374982-azure-active-directory-application-requests/filters/new?category_id=172035).

> [!NOTE]
> Para que una aplicación admita el aprovisionamiento automático de usuarios, primero debe proporcionar las API de administración de usuarios necesarias que permitan a los programas externos automatizar la creación, el mantenimiento y la eliminación de usuarios. Por lo tanto, no todas las aplicaciones SaaS son compatibles con esta característica. En el caso de las aplicaciones que sí son compatibles con las API de administración de usuarios, el equipo de ingeniería de Azure AD puede crear un conector de aprovisionamiento para esas aplicaciones, y las prioridades para realizar este trabajo se basan en las necesidades de los clientes actuales y potenciales.

### <a name="connecting-applications-that-support-scim-20"></a>Conexión de aplicaciones que admiten SCIM 2.0

Para obtener información sobre cómo conectar aplicaciones de forma general que implementen API de administración de usuario basado en SCIM 2.0, consulte [Uso de SCIM para aprovisionar automáticamente a los usuarios y grupos de Azure Active Directory para aplicaciones](use-scim-to-provision-users-and-groups.md).

## <a name="how-do-i-set-up-automatic-provisioning-to-an-application"></a>¿Cómo configuro el aprovisionamiento automático para una aplicación?

> [!VIDEO https://www.youtube.com/embed/pKzyts6kfrw]

Use el portal de Azure Active Directory para configurar el servicio de una aplicación seleccionada de aprovisionamiento de Azure AD.

1. Abra el **[portal de Azure Active Directory](https://aad.portal.azure.com)** .
1. En el panel izquierdo, seleccione **Aplicaciones empresariales**. Se muestra una lista de todas las aplicaciones configuradas.
1. Elija **+ Nueva aplicación** para agregar una aplicación. Agregue uno de los siguientes elementos en función del escenario:

   - La opción **Agregar aplicación propia** admite integraciones de SCIM desarrolladas de manera personalizada.
   - Todas las aplicaciones de la sección **Agregar desde la galería** > **Aplicaciones destacadas** admiten el aprovisionamiento automático. Consulte la [lista de tutoriales de aplicaciones para el aprovisionamiento de usuarios](../saas-apps/tutorial-list.md) para ver más.

1. Proporcione los detalles y seleccione **Agregar**. La nueva aplicación se agrega a la lista de aplicaciones empresariales y se abre en su pantalla de administración de la aplicación.
1. Seleccione **Aprovisionamiento** para administrar la configuración de la aplicación de aprovisionamiento de cuentas de usuario.

   ![Muestra la pantalla de configuración de aprovisionamiento](./media/user-provisioning/provisioning_settings0.PNG)

1. Seleccione la opción Automático en **Modo de aprovisionamiento** para especificar la configuración de las credenciales de administrador, asignaciones, inicio y parada, y sincronización.

   - Expanda **Credenciales de administrador** para especificar las credenciales necesarias para que Azure AD se conecte a la API de administración de usuarios de la aplicación. En esta sección también se puede habilitar notificaciones por correo electrónico si se produce un error con las credenciales o si el trabajo de aprovisionamiento entra en [cuarentena](#quarantine).
   - Expanda **Asignaciones** para ver y modificar los atributos de usuario que fluyen entre Azure AD y la aplicación de destino cuando las cuentas de usuario se aprovisionan o se actualizan. Si la aplicación de destino lo admite, en esta sección se puede configurar opcionalmente el aprovisionamiento de grupos y cuentas de usuario. Seleccione una asignación de la tabla para abrir el editor de asignaciones a la derecha, donde puede ver y personalizar los atributos de usuario.

     Los **filtros de ámbito** le indican al servicio de aprovisionamiento qué usuarios y grupo del sistema de origen se deben aprovisionar o desaprovisionar para el sistema de destino. En el panel **Asignaciones de atributos**, seleccione **Ámbito de objeto de origen** para filtrar por valores de atributo concretos. Por ejemplo, puede especificar que solo los usuarios con el atributo "Department" (Departamento) de "Sales" (Ventas) deben estar en el ámbito del aprovisionamiento. Para más información, consulte [Uso de filtros de ámbito](define-conditional-rules-for-provisioning-user-accounts.md).

     Para obtener más información, vea [Personalización de la asignación de atributos](customize-application-attributes.md).

   - La opción **Configuración** controla el funcionamiento del servicio de aprovisionamiento de una aplicación, incluso si se está ejecutando. El menú **Ámbito** permite especificar si solo los usuarios y grupos asignados deben incluirse en el ámbito para el aprovisionamiento, o si se deben aprovisionar todos los usuarios en el directorio de Azure AD. Para obtener más información sobre la "asignación" de usuarios y grupos, consulte [Asignación de un usuario o un grupo a una aplicación empresarial en Azure Active Directory](assign-user-or-group-access-portal.md).

En la pantalla de administración de la aplicación, seleccione **Registros de aprovisionamiento (versión preliminar)** para ver los registros de cada operación que el servicio de aprovisionamiento de Azure AD ejecuta. Para obtener más información, consulte la [guía de informes de aprovisionamiento](check-status-user-account-provisioning.md).

![Ejemplo: pantalla de registros de aprovisionamiento para una aplicación](./media/user-provisioning/audit_logs.PNG)

> [!NOTE]
> El servicio de aprovisionamiento de usuarios de Azure AD también se puede configurar y administrar mediante [Microsoft Graph API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-overview).

## <a name="what-happens-during-provisioning"></a>¿Qué ocurre durante el aprovisionamiento?

Si Azure AD es el sistema de origen, el servicio de aprovisionamiento usa la [característica de consulta diferencial de Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-differential-query) para supervisar usuarios y grupos. El servicio de aprovisionamiento ejecuta un ciclo inicial en el sistema de origen y el sistema de destino, seguido de ciclos incrementales periódicos.

### <a name="initial-cycle"></a>Ciclo inicial

Cuando se inicia el servicio de aprovisionamiento, la primera sincronización ejecutada será así:

1. Se consultan todos los usuarios y grupos del sistema de origen y se recuperan todos los atributos definidos en la [asignación de atributos](customize-application-attributes.md).
1. Se filtran los usuarios y grupos devueltos mediante cualquier [asignación](assign-user-or-group-access-portal.md) o [filtro de ámbito basado en atributos](define-conditional-rules-for-provisioning-user-accounts.md) configurados.
1. Cuando un usuario se ha asignado o está en ámbito para el aprovisionamiento, el servicio consulta el sistema de destino en busca de un usuario coincidente mediante los [atributos coincidentes](customize-application-attributes.md#understanding-attribute-mapping-properties) especificados. Ejemplo: Si el nombre de userPrincipal en el sistema de origen es el atributo coincidente y se asigna a userName en el sistema de destino, el servicio de aprovisionamiento consulta el sistema de destino en busca de valores userName que coincidan con los valores de nombre del sistema de origen.
1. Si no se encuentra un usuario coincidente en el sistema de destino, se crea mediante los atributos que devuelve el sistema de origen. Una vez que se crea la cuenta de usuario, el servicio de aprovisionamiento detecta y almacena en caché el identificador del sistema de destino del nuevo usuario, que se usa para ejecutar todas las futuras operaciones en dicho usuario.
1. Si se encuentra un usuario coincidente, se actualiza mediante los atributos que proporciona el sistema de origen. Una vez que la cuenta de usuario coincide, el servicio de aprovisionamiento detecta y almacena en caché el identificador del sistema de destino del nuevo usuario, que se usa para ejecutar todas las futuras operaciones en dicho usuario.
1. Si las asignaciones de atributos contienen atributos de "referencia", el servicio realiza actualizaciones adicionales en el sistema de destino para crear y vincular los objetos a los que se hace referencia. Por ejemplo, un usuario puede tener un atributo "Administrador" en el sistema de destino, que está vinculado a otro usuario creado en el sistema de destino.
1. Se conserva una marca de agua al final del ciclo inicial, que proporciona el punto de partida para los ciclos incrementales posteriores.

Algunas aplicaciones, como ServiceNow, G Suite y Box no solo admiten el aprovisionamiento de usuarios, sino también el de los grupos y sus miembros. En esos casos, si el aprovisionamiento de grupos está habilitado en las [asignaciones](customize-application-attributes.md), el servicio de aprovisionamiento sincroniza los usuarios y los grupos, y luego las pertenencias del grupo.

### <a name="incremental-cycles"></a>Ciclos incrementales

Después del ciclo inicial, todos los demás ciclos harán lo siguiente:

1. Se consulta el sistema de origen de los usuarios y grupos que se actualizaron desde la última marca de agua almacenada.
1. Se filtran los usuarios y grupos devueltos mediante cualquier [asignación](assign-user-or-group-access-portal.md) o [filtro de ámbito basado en atributos](define-conditional-rules-for-provisioning-user-accounts.md) configurados.
1. Cuando un usuario se ha asignado o está en ámbito para el aprovisionamiento, el servicio consulta el sistema de destino en busca de un usuario coincidente mediante los [atributos coincidentes](customize-application-attributes.md#understanding-attribute-mapping-properties) especificados.
1. Si no se encuentra un usuario coincidente en el sistema de destino, se crea mediante los atributos que devuelve el sistema de origen. Una vez que se crea la cuenta de usuario, el servicio de aprovisionamiento detecta y almacena en caché el identificador del sistema de destino del nuevo usuario, que se usa para ejecutar todas las futuras operaciones en dicho usuario.
1. Si se encuentra un usuario coincidente, se actualiza mediante los atributos que proporciona el sistema de origen. Si es una cuenta recién asignada la que coincide, el servicio de aprovisionamiento detecta y almacena en caché el identificador del sistema de destino del nuevo usuario, que se usa para ejecutar todas las futuras operaciones en dicho usuario.
1. Si las asignaciones de atributos contienen atributos de "referencia", el servicio realiza actualizaciones adicionales en el sistema de destino para crear y vincular los objetos a los que se hace referencia. Por ejemplo, un usuario puede tener un atributo "Administrador" en el sistema de destino, que está vinculado a otro usuario creado en el sistema de destino.
1. Si un usuario que estaba anteriormente en ámbito del aprovisionamiento se quita del ámbito (lo que incluye estar sin asignar), el servicio deshabilita al usuario en el sistema de destino mediante una actualización.
1. Si un usuario que estaba anteriormente en ámbito del aprovisionamiento se deshabilita o se elimina temporalmente en el sistema de origen, el servicio deshabilita al usuario en el sistema de destino mediante una actualización.
1. Si un usuario que estaba anteriormente en ámbito del aprovisionamiento se elimina permanentemente en el sistema de origen, el servicio elimina al usuario en el sistema de destino. En Azure AD, los usuarios se eliminan permanentemente 30 días después de que se hayan eliminado temporalmente.
1. Se conserva una nueva marca de agua al final del ciclo incremental, que proporciona el punto de partida para los ciclos incrementales posteriores.

> [!NOTE]
> Opcionalmente, puede deshabilitar las operaciones **Crear**, **Actualizar** o **Eliminar** mediante las casillas **Acciones del objeto de destino** de la sección [Asignaciones](customize-application-attributes.md). La lógica para deshabilitar un usuario durante una actualización también se controla mediante una asignación de atributos desde un campo como "accountEnabled".

El servicio de aprovisionamiento sigue ejecutando ciclos incrementales opuestos de manera indefinida, según intervalos definidos en el [tutorial específico de cada aplicación](../saas-apps/tutorial-list.md), hasta que se produzca uno de los siguientes eventos:

- El servicio se detiene manualmente mediante Azure Portal o por medio del comando de Graph API adecuado. 
- Se desencadena un nuevo ciclo inicial mediante la opción **Clear state and restart** (Borrar estado y reiniciar) de Azure Portal o por medio del comando de Graph API adecuado. Esta acción también borra cualquier marca de agua almacenada y hace que todos los objetos de origen se evalúen de nuevo.
- Se desencadena un nuevo ciclo inicial debido a un cambio en las asignaciones de atributos o los filtros de ámbito. Esta acción también borra cualquier marca de agua almacenada y hace que todos los objetos de origen se evalúen de nuevo.
- El proceso de aprovisionamiento entra en cuarentena (ver a continuación) debido a una alta tasa de errores y permanece en cuarentena durante más de cuatro semanas. En este caso, el servicio se deshabilita automáticamente.

### <a name="errors-and-retries"></a>Errores y reintentos

Si un usuario individual no se puede agregar, actualizar o eliminar en el sistema de destino debido a un error en dicho sistema, la operación se reintenta en el siguiente ciclo de sincronización. Si el error continúa, los reintentos comienzan a producirse según una frecuencia reducida y disminuyen gradualmente hasta un solo intento al día. Para resolver el error, los administradores deben comprobar los [registros de aprovisionamiento](../reports-monitoring/concept-provisioning-logs.md?context=azure/active-directory/manage-apps/context/manage-apps-context) para determinar la causa principal y realizar la acción apropiada. Algunos errores comunes son:

- Usuarios que no tienen relleno un atributo en el sistema de origen que es necesario en el sistema de destino
- Usuarios que tienen un valor de atributo en el sistema de origen para el que existe una restricción única en el sistema de destino, y el mismo valor está presente en otro registro de usuario

Estos errores pueden resolverse mediante el ajuste de los valores de atributo del usuario afectado en el sistema de origen, o por medio del ajuste de las asignaciones de atributos para no provocar conflictos.

### <a name="quarantine"></a>Cuarentena

Si todas o la mayoría de las llamadas realizadas al sistema de destino no tienen éxito sistemáticamente debido a un error (como en el caso de credenciales de administrador no válidas), el trabajo de aprovisionamiento entra en estado de "cuarentena". Este estado se indica en el [informe de resumen de aprovisionamiento](check-status-user-account-provisioning.md) y por correo electrónico si se han configurado las notificaciones por correo electrónico en Azure Portal.

En cuarentena, la frecuencia de los ciclos incrementales se reduce gradualmente a una vez al día.

El trabajo de aprovisionamiento se quita de la cuarentena después de que se hayan resuelto todos los errores causantes y se inicie el siguiente ciclo de sincronización. Si el trabajo de aprovisionamiento permanece en cuarentena durante más de cuatro semanas, se deshabilita.

## <a name="how-long-will-it-take-to-provision-users"></a>¿Cuánto tiempo se tarda en aprovisionar usuarios?

El rendimiento depende de si el trabajo de aprovisionamiento ejecuta un ciclo de aprovisionamiento inicial o un ciclo incremental. Para obtener detalles sobre el tiempo que tarda el aprovisionamiento y cómo supervisar el estado del servicio de aprovisionamiento, consulte [Averiguar cuándo un usuario específico podrá acceder a una aplicación](application-provisioning-when-will-provisioning-finish-specific-user.md).

## <a name="how-can-i-tell-if-users-are-being-provisioned-properly"></a>¿Cómo puedo saber si los usuarios se aprovisionan correctamente?

Todas las operaciones que ejecute el servicio de aprovisionamiento de usuarios se registran en los [registros de aprovisionamiento (versión preliminar)](../reports-monitoring/concept-provisioning-logs.md?context=azure/active-directory/manage-apps/context/manage-apps-context) de Azure AD. Esto incluye todas las operaciones de lectura y escritura realizadas en los sistemas de origen y de destino, así como los datos del usuario que se leyeron o escribieron durante cada operación.

Para obtener información sobre cómo leer los registros de aprovisionamiento en Azure Portal, consulte la [guía de informes de aprovisionamiento](check-status-user-account-provisioning.md).

## <a name="how-do-i-troubleshoot-issues-with-user-provisioning"></a>¿Cómo resolver problemas con el aprovisionamiento de usuarios?

Para ver instrucciones basadas en escenarios sobre cómo solucionar problemas de aprovisionamiento automático de usuarios, consulte [Problemas al configurar y aprovisionar usuarios en una aplicación](application-provisioning-config-problem.md).

## <a name="what-are-the-best-practices-for-rolling-out-automatic-user-provisioning"></a>¿Cuáles son los procedimientos recomendados para la implementación del aprovisionamiento automático de usuarios?

> [!VIDEO https://www.youtube.com/embed/MAy8s5WSe3A]

Para obtener un ejemplo del plan de implementación detallado para el aprovisionamiento de usuarios saliente a una aplicación, consulte la [guía de implementación de identidad para el aprovisionamiento de usuarios](https://aka.ms/userprovisioningdeploymentplan).

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

### <a name="does-automatic-user-provisioning-to-saas-apps-work-with-b2b-users-in-azure-ad"></a>¿Funciona el aprovisionamiento automático de usuarios para aplicaciones SaaS con usuarios de B2B en Azure AD?

Sí, el servicio de aprovisionamiento de usuarios de Azure AD se puede usar para aprovisionar usuarios de B2B (o invitados) en Azure AD para aplicaciones SaaS.

Sin embargo, para que los usuarios de B2B inicien sesión en la aplicación SaaS mediante Azure AD, esta debe tener su funcionalidad de inicio de sesión único basado en SAML configurada de una forma concreta. Para más información acerca de cómo configurar aplicaciones SaaS para admitir inicios de sesión de usuarios de B2B, consulte [Configuración de aplicaciones de SaaS para la colaboración B2B]( https://docs.microsoft.com/azure/active-directory/b2b/configure-saas-apps).

### <a name="does-automatic-user-provisioning-to-saas-apps-work-with-dynamic-groups-in-azure-ad"></a>¿Funciona el aprovisionamiento automático de usuarios para aplicaciones SaaS con grupos dinámicos en Azure AD?

Sí. Si está configurado para "sincronizar solo usuarios y grupos asignados", el servicio de aprovisionamiento de usuarios de Azure AD puede aprovisionar o desaprovisionar los usuarios de una aplicación SaaS en función de si son miembros de un [grupo dinámico](../users-groups-roles/groups-create-rule.md). Los grupos dinámicos también funcionan con la opción "sincronizar todos los usuarios y grupos".

Sin embargo, el uso de grupos dinámicos puede afectar al rendimiento general del aprovisionamiento de usuarios de un extremo a otro desde Azure AD a las aplicaciones SaaS. Cuando use grupos dinámicos, tenga en cuenta estas advertencias y recomendaciones:

- La velocidad con la se aprovisiona o desaprovisiona un usuario de un grupo dinámico en una aplicación SaaS depende de la rapidez con la que el grupo dinámico pueda evaluar los cambios de pertenencia. Para obtener información acerca de cómo comprobar el estado de procesamiento de un grupo dinámico, consulte [Comprobación del estado de procesamiento de una regla de pertenencia](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-create-rule).

- Cuando se usan grupos dinámicos, deben tenerse muy en cuenta las reglas y pensar en el aprovisionamiento y desaprovisionamiento de usuarios, ya que una pérdida de pertenencia provocará un evento de desaprovisionamiento.

### <a name="does-automatic-user-provisioning-to-saas-apps-work-with-nested-groups-in-azure-ad"></a>¿Funciona el aprovisionamiento automático de usuarios para aplicaciones SaaS con grupos anidados en Azure AD?

No. Cuando se configura para "sincronizar solo los usuarios y grupos asignados", el servicio de aprovisionamiento de usuarios de Azure AD no puede leer ni aprovisionar usuarios que estén en grupos anidados. Solo puede leer y aprovisionar aquellos usuarios que son miembros inmediatos del grupo asignado explícitamente.

Esta es una limitación de las "asignaciones basadas en grupos a aplicaciones" que también afecta al inicio de sesión único y se describe en [Uso de un grupo para administrar el acceso a aplicaciones SaaS](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-saasapps ).

Como alternativa, debe asignar explícitamente (o de otro modo [definir el ámbito](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)) los grupos que contengan los usuarios que se deban aprovisionar.

### <a name="is-provisioning-between-azure-ad-and-a-target-application-using-an-encrypted-channel"></a>¿Se está aprovisionando entre Azure AD y una aplicación de destino mediante un canal cifrado?

Sí. Usamos el cifrado SSL de HTTPS para el destino de servidor.

## <a name="related-articles"></a>Artículos relacionados

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS](../saas-apps/tutorial-list.md)
- [Personalización de asignaciones de atributos para el aprovisionamiento de usuarios](customize-application-attributes.md)
- [Escritura de expresiones para la asignación de atributos](functions-for-customizing-application-data.md)
- [Filtros de ámbito para el aprovisionamiento de usuarios](define-conditional-rules-for-provisioning-user-accounts.md)
- [Uso de SCIM para habilitar el aprovisionamiento automático de usuarios y grupos de Azure AD a aplicaciones](use-scim-to-provision-users-and-groups.md)
- [Introducción a la API de sincronización de Azure AD](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-overview)
