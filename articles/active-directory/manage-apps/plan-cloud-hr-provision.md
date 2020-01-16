---
title: Planeamiento de la aplicación de RR. HH. en la nube para el aprovisionamiento de usuarios de Azure Active Directory
description: En este artículo se describe el proceso de implementación de la integración de sistemas de RR. HH. en la nube, como WorkDay y Sucessfactors, con Azure Active Directory. La integración de Azure AD con el sistema de RR. HH. en la nube tiene como resultado un sistema completo de administración del ciclo de vida de la identidad.
services: active-directory
author: martincoetzer
manager: CelesteDG
tags: azuread
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: conceptual
ms.workload: identity
ms.date: 11/22/2019
ms.author: martinco
ms.reviewer: arvindha
ms.openlocfilehash: 6f72371077aab813cc22c9bbbe755fdfaa9ac00a
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75433834"
---
# <a name="plan-cloud-hr-application-to-azure-active-directory-user-provisioning"></a>Planeamiento de la aplicación de RR. HH. en la nube para el aprovisionamiento de usuarios de Azure Active Directory

Históricamente, el personal de TI ha confiado en métodos manuales de creación, actualización y eliminación de empleados, como la carga de archivos CSV o scripts personalizados para sincronizar los datos de los empleados. Estos procesos de aprovisionamiento son propensos a errores, inseguros y difíciles de administrar.

Para administrar sin problemas los ciclos de vida de la identidad de un extremo a otro de los empleados, proveedores o trabajadores temporales, el [servicio de aprovisionamiento de usuarios de Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning) ofrece integración con aplicaciones de recursos humanos (RR. HH.) basadas en la nube, como WorkDay o SuccessFactors.

Azure AD usa esta integración para habilitar los siguientes flujos de trabajo de aplicación de RR. HH. en la nube:

- **Aprovisionamiento de usuarios en AD**: aprovisione conjuntos de usuarios seleccionados de una aplicación de RR. HH. en la nube en uno o varios dominios de Active Directory (AD).
- **Aprovisionamiento de usuarios solo en la nube en Azure AD**: aprovisione usuarios directamente de la aplicación de RR. HH. en la nube en Azure AD en escenarios donde no se usa AD.
- **Reescriba en la aplicación de RR. HH. en la nube**: reescriba los atributos de nombre de usuario y dirección de correo electrónico de Azure AD en la aplicación de RR. HH. en la nube.

> [!NOTE]
> En este plan de implementación se explica cómo implementar los flujos de trabajo de aplicaciones de RR. HH. en la nube con el aprovisionamiento de usuarios de Azure AD. Para obtener información sobre cómo implementar el aprovisionamiento automático de usuarios para aplicaciones SaaS, consulte [Planeamiento de una implementación del aprovisionamiento automático de usuarios](https://aka.ms/deploymentplans/provisioning).

## <a name="enabled-hr-scenarios"></a>Escenarios de RR. HH. habilitados

El servicio de aprovisionamiento de usuarios de Azure AD permite la automatización de los siguientes escenarios de administración del ciclo de vida de la identidad basados en RR. HH.:

- **Contratación de empleados nuevos**: cuando se agrega un empleado nuevo a la aplicación de RR. HH. en la nube, se crea automáticamente una cuenta de usuario en AD y Azure AD con la opción de reescribir los atributos de nombre de usuario y dirección de correo electrónico en la aplicación de RR. HH. en la nube.
- **Actualización de atributos y perfiles de empleados**: cuando se actualiza el registro de un empleado en la aplicación de RR. HH. en la nube (por ejemplo, su nombre, título o administrador), su cuenta de usuario se actualizará automáticamente en AD y Azure AD.
- **Finalizaciones de empleados**: cuando se da de baja a un empleado en la aplicación de RR. HH. en la nube, su cuenta de usuario se deshabilita automáticamente en AD y Azure AD.
- **Recontratación de empleados**: cuando se vuelve a contratar a un empleado en la aplicación de RR. HH. en la nube, su cuenta anterior se puede volver a activar o reaprovisionar automáticamente en AD y Azure AD.

## <a name="who-is-this-integration-best-suited-for"></a>¿Para quién es idónea esta integración?

La integración de aplicaciones de RR. HH. en la nube con el aprovisionamiento de usuarios de Azure AD es ideal para organizaciones que:

- buscan una solución basada en la nube precompilada para el aprovisionamiento de usuarios de RR. HH. en la nube;
- necesitan un aprovisionamiento de usuarios directos de la aplicación de RR. HH. en la nube para AD o Azure AD;
- necesitan que los usuarios se aprovisionen con los datos obtenidos de la aplicación de RR. HH. en la nube;
- necesitan combinar y mover usuarios y dejarles que se sincronicen con uno o varios bosques, dominios o unidades organizativas de AD en función de únicamente la información de cambio detectada en la aplicación de RR. HH. en la nube;
- usan Office 365 para el correo electrónico.

## <a name="learn"></a>Obtener información

El aprovisionamiento de usuarios crea las bases de una gobernanza continua de identidades y mejora la calidad de los procesos de negocio que se basan en datos de identidades fidedignos.

### <a name="terms"></a>Términos

En este artículo se usan los términos siguientes:

- **Sistema de origen**: el repositorio de usuarios del que se aprovisiona Azure AD (por ejemplo, una aplicación de RR. HH. en la nube, como WorkDay y SuccessFactors).
- **Sistema de destino**: el repositorio de usuarios al que aprovisiona Azure AD (por ejemplo, AD, Azure AD, Office365 u otras aplicaciones SaaS).
- **Proceso de incorporaciones, cambios y bajas**: un término que se usa para nuevas contrataciones, transferencias y finalización con una aplicación de RR. HH. en la nube como sistema de registros. El proceso se completa cuando el servicio aprovisiona correctamente los atributos necesarios en el sistema de destino.

### <a name="key-benefits"></a>Ventajas principales

Esta funcionalidad del aprovisionamiento de TI controlada por RR. HH. ofrece importantes ventajas empresariales, como se indica a continuación:

- **Aumento de la productividad**: ahora puede automatizar la asignación de las cuentas de usuario, las licencias de Office365 y proporcionar acceso a los grupos de claves. La automatización de las asignaciones proporciona a las nuevas contrataciones acceso inmediato a sus herramientas de trabajo y aumenta la productividad.
- **Administración de riesgos**: para aumentar la seguridad, puede automatizar los cambios en función del estado de los empleados o de las pertenencias a grupos con datos que fluyen desde la aplicación de RR. HH. en la nube. La automatización de los cambios garantiza que las identidades de los usuarios y el acceso a las aplicaciones principales se actualizan automáticamente cuando los usuarios se mueven o salen de la organización.
- **Control de cumplimiento y gobernanza**: Azure AD admite registros de auditoría nativos para las solicitudes de aprovisionamiento de usuarios realizadas por las aplicaciones de los sistemas de origen y de destino. La auditoría le permite realizar un seguimiento de quién tiene acceso a las aplicaciones desde una sola pantalla.
- **Administración de costos**: el aprovisionamiento automático reduce los costos al evitar ineficiencias y errores humanos asociados con el aprovisionamiento manual. Reduce la necesidad de desarrollar soluciones personalizadas de aprovisionamiento de usuarios a lo largo del tiempo mediante plataformas heredadas y obsoletas.

### <a name="licensing"></a>Licencias

Para configurar la aplicación de RR. HH. en la nube para la integración de aprovisionamiento de usuarios de Azure AD, necesita una [licencia de Azure AD Premium válida](https://azure.microsoft.com/pricing/details/active-directory/) y una licencia para la aplicación de RR. HH. en la nube, como WorkDay o SuccessFactors.

También necesita una licencia de suscripción de Azure AD Premium P1 o superior válida para cada usuario que se obtenga de la aplicación de RR. HH. en la nube y que se aprovisionará en AD o en Azure AD. Un número incorrecto de las licencias que se poseen en la aplicación de RR. HH. en la nube puede dar lugar a errores durante el aprovisionamiento de usuarios.

### <a name="prerequisites"></a>Prerequisites

- Acceso del administrador global de Azure AD para configurar el agente de aprovisionamiento de Azure AD Connect.
- Una instancia de prueba y producción de la aplicación de RR. HH. en la nube.
- Permisos de administrador en la aplicación de RR. HH. en la nube para crear un usuario de integración del sistema y efectuar cambios para probar los datos de los empleados con fines de pruebas.
- Para el aprovisionamiento de usuarios en AD, se necesita un servidor que ejecute Windows Server 2012 o una versión posterior con el entorno de ejecución .NET 4.7.1+ a fin de hospedar el [agente de aprovisionamiento de Azure AD Connect](https://go.microsoft.com/fwlink/?linkid=847801).
- [Azure AD Connect](https://docs.microsoft.com/azure/active-directory/hybrid/whatis-azure-ad-connect) para la sincronización de usuarios entre AD y Azure AD.

### <a name="training-resources"></a>Recursos de aprendizaje

| **Recursos** | **Vínculo y descripción** |
|:-|:-|
| Vídeos | [¿Qué es el aprovisionamiento de usuarios en Active Azure Directory?](https://youtu.be/_ZjARPpI6NI) |
| | [¿Cómo implementar el aprovisionamiento de usuarios en Azure Active Directory?](https://youtu.be/pKzyts6kfrw) |
| Tutoriales | Consulte la [lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list). |
| | [Tutorial: Configuración de Workday para el aprovisionamiento automático de usuarios](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial#frequently-asked-questions-faq) |
| Preguntas más frecuentes | [Aprovisionamiento automático de usuarios](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning#what-applications-and-systems-can-i-use-with-azure-ad-automatic-user-provisioning). |
| | [Aprovisionamiento de WorkDay a Azure AD](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial#frequently-asked-questions-faq) |

### <a name="solution-architecture"></a>Arquitectura de la solución

En el ejemplo siguiente se describe la arquitectura de la solución de aprovisionamiento de usuarios de un extremo a otro para entornos híbridos comunes, e incluye lo siguiente:

- **Flujo de datos de RR. HH. autorizado, desde la aplicación de RR. HH. en la nube hasta AD**: en este flujo, el evento de RR. HH. (proceso de incorporaciones, cambios y bajas) se inicia en el inquilino de la aplicación de RR. HH. en la nube. El servicio de aprovisionamiento de Azure AD y el agente de aprovisionamiento de Azure AD Connect aprovisionan los datos de los usuarios desde el inquilino de la aplicación de recursos humanos en la nube en AD. En función del evento, puede dar lugar a las operaciones de creación, actualización, habilitación o deshabilitación en AD.
- **Sincronización con Azure AD y el nombre de usuario y correo electrónico de escritura diferida de AD local a la aplicación de RR. HH. en la nube**: cuando se hayan actualizado las cuentas en AD, este se sincroniza con Azure AD a través de Azure AD Connect, y los atributos de nombre de usuario y dirección de correo electrónico se pueden reescribir en el inquilino de la aplicación de RR. HH.

![Diagrama de flujo de trabajo](./media/plan-cloudhr-provisioning/plan-cloudhr-provisioning-img1.png)

#### <a name="description-of-workflow"></a>Descripción del flujo de trabajo

Los pasos clave indicados en el diagrama son:  

1. El **equipo de recursos humanos** realiza las transacciones en el inquilino de la aplicación de RR. HH. en la nube.
2. **Servicio de aprovisionamiento de Azure AD** ejecuta los ciclos programados desde el inquilino de la aplicación de RR. HH. en la nube e identifica los cambios que deben procesarse para la sincronización con AD.
3. El **servicio de aprovisionamiento de Azure AD** invoca al agente de aprovisionamiento de Azure AD Connect con una carga de solicitud que contiene las operaciones de creación, actualización, habilitación o deshabilitación de las cuentas de AD.
4. El **agente de aprovisionamiento de Azure AD Connect** usa una cuenta de servicio para administrar los datos de la cuenta de AD.
5. **Azure AD Connect** ejecuta una [sincronización](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-whatis) diferencial para extraer las actualizaciones de AD.
6. Las actualizaciones de **AD** se sincronizan con Azure AD.
7. El **servicio de aprovisionamiento de Azure AD**  reescribe el atributo de correo electrónico y el nombre de usuario de Azure AD en el inquilino de la aplicación de RR. HH. en la nube.

## <a name="plan-the-deployment-project"></a>Planeamiento del proyecto de implementación

Tenga en cuenta las necesidades de su organización al determinar la estrategia de esta implementación en su entorno.

### <a name="engage-the-right-stakeholders"></a>Interactuar con las partes interesadas adecuadas

Cuando fracasan los proyectos tecnológicos, normalmente se debe a expectativas incorrectas relacionadas con el impacto, los resultados y las responsabilidades. Para evitar estos problemas, [asegúrese de involucrar a las partes interesadas correctas](https://aka.ms/deploymentplans) y que los roles de las partes interesadas en el proyecto se entiendan bien; para ello, documente las partes interesadas y sus aportes y responsabilidades en el proyecto.

Debe incluir un representante de la organización de recursos humanos que pueda proporcionar entradas sobre los procesos de negocio de recursos humanos y los requisitos de procesamiento de datos e identidad del trabajador existentes.

### <a name="plan-communications"></a>Planeamiento de las comunicaciones

La comunicación es fundamental para el éxito de cualquier servicio nuevo. Debería comunicar de forma proactiva a los usuarios cómo y cuándo va a cambiar su experiencia, y cómo obtener soporte técnico si tienen cualquier problema.

### <a name="plan-a-pilot"></a>Planeamiento de un piloto

La integración de procesos de negocio y flujos de trabajo de identidad desde la aplicación de RR. HH. en la nube en los sistemas de destino requiere una gran cantidad de validación, transformación y limpieza de datos, y pruebas de un extremo a otro antes de implementar la solución en producción.

Se recomienda que la configuración inicial se ejecute en un [entorno piloto](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-deployment-plans#best-practices-for-a-pilot) antes de escalarla a todos los usuarios de producción.

## <a name="select-cloud-hr-provisioning-connector-apps"></a>Selección de aplicaciones de conectores de aprovisionamiento de RR. HH. en la nube

Para facilitar el aprovisionamiento de Azure AD de flujos de trabajo entre la aplicación de RR. HH. en la nube y AD, hay varias aplicaciones de conectores de aprovisionamiento que puede agregar desde la galería de aplicaciones de Azure AD:

- **Aprovisionamiento de usuarios de la aplicación de RR. HH. en la nube en AD**: esta aplicación de conector de aprovisionamiento facilita el aprovisionamiento de cuentas de usuario de la aplicación de RR. HH. en la nube a un solo dominio de AD. Si tiene varios dominios, puede agregar una instancia de esta aplicación desde la galería de aplicaciones de Azure AD para cada dominio de AD en el que deba efectuar un aprovisionamiento.
- **Aprovisionamiento de usuarios de la aplicación de RR. HH. en la nube en Azure AD**: mientras Azure AD Connect es la herramienta que se debe usar para sincronizar los usuarios de AD con Azure AD, esta aplicación de conector de aprovisionamiento se puede usar para facilitar el aprovisionamiento de usuarios que solo están en la nube desde la aplicación de RR. HH. en la nube a un inquilino de Azure AD.
- **Escritura diferida de la aplicación de RR. HH. en la nube**: esta aplicación de conector de aprovisionamiento facilita la escritura diferida de las direcciones de correo electrónico del usuario de Azure AD a la aplicación de RR. HH. en la nube.

Por ejemplo, en la imagen siguiente se enumeran las aplicaciones de conector de WorkDay disponibles en la galería de aplicaciones de Azure AD.

![Galería de aplicaciones del portal de Azure Active Directory](./media/plan-cloudhr-provisioning/plan-cloudhr-provisioning-img2.png)

### <a name="decision-flowchart"></a>Diagrama de flujo de decisiones

Use el siguiente diagrama de decisiones para identificar qué aplicaciones de aprovisionamiento de RR. HH. en la nube son pertinentes para su escenario.

![Diagrama de flujo de decisiones](./media/plan-cloudhr-provisioning/plan-cloudhr-provisioning-img3.png)

## <a name="design-azure-ad-connect-provisioning-agent-deployment-topology"></a>Diseño de la topología de implementación del agente de aprovisionamiento de Azure AD Connect

La integración de aprovisionamiento entre la aplicación de RR. HH. en la nube y AD requiere estos cuatro componentes:

- Inquilino de la aplicación de RR. HH. en la nube
- Aplicación de conector de aprovisionamiento
- Agente de aprovisionamiento de Azure AD Connect
- Dominio de AD

La topología de implementación del agente de aprovisionamiento de Azure AD Connect depende del número de inquilinos de la aplicación de RR. HH. en la nube y de los dominios secundarios de AD que se van a integrar. Si tiene varios dominios de AD, dependerá de si los dominios de AD son contiguos o están [separados](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/disjoint-namespace).

En función de su decisión, elija uno de los escenarios de implementación siguientes:

- Inquilino de una aplicación de RR. HH. en la nube único -> dirigido a uno o varios dominios secundarios de AD en un bosque de confianza
- Inquilino de una aplicación de RR. HH. en la nube único -> dirigido a varios dominios secundarios en un bosque de AD no contiguo.

### <a name="single-cloud-hr-app-tenant---single-or-multiple-target-ad-child-domains-in-a-trusted-forest"></a>Inquilino de una aplicación de RR. HH. en la nube único -> uno o varios dominios secundarios de AD de destino en un bosque de confianza

Se recomienda la siguiente configuración de producción:

|Requisito|Recomendación|
|:-|:-|
|Número de agentes de aprovisionamiento de Azure AD Connect que se van a implementar|2 (para alta disponibilidad y conmutación por error)
|Número de aplicaciones de conectores de aprovisionamiento que se van a configurar|Una aplicación por dominio secundario|
|Host de servidor para el agente de aprovisionamiento de Azure AD Connect|Windows 2012 R2+ con campo visual a los controladores de dominio de AD ubicados geográficamente</br>Puede coexistir con el servicio de Azure AD Connect|

![Flujo a los agentes locales](./media/plan-cloudhr-provisioning/plan-cloudhr-provisioning-img4.png)

### <a name="single-cloud-hr-app-tenant---target-multiple-child-domains-in-a-disjoint-ad-forest"></a>Inquilino de una aplicación de RR. HH. en la nube único -> dirigido a varios dominios secundarios en un bosque de AD no contiguo.

Este escenario implica el aprovisionamiento de usuarios desde la aplicación de RR. HH. en dominios de bosques de AD no contiguos.

Se recomienda la siguiente configuración de producción:

|Requisito|Recomendación|
|:-|:-|
|Número de agentes de aprovisionamiento de Azure AD Connect que se van a implementar localmente|2 por cada bosque de AD no contiguo|
|Número de aplicaciones de conectores de aprovisionamiento que se van a configurar|Una aplicación por dominio secundario|
|Host de servidor para el agente de aprovisionamiento de Azure AD Connect|Windows 2012 R2+ con campo visual a los controladores de dominio de AD ubicados geográficamente</br>Puede coexistir con el servicio de Azure AD Connect|

![Bosque de AD no contiguo del inquilino de la aplicación de RR. HH. en la nube único](./media/plan-cloudhr-provisioning/plan-cloudhr-provisioning-img5.png)

### <a name="azure-ad-connect-provisioning-agent-requirements"></a>Requisitos del agente de aprovisionamiento de Azure AD Connect

La solución de aprovisionamiento de usuarios de la aplicación de RR. HH. en AD requiere la implementación de uno o varios agentes de aprovisionamiento de Azure AD Connect en servidores que ejecutan Windows 2012 R2 o posterior con, al menos, 4 GB de RAM y .NET 4.7.1+ en tiempo de ejecución. Asegúrese de que el servidor host tiene acceso de red para el dominio de AD de destino.

Para preparar el entorno local, el asistente para la configuración del agente de aprovisionamiento de Azure AD Connect registra el agente en el inquilino de Azure AD, [abre los puertos](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application#open-ports), [permite el acceso a las direcciones URL](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application#allow-access-to-urls) y admite [la configuración de proxy HTTPS saliente](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial#how-do-i-configure-the-provisioning-agent-to-use-a-proxy-server-for-outbound-http-communication).

El agente de aprovisionamiento usa una cuenta de servicio para comunicarse con los dominios de AD. Antes de la instalación del agente, se recomienda crear una cuenta de servicio en los usuarios y equipos de AD que cumplan los requisitos siguientes:

- Una contraseña que no expire
- Permisos de control delegado para leer, crear, eliminar y administrar cuentas de usuario

Puede seleccionar los controladores de dominio que deben controlar las solicitudes de aprovisionamiento. Si tiene varios controladores de dominio distribuidos geográficamente, instale al agente de aprovisionamiento en el mismo sitio que los controladores de dominio preferidos para mejorar la confiabilidad y el rendimiento de la solución de un extremo a otro.

Para lograr una alta disponibilidad, puede implementar más de un agente de aprovisionamiento de Azure AD Connect y registrarlos para controlar el mismo conjunto de dominios de Active Directory locales.

## <a name="plan-scoping-filters-and-attribute-mapping"></a>Planeamiento de los filtros de ámbito y la asignación de atributos

Al habilitar el aprovisionamiento desde la aplicación de RR. HH. en la nube en AD o Azure AD, Azure Portal controla los valores de atributo mediante la asignación de atributos.

### <a name="define-scoping-filters"></a>Definición de filtros de ámbito

Use [filtros de ámbito](https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters) para definir las reglas basadas en atributos que determinan qué usuarios se deben aprovisionar desde la aplicación de RR. HH. en la nube en AD o Azure AD.

Al iniciar el proceso de las incorporaciones, reúna los siguientes requisitos:

- ¿La aplicación de RR. HH. en la nube se usa tanto para los empleados como para los trabajadores temporales?
- ¿Planea usar la aplicación de RR. HH. en la nube para el aprovisionamiento de usuarios de Azure AD para administrar tanto a los empleados como a los trabajadores temporales?
- ¿Tiene pensado implementar la aplicación de RR. HH. en la nube para el aprovisionamiento de usuarios de Azure AD solo para un subconjunto de usuarios de la aplicación de RR. HH. en la nube (por ejemplo, solo los empleados)?

En función de sus requisitos, al configurar las asignaciones de atributos, puede establecer el campo **Ámbito de objeto de origen** para seleccionar los conjuntos de usuarios de la aplicación de RR. HH. en la nube que deben estar en el ámbito para el aprovisionamiento en AD. Consulte el tutorial de la aplicación de RR. HH. en la nube para ver los filtros de ámbito usados con más frecuencia.

### <a name="determine-matching-attributes"></a>Determinar los atributos coincidentes

Con el aprovisionamiento, obtiene la capacidad de hacer coincidir las cuentas existentes entre el sistema de origen y el de destino. Al integrar la aplicación de RR. HH. en la nube con el servicio de aprovisionamiento de Azure AD, puede [configurar la asignación de atributos](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-automatic-user-provisioning-portal#mappings) para determinar qué datos de usuario deben fluir desde la aplicación de RR. HH. en la nube a AD o Azure AD.

Al iniciar el proceso de las incorporaciones, reúna los siguientes requisitos:

- ¿Cuál es el identificador único de esta aplicación de RR. HH. en la nube que se usa para identificar a cada usuario?
- Desde la perspectiva del ciclo de vida de las identidades, ¿cómo se administran las recontrataciones? En las recontrataciones, ¿se conserva el identificador de empleado anterior?
- ¿Procesa las contrataciones con fecha futura y cuentas de AD creadas para ellos de antemano?
- Desde la perspectiva del ciclo de vida de las identidades, ¿cómo se controla la conversión de empleado a trabajador temporal o a la inversa?
- ¿Los usuarios mantienen su cuenta de AD anterior u obtienen una nueva?

En función de sus requisitos, Azure AD admite la asignación de atributo a atributo directa, lo que proporciona valores constantes o permite [escribir expresiones para asignaciones de atributos](https://docs.microsoft.com/azure/active-directory/active-directory-saas-writing-expressions-for-attribute-mappings). Esta flexibilidad le brinda un mayor control de lo que se rellenará en el atributo de la aplicación de destino. Puede usar [Microsoft Graph API](https://docs.microsoft.com/azure/active-directory/manage-apps/export-import-provisioning-configuration) y Probador de Graph para exportar el esquema y las asignaciones de atributos de aprovisionamiento de usuarios a un archivo JSON e importarlos de nuevo en Azure AD.

**De forma predeterminada**, el atributo de la aplicación de RR. HH. en la nube que representa el identificador de empleado único se usa como atributo coincidente *asignado al atributo único en AD.* Por ejemplo, en el escenario de la aplicación WorkDay, el atributo *WorkerID* de *WorkDay* está asignado al atributo *employeeID* de AD.

Puede establecer varios atributos coincidentes y asignar una precedencia de coincidencia. Se evalúan según la precedencia de coincidencia. En el momento en que se encuentre una coincidencia, no se evaluarán más atributos coincidentes.

También puede [personalizar las asignaciones de atributos predeterminadas](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes#understanding-attribute-mapping-types), como cambiar o eliminar asignaciones de atributos existentes o crear nuevas asignaciones de atributos según sus necesidades empresariales. Consulte el tutorial de la aplicación de RR. HH. en la nube (por ejemplo, [WorkDay](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial#planning-workday-to-active-directory-user-attribute-mapping-and-transformations)) para obtener una lista de los atributos personalizados que se pueden asignar.

### <a name="determine-user-account-status"></a>Determinar el estado de la cuenta de usuario

De forma predeterminada, la aplicación de conector de aprovisionamiento asigna el **estado de perfil de usuario de RR. HH.** al **estado de la cuenta de usuario** en AD o Azure AD para determinar si se debe habilitar o deshabilitar la cuenta de usuario.

Al iniciar el proceso de las incorporaciones o bajas, reúna los siguientes requisitos:

| Proceso | Requisitos |
| - | - |
| **Incorporaciones** | Desde la perspectiva del ciclo de vida de las identidades, ¿cómo se administran las recontrataciones? En las recontrataciones, ¿se conserva el identificador de empleado anterior? |
| | ¿Procesa las contrataciones con fecha futura y crea cuentas de AD para ellos de antemano? ¿Estas cuentas se crean con el estado habilitado/deshabilitado? |
| | Desde la perspectiva del ciclo de vida de las identidades, ¿cómo se controla la conversión de empleado a trabajador temporal o a la inversa? |
| | ¿Los usuarios mantienen su cuenta de AD anterior u obtienen una nueva? |
| **Bajas** | ¿Se administran las bajas de manera diferente para los empleados y los trabajadores temporales en AD? |
| | ¿Qué fechas vigentes se tienen en cuenta para procesar la baja del usuario? |
| | ¿Cómo afectan a las cuentas de AD existentes las conversiones de empleados y trabajadores temporales? |
| | ¿Cómo se procesa la operación "Rescindir" en AD? Las operaciones de rescisión deben controlarse si se crean contrataciones futuras en AD como parte del proceso de incorporación. |

En función de sus requisitos, puede personalizar la lógica de asignación mediante [expresiones de Azure AD](https://docs.microsoft.com/azure/active-directory/manage-apps/functions-for-customizing-application-data) para que la cuenta de AD esté habilitada o deshabilitada en función de una combinación de puntos de datos.

### <a name="map-cloud-hr-app-to-ad-user-attributes"></a>Asignación de la aplicación de RR. HH. en la nube a los atributos de usuario de AD

Cada aplicación de RR. HH. en la nube se incluye con una aplicación de RR. HH. en la nube predeterminada para las asignaciones de AD.

Al iniciar el proceso de incorporaciones, cambios o bajas, reúna los siguientes requisitos:

| Proceso | Requisitos |
| - | - |
| **Incorporaciones** | ¿El proceso de creación de cuentas de AD es manual, automatizado o parcialmente automatizado? |
| | ¿Tiene pensado propagar los atributos personalizados de la aplicación de RR. HH. de la nube a AD? |
| **Cambios** | ¿Qué atributos quiere procesar cada vez que se realice una operación de cambio en la aplicación de RR. HH. en la nube? |
| | ¿Se realizan validaciones de atributo específicas en el momento de realizar las actualizaciones de usuarios? En caso afirmativo, proporcione los detalles aquí. |
| **Bajas** | ¿Se administran las bajas de manera diferente para los empleados y los trabajadores temporales en AD? |
| | ¿Qué fechas vigentes se tienen en cuenta para procesar la baja del usuario? |
| | ¿Cómo afectan a las cuentas de AD existentes las conversiones de empleados y trabajadores temporales? |

En función de sus requisitos, puede modificar las asignaciones para satisfacer sus objetivos de integración. Consulte el tutorial específico de la aplicación de RR. HH. en la nube (por ejemplo, [WorkDay](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial#planning-workday-to-active-directory-user-attribute-mapping-and-transformations)) para obtener una lista de los atributos personalizados que se pueden asignar.

### <a name="generate-unique-attribute-value"></a>Generación de un valor de atributo único

Al iniciar el proceso de incorporaciones, es posible que tenga que generar valores de atributo únicos al establecer atributos como CN, samAccountName y UPN que tengan restricciones únicas.

La función de Azure AD [SelectUniqueValues](https://docs.microsoft.com/azure/active-directory/manage-apps/functions-for-customizing-application-data#selectuniquevalue) evalúa cada regla y, después, comprueba la unicidad del valor generado en el sistema de destino. Vea el ejemplo [Generación de un valor único para el atributo userPrincipalName (UPN)](https://docs.microsoft.com/azure/active-directory/manage-apps/functions-for-customizing-application-data#generate-unique-value-for-userprincipalname-upn-attribute).

> [!NOTE]
> Esta función solo se admite actualmente para "WorkDay para el aprovisionamiento de usuarios de Active Directory". No se puede usar con otras aplicaciones de aprovisionamiento.

### <a name="configure-ad-ou-container-assignment"></a>Configuración de la asignación de contenedor de la unidad organizativa de AD

Un requisito común es colocar las cuentas de usuario de AD en contenedores en función de las unidades de negocio, ubicaciones y departamentos. Cuando se inicia un proceso de cambio y, si hay un cambio en la organización de supervisión, es posible que tenga que mover al usuario de una unidad organizativa a otra en AD.

Use la función [Switch()](https://docs.microsoft.com/azure/active-directory/manage-apps/functions-for-customizing-application-data#switch) para configurar la lógica de negocios para la asignación de la unidad organizativa y asígnela al atributo de AD *parentDistinguishedName*.

Por ejemplo, si quiere crear usuarios en la unidad organizativa según el atributo de RR. HH. "Municipio", puede utilizar la siguiente expresión.

`
Switch([Municipality], "OU=Default,OU=Users,DC=contoso,DC=com", "Dallas", "OU=Dallas,OU=Users,DC=contoso,DC=com", "Austin", "OU=Austin,OU=Users,DC=contoso,DC=com", "Seattle", "OU=Seattle,OU=Users,DC=contoso,DC=com", "London", "OU=London,OU=Users,DC=contoso,DC=com")
`

Con esta expresión, si el valor de Municipio es Dallas, Austin, Seattle o Londres, la cuenta de usuario se creará en la unidad organizativa correspondiente. Si no hay ninguna coincidencia, se creará la cuenta en la unidad organizativa predeterminada.

## <a name="plan-for-password-delivery-of-new-user-accounts"></a>Planeamiento de la entrega de contraseñas de nuevas cuentas de usuario

Al iniciar el proceso de incorporaciones, deberá establecer y proporcionar una contraseña temporal de las nuevas cuentas de usuario. Con el aprovisionamiento de usuarios de RR. HH. en la nube en Azure AD, puede implementar la funcionalidad de Azure AD de [autoservicio de restablecimiento de contraseña](https://docs.microsoft.com/azure/active-directory/authentication/quickstart-sspr) (SSPR) para el usuario, desde el primer día.

SSPR es un medio sencillo con el que los administradores de TI pueden permitir que los usuarios restablezcan sus contraseñas o desbloqueen sus cuentas. Puede aprovisionar el atributo **número de móvil** desde la aplicación de RR., HH. en la nube en AD y sincronizarlo con Azure AD. Cuando ya exista el atributo **Número de móvil** en Azure AD, puede habilitar SSPR para la cuenta del usuario, para que, desde el primer día, los usuarios nuevos puedan usar el número de móvil registrado y verificado para su autenticación.

## <a name="plan-for-initial-cycle"></a>Planeamiento del ciclo inicial

Cuando el servicio de aprovisionamiento de Azure AD se ejecuta por primera vez, se ejecuta un [ciclo inicial](https://docs.microsoft.com/azure/active-directory/manage-apps/how-provisioning-works#initial-cycle) en la aplicación de RR. HH. en la nube para crear una instantánea de todos los objetos de usuario en la aplicación de RR. HH. en la nube. El tiempo que tardan los ciclos iniciales depende directamente del número de usuarios que haya en el sistema de origen. El ciclo inicial de algunos inquilinos de aplicaciones de RR. HH. en la nube con más de 100 000 usuarios puede llevar mucho tiempo.

**En el caso de inquilinos de aplicaciones de RR. HH. en la nube de gran tamaño (> 30 000 usuarios), se recomienda** ejecutar el ciclo inicial en fases progresivas e iniciar las actualizaciones incrementales solo después de validar que están establecidos los atributos correctos en AD para los diferentes escenarios de aprovisionamiento de usuarios. Siga el orden siguiente:

1. Ejecute el ciclo inicial solo para un conjunto limitado de usuarios estableciendo el [filtro de ámbito](#plan-scoping-filters-and-attribute-mapping).
2. Compruebe el aprovisionamiento de cuentas de AD y los valores de atributo establecidos para los usuarios seleccionados para la primera ejecución. Si el resultado cumple con sus expectativas, expanda el filtro de ámbito para que incluya más usuarios de forma progresiva y compruebe los resultados de la segunda ejecución.

Cuando esté satisfecho con los resultados del ciclo inicial para los usuarios de prueba, puede iniciar las [actualizaciones incrementales](https://docs.microsoft.com/azure/active-directory/manage-apps/how-provisioning-works#incremental-cycles).

## <a name="plan-testing-and-security"></a>Planeamiento de pruebas y seguridad

En cada fase de la implementación desde el piloto inicial hasta el aprovisionamiento de usuarios, asegúrese de que está probando que los resultados son los esperados y auditando los ciclos de aprovisionamiento.

### <a name="plan-testing"></a>Planeamiento de pruebas

Una vez que haya configurado el aprovisionamiento de usuarios de la aplicación de RR. HH. en la nube en Azure AD, ejecutará casos de prueba para comprobar si esta solución cumple con los requisitos de su organización.

|Escenarios|Resultados esperados|
|:-|:-|
|Nueva contratación de empleados en la aplicación de RR. HH. en la nube| - La cuenta de usuario se aprovisiona en AD.</br>- Los usuarios pueden iniciar sesión en aplicaciones de dominio de AD y realizar las acciones deseadas.</br>- Si está configurada la sincronización de AAD Connect, la cuenta de usuario también se creará en Azure AD.
|Se da de baja al usuario en la aplicación de RR. HH. en la nube.|- Se deshabilita la cuenta de usuario en AD.</br>- El usuario no puede iniciar sesión en las aplicaciones empresariales protegidas por AD.
|La organización de supervisión de usuarios se actualiza en la aplicación de RR. HH. en la nube|En función de la asignación de atributos, la cuenta de usuario se mueve de una unidad organizativa a otra en AD.|
|RR. HH. actualiza el administrador del usuario en la aplicación de RR. HH. en la nube|El campo de administrador de AD se actualiza para que refleje el nombre del nuevo administrador.|
|RR. HH. recontrata a un empleado en un nuevo rol.|El comportamiento depende de cómo esté configurada la aplicación de RR. HH. en la nube para generar identificadores de empleados:</br>- Si el identificador de empleado anterior se reutiliza para las recontrataciones, el conector habilitará la cuenta de AD existente para el usuario.</br>- Si las recontrataciones obtienen un nuevo Id. de empleado, el conector creará una cuenta de AD nueva para el usuario.|
|RR. HH. convierte al empleado en trabajador temporal o viceversa|Se crea una nueva cuenta de AD para el nuevo rol y la cuenta anterior se deshabilita en la fecha de efecto de la conversión.|

Use los resultados anteriores para determinar cómo realizar la transición de la implementación automática del aprovisionamiento de usuarios en producción en función de las escalas de tiempo establecidas.

> [!TIP]
> Se recomienda usar técnicas como la reducción y la limpieza de datos al actualizar el entorno de prueba con los datos de producción para quitar o enmascarar datos confidenciales de DCP (información de identificación personal) para cumplir con los estándares de privacidad y seguridad.

### <a name="plan-security"></a>Planeamiento de seguridad

Es habitual que se requiera una revisión de seguridad como parte de la implementación de un nuevo servicio. Si se requiere una revisión de seguridad o aún no se ha realizado, revise las numerosas [notas del producto](https://www.microsoft.com/download/details.aspx?id=36391) de Azure AD, que proporcionan información general sobre la identidad como servicio.

### <a name="plan-rollback"></a>Planeamiento de la reversión

Si la implementación del aprovisionamiento de usuarios de RR. HH. en la nube no funciona como se desea en el entorno de producción, los pasos de reversión siguientes pueden ayudarlo a revertir a un estado correcto conocido anterior:

1. Revise el [informe de resumen de aprovisionamiento](check-status-user-account-provisioning.md#getting-provisioning-reports-from-the-azure-portal) y los [registros de aprovisionamiento](check-status-user-account-provisioning.md#provisioning-logs-preview) (consulte [Administración del aprovisionamiento de usuario de la aplicación de RR. HH. en la nube](#manage-your-configuration)) para determinar qué operaciones incorrectas se realizaron en los usuarios o grupos afectados.
2. El último estado válido conocido de los usuarios o grupos afectados puede determinarse a través de los registros de auditoría de aprovisionamiento o revisando los sistemas de destino (Azure AD o AD).
3. Trabaje con el propietario de la aplicación para actualizar los usuarios o grupos afectados directamente en la aplicación con los últimos valores de estado correctos conocidos.

## <a name="deploy-the-cloud-hr-app"></a>Implementación de la aplicación de RR. HH. en la nube

Elija la aplicación de RR. HH. en la nube que se alinee con los requisitos de la solución.

**WorkDay**: Consulte [Tutorial: Configuración de Workday para el aprovisionamiento automático de usuarios](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial#planning-your-deployment) para ver los pasos que debe llevar a cabo para importar perfiles de trabajo de Workday a AD y Azure AD, con la escritura diferida opcional de la dirección de correo electrónico y el nombre de usuario en Workday.

## <a name="manage-your-configuration"></a>Administración de la configuración

Azure AD puede proporcionar información adicional acerca del uso del aprovisionamiento de usuarios y el estado operativo mediante registros de auditoría e informes.

### <a name="gain-insights-from-reports-and-logs"></a>Obtención de información a partir de informes y registros

Después de un [ciclo inicial](https://docs.microsoft.com/azure/active-directory/manage-apps/how-provisioning-works#initial-cycle) correcto, el servicio de aprovisionamiento de Azure AD seguirá ejecutando actualizaciones incrementales de manera indefinida, a intervalos definidos en los tutoriales específicos para cada aplicación, hasta que se produzca alguno de los siguientes eventos:

- El servicio se detiene de manera manual y se desencadena un ciclo inicial nuevo mediante [Azure Portal](https://portal.azure.com/) o con el comando adecuado de [Microsoft Graph API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-overview).
- Se desencadena un nuevo ciclo inicial debido a un cambio en las asignaciones de atributos o los filtros de ámbito.
- El proceso de aprovisionamiento entra en cuarentena debido a una alta tasa de errores y permanece en ese estado durante más de cuatro semanas, tras las cuales se deshabilitará automáticamente.

Para revisar estos eventos y todas las demás actividades realizadas por el servicio de aprovisionamiento, [aprenda a revisar los registros y obtener informes sobre la actividad de aprovisionamiento](https://docs.microsoft.com/azure/active-directory/manage-apps/check-status-user-account-provisioning).

#### <a name="azure-monitor-logs"></a>Registros de Azure Monitor

Todas las actividades realizadas por el servicio de aprovisionamiento se registran en los **registros de aprovisionamiento de Azure AD**. Puede enrutar los registros de auditoría de Azure AD a registros de Azure Monitor para su posterior análisis. **Registros de Azure Monitor (también conocido como área de trabajo de Log Analytics)** permite consultar los datos para buscar eventos, analizar tendencias y realizar la correlación entre varios orígenes de datos. Vea el siguiente [vídeo](https://youtu.be/MP5IaCTwkQg) para conocer las ventajas del uso de registros de Azure Monitor para registros de Azure AD en escenarios de usuario prácticos.

También puede instalar las [vistas de Log Analytics para los registros de actividad de Azure AD](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-install-use-log-analytics-views) para acceder a [informes pregenerados](https://github.com/AzureAD/Deployment-Plans/tree/master/Log%20Analytics%20Views) sobre eventos de aprovisionamiento en el entorno.

Para obtener más información, consulte [Análisis de registros de actividad de Azure AD con registros de Azure Monitor](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-analyze-activity-logs-log-analytics).

### <a name="manage-personal-data"></a>Administración de datos personales

El agente de aprovisionamiento de Azure AD Connect instalado en Windows Server crea registros en el registro de eventos de Windows que pueden contener datos personales en función de las asignaciones de atributos de la aplicación de RR. HH. en la nube en AD. Para cumplir las obligaciones de privacidad del usuario, puede configurar una tarea programada de Windows para borrar el registro de eventos y asegurarse de que no se conserva ningún dato durante más de 48 horas.

El servicio de aprovisionamiento de Azure AD no genera informes, ni realiza análisis ni proporciona información después de 30 días. Por tanto, el servicio de aprovisionamiento de Azure AD no almacena, procesa ni retiene ningún dato más allá de 30 días. 

### <a name="troubleshoot"></a>Solución de problemas

Consulte los vínculos siguientes para solucionar los problemas que pueden surgir durante el aprovisionamiento:

- [Problema al configurar el aprovisionamiento de usuarios para una aplicación de la galería de Azure AD](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-config-problem)
- [Sincronización de un atributo de Active Directory local con Azure AD para el aprovisionamiento en una aplicación](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning-sync-attributes-for-mapping)
- [Aprovisionamiento de usuarios a una aplicación de la galería de Azure AD que tarda horas o más](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-when-will-provisioning-finish)
- [Problema al guardar las credenciales de administrador al configurar el aprovisionamiento de usuarios em una aplicación de galería de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-config-problem-storage-limit)
- [No se aprovisionan usuarios en una aplicación de la galería de Azure AD](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-config-problem-no-users-provisioned)
- [Aprovisionamiento de un conjunto de usuarios incorrecto en una aplicación de la galería de Azure AD](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-config-problem-wrong-users-provisioned)
- [Configuración del Visor de eventos de Windows para solucionar problemas del agente](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial#setting-up-windows-event-viewer-for-agent-troubleshooting)
- [Configuración de registros de auditoría de Azure Portal para solucionar problemas con el servicio](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial#setting-up-azure-portal-audit-logs-for-service-troubleshooting)
- [Información sobre registros para las operaciones de creación de cuentas de usuario de AD](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial#understanding-logs-for-ad-user-account-create-operations)
- [Información sobre registros para las operaciones de actualización del administrador](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial#understanding-logs-for-manager-update-operations)
- [Resolución de errores comunes](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial#resolving-commonly-encountered-errors)

### <a name="next-steps"></a>Pasos siguientes

- [Escritura de expresiones para la asignación de atributos](https://docs.microsoft.com/azure/active-directory/manage-apps/functions-for-customizing-application-data)
- [Introducción a la API de sincronización de Azure AD](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-overview)
- [Omisión de la eliminación de cuentas de usuario que están fuera de ámbito](https://docs.microsoft.com/azure/active-directory/manage-apps/skip-out-of-scope-deletions)
- [Agente de aprovisionamiento de Azure AD Connect: historial de versiones](https://docs.microsoft.com/azure/active-directory/manage-apps/provisioning-agent-release-version-history)
