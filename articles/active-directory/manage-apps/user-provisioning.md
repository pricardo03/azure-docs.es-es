---
title: Aprovisionamiento automatizado de usuarios para aplicaciones SaaS en Azure AD | Microsoft Docs
description: Una introducción sobre cómo puede usar Azure AD para el aprovisionamiento, el desaprovisionamiento y la actualización continua de cuentas de usuario de manera automática en varias aplicaciones SaaS de terceros.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/30/2018
ms.author: barbkess
ms.reviewer: asmalser
ms.openlocfilehash: ac58c6b951a03b403375fdc17dcd45f8e624deac
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2018
ms.locfileid: "52311460"
---
# <a name="automate-user-provisioning-and-deprovisioning-to-saas-applications-with-azure-active-directory"></a>Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory

## <a name="what-is-automated-user-provisioning-for-saas-apps"></a>¿Qué es el aprovisionamiento automático de usuarios para aplicaciones SaaS?
Azure Active Directory (Azure AD) permite automatizar la creación, el mantenimiento y la eliminación de identidades de usuario en aplicaciones de nube ([SaaS](https://azure.microsoft.com/overview/what-is-saas/)) como Dropbox, Salesforce, ServiceNow y muchas más.

> [!VIDEO https://www.youtube.com/embed/_ZjARPpI6NI]

**A continuación se muestran algunos ejemplos de lo que esta característica permite hacer:**

* Crear cuentas automáticamente en los sistemas adecuados para los usuarios nuevos cuando se unen a su equipo u organización.
* Desactivar las cuentas automáticamente en los sistemas adecuados cuando los usuarios dejan el equipo o la organización.
* Asegurarse de que las identidades de las aplicaciones y sistemas se mantienen actualizadas con los cambios del directorio o el sistema de recursos humanos.
* Aprovisionar los objetos que no sean de los usuarios, como los grupos, para las aplicaciones que los admitan.

**El aprovisionamiento automático de usuarios también incluye la funcionalidad siguiente:**

* La capacidad de hacer coincidir las identidades existentes entre los sistemas de origen y de destino.
* Asignaciones de atributos personalizables que definen qué datos del usuario deben fluir del sistema de origen al sistema de destino.
* Alertas de correo electrónico opcionales para errores de aprovisionamiento.
* Informes y registros de actividades para facilitar la supervisión y solución de problemas.

## <a name="why-use-automated-provisioning"></a>¿Por qué usar el aprovisionamiento automático?

Las razones habituales por las que se debe usar esta característica son:

* Evitar los costos, las ineficiencias y los errores humanos asociados con los procesos de aprovisionamiento manuales.
* Evitar los costos asociados al hospedaje y el mantenimiento de scripts y soluciones de aprovisionamiento desarrollados de forma personalizada.
* Proteger su organización mediante la eliminación instantánea de las identidades de los usuarios de aplicaciones SaaS claves cuando estos abandonan la organización.
* Importar fácilmente una gran cantidad de usuarios a una aplicación o sistema SaaS concretos.
* Disfrutar de tener un único conjunto de directivas para determinar quién se aprovisiona y quién puede iniciar sesión en una aplicación.

## <a name="how-does-automatic-provisioning-work"></a>¿Cómo funciona el aprovisionamiento automático?
    
El **servicio de aprovisionamiento de Azure AD** aprovisiona usuarios para las aplicaciones SaaS y otros sistemas mediante la conexión de los puntos de conexión de la API de administración de usuarios proporcionados por el proveedor de cada aplicación. Estos puntos de conexión de la API de administración de usuarios permiten a Azure AD crear, actualizar y quitar usuarios mediante programación. Para las aplicaciones seleccionadas, el servicio de aprovisionamiento también puede crear, actualizar y quitar objetos adicionales relacionados con la identidad, como los grupos y los roles. 

![Aprovisionamiento](./media/user-provisioning/provisioning0.PNG)
*Figura 1: Servicio de aprovisionamiento de Azure AD*

![Aprovisionamiento de salida](./media/user-provisioning/provisioning1.PNG)
*Figura 2: Flujo de trabajo "saliente" del aprovisionamiento de usuarios desde Azure AD a aplicaciones SaaS populares*

![Aprovisionamiento de entrada](./media/user-provisioning/provisioning2.PNG)
*Figura 3: Flujo de trabajo "entrante" del aprovisionamiento de usuarios desde aplicaciones populares de administración del capital humano (HCM) a Azure Active Directory y Windows Server Active Directory*


## <a name="what-applications-and-systems-can-i-use-with-azure-ad-automatic-user-provisioning"></a>¿Qué aplicaciones y sistemas puedo usar con el aprovisionamiento automático de usuarios de Azure AD?

Las características preintegradas de Azure AD admiten una gran variedad de aplicaciones SaaS populares y sistemas de recursos humanos, así como compatibilidad genérica con aplicaciones que implementan determinadas partes del estándar SCIM 2.0.

### <a name="pre-integrated-applications"></a>Aplicaciones preintegradas

Para ver una lista de todas las aplicaciones en las que Azure AD admite un conector de aprovisionamiento previamente integrado, consulte la [lista de tutoriales de aplicaciones de aprovisionamiento de usuarios](../saas-apps/tutorial-list.md).

Si desea ponerse en contacto con el equipo de ingeniería de Azure AD para solicitar soporte técnico para el aprovisionamiento de aplicaciones adicionales, envíe un mensaje a través del [foro de comentarios de Azure Active Directory](https://feedback.azure.com/forums/374982-azure-active-directory-application-requests/filters/new?category_id=172035).

> [!NOTE]
> Para que una aplicación admita el aprovisionamiento automático de usuarios, primero debe proporcionar las API de administración de usuarios necesarias que permitan a los programas externos automatizar la creación, el mantenimiento y la eliminación de usuarios. Por lo tanto, no todas las aplicaciones SaaS son compatibles con esta característica. En el caso de las aplicaciones que sí son compatibles con las API de administración de usuarios, el equipo de ingeniería de Azure AD podrá crear un conector de aprovisionamiento para esas aplicaciones, y las prioridades para realizar este trabajo se basan en las necesidades de los clientes actuales y potenciales. 

### <a name="connecting-applications-that-support-scim-20"></a>Conexión de aplicaciones que admiten SCIM 2.0

Para obtener información sobre cómo conectar aplicaciones de forma general que implementen API de administración de usuario basado en SCIM 2.0, consulte [Uso de SCIM para aprovisionar automáticamente a los usuarios y grupos de Azure Active Directory para aplicaciones](use-scim-to-provision-users-and-groups.md).

    
## <a name="how-do-i-set-up-automatic-provisioning-to-an-application"></a>¿Cómo configuro el aprovisionamiento automático para una aplicación?

> [!VIDEO https://www.youtube.com/embed/pKzyts6kfrw]

La configuración del servicio de aprovisionamiento de Azure AD para una aplicación seleccionada se inicia en  **[Azure Portal](https://portal.azure.com)**. En la sección **Azure Active Directory > Aplicaciones empresariales**, seleccione **Agregar**, **Todos** y, a continuación, agregue lo que necesite entre los elementos siguientes en función de su escenario:

* Todas las aplicaciones de la sección **Aplicaciones destacadas** admiten el aprovisionamiento automático. Consulte la [lista de tutoriales de aplicaciones para el aprovisionamiento de usuarios](../saas-apps/tutorial-list.md) para ver más.

* Use la opción "Aplicación situada fuera de la galería" para las integraciones de SCIM desarrolladas de forma personalizada

![Gallery](./media/user-provisioning/gallery.png)

En la pantalla de administración de aplicaciones, el aprovisionamiento se configura en la pestaña **Aprovisionamiento**.

![Configuración](./media/user-provisioning/provisioning_settings0.PNG)


* Se deben proporcionar las **credenciales de administrador** al servicio de aprovisionamiento de Azure AD que le permitirá conectarse a la API de administración de usuarios proporcionada por la aplicación. Esta sección también le permite habilitar notificaciones por correo electrónico si las credenciales producen error o el trabajo de aprovisionamiento entra en [cuarentena](#quarantine).

* Se pueden configurar **asignaciones de atributo** que especifiquen qué campos del sistema de origen (ejemplo: Azure AD) tendrán su contenido sincronizado con los campos del sistema de destino (ejemplo: ServiceNow). Si la aplicación de destino lo admite, esta sección le permitirá configurar, si lo desea, el aprovisionamiento de grupos, además de las cuentas de usuario. La "búsqueda de coincidencia de propiedades" le permite seleccionar los campos que se usan para buscar cuentas entre los sistemas. Las "[expresiones](functions-for-customizing-application-data.md)" le permiten modificar y transformar los valores recuperados del sistema de origen antes de que se escriban en el sistema de destino. Para obtener más información, vea [Personalización de la asignación de atributos](customize-application-attributes.md).

![Configuración](./media/user-provisioning/provisioning_settings1.PNG)

* Los **filtros de ámbito** le indican al servicio de aprovisionamiento qué usuarios y grupo del sistema de origen se deben aprovisionar o desaprovisionar para el sistema de destino. Hay dos aspectos en los filtros de ámbito que se evalúan conjuntamente y que determinan quién está en el ámbito para el aprovisionamiento:

    * **Filter on attribute values** (Filtrar por valores de atributo): el menú "Ámbito de objeto de origen" en las asignaciones de atributos permite el filtrado según valores de atributo determinados. Por ejemplo, puede especificar que solo los usuarios con el atributo "Department" (Departamento) de "Sales" (Ventas) deben estar en el ámbito del aprovisionamiento. Para más información, consulte [Uso de filtros de ámbito](define-conditional-rules-for-provisioning-user-accounts.md).

    * **Filter on assignments** (Filtrar por asignaciones): el menú "Ámbito" en la sección Aprovisionamiento &gt; Configuración del portal le permite especificar si solo los usuarios y los grupos "asignados" deben estar en el ámbito de aprovisionamiento o si se deben aprovisionar todos los usuarios del directorio de Azure AD. Para obtener más información sobre la "asignación" de usuarios y grupos, consulte [Asignación de un usuario o un grupo a una aplicación empresarial en Azure Active Directory](assign-user-or-group-access-portal.md).
    
* La **configuración** controla el funcionamiento del servicio de aprovisionamiento de una aplicación, tanto si se está ejecutando como si no.

* Los **registros de auditoría** proporcionan registros de todas las operaciones realizadas por el servicio de aprovisionamiento de Azure AD. Para obtener más detalles, consulte la [guía de informes de aprovisionamiento](check-status-user-account-provisioning.md).

![Configuración](./media/user-provisioning/audit_logs.PNG)

> [!NOTE]
> El servicio de aprovisionamiento de usuarios de Azure AD también se puede configurar y administrar mediante [Microsoft Graph API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-overview).


## <a name="what-happens-during-provisioning"></a>¿Qué ocurre durante el aprovisionamiento?

Si Azure AD es el sistema de origen, el servicio de aprovisionamiento usa la [característica de consulta diferencial de Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-differential-query) para supervisar usuarios y grupos. El servicio de aprovisionamiento ejecuta una sincronización con el sistema de origen y el sistema de destino, seguida de sincronizaciones incrementales periódicas. 

### <a name="initial-sync"></a>Sincronización inicial

Cuando se inicia el servicio de aprovisionamiento, la primera sincronización realizada será así:

1. Se consultan todos los usuarios y grupos del sistema de origen y se recuperan todos los atributos definidos en la [asignación de atributos](customize-application-attributes.md).
2. Se filtran los usuarios y grupos devueltos mediante cualquier [asignación](assign-user-or-group-access-portal.md) o [filtro de ámbito basado en atributos](define-conditional-rules-for-provisioning-user-accounts.md) configurados.
3. Cuando se detecta que un usuario se ha asignado o está en ámbito para el aprovisionamiento, el servicio consulta el sistema de destino en busca de un usuario coincidente mediante los [atributos coincidentes](customize-application-attributes.md#understanding-attribute-mapping-properties) designados. Ejemplo: si el nombre de userPrincipal en el sistema de origen es el atributo coincidente y se asigna a userName en el sistema de destino, el servicio de aprovisionamiento consulta el sistema de destino en busca de valores userName que coincidan con los valores de nombre del sistema de origen.
4. Si no se encuentra un usuario coincidente en el sistema de destino, se crea mediante los atributos devueltos por el sistema de origen. Una vez que se crea la cuenta de usuario, el servicio de aprovisionamiento detecta y almacena en la caché el identificador del sistema de destino del nuevo usuario, que se usa para realizar todas las futuras operaciones en dicho usuario.
5. Si se encuentra un usuario coincidente, se actualiza mediante los atributos proporcionados por el sistema de origen. Una vez que la cuenta de usuario coincide, el servicio de aprovisionamiento detecta y almacena en la caché el identificador del sistema de destino del nuevo usuario, que se usa para realizar todas las futuras operaciones en dicho usuario.
6. Si las asignaciones de atributos contienen atributos de "referencia", el servicio realiza actualizaciones adicionales en el sistema de destino para crear y vincular los objetos a los que se hace referencia. Por ejemplo, un usuario puede tener un atributo "Administrador" en el sistema de destino, que está vinculado a otro usuario creado en el sistema de destino.
7. Se conserva una marca de agua al final de la sincronización inicial, que proporciona el punto de partida para las sucesivas sincronizaciones incrementales.

Algunas aplicaciones, como ServiceNow, Google Apps y Box no solo admiten el aprovisionamiento de usuarios, sino también el de los grupos y sus miembros. En esos casos, si el aprovisionamiento de grupos está habilitado en las [asignaciones](customize-application-attributes.md), el servicio de aprovisionamiento sincroniza los usuarios y los grupos y luego las pertenencias del grupo. 

### <a name="incremental-syncs"></a>Sincronizaciones incrementales

Después de la sincronización inicial, todas las sincronizaciones posteriores serán así:

1. Se consulta el sistema de origen de los usuarios y grupos que se actualizaron desde la última marca de agua almacenada.
2. Se filtran los usuarios y grupos devueltos mediante cualquier [asignación](assign-user-or-group-access-portal.md) o [filtro de ámbito basado en atributos](define-conditional-rules-for-provisioning-user-accounts.md) configurados.
3. Cuando se detecta que un usuario se ha asignado o está en ámbito para el aprovisionamiento, el servicio consulta el sistema de destino en busca de un usuario coincidente mediante los [atributos coincidentes](customize-application-attributes.md#understanding-attribute-mapping-properties) designados.
4. Si no se encuentra un usuario coincidente en el sistema de destino, se crea mediante los atributos devueltos por el sistema de origen. Una vez que se crea la cuenta de usuario, el servicio de aprovisionamiento detecta y almacena en la caché el identificador del sistema de destino del nuevo usuario, que se usa para realizar todas las futuras operaciones en dicho usuario.
5. Si se encuentra un usuario coincidente, se actualiza mediante los atributos proporcionados por el sistema de origen. SI es una cuenta recién asignada la que coincide, el servicio de aprovisionamiento detecta y almacena en la caché el identificador del sistema de destino del nuevo usuario, que se usa para realizar todas las futuras operaciones en dicho usuario.
6. Si las asignaciones de atributos contienen atributos de "referencia", el servicio realiza actualizaciones adicionales en el sistema de destino para crear y vincular los objetos a los que se hace referencia. Por ejemplo, un usuario puede tener un atributo "Administrador" en el sistema de destino, que está vinculado a otro usuario creado en el sistema de destino.
7. Si un usuario que estaba anteriormente en ámbito del aprovisionamiento se quita del ámbito (lo que incluye estar sin asignar), el servicio deshabilita al usuario en el sistema de destino mediante una actualización.
8. Si un usuario que estaba anteriormente en ámbito del aprovisionamiento se deshabilita o se elimina temporalmente en el sistema de origen, el servicio deshabilita al usuario en el sistema de destino mediante una actualización.
9. Si un usuario que estaba anteriormente en ámbito del aprovisionamiento se elimina permanentemente en el sistema de origen, el servicio elimina al usuario en el sistema de destino. En Azure AD, los usuarios se eliminan permanentemente 30 días después de que se han eliminado temporalmente.
10. Se conserva una nueva marca de agua al final de la sincronización incremental, que proporciona el punto de partida para las sucesivas sincronizaciones incrementales.

>[!NOTE]
> Opcionalmente, puede deshabilitar las operaciones de creación, actualización o eliminación mediante las casillas **Acciones del objeto de destino** de la sección [Asignaciones de atributos](customize-application-attributes.md). La lógica para deshabilitar un usuario durante una actualización también se controla mediante una asignación de atributos desde un campo como "accountEnabled".

El servicio de aprovisionamiento sigue ejecutando sincronizaciones incrementales opuestas de manera indefinida, según intervalos definidos en el [tutorial específico de cada aplicación](../saas-apps/tutorial-list.md), hasta que se produce uno de los siguientes eventos:

* El servicio se detiene manualmente mediante Azure Portal o por medio del comando de Graph API adecuado. 
* Se desencadena una nueva sincronización inicial mediante la opción **Clear state and restart** (Borrar estado y reiniciar) de Azure Portal o por medio del comando de Graph API adecuado. Esta sincronización borra cualquier marca de agua almacenada y hace que todos los objetos de origen se evalúen de nuevo.
* Se desencadena una nueva sincronización inicial debido a un cambio en las asignaciones de atributos o los filtros de ámbito. Esta sincronización también borra cualquier marca de agua almacenada y hace que todos los objetos de origen se evalúen de nuevo.
* El proceso de aprovisionamiento entra en cuarentena (ver a continuación) debido a una alta tasa de errores y permanece en cuarentena durante más de cuatro semanas. En este caso, el servicio se deshabilita automáticamente.

### <a name="errors-and-retries"></a>Errores y reintentos

Si un usuario individual no se puede agregar, actualizar o eliminar en el sistema de destino debido a un error en dicho sistema, la operación se reintenta en el siguiente ciclo de sincronización. Si el error continúa, los reintentos comienzan a producirse según una frecuencia reducida y disminuyen gradualmente hasta un solo intento al día. Para resolver el error, los administradores deben comprobar los [registros de auditoría](check-status-user-account-provisioning.md) en busca de eventos de "custodia de proceso" para determinar la causa y realizar la acción apropiada. Algunos errores comunes son:

* Usuarios que no tienen relleno un atributo en el sistema de origen que es necesario en el sistema de destino
* Usuarios que tienen un valor de atributo en el sistema de origen para el que existe una restricción única en el sistema de destino, y el mismo valor está presente en otro registro de usuario

Estos errores pueden resolverse mediante el ajuste de los valores de atributo del usuario afectado en el sistema de origen, o por medio del ajuste de las asignaciones de atributos para no provocar conflictos.   

### <a name="quarantine"></a>Cuarentena

Si todas o la mayoría de las llamadas realizadas al sistema de destino no tienen éxito sistemáticamente debido a un error (como en el caso de credenciales de administrador no válidas), el trabajo de aprovisionamiento entra en estado de "cuarentena". Esto se indica en el [informe de resumen de aprovisionamiento](check-status-user-account-provisioning.md) y por correo electrónico si se han configurado las notificaciones por correo electrónico en Azure Portal. 

En cuarentena, la frecuencia de las sincronizaciones incrementales se reduce gradualmente a una vez al día. 

El trabajo de aprovisionamiento se quita de la cuarentena después de que se han resuelto todos los errores causantes y se inicia el siguiente ciclo de sincronización. Si el trabajo de aprovisionamiento permanece en cuarentena durante más de cuatro semanas, se deshabilita.


## <a name="how-long-will-it-take-to-provision-users"></a>¿Cuánto tiempo se tarda en aprovisionar usuarios?

El rendimiento será diferente en función de si el trabajo de aprovisionamiento realiza una sincronización inicial o una sincronización incremental, tal como se describió en la sección anterior.

Para las **sincronizaciones iniciales**, el tiempo de trabajo depende de ciertos factores, entre los que se incluye la cantidad de usuarios y grupos en el ámbito para realizar el aprovisionamiento, y la cantidad total de usuarios y grupos en el sistema de origen. En esta sección se indica más adelante una lista completa de factores que afectan al rendimiento inicial de la sincronización.

En cuanto a las **sincronizaciones incrementales**, el tiempo de trabajo depende del número de cambios detectados en ese ciclo de sincronización. Si hay menos de 5000 cambios de pertenencia de un grupo o usuario, el trabajo puede finalizar dentro de un solo ciclo de sincronización incremental. 

En la siguiente tabla se resumen los tiempos de sincronización de escenarios comunes de aprovisionamiento. En estos escenarios, el sistema de origen es Azure AD y el sistema de destino es una aplicación SaaS. Los tiempos de sincronización se derivan de un análisis estadístico de trabajos de sincronización para las aplicaciones SaaS de ServiceNow, Workplace, Salesforce y Google Apps.


| Configuración de ámbito | Usuarios, grupos y miembros del ámbito | Hora de sincronización inicial | Hora de sincronización incremental |
| -------- | -------- | -------- | -------- |
| Sincronizar solo los usuarios y grupos asignados |  < 1000 |  < 30 minutos | < 30 minutos |
| Sincronizar solo los usuarios y grupos asignados |  1000 - 10.000 | 142 - 708 minutos | < 30 minutos |
| Sincronizar solo los usuarios y grupos asignados |   10.000 - 100.000 | 1170 - 2340 minutos | < 30 minutos |
| Sincronizar todos los usuarios y grupos en Azure AD |  < 1000 | < 30 minutos  | < 30 minutos |
| Sincronizar todos los usuarios y grupos en Azure AD |  1000 - 10.000 | < 30 - 120 minutos | < 30 minutos |
| Sincronizar todos los usuarios y grupos en Azure AD |  10.000 - 100.000  | 713 - 1425 minutos | < 30 minutos |
| Sincronizar todos usuarios en Azure AD|  < 1000  | < 30 minutos | < 30 minutos |
| Sincronizar todos usuarios en Azure AD | 1000 - 10.000  | 43 - 86 minutos | < 30 minutos |


Para la configuración dedicada a **sincronizar únicamente los usuarios y grupos asignados**, puede usar las siguientes fórmulas para determinar aproximadamente las veces que se realiza la **sincronización inicial** mínima y máxima esperada:

    Minimum minutes =  0.01 x [Number of assigned users, groups, and group members]
    Maximum minutes = 0.08 x [Number of assigned users, groups, and group members] 
    
Resumen de los factores que influyen en el tiempo que lleva completar una **sincronización inicial**:

* El número total de usuarios y grupos del ámbito para el aprovisionamiento.

* La cantidad total de usuarios, grupos y miembros del grupo presentes en el sistema de origen (Azure AD).

* Si los usuarios del ámbito de aprovisionamiento se corresponden o no con los usuarios existentes en la aplicación de destino, o si deben crearse por primera vez. Las tareas de sincronización para las cuales se crean todos los usuarios por primera vez, llevan aproximadamente *el doble de tiempo* que los trabajos de sincronización para los cuales todos los usuarios se corresponden con usuarios existentes.

* Número de errores en los [registros de auditoría](check-status-user-account-provisioning.md). El rendimiento es también menor si hay muchos errores y si el servicio de aprovisionamiento ha quedado en un estado de "cuarentena". 

* Solicitar los límites de velocidad y otras limitaciones que haya implementado el sistema de destino. Algunos sistemas de destino implementan límites de velocidad y otras limitaciones en las solicitudes, que pueden afectar al rendimiento durante operaciones de sincronización grandes. En estos casos, una aplicación que recibe demasiadas solicitudes demasiado rápido puede ralentizar su velocidad de respuesta o cerrar la conexión. Para mejorar el rendimiento, el conector debe ajustarse; para ello, no debe enviar las solicitudes de la aplicación más rápido de lo que la aplicación puede procesarlas. Los conectores de aprovisionamiento que ha compilado Microsoft hacen este ajuste. 

* La cantidad y el tamaño de los grupos asignados. Sincronizar grupos asignados lleva más tiempo que sincronizar usuarios. Tanto el número como los tamaños de los grupos asignados afectan al rendimiento. Si una aplicación tiene [asignaciones habilitadas para la sincronización de objetos de grupo](customize-application-attributes.md#editing-group-attribute-mappings), las propiedades del grupo, como los nombres y la pertenencia, se sincronizan además de los usuarios. Estas sincronizaciones adicionales tomarán más tiempo que solo la sincronización de objetos de usuario.


## <a name="how-can-i-tell-if-users-are-being-provisioned-properly"></a>¿Cómo puedo saber si los usuarios se aprovisionan correctamente?

Todas las operaciones realizadas por el servicio de aprovisionamiento de usuarios se registran en los registros de auditoría de Azure AD. Esto incluye todas las operaciones de lectura y escritura realizadas en los sistemas de origen y de destino, así como los datos del usuario que se leyeron o escribieron durante cada operación.

Para obtener información sobre cómo leer los registros de auditoría en Azure Portal, consulte la [guía de informes de aprovisionamiento](check-status-user-account-provisioning.md).


## <a name="how-do-i-troubleshoot-issues-with-user-provisioning"></a>¿Cómo resolver problemas con el aprovisionamiento de usuarios?

Para ver instrucciones basadas en escenarios sobre cómo solucionar problemas de aprovisionamiento automático de usuarios, consulte [Problemas al configurar y aprovisionar usuarios en una aplicación](application-provisioning-config-problem.md).


## <a name="what-are-the-best-practices-for-rolling-out-automatic-user-provisioning"></a>¿Cuáles son los procedimientos recomendados para la implementación del aprovisionamiento automático de usuarios?

> [!VIDEO https://www.youtube.com/embed/MAy8s5WSe3A]

Para obtener un ejemplo del plan de implementación detallado para el aprovisionamiento de usuarios saliente a una aplicación, consulte la [guía de implementación de identidad para el aprovisionamiento de usuarios](https://aka.ms/userprovisioningdeploymentplan).

## <a name="more-frequently-asked-questions"></a>Preguntas más frecuentes

### <a name="does-automatic-user-provisioning-to-saas-apps-work-with-b2b-users-in-azure-ad"></a>¿Funciona el aprovisionamiento automático de usuarios para aplicaciones SaaS con usuarios de B2B en Azure AD?

Sí, el servicio de aprovisionamiento de usuarios de Azure AD se puede usar para aprovisionar usuarios de B2B (o invitados) en Azure AD para aplicaciones SaaS.

Sin embargo, para que los usuarios de B2B puedan iniciar sesión en la aplicación SaaS mediante Azure AD, esta debe tener su funcionalidad de inicio de sesión único basado en SAML configurada de una forma concreta. Para más información acerca de cómo configurar aplicaciones SaaS para admitir inicios de sesión de usuarios de B2B, consulte [Configuración de aplicaciones de SaaS para la colaboración B2B]( https://docs.microsoft.com/azure/active-directory/b2b/configure-saas-apps).

### <a name="does-automatic-user-provisioning-to-saas-apps-work-with-dynamic-groups-in-azure-ad"></a>¿Funciona el aprovisionamiento automático de usuarios para aplicaciones SaaS con grupos dinámicos en Azure AD?

Sí. Si está configurado para "sincronizar solo usuarios y grupos asignados", el servicio de aprovisionamiento de usuarios de Azure AD puede aprovisionar o desaprovisionar los usuarios de una aplicación SaaS en función de que sean miembros, o no, de un [grupo dinámico](../users-groups-roles/groups-create-rule.md). Los grupos dinámicos también funcionan con la opción "sincronizar todos los usuarios y grupos".

Sin embargo, el uso de grupos dinámicos puede afectar al rendimiento general del aprovisionamiento de usuarios de un extremo a otro desde Azure AD a las aplicaciones SaaS. Cuando use grupos dinámicos, tenga en cuenta estas advertencias y recomendaciones:

* La velocidad con la se aprovisiona o desaprovisiona un usuario de un grupo dinámico en una aplicación SaaS depende de la rapidez con la que el grupo dinámico pueda evaluar los cambios de pertenencia. Para obtener información acerca de cómo comprobar el estado de procesamiento de un grupo dinámico, consulte [Comprobación del estado de procesamiento de una regla de pertenencia](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-create-rule#check-processing-status-for-a-membership-rule).

* Cuando se usan grupos dinámicos, deben tenerse muy en cuenta y se debe pensar en el aprovisionamiento y desaprovisionamiento de usuarios, ya que una pérdida de pertenencia provocará un evento de desaprovisionamiento.

### <a name="does-automatic-user-provisioning-to-saas-apps-work-with-nested-groups-in-azure-ad"></a>¿Funciona el aprovisionamiento automático de usuarios para aplicaciones SaaS con grupos anidados en Azure AD?

 No. Cuando se configura para "sincronizar solo los usuarios y grupos asignados", el servicio de aprovisionamiento de usuarios de Azure AD no puede leer ni aprovisionamiento usuarios que estén en grupos anidados. Solo puede leer y aprovisionar aquellos usuarios que son miembros inmediatos del grupo asignado explícitamente.

Esta es una limitación de las "asignaciones basadas en grupos a aplicaciones" que también afecta al inicio de sesión único y se describe en [Uso de un grupo para administrar el acceso a aplicaciones SaaS](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-saasapps ).

Como alternativa, debe asignar explícitamente (o [definir el ámbito](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts) de cualquier otro modo) los grupos que contengan los usuarios que tenga que aprovisionar.

## <a name="related-articles"></a>Artículos relacionados

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS](../saas-apps/tutorial-list.md)
* [Personalización de asignaciones de atributos para el aprovisionamiento de usuarios](customize-application-attributes.md)
* [Escritura de expresiones para asignaciones de atributos](functions-for-customizing-application-data.md)
* [Filtros de ámbito para el aprovisionamiento de usuario](define-conditional-rules-for-provisioning-user-accounts.md)
* [Uso de SCIM para habilitar el aprovisionamiento automático de usuarios y grupos de Azure Active Directory a aplicaciones](use-scim-to-provision-users-and-groups.md)
* [Introducción a la API de sincronización de Azure AD](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-overview)
