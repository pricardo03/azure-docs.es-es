---
title: Planear una implementación de Proxy de aplicación de Azure Active Directory
description: Una guía de extremo a otro para planear la implementación del proxy de aplicación dentro de su organización
services: active-directory
documentationcenter: azure
author: barbaraselden
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04-04-2019
ms.author: barbaraselden
ms.reviewer: ''
ms.openlocfilehash: d8686b9296c8b1d7c5232e2e46a0e66a9896656b
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66113017"
---
# <a name="plan-an-azure-ad-application-proxy-deployment"></a>Planear una implementación de Azure AD Application Proxy

Proxy de aplicación de Azure Active Directory (Azure AD) es una solución de acceso remoto seguro y rentable para las aplicaciones locales. Proporciona una ruta de acceso de la transición inmediata para las organizaciones de "Nube primero" administrar el acceso a heredado aplicaciones locales que aún no se pueden usar protocolos modernos. Para obtener más información introductoria, consulte [¿qué es el Proxy de aplicación](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy).

Se recomienda el Proxy de aplicación para dar a los usuarios remotos acceso a los recursos internos. Proxy de aplicación reemplaza la necesidad de una VPN o proxy inverso para estos casos de uso de acceso remoto. No está pensado para los usuarios que están en la red corporativa. Estos usuarios que usan el Proxy de aplicación para acceso a la intranet pueden experimentar problemas de rendimiento no deseado.

En este artículo incluye los recursos que necesita para planificar, operar y administrar a Azure AD Application Proxy. 

## <a name="plan-your-implementation"></a>Planeamiento de la implementación

La siguiente sección proporciona una visión amplia de la elementos que se establecerán para una experiencia de implementación eficaz de planeamiento de la clave. 

### <a name="prerequisites"></a>Requisitos previos

Debe cumplir los siguientes requisitos previos antes de comenzar la implementación. Puede ver más información sobre cómo configurar su entorno, incluidos estos requisitos previos, en este [tutorial](application-proxy-add-on-premises-application.md).

* **Conectores**: Los conectores son agentes ligeros que puede implementar en:
   * Hardware físico en el entorno local
   * Una máquina virtual hospedada en cualquier solución de hipervisor
   * Una máquina virtual hospedada en Azure para habilitar una conexión saliente con el servicio de Proxy de aplicación.

* Consulte [conectores comprender el Proxy de aplicación de Azure AD](application-proxy-connectors.md) para una descripción más detallada.

     * Conector de equipos debe [habilitarse para TLS 1.2](application-proxy-add-on-premises-application.md) antes de instalar los conectores.

     * Si es posible, implementar conectores en el [misma red](application-proxy-network-topology.md) y segmento, como los servidores de aplicaciones web de back-end. Es mejor implementar conectores después de completar una detección de aplicaciones.
     * Se recomienda que cada grupo de conectores tiene al menos dos conectores para proporcionar escalabilidad y alta disponibilidad. Tener tres conectores es óptimo en caso de que necesite una máquina en cualquier punto de servicio. Revise el [tabla de capacidad de conector](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-connectors#capacity-planning) para ayudar a decidir qué tipo de máquina para instalar conectores en. Cuanto mayor sea el equipo el búfer más y eficaz será el conector.

* **Configuración de acceso de red**: Los conectores de Azure AD Application Proxy [conectarse a Azure a través de HTTPS (puerto TCP 443) y HTTP (puerto TCP 80)](application-proxy-add-on-premises-application.md). 

   * Conector de terminación del tráfico TLS no se admite y evitará que los conectores de establecer un canal seguro con sus respectivos puntos de conexión de Proxy de aplicación de Azure.

   * Evitar todas las formas de inspección en línea en las comunicaciones salientes de TLS entre los conectores y Azure. Inspección interno entre las aplicaciones de conector y el back-end es posible, pero podría degradar la experiencia del usuario y, por lo tanto, no se recomienda.

   * Equilibrio de carga de los conectores a sí mismos también no es compatible, o incluso necesarios.

### <a name="important-considerations-before-configuring-azure-ad-application-proxy"></a>Consideraciones importantes antes de configurar Azure AD Application Proxy

Deben cumplirse los siguientes requisitos principales para configurar e implementar el Proxy de aplicación de Azure AD.

*  **Incorporación de Azure**: Antes de implementar el proxy de aplicación, identidades de usuario deben ser sincronizadas desde un directorio local o creadas directamente dentro de los inquilinos de Azure AD. Sincronización de identidades permite realizar la autenticación previa de los usuarios antes de concederles acceso al Proxy de aplicación de las aplicaciones publican y tener la información del identificador de usuario necesarios para realizar el inicio de sesión único (SSO) de Azure AD.

* **Requisitos de acceso condicional**: No se recomienda mediante Proxy de aplicación para el acceso a la intranet, ya que esto agrega latencia que afectará a los usuarios. Se recomienda usar el Proxy de aplicación con directivas de acceso condicional y de autenticación previa para el acceso remoto desde internet.  Un enfoque para proporcionar acceso condicional para su uso de la intranet es modernizar las aplicaciones para que puedan diretly autenticarse con AAD. Consulte [los recursos para migrar aplicaciones a AAD](https://docs.microsoft.com/azure/active-directory/manage-apps/migration-resources) para obtener más información. 

* **Límites de servicio**: Para protegerse frente a consumo excesivo de recursos por los inquilinos individuales son valores de limitación por cada aplicación y de inquilinos. Para ver estos límites, consulte [restricciones y límites del servicio de Azure AD](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-service-limits-restrictions). Estos límites se basan en una prueba comparativa muy por encima del volumen de uso típico y proporciona un búfer suficiente para la mayoría de las implementaciones.

* **Certificado público**: Si usa los nombres de dominio personalizado, debe adquirir un certificado público emitido por una entidad de certificación de confianza que no sean de Microsoft. Según los requisitos de su organización, cómo obtener un certificado puede tardar algún tiempo y se recomienda comenzar el proceso tan pronto como sea posible. Proxy de aplicación de Azure es compatible con el estándar, [comodín](application-proxy-wildcard.md), o certificados basados en SAN.

* **Requisitos de dominio**: Inicio de sesión único para las aplicaciones publicadas mediante la delegación limitada de Kerberos (KCD) requiere que un host de conector esté unido al dominio en el mismo dominio de AD que las aplicaciones que se va a publicar. Para obtener información detallada sobre el tema, consulte [KCD para el inicio de sesión único](application-proxy-configure-single-sign-on-with-kcd.md) con el Proxy de aplicación. El servicio de conectores se ejecuta en el contexto del sistema local y no debe configurarse para usar una identidad personalizada.

* **Registros de DNS para direcciones URL**

   * Antes de usar dominios personalizados en el Proxy de aplicación debe crear un registro CNAME en DNS público, lo que permite a los clientes resolver la dirección URL personalizada de externa definida para la dirección de Proxy de aplicación predefinida. No se puede crear un registro CNAME para una aplicación que usa un dominio personalizado impedirá que los usuarios remotos conectarse a la aplicación. Pasos necesarios para agregar registros CNAME pueden variar de DNS del proveedor, así que aprenda cómo [administrar conjuntos de registros y registros DNS mediante Azure portal](https://docs.microsoft.com/azure/dns/dns-operations-recordsets-portal).

   * De forma similar, los hosts de conector deben ser capaces de resolver la dirección URL interna de las aplicaciones que se está publicando.

* **Roles y derechos administrativos**

   * **Instalación del conector** requiere derechos de administrador local en el servidor de Windows que se está instalando en. También requiere un mínimo de un *Administrador de la aplicación* rol para autenticarse y registrar la instancia del conector para su inquilino de Azure AD. 

   * **Publicación de aplicaciones y administración** requieren el *Administrador de la aplicación* rol. Administradores de la aplicación pueden administrar todas las aplicaciones del directorio incluidos registros, configuración de SSO, usuario y las asignaciones de grupo y las licencias, configuración de Proxy de aplicación y consentimiento. No concede la capacidad de administrar el acceso condicional. El *Administrador de la aplicación en la nube* rol tiene todas las capacidades de administrador de la aplicación, salvo que no permite la administración de configuración de Proxy de aplicación.

* **Licencia**: Proxy de aplicación está disponible a través de la suscripción de Azure AD Basic. Hacer referencia a la [página precios de Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/) para obtener una lista completa de características y opciones de licencia.  

### <a name="application-discovery"></a>Detección de aplicaciones

Compilar un inventario de todas las aplicaciones en el ámbito que se están publicando a través de Proxy de aplicación mediante la recopilación de la siguiente información:

| Tipo de información| Información para recopilar |
|---|---|
| Tipo de servicio| Por ejemplo: SharePoint, SAP, CRM, API, aplicación Web personalizada |
| Plataforma de aplicaciones | Por ejemplo: Windows IIS, Apache en Linux, Tomcat, NGINX, etc. |
| Pertenencia al dominio| Nombre del servidor Web de dominio completo (FQDN) |
| Ubicación de la aplicación | Dónde se encuentra el servidor web o granja de servidores en su infraestructura |
| Acceso interno | La dirección URL exacta usa al acceder a la aplicación internamente. <br> ¿Si una granja de servidores, qué tipo de equilibrio de carga está en uso? <br> Si la aplicación dibuja el contenido de orígenes distintos a sí mismo.<br> Determinar si la aplicación funciona a través de WebSockets. |
| Acceso externo | La solución de proveedor que la aplicación ya está expuesta a externamente. <br> La dirección URL que desea usar para el acceso externo. Si SharePoint, asegúrese de que se configuran asignaciones de acceso alternativas por [esta guía](https://docs.microsoft.com/SharePoint/administration/configure-alternate-access-mappings). Si no es así, deberá definir direcciones URL externas. |
| Certificado público | Si usa un dominio personalizado, adquiera un certificado con un nombre de sujeto correspondiente. Tenga en cuenta el número de serie y la ubicación desde donde se puede obtener si existe un certificado. |
| Tipo de autenticación| El tipo de autenticación admitido por la compatibilidad de aplicaciones, como Basic, autenticación de integración de Windows, basada en formularios, basada en encabezados y notificaciones. <br>Si la aplicación está configurada para ejecutarse bajo una cuenta de dominio específicos, anote el nombre de dominio completo (FQDN) de la cuenta de servicio.<br> Si basado en SAML, las direcciones URL de identificador y la respuesta. <br> Si basada en el encabezado, escriba la solución de proveedor y un requisito específico para controlar la autenticación. |
| Nombre del grupo de conector | El nombre lógico para el grupo de conectores que se designarán para proporcionar la canalización y el inicio de sesión único a esta aplicación de back-end. |
| Acceso de usuarios o grupos | Los usuarios o grupos de usuarios que se le concederá acceso externo a la aplicación. |
| Requisitos adicionales | Tenga en cuenta cualquier acceso remoto adicional o requisitos de seguridad que deben tenerse en cuenta en la aplicación de publicación. |

Puede descargar este [hoja de cálculo de inventario de aplicaciones](https://aka.ms/appdiscovery) un inventario de sus aplicaciones.

### <a name="define-organizational-requirements"></a>Definir los requisitos organizativos

Las siguientes son las áreas para las que debe definir los requisitos empresariales de su organización. Cada área contiene ejemplos de requisitos

 **Acceder**

* Los usuarios remotos con Unidos a un dominio o los usuarios de dispositivos Unidos a Azure AD pueden tener acceso a las aplicaciones publicadas de forma segura con conexión directa single sign-on (SSO).

* Los usuarios remotos con dispositivos personales aprobados pueden acceso seguro a aplicaciones publicadas siempre que están inscritos en MFA y han registrado la aplicación Microsoft Authenticator en su teléfono móvil como método de autenticación.

**Gobierno** 

* Los administradores pueden definir y supervisar el ciclo de vida de las asignaciones de usuario a las aplicaciones publicadas a través del Proxy de aplicación.

**Seguridad**

* Solo los usuarios asignados a las aplicaciones a través de la pertenencia a grupos o individualmente pueden tener acceso a esas aplicaciones.

**Rendimiento**

* No hay ninguna degradación del rendimiento de la aplicación en comparación con acceso a la aplicación desde la red interna.

**Experiencia del usuario**

* Los usuarios sean conscientes de cómo obtener acceso a sus aplicaciones mediante el uso de direcciones URL de la empresa familiar en cualquier plataforma de dispositivo.

**Auditoría**
* Los administradores pueden auditar la actividad de acceso de usuario.


### <a name="best-practices-for-a-pilot"></a>Procedimientos recomendados para una prueba piloto

Determinar la cantidad de tiempo y esfuerzo necesarios para una sola aplicación para el acceso remoto con el inicio de sesión único (SSO) de la Comisión por completo. Puede hacerlo mediante la ejecución de un programa piloto que considera que su detección inicial, publicación y realización de pruebas generales. Mediante una aplicación web simple basada en IIS que ya está preconfigurada para la autenticación de Windows integrada (IWA) ayudaría a establecer una línea base, ya que este programa de instalación requiere un esfuerzo mínimo para el acceso remoto piloto correctamente y de inicio de sesión único.

Los siguientes elementos de diseño deben aumentar el éxito de su implementación piloto directamente en un inquilino de producción.  

**Administración del conector**:  

* Conectores desempeñan un papel clave en la que proporciona el conducto de local a las aplicaciones. Mediante el **predeterminado** grupo de conectores es adecuado para la prueba piloto inicial de las aplicaciones publicadas antes de la puesta en servicio en producción. A continuación, se pueden mover aplicaciones probadas correctamente a los grupos de conectores de producción.

**Administración de aplicaciones**:

* Los empleados es más probable que recordar que una dirección URL externa es familiar y pertinente. Evite publicar su aplicación con nuestra sufijos msappproxy.net o onmicrosoft.com predefinidos. En su lugar, proporcione un dominio comprobado familiar nivel superior, con un nombre de host lógico como el prefijo *intranet. < customers_domain > .com*.

* Restringir la visibilidad del icono de la aplicación piloto a un grupo piloto ocultando su forma de icono de inicio del portal mis aplicaciones de Azure. Cuando esté listo para producción puede restringir el ámbito de la aplicación a su respectivo audiencia de destino, en el mismo inquilino de preproducción o publicando también la aplicación en el inquilino de producción.

**Configuración de inicio de sesión único**: Algunas opciones de inicio de sesión único tienen dependencias específicas que pueden tardar tiempo en configurar, lo que evita retrasos asegurándose de que las dependencias se direccionan antes de tiempo de control de cambios. Esto incluye los hosts de conector para llevar a cabo SSO mediante delegación limitada de Kerberos (KCD) y se aseguran de otras actividades que requieren mucho tiempo de unión de dominio. Por ejemplo, configurar una instancia de PING Access, si la necesidad de inicio de sesión único basado en encabezados.

**SSL entre el Host de conector y la aplicación de destino**: La seguridad es fundamental, por lo que siempre se debe usar TLS entre las aplicaciones de host y de destino del conector. Especialmente si la aplicación web está configurada para la autenticación basada en formularios (FBA), como las credenciales de usuario, a continuación, se transmiten de forma eficaz en texto no cifrado.

**Implementar de forma incremental y probar cada paso**. Realizar pruebas funcionales básicas después de publicar una aplicación para asegurarse de que se cumplen todos los requisitos empresariales y de usuario siguiendo estas instrucciones:

1. Probar y validar el acceso general a la aplicación web con la autenticación previa deshabilitada.
2. Si se realiza correctamente, habilite la autenticación previa y asignar usuarios y grupos. Probar y validar el acceso.
3. A continuación, agregue el método de inicio de sesión único para la aplicación y pruebe de nuevo para validar el acceso.
4. Aplicar el acceso condicional y las directivas MFA según sea necesario. Probar y validar el acceso.

**Herramientas para solucionar problemas**: Para solucionar el problema, siempre empiece por validar el acceso a la aplicación publicada desde el explorador en el host de conector y confirme que la aplicación funciona según lo esperado. Cuanto más simple sea la configuración, más fácil para determinar la causa raíz, por lo que puede intentar reproducir problemas con una configuración mínima, por ejemplo, utilizando solo un único conector y ningún inicio de sesión único. En algunos casos, la web, herramientas de depuración como Fiddler de Telerik puede resultar indispensable para solucionar problemas de acceso o el contenido en las aplicaciones que se tiene acceso a través de un servidor proxy. Fiddler puede actuar también como un proxy para ayudar a seguimiento y depurar el tráfico para plataformas móviles como iOS y Android, y prácticamente cualquier cosa que puede configurarse para enrutar a través de un servidor proxy. Consulte la [Guía de solución](application-proxy-troubleshoot.md) para obtener más información.

## <a name="implement-your-solution"></a>Implementar la solución

### <a name="deploy-application-proxy"></a>Implementar el Proxy de aplicación

Los pasos para implementar el Proxy de aplicación se tratan en este [tutorial para agregar una aplicación de forma local para el acceso remoto](application-proxy-add-on-premises-application.md). Si no se realiza correctamente la instalación, seleccione **solucionar problemas de Proxy de aplicación** en el portal o use la Guía de solución de problemas [para problemas de instalación del conector de agente de Proxy de aplicación](application-proxy-connector-installation-problem.md).

### <a name="publish-applications-via-application-proxy"></a>Publicar aplicaciones a través de Proxy de aplicación

Publicación de aplicaciones se supone que se han cumplido los requisitos previos y que tiene varios conectores que se muestra como registrado y activas en la página del Proxy de aplicación.

También puede publicar aplicaciones mediante [PowerShell](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview).

A continuación se muestran algunas prácticas recomendadas al publicar una aplicación:

* **Usar grupos de conectores**: Asignar a un grupo de conectores que se ha designado para la publicación de cada aplicación. Se recomienda que cada grupo de conectores tiene al menos dos conectores para proporcionar escalabilidad y alta disponibilidad. Tener tres conectores es óptimo en caso de que necesite una máquina en cualquier punto de servicio. Además, consulte [publicar aplicaciones en redes independientes y ubicaciones mediante grupos de conectores](application-proxy-connector-groups.md) para ver cómo puede utilizar también grupos de conectores para segmentar los conectores de red o ubicación.

* **Establecer tiempo de espera de aplicación de back-end**: Esta configuración es útil en escenarios donde la aplicación puede requerir más de 75 segundos procesar una transacción de cliente. Por ejemplo cuando un cliente envía una consulta a una aplicación web que actúa como un front-end para una base de datos. El front-end envía esta consulta a su servidor de base de datos back-end y espera una respuesta, pero cuando que recibe una respuesta, tiempo de espera del cliente de la conversación. Establecer el tiempo de espera a la larga ofrece 180 segundos para las transacciones más tiempo en completarse.

* **Usar tipos adecuados de Cookie**

   * **HTTP-Only Cookie**: Proporciona seguridad adicional al tener el Proxy de aplicación incluyen la marca HTTPOnly en los encabezados de respuesta HTTP set-cookie. Esta configuración ayuda a mitigar las vulnerabilidades como scripting entre sitios (XSS). Deje este valor en No para los agentes de usuario o los clientes que requieren acceso a la cookie de sesión. Por ejemplo, el cliente RDP/MTSC que se conecta a una puerta de enlace de escritorio remoto se publica a través de Proxy de aplicación.

   * **Proteger la Cookie**: Cuando se establece una cookie con el atributo Secure, el agente de usuario (aplicación de cliente) solo incluirá la cookie en solicitudes HTTP si la solicitud se transmite a través de un canal seguro TLS. Esto ayuda a mitigar el riesgo de una cookie que se encuentren en peligro a través de canales de texto no cifrado, por lo que debe estar habilitada.

   * **Cookie persistente**: Permite que la cookie de sesión del Proxy de aplicación se conserven entre los cierres de explorador por siga siendo válido hasta que expire o se elimina. Se utiliza para escenarios donde una aplicación enriquecida, como office tiene acceso a un documento dentro de una aplicación web publicada, sin que el usuario vuelva a que se solicitará la autenticación. Habilitar con precaución, sin embargo, como cookies persistentes puede en última instancia dejar un servicio corre el riesgo de acceso no autorizado, si no se utiliza junto con otros controles de compensación. Esta configuración solo debe usarse para aplicaciones más antiguas que no se pueden compartir cookies entre procesos. Es mejor actualizar la aplicación para controlar el uso compartidas cookies entre procesos en lugar de usar esta configuración.

* **Traducir URL en encabezados**: Habilite esta opción para escenarios donde no se puede configurar DNS interno para que coincida con el espacio de nombres de la organización pública (conocidas también como DNS dividido). A menos que la aplicación requiere el encabezado de host original en la solicitud de cliente, deje este valor establecido en Sí. La alternativa es que el conector de usar el FQDN en la dirección URL interna para el enrutamiento de tráfico real y el FQDN de la dirección URL externa, como el encabezado de host. En la mayoría de los casos esta alternativa debe permitir la aplicación funcione con normalidad cuando acceso remoto, pero los usuarios pierden las ventajas de tener una coincidencia dentro y fuera de la dirección URL.

* **Traducir direcciones URL en el cuerpo de la aplicación**: Activar la traducción de vínculos del cuerpo de la aplicación para una aplicación cuando desee que los vínculos de esa aplicación que se traducirá en las respuestas al cliente. Si está habilitada, esta función proporciona un intento de mejor esfuerzo traducir todos los vínculos internos que Proxy de aplicación busca en las respuestas HTML y CSS que se devuelven a los clientes. Resulta útil cuando la publicación que contienen rígida absoluto o vínculos de NetBIOS Nombre corto en el contenido o aplicaciones con contenido que se vincula a otras aplicaciones locales.

Para escenarios donde las aplicaciones publican de vínculos a otra aplicación publicada, habilitar la traducción de vínculos para cada aplicación para que tenga control sobre la experiencia del usuario en el nivel por aplicación.

Por ejemplo, imagine que tiene tres aplicaciones publicadas a través del Proxy de aplicación que se vinculan entre sí: Beneficios, gastos y viajes, más una cuarta aplicación, comentarios, que no se publiquen a través del Proxy de aplicación.

![Imagen 1](media/App-proxy-deployment-plan/link-translation.png)

Al habilitar la traducción de vínculos para la aplicación de las ventajas, los vínculos a gastos y viajes se redirigen a las direcciones URL externas de esas aplicaciones, para que los usuarios que acceden a las aplicaciones desde fuera de la red corporativa pueden tener acceso a ellos. Vínculos de gastos y viajes a beneficios no funcionan porque aún no ha habilitado la traducción de vínculos para esas dos aplicaciones. El vínculo a los comentarios no se redirige porque no hay ninguna dirección URL externa, por lo que los usuarios mediante la aplicación de beneficios no podrán tener acceso a la aplicación de comentarios desde fuera de la red corporativa. Ver información detallada sobre [vincular traducción y otras opciones de redireccionamiento](application-proxy-configure-hard-coded-link-translation.md).

### <a name="access-your-application"></a>Obtener acceso a la aplicación

Existen varias opciones para administrar el acceso al Proxy de aplicación de los recursos publicados, así que elija las más adecuadas para sus necesidades de escalabilidad y el escenario determinados. Enfoques comunes incluyen: crear grupos dinámicos en Azure AD mediante grupos locales que se están sincronizando a través de Azure AD Connect, basándose en atributos de usuario, uso de grupos de autoservicio administrados por el propietario de un recurso o una combinación de todos ellos. Consulte los recursos vinculados para los beneficios de cada uno.

La manera más sencilla de asignar los usuarios acceso a una aplicación que se va a la **usuarios y grupos** opciones desde el panel izquierdo de la aplicación publicada y la asignación directa de grupos o individuos.

![Imagen 24](media/App-proxy-deployment-plan/add-user.png)

También puede permitir que los usuarios acceso de autoservicio a su aplicación mediante la asignación de un grupo que no están actualmente un miembro de y configurar las opciones de autoservicio.

![Imagen 25](media/App-proxy-deployment-plan/allow-access.png)

Si está habilitada, los usuarios, a continuación, podrán iniciar sesión en el portal y solicitar acceso de mis aplicaciones, y ya sea automáticamente aprobado y agregado a los grupos de autoservicio ya permitidos o necesitan aprobación de un aprobador designado.

También pueden ser usuarios invitados [invitado a acceder a las aplicaciones internas que se publican a través de Proxy de aplicación a través de Azure AD B2B](https://docs.microsoft.com/azure/active-directory/b2b/add-users-information-worker).

Para las aplicaciones locales que son normalmente accesibles de forma anónima, que requieren sin autenticación, puede que prefiera deshabilitar la opción que se encuentra en la aplicación **propiedades**.

![Imagen de 26](media/App-proxy-deployment-plan/assignment-required.png)


Salir de esta opción establecida en No permite a los usuarios tener acceso a la aplicación en el entorno local a través de Proxy de aplicación de Azure AD sin permisos, así que úselas con precaución.

Una vez publicada la aplicación, debe ser accesible para ello, escriba su dirección URL externa en un explorador o por su icono en [ https://myapps.microsoft.com ](https://myapps.microsoft.com/).

### <a name="enable-pre-authentication"></a>Habilitar la autenticación previa

Compruebe que la aplicación es accesible a través del Proxy de aplicación tener acceso a él a través de la dirección URL externa. 

1. Vaya a **Azure Active Directory** > **Aplicaciones empresariales** > **Todas las aplicaciones** y elija la aplicación que quiere administrar.

2. Seleccione **Proxy de la aplicación**.

3. En el **autenticación previa** campo, use la lista desplegable para seleccionar **Azure Active Directory**y seleccione **guardar**.

Con la autenticación previa habilitada, Azure AD supondrán un reto para los usuarios en primer lugar para la autenticación y si el inicio de sesión único está configurado con, a continuación, la aplicación back-end también comprobará el usuario antes de que se concede acceso a la aplicación. También cambio del modo de autenticación previa de acceso directo a Azure AD configura la dirección URL externa con HTTPS, por lo que cualquier aplicación que se configura inicialmente para HTTP ahora estará protegido con HTTPS.

### <a name="enable-single-sign-on"></a>Habilitar el inicio de sesión único

Dado que los usuarios solo necesitan iniciar sesión una vez al obtener acceso a Azure AD, SSO proporciona la mejor experiencia posible y la seguridad. Una vez que un usuario se ha autenticado previamente, inicio de sesión único realiza la autenticación de conector de Proxy de aplicación a la aplicación de forma local, en nombre del usuario. La aplicación de back-end procesa el inicio de sesión como si fuera el usuario a sí mismos. 

Elegir el **Passthrough** opción permite a los usuarios tener acceso a la aplicación publicada, sin tener que autenticarse en Azure AD.

Realizar el inicio de sesión único solo es posible que si Azure AD pueda identificar el usuario que solicita acceso a un recurso, por lo que la aplicación debe configurarse para la autenticación previa de usuarios con Azure AD al acceso de inicio de sesión único funcione, en caso contrario, se deshabilitarán las opciones de inicio de sesión único.

Lectura [inicio de sesión único para aplicaciones de Azure AD](what-is-single-sign-on.md) para ayudarle a elegir el método de inicio de sesión único más adecuado al configurar las aplicaciones.

###  <a name="working-with-other-types-of-applications"></a>Trabajar con otros tipos de aplicaciones

Proxy de aplicación de Azure AD también pueden admitir aplicaciones que se han desarrollado para usar nuestra biblioteca de autenticación de Azure AD ([ADAL](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-libraries)) o biblioteca de autenticación de Microsoft ([MSAL](https://azure.microsoft.com/blog/start-writing-applications-today-with-the-new-microsoft-authentication-sdks/)). Es compatible con las aplicaciones cliente nativas por consumo de Azure AD emitida tokens recibidos en la información de encabezado de solicitud de cliente para realizar la autenticación previa en nombre de los usuarios.

Lectura [publicar las aplicaciones cliente nativas y móviles](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-native-client) y [aplicaciones basadas en notificaciones](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-claims-aware-apps) para obtener información sobre las configuraciones disponibles del Proxy de aplicación.

### <a name="use-conditional-access-to-strengthen-security"></a>Use el acceso condicional para reforzar la seguridad

Seguridad de la aplicación requiere un conjunto avanzado de capacidades de seguridad que pueden impedir y responder a amenazas complejas de forma local y en la nube. Más a menudo, los atacantes obtienen acceso a la red corporativa a través de escasa, predeterminada o credenciales de usuario robadas.  Seguridad basada en identidades de Microsoft reduce el uso de credenciales robadas al administrar y proteger las identidades con privilegios y sin privilegios.

Las siguientes funcionalidades pueden usarse para admitir el Proxy de aplicación de Azure AD:

* Usuario y el acceso condicional basado en la ubicación: Mantener los datos confidenciales protegidos por limitar el acceso de usuario según la ubicación geográfica o una dirección IP con [las directivas de acceso condicional basado en ubicación](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-locations).

* Acceso condicional basado en dispositivo: Asegúrese de que solo los dispositivos inscritos, aprobados y compatibles pueden tener acceso a los datos corporativos con [acceso condicional basado en dispositivo](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-policy-connected-applications).

* Acceso condicional basado en la aplicación: Trabajo no tiene que detener cuando un usuario no se encuentra en la red corporativa. [Acceso seguro a aplicaciones locales y en la nube corporativas](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-mam) y mantener el control con el acceso condicional.

* Acceso condicional basado en riesgos: Proteger los datos de los piratas informáticos con un [directiva de acceso condicional basado en riesgos](https://www.microsoft.com/cloud-platform/conditional-access) que puede aplicarse a todas las aplicaciones y todos los usuarios, si en el entorno local o en la nube.

* Panel de acceso de Azure AD: Con el servicio de Proxy de aplicación implementado y las aplicaciones publicadas de forma segura, ofrecer a los usuarios un concentrador sencillo para detectar y tener acceso a todas sus aplicaciones. Aumente la productividad con capacidades de autoservicio, como la capacidad de solicitar acceso a nuevas aplicaciones y grupos o administrar el acceso a estos recursos en nombre de otros, a través de la [Panel de acceso](https://aka.ms/AccessPanelDPDownload).

## <a name="manage-your-implementation"></a>Administración de la implementación

### <a name="required-roles"></a>Roles necesarios

Microsoft encarga el principio de conceder los privilegios mínimos para llevar a cabo las tareas necesarias con Azure AD. [Revise los distintos roles de Azure que están disponibles](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal) y elegir la correcta para abordar las necesidades de cada rol. Algunos roles que deba aplicarse temporalmente y quitará una vez completada la implementación.

| Función de negocio| Tareas empresariales| Roles de Azure AD |
|---|---|---|
| Ayudar al administrador del servicio de asistencia | Normalmente, limitado a calificar usuario final informa de problemas y realizar tareas limitadas, como cambiar las contraseñas de usuario, lo que invalida los tokens de actualización y supervisar el estado del servicio. | Administrador del departamento de soporte técnico |
| Administración de identidad| Problemas relacionados con la sesión de AD Azure de lectura en los informes y los registros de auditoría para depurar el Proxy de aplicación.| Lector de seguridad |
| Propietario de la aplicación| Crear y administrar todos los aspectos de las aplicaciones empresariales, registros de aplicación y configuración de proxy de aplicación.| Administración de aplicaciones |
| Administrador de infraestructura | Propietario de la sustitución de certificados | Administración de aplicaciones |

Minimizar el número de personas que tienen acceso a información segura o a los recursos le ayudará a reducir la posibilidad de que un individuo malintencionado obtener acceso no autorizado o un usuario autorizado sin darse cuenta que afectan a recursos sensibles. 
 
Sin embargo, los usuarios aún necesitan llevar a cabo operaciones con privilegios de un día para otro, por lo que aplicar en función just-in-time (JIT) [Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure) directivas para proporcionar y a petición con privilegios de acceso a recursos de Azure y Azure AD es nuestro recomienda enfoque hacia la administración del acceso administrativo de forma eficaz y auditoría.

### <a name="reporting-and-monitoring"></a>Creación de informes y supervisión

Azure AD puede proporcionar información adicional en uso y estado de las operaciones a través de los registros de auditoría e informes de aprovisionamiento de usuarios de su organización. 

#### <a name="application-audit-logs"></a>Registros de auditoría de la aplicación

Estos registros proporcionan información detallada sobre los inicios de sesión para las aplicaciones configuradas con el Proxy de aplicación y el dispositivo y el usuario tiene acceso a la aplicación. Los registros de auditoría se encuentran en el portal de Azure y en la API de auditoría para la exportación.

#### <a name="windows-event-logs-and-performance-counters"></a>Contadores de rendimiento y registros de eventos de Windows

Los conectores tienen la administración y sesión registros. Los registros de administración incluyen eventos importantes y sus errores. Los registros de sesión incluyen todas las transacciones y sus detalles de procesamiento. Contadores y registros se encuentran en registros de eventos de Windows y seguir este [tutorial para configurar orígenes de datos de registro de eventos en Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/data-sources-windows-events).

### <a name="troubleshooting-guide-and-steps"></a>Guía de solución de problemas y pasos

Más información sobre los problemas comunes y cómo resolverlos con nuestra guía para [solución de problemas de](application-proxy-troubleshoot.md) mensajes de error. 

Los siguientes artículos cubren escenarios comunes que pueden usarse también para crear a guías de solución de problemas para su organización de soporte técnico. 

* [Página de aplicación que muestra los problemas](application-proxy-page-appearance-broken-problem.md)
* [La carga de la aplicación es demasiado larga](application-proxy-page-load-speed-problem.md)
* [Los vínculos en la página de aplicación no funcionan](application-proxy-page-links-broken-problem.md)
* [¿Qué puertos se deben abrir para mi aplicación?](application-proxy-connectivity-ports-how-to.md)
* [No hay ningún conector funcionando en un grupo de conectores para mi aplicación](application-proxy-connectivity-no-working-connector.md)
* [Configuración en el portal de administración](application-proxy-config-how-to.md)
* [Configuración del inicio de sesión único en mi aplicación](application-proxy-config-sso-how-to.md)
* [Problema al crear una aplicación en el portal de administración](application-proxy-config-problem.md)
* [Configuración de la delegación limitada de Kerberos](application-proxy-back-end-kerberos-constrained-delegation-how-to.md)
* [Configuración con PingAccess](application-proxy-back-end-ping-access-how-to.md)
* [No se puede obtener acceso a este error de aplicación corporativa](application-proxy-sign-in-bad-gateway-timeout-error.md)
* [Problema al instalar el conector de agente del proxy de aplicación](application-proxy-connector-installation-problem.md)
* [Problema de inicio de sesión](application-sign-in-problem-on-premises-application-proxy.md)
