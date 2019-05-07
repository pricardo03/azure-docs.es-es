---
title: Preguntas más frecuentes sobre Azure Active Directory Connect | Microsoft Docs
description: En este artículo se responden a las preguntas más frecuentes sobre Azure AD Connect.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 4e47a087-ebcd-4b63-9574-0c31907a39a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 05/03/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2caca430de5ad666f4f4341e0723bc3173d6d91a
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65137801"
---
# <a name="azure-active-directory-connect-faq"></a>Preguntas más frecuentes sobre Azure Active Directory Connect

## <a name="general-installation"></a>Instalación general

**P: ¿Cómo puedo proteger mi servidor de Azure AD Connect para reducir la superficie de ataque de seguridad?**

Microsoft recomienda proteger el servidor de Azure AD Connect para reducir la superficie de ataque de seguridad para este componente esencial de su entorno de TI.  Siga las recomendaciones siguientes se reducirá los riesgos de seguridad para su organización.

* Implementar Azure AD Connect en un servidor unido a un dominio y restringir el acceso administrativo a los administradores de dominio u otros grupos de seguridad rigurosamente controladas

Para obtener más información, consulte: 

* [Protección de los grupos de administradores](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/appendix-g--securing-administrators-groups-in-active-directory)

* [Protección de cuentas de administrador integrada](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/appendix-d--securing-built-in-administrator-accounts-in-active-directory)

* [Mejora de seguridad y sustainment reduciendo las superficies de ataque](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access#2-reduce-attack-surfaces )

* [Reducir la superficie de ataque de Active Directory](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/reducing-the-active-directory-attack-surface)

**P: ¿Funcionará la instalación si el administrador global de Azure Active Directory (Azure AD) tiene habilitada la autenticación en dos fases (2FA)?**  
A partir de las compilaciones de febrero de 2016, se admite este escenario.

**P: ¿Existe alguna manera de instalar Azure AD Connect de manera desatendida?**  
La instalación de Azure AD Connect se admite solo cuando se usa el Asistente para la instalación. No se admite la instalación silenciosa desatendida.

**P: Tengo un bosque en el que no se puede establecer comunicación con un dominio. ¿Cómo se puede instalar Azure AD Connect?**  
A partir de las compilaciones de febrero de 2016, se admite este escenario.

**P: ¿Funciona el agente de estado de Azure Active Directory Domain Services (Azure AD DS) en Server Core?**  
Sí. Después de instalar el agente, puede completar el proceso de registro mediante el siguiente cmdlet de PowerShell: 

`Register-AzureADConnectHealthADDSAgent -Credentials $cred`

**P: ¿Admite Azure AD Connect la sincronización de dos dominios en Azure AD?**  
Sí, este escenario se admite. Consulte [Varios dominios](how-to-connect-install-multiple-domains.md).
 
**P: ¿Se pueden tener varios conectores para el mismo dominio de Active Directory en Azure AD Connect?**  
No, no se admiten varios conectores para el mismo dominio de AD. 

**P: ¿Se puede mover la base de datos de Azure AD Connect de la base de datos local a una instancia remota de SQL Server?**   
Sí, en los pasos siguientes se proporcionan instrucciones generales sobre cómo hacerlo. Actualmente estamos trabajando en un documento más detallado.
1. Realice una copia de seguridad de la base de datos LocalDB ADSync.
La manera más sencilla de hacerlo es usar SQL Server Management Studio instalado en el mismo equipo que Azure AD Connect. Conéctese a *(LocalDb).\ADSync* y, a continuación, realice una copia de seguridad de la base de datos ADSync.

2. Restaure la base de datos ADSync en la instancia remota de SQL Server.

3. Instale Azure AD Connect en la instancia remota de [SQL Database](how-to-connect-install-existing-database.md) existente.
   En el artículo se muestra cómo migrar para usar una instancia local de SQL Database. Si realiza la migración para usar una instancia remota de SQL Database, en el paso 5 del proceso también debe especificar una cuenta de servicio existente para la ejecución del servicio de sincronización de Windows. Esta cuenta de servicio del motor de sincronización se describe aquí:
   
      **Uso de una cuenta de servicio existente**: de forma predeterminada, Azure AD Connect usa una cuenta de servicio virtual para que la utilicen los servicios de sincronización. Si usa una instancia remota de SQL Server o un proxy que requiera autenticación, debe usar una cuenta de servicio administrada o una cuenta de servicio en el dominio, y conocer la contraseña. En esos casos, especifique la cuenta que se va a usar. Asegúrese de que los usuarios que ejecuten la instalación sean administradores del sistema de SQL para que se pueden crear credenciales de inicio de sesión para la cuenta de servicio. Para más información, consulte [Azure AD Connect: cuentas y permisos](reference-connect-accounts-permissions.md#adsync-service-account). 
   
      Con la versión más reciente, el administrador SQL puede realizar ahora el aprovisionamiento de la base de datos fuera de banda y luego el administrador de Azure AD Connect puede instalarlo con derechos de propietario de la base de datos. Para más información, consulte [Instalación de Azure AD Connect con permisos de administrador delegado de SQL](how-to-connect-install-sql-delegation.md).

Para simplificar las cosas, se recomienda que los usuarios que instalen Azure AD Connect sean administradores del sistema en SQL. Sin embargo, con las últimas compilaciones se pueden usar administradores de SQL delegados, como se describe en [Instalación de Azure AD Connect con permisos de administrador delegado de SQL](how-to-connect-install-sql-delegation.md).

**P: ¿Cuáles son algunas de las prácticas recomendadas del campo?**  

El siguiente es un documento informativo que presenta algunas de las prácticas recomendadas que admiten la ingeniería, y nuestros consultores han desarrollado con los años.  Esto se presenta en una lista con viñetas que puede hacer referencia rápidamente.  Aunque se trata de esta lista esté completa, puede haber recomendaciones adicionales que es posible que no lo hacen en la lista aún.

- Si usa SQL completo, a continuación, debe seguir siendo local frente a remotos
    - Número menor de saltos
    - Más fácil solucionar
    - Menor complejidad
    - Debe designar recursos para SQL y permitir una sobrecarga para Azure AD Connect y del sistema operativo
- Omitir Proxy si es posible, si no puede omitir el proxy, deberá asegurarse de que el valor de tiempo de espera es mayor que 5 minutos.
- Si el proxy es necesario, a continuación, debe agregar al proxy al archivo machine.config
- Tenga en cuenta los trabajos locales de SQL y cómo afectan a Azure AD Connect: especialmente reindexación y mantenimiento
- Asegúrese de que DNS pueda resolver externamente
- Asegúrese de que [especificaciones del servidor](how-to-connect-install-prerequisites.md#hardware-requirements-for-azure-ad-connect) son por recomendación si usa servidores físicos o virtuales
- Asegúrese de que si usa un servidor virtual que se dedican los recursos necesarios
- Asegúrese de que tiene el disco y la configuración de disco cumple las prácticas recomendadas para SQL Server
- Instalar y configurar Azure AD Connect Health para la supervisión
- Utilice el umbral de eliminación que está integrada en Azure AD Connect.
- Cuidadosamente actualizaciones de versión de revisión para estar preparado para todos los cambios y los nuevos atributos que pueden agregarse
- Copia de seguridad de todo
    - Claves de copia de seguridad
    - Reglas de sincronización copia de seguridad
    - Configuración del servidor de copia de seguridad
    - Base de datos de copia de seguridad de SQL
- Asegúrese de que no hay ningún agente de copia de seguridad de 3ª parte que se copia de seguridad de SQL sin que el escritor de VSS de SQL (común en los servidores virtuales con instantáneas de 3ª parte)
- Limitar la cantidad de reglas de sincronización personalizadas que se usan a medida que agregan complejidad
- Azure AD TREAT conectar servidores como nivel 0 servidores
- Ser de modificación de las reglas de sincronización en la nube sin excelente comprender el impacto y los controladores de negocio correctas
- Asegúrese de que la dirección URL correcta y los puertos de Firewall estén abiertos para obtener soporte técnico de Azure AD Connect y Azure AD Connect Health
- Aproveche el atributo filtrado en la nube para solucionar problemas y evitar que los objetos fantasmas
- Con el servidor de almacenamiento provisional, asegúrese de que están usando a Documentador de configuración de AD Connect Azure para mantener la coherencia entre los servidores
- Servidores de ensayo deben estar en los centros de datos independiente (ubicaciones físicas
- Servidores de ensayo no están diseñados para ser una solución de alta disponibilidad, pero puede tener varios servidores de ensayo
- Introducción a los servidores de ensayo de "Lag" podría mitigar algunos posibles tiempos de inactividad en caso de error
- Probar y validar todas las actualizaciones en el servidor de ensayo en primer lugar
- Valide siempre las exportaciones antes de cambiar el almacenamiento provisional serverLeverage el servidor de ensayo de importación completa y las sincronizaciones completas reducir el impacto de negocio
- Mantener la coherencia de versión entre servidores de Azure AD Connect tanto como sea posible 

**P: ¿Permitir que Azure AD Connect para crear la cuenta de conector de Azure AD en el equipo de grupo de trabajo?**
 No.  Para permitir que Azure AD Connect para la creación automática de la cuenta del conector de Azure AD, el equipo debe estar unido al dominio.  

## <a name="network"></a>Red
**P: Tengo un firewall, un dispositivo de red u otro elemento que limita el tiempo que las conexiones pueden permanecer abiertas en mi red. ¿Cuál debería ser mi umbral de tiempo de expiración del cliente al usar Azure AD Connect?**  
Todo el software de redes, dispositivos físicos o cualquier otra cosa que limite el tiempo máximo que las conexiones permanecen abiertas deben usar un umbral de un mínimo de cinco minutos (300 segundos) para la conectividad entre el servidor donde está instalado el cliente de Azure AD Connect y Azure Active Directory. Esta recomendación también se aplica a todas las herramientas de sincronización de identidades de Microsoft publicadas anteriormente.

**P: ¿Se admiten los dominios de una sola etiqueta (SLD)?**  
Aunque no se recomienda esta configuración de red ([consulte el artículo](https://support.microsoft.com/help/2269810/microsoft-support-for-single-label-domains)), se admite el uso de la sincronización de Azure AD Connect con un dominio de una sola etiqueta, siempre que la configuración de red para el dominio de un solo nivel funcione correctamente.

**P: ¿Se admiten los bosques con dominios de AD no contiguos?**  
No, Azure AD Connect no admite bosques locales que contengan espacios de nombres no contiguos.

**P: ¿Se admiten los nombres de NetBios "con puntos"?**  
No, Azure AD Connect no admite bosques o dominios locales en los que el nombre de NetBios contenga un punto (.).

**P: ¿Se admiten entornos puros de IPv6?**  
No, Azure AD Connect no admite entornos puros de IPv6.

**P: Tengo un entorno de varios bosques y la red entre los dos bosques es mediante NAT (traducción de direcciones de red). ¿Usa Azure AD Connect entre estos dos bosques admitidos?**</br>
 No, no se admite el uso de Azure AD Connect sobre NAT. 

## <a name="federation"></a>Federación
**P: ¿Qué debo hacer si recibo un correo electrónico que me pide que renueve el certificado de Office 365?**  
Para obtener instrucciones acerca de cómo renovar el certificado, consulte el artículo sobre la [renovación de certificados](how-to-connect-fed-o365-certs.md).

**P: Tengo establecido "Actualizar automáticamente usuario confianza" para el usuario de confianza de Office 365. ¿Es necesario realizar alguna acción cuando se implemente automáticamente mi certificado de firma de tokens?**  
Siga las instrucciones que se describen en el artículo sobre la [renovación de certificados](how-to-connect-fed-o365-certs.md).

## <a name="environment"></a>Entorno
**P: ¿Se admite cambiar el nombre del servidor después de haber instalado Azure AD Connect?**  
 No. Cambiar el nombre de servidor provoca que el motor de sincronización no se pueda conectar a la instancia de SQL Database y no se pueda iniciar el servicio.

**P: ¿Se admiten las reglas de sincronización de próxima generación criptográfico (NGC o cualquier) en un equipo habilitado para FIPS?**  
 No.  No se admiten.

## <a name="identity-data"></a>Datos de identidad
**P: ¿Por qué el atributo userPrincipalName (UPN) de Azure AD no coincide con el UPN local?**  
Para obtener más información, consulte estos artículos:

* [Usernames in Office 365, Azure, or Intune don't match the on-premises UPN or alternate login ID](https://support.microsoft.com/kb/2523192) (Los nombres de usuario de Office 365, Azure o Intune no coinciden con el UPN local o la id. de inicio de sesión alternativo)
* [Changes aren't synced by the Azure Active Directory sync tool after you change the UPN of a user account to use a different federated domain](https://support.microsoft.com/kb/2669550) (La herramienta de sincronización de Azure Active Directory no sincroniza los cambios después de cambiar el UPN de una cuenta de usuario para usar otro dominio federado)

También puede configurar Azure AD para permitir que el motor de sincronización actualice el UPN, como se describe en [Características del servicio de sincronización de Azure AD Connect](how-to-connect-syncservice-features.md).

**P: ¿Se admiten coincidencias parciales de objetos de contacto o grupo de Azure AD locales con objetos de contacto o grupo de Azure AD existentes?**  
Sí, esta coincidencia parcial se basa en la propiedad proxyAddress. No se admite la coincidencia parcial en grupos no habilitados para correo.

**P: ¿Se admite configurar manualmente el atributo ImmutableId en objetos de contacto o grupo de Azure AD existentes para conseguir una coincidencia exacta con objetos de contacto o grupo de Azure AD locales?**  
No, establecer manualmente el atributo ImmutableId en un objeto de contacto o grupo de Azure AD existente para conseguir una coincidencia exacta no se admite actualmente.

## <a name="custom-configuration"></a>Configuración personalizada
**P: ¿Dónde se documentan los cmdlets de PowerShell para Azure AD Connect?**  
A excepción de los cmdlets documentados en este sitio, el resto de cmdlets de PowerShell que se encuentran en Azure AD Connect no se admiten para uso del cliente.

**P: ¿Puedo usar la opción "Server export/server import" (Exportación/importación de servidor) que se encuentra en Synchronization Service Manager para mover la configuración entre servidores?**  
 No. Esta opción no recupera todas las opciones de configuración y no debe usarse. En su lugar, use el asistente para crear la configuración base en el segundo servidor y utilizar el editor de reglas de sincronización para generar scripts de PowerShell para mover cualquier regla personalizada entre servidores. Para más información, consulte [Migración oscilante](how-to-upgrade-previous-version.md#swing-migration).

**P: ¿Se pueden almacenar en caché las contraseñas de la página de inicio de sesión de Azure y se puede impedir este almacenamiento en caché porque contenga un elemento de entrada de contraseña con el atributo *autocomplete = "false"*?**  
Actualmente, no se admite la modificación de los atributos HTML del campo **Contraseña**, incluida la etiqueta de autocompletar. Estamos trabajando en una característica que permita JavaScript personalizado, que permita agregar cualquier atributo al campo **Contraseña**.

**P: En la página de inicio de sesión de Azure, se muestran nombres de usuario de los usuarios que han iniciado sesión anteriormente de forma correcta. ¿Este comportamiento se puede desactivar?**  
Actualmente no se admite la modificación de los atributos HTML del campo de entrada **Contraseña**, incluida la etiqueta de autocompletar. Estamos trabajando en una característica que permita JavaScript personalizado, que permita agregar cualquier atributo al campo **Contraseña**.

**P: ¿Existe alguna manera de evitar las sesiones simultáneas?**  
 No.

## <a name="auto-upgrade"></a>Actualización automática

**P: ¿Cuáles son las ventajas y consecuencias de usar la actualización automática?**  
Se recomienda a todos los clientes que habiliten la actualización automática para la instalación de Azure AD Connect. Las ventajas son que siempre se reciben las revisiones más recientes, incluidas las actualizaciones de seguridad para vulnerabilidades detectadas en Azure AD Connect. El proceso de actualización es sencillo y se realiza automáticamente en cuanto se encuentra disponible una versión nueva. Muchos miles de clientes de Azure AD Connect usan la actualización automática con cada nueva versión.

El proceso de actualización automática siempre establece en primer lugar si una instalación es apta para actualización automática. Si es apta, se realiza y prueba la actualización. El proceso también incluye la búsqueda de cambios personalizados en las reglas y los factores de entorno específicos. Si las pruebas revelan que una actualización no se realizó correctamente, se restaura automáticamente la versión anterior.

Según el tamaño del entorno, el proceso puede tardar un par de horas. Mientras la actualización está en curso, no se produce ninguna sincronización entre Windows Server Active Directory y Azure AD.

**P: Recibí un correo electrónico para comunicarme que mi versión de actualización automática ya no funciona y que tengo que instalar una nueva. ¿Por qué tengo que hacerlo?**  
El año pasado se publicó una versión de Azure AD Connect que, en determinadas circunstancias, podría haber deshabilitado la característica de actualización automática en el servidor. Se ha corregido el problema en Azure AD Connect versión 1.1.750.0. Si se ha visto afectado por el problema, puede mitigarlo con la ejecución de un script de PowerShell para repararlo o actualizando manualmente a la versión más reciente de Azure AD Connect. 

Para ejecutar el script de PowerShell, [descargue el script](https://aka.ms/repairaadconnect) y ejecútelo en el servidor de Azure AD Connect en una ventana de PowerShell administrativa. Para obtener información sobre cómo ejecutar el script, [vea este breve vídeo](https://aka.ms/repairaadcau).

Para actualizar manualmente, debe descargar y ejecutar la última versión del archivo AADConnect.msi.
 
-  Si su versión actual es anterior a la 1.1.750.0, [descargue y actualice a la última versión](https://www.microsoft.com/download/details.aspx?id=47594).
- Si la versión de Azure AD Connect es 1.1.750.0 o posterior, no es necesario realizar ninguna otra acción. Ya está usando la versión que contiene la corrección de la actualización automática. 

**P: Recibí un correo electrónico donde se me pide que actualice a la última versión para volver a habilitar la actualización automática. Uso a versión 1.1.654.0. ¿Tengo que actualizarla?**  
Sí, tiene que actualizar a la versión 1.1.750.0 o posterior para volver a habilitar la actualización automática. [Descargar y actualizar a la versión más reciente](https://www.microsoft.com/download/details.aspx?id=47594).

**P: Recibí un correo electrónico donde se me pide que actualice a la última versión para volver a habilitar la actualización automática. Si usé PowerShell para habilitar la actualización automática, ¿también necesito instalar la última versión?**  
Sí, deberá actualizar a la versión 1.1.750.0 o posterior. Habilitar el servicio de actualización automática con PowerShell no mitiga el problema de actualización automática detectado en las versiones anteriores a la 1.1.750.0.

**P: ¿Quiero actualizar a una versión más reciente, pero no estoy seguro de quién instaló Azure AD Connect y no disponemos del nombre de usuario y la contraseña. ¿Necesito estos datos?**
No es necesario saber el nombre de usuario y la contraseña que se usó inicialmente para actualizar Azure AD Connect. Use cualquier cuenta de Azure AD que tenga el rol de administrador global.

**P: ¿Cómo puedo saber qué versión de Azure AD Connect estoy usando?**  
Para comprobar qué versión de Azure AD Connect está instalada en el servidor, vaya al Panel de control y busque la versión instalada de Microsoft Azure AD Connect seleccionando **Programas** > **Programas y características**, tal como se muestra a continuación:

![Versión de Azure AD Connect en el Panel de control](./media/reference-connect-faq/faq1.png)

**P: ¿Cómo actualizo a la versión más reciente de Azure AD Connect?**  
Para saber cómo actualizar a la versión más reciente, consulte [Azure AD Connect: actualización de una versión anterior a la versión más reciente](how-to-upgrade-previous-version.md) 

**P: Ya actualizamos a la última versión de Azure AD Connect el año pasado. ¿Es necesario actualizarla de nuevo?**  
El equipo de Azure AD Connect realiza actualizaciones frecuentes en el servicio. Para beneficiarse de las correcciones de errores y las actualizaciones de seguridad, así como de las nuevas características, es importante mantener al día el servidor con la versión más reciente. Si habilita la actualización automática, se actualiza automáticamente la versión del software. Para encontrar el historial de versiones de Azure AD Connect, consulte [Azure AD Connect: historial de versiones](reference-connect-version-history.md).

**P: ¿Cuánto tarda en realizarse la actualización y en qué afecta a mis usuarios?**  
El tiempo necesario para la actualización depende del tamaño de su inquilino. Para las organizaciones más grandes, podría ser mejor realizar la actualización por la noche o durante el fin de semana. Durante la actualización, no se realiza ninguna actividad de sincronización.

**P: Creo que actualicé a Azure AD Connect, pero en el portal de Office todavía se menciona DirSync. ¿Por qué ocurre esto?**  
El equipo de Office está trabajando para que las actualizaciones del portal de Office reflejen el nombre actual del producto. No refleja la herramienta de sincronización que se usa.

**P: Mi estado de la actualización automática indica "Suspendida". ¿Por qué está suspendida? ¿Debo habilitarla?**  
Se introdujo un error en una versión anterior que, en determinadas circunstancias, deja el estado de la actualización automática establecido en "Suspendida". Técnicamente, es posible habilitarla manualmente, pero requiere realizar varios pasos complejos. Lo mejor que se puede hacer es instalar la versión más reciente de Azure AD Connect.

**P: Mi empresa tiene requisitos estrictos de administración de cambios y me gustaría estar al tanto de los nuevos lanzamientos. ¿Puedo controlar cuándo se lanza la actualización automática?**  
No, no existe esta característica actualmente. La característica se está evaluando para futuras versiones.

**P: ¿Recibiré un correo electrónico si la actualización automática presenta algún error? ¿Cómo puedo saber si la actualización se realizó correctamente?**  
No se le notificará el resultado de la actualización. La característica se está evaluando para futuras versiones.

**P: ¿Se publica alguna escala de tiempo de la previsión de lanzamiento de actualizaciones automáticas?**  
La actualización automática es el primer paso del proceso de lanzamiento de una nueva versión. Cada vez que hay una nueva versión, se insertan automáticamente las actualizaciones. Las versiones más recientes de Azure AD Connect se anuncian con anticipación en la [hoja de ruta de Azure AD](../fundamentals/whats-new.md).

**P: ¿La actualización automática también actualiza Azure AD Connect Health?**  
Sí, la actualización automática también actualiza Azure AD Connect Health.

**P: ¿También se actualizan automáticamente los servidores de Azure AD Connect en modo de almacenamiento provisional?**  
Sí, puede actualizar automáticamente un servidor de Azure AD Connect que se encuentre en modo de almacenamiento provisional.

**P: Si la actualización automática genera algún error y mi servidor de Azure AD Connect no se inicia, ¿qué debo hacer?**  
En raras ocasiones, el servicio Azure AD Connect no se inicia después de realizar la actualización. En esos casos, reiniciar el servidor suele corregir el problema. Si el servicio Azure AD Connect sigue sin iniciarse, abra un vale de soporte. Para obtener más información, consulte [Create a service request to contact Office 365 support](https://blogs.technet.microsoft.com/praveenkumar/2013/07/17/how-to-create-service-requests-to-contact-office-365-support/) (Crear una solicitud de servicio para ponerse en contacto con el soporte técnico de Office 365). 

**P: No estoy seguro de los riesgos derivados de la actualización a una versión más reciente de Azure AD Connect. ¿Puede llamarme para ayudarme con la actualización?**  
Si necesita ayuda para actualizar a una versión más reciente de Azure AD Connect, abra un vale de soporte; en [Create a service request to contact Office 365 support](https://blogs.technet.microsoft.com/praveenkumar/2013/07/17/how-to-create-service-requests-to-contact-office-365-support/) (Crear una solicitud de servicio para ponerse en contacto con el soporte técnico de Office 365).

## <a name="troubleshooting"></a>solución de problemas
**P: ¿Cómo puedo obtener ayuda con Azure AD Connect?**

[Buscar en Microsoft Knowledge Base (KB)](https://www.microsoft.com/en-us/search/result.aspx?q=azure+active+directory+connect)

* Busque en KB soluciones técnicas a los problemas de break-fix más comunes sobre soporte técnico de Azure AD Connect.

[Foros de Azure Active Directory](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)

* Busque preguntas y respuestas técnicas o formule sus propias preguntas en [la comunidad de Azure AD](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).

[Obtener soporte técnico para Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-troubleshooting-support-howto)
