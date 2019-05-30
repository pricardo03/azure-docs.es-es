---
title: '¿Por qué las estaciones de trabajo seguras son importantes: Azure Active Directory'
description: Obtenga información sobre por qué las organizaciones deben crear estaciones de trabajo seguras administrados de Azure
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 05/28/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: frasim
ms.collection: M365-identity-device-management
ms.openlocfilehash: b7e9ca9fa26e9744eb0a9bfafe692a096825b0b5
ms.sourcegitcommit: 8e76be591034b618f5c11f4e66668f48c090ddfd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66357050"
---
# <a name="building-secure-workstations"></a>Creación de estaciones de trabajo seguras

Las estaciones de trabajo aislados protegidos son cruciales para la seguridad de funciones confidenciales, como administradores, desarrolladores y operadores de los servicios críticos. Muchos otros controles de seguridad y las garantías se producirá un error o no tienen ningún efecto si se ha puesto en peligro la seguridad de estación de trabajo cliente subyacente.

Este documento explica lo que se necesita para crear una estación de trabajo de cliente segura con las instrucciones paso a paso detalladas, incluido cómo configurar a partir de los controles de seguridad. Este tipo de estaciones de trabajo a veces se denomina una estación de trabajo de acceso con privilegios (PAW), que esta referencia se usa y se basa en. Sin embargo, las instrucciones se parece a la tecnología basada en la nube para administrar el servicio y presentan las funciones de seguridad introducidas a partir de Windows 10RS5, Microsoft Defender ATP, Azure Active Directory y Intune.

## <a name="why-securing-workstation-access-is-important"></a>¿Por qué es importante proteger el acceso de la estación de trabajo

La rápida adopción de servicios en la nube y la capacidad de trabajar desde cualquier lugar ha creado un nuevo método de explotación. Los atacantes aprovechen de controles de seguridad débil en los dispositivos donde los administradores trabajar y pueden tener acceso a recursos con privilegios.

Como se documenta en el [informe frente a amenazas Verizon](https://enterprise.verizon.com/resources/reports/dbir/), y [Security Intelligence Report](https://aka.ms/sir) hacer un mal uso de privilegios y ataques de cadena de suministro se encuentran entre los mecanismos de cinco principales utilizados para infringir las organizaciones y la en segundo lugar normalmente detectado táctica en incidentes informados en 2018.

La mayoría de los atacantes siguen la ruta de acceso siguiente:

* Comience con reconocimiento, a menudo específico de un sector, para encontrar una manera de
* Analizar la información recopilada para identificar la mejor manera de obtener acceso (infiltración) de una estación de trabajo percibido valor bajo
* Persistencia y mire significa mover [lateralmente](https://en.wikipedia.org/wiki/Network_Lateral_Movement)
* Extraer datos de confidencial

Con frecuencia, los atacantes infiltrarse en dispositivos que parecen bajo riesgo o infravalorados de reconocimiento. Estos dispositivos vulnerables, a continuación, se usan para localizar la oportunidad de desplazamiento lateral, encontrar los usuarios administrativos y los dispositivos e identificar alta datos importantes, a la información de extraer correctamente una vez que obtienen estos roles de usuario con privilegios.

![Patrón típico de poner en peligro](./media/concept-azure-managed-workstation/typical-timeline.png)

Este documento proporciona una solución para ayudar a proteger los dispositivos informáticos aislando la administración y servicios para ayudar a proteger contra el movimiento lateral o ataques desde dispositivos de productividad menos valiosos. El diseño ayuda a reducir la capacidad de ejecutar correctamente una infracción dividiendo la cadena antes de la infiltración del dispositivo usado para administrar o tener acceso a recursos confidenciales en la nube. La solución descrita utilizará los servicios de Azure nativos que forman parte de la pila de Microsoft 365 Enterprise, incluidos:

* Intune para la administración de dispositivos, incluidas aplicaciones así como listas blancas de dirección URL
* AutoPilot para la configuración del dispositivo y la implementación y actualización 
* Azure AD para la administración de usuarios, el acceso condicional y Multi-factor authentication
* Windows 10 (versión actual) para la experiencia del usuario y la atestación de estado dispositivo
* Microsoft Defender amenaza protección avanzada (ATP) de endpoint protection, detección y respuesta con la administración en la nube
* PIM de Azure AD para administrar la autorización, incluidos Just In Time (JIT) con privilegios de acceso a los recursos

## <a name="who-benefit-from-using-a-secure-workstation"></a>Quién beneficiarse del uso de una estación de trabajo segura

Todos los usuarios y los operadores beneficiarán del uso de una estación de trabajo segura. Un atacante que pone en peligro un equipo o dispositivo puede hacer, entre otras cosas suplantan todas las cuentas en caché y usar credenciales y tokens que se usan en ese dispositivo mientras hay una sesión iniciada. Este riesgo permite proteger los dispositivos usados para cualquier rol con privilegios, incluidos derechos administrativos tan importantes como los dispositivos donde se usa una cuenta con privilegios son objetivos de desplazamiento lateral y los ataques de elevación de privilegios. Estas cuentas pueden utilizarse para una variedad de recursos, como:

* Administradores de un entorno local y sistemas basados en la nube
* Estaciones de trabajo de desarrollador para sistemas críticos
* Administrador de cuentas de medios sociales con alta exposición
* Estaciones de trabajo altamente confidenciales, como los terminales de pago SWIFT
* Administrar secretos de estaciones de trabajo

Microsoft recomienda implementar controles de seguridad con privilegios elevados para estaciones de trabajo con privilegios que estas cuentas se utilizan para reducir el riesgo. Pueden encontrar instrucciones adicionales en el [Guía de implementación de características de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-deployment-checklist-p2), [guía básica de Office 365](https://aka.ms/o365secroadmap), y [guía básica de la protección del acceso con privilegios](https://aka.ms/sparoadmap)).

## <a name="why-dedicated-workstations"></a>¿Por qué dedicado a las estaciones de trabajo

Aunque es posible agregar seguridad a un dispositivo existente, es mejor empezar con una plataforma segura. A partir de un dispositivo que funciona bien y un conjunto de puts controles de seguridad conocidas su organización la mejor posición para mantener aumenta el nivel de seguridad. Con el creciente número de vectores de ataque permitido por correo electrónico ocasional y exploración web, resulta cada vez más difícil para asegurarse de que se puede confiar en un dispositivo. Esto funciona de la guía con la asunción de una estación de trabajo dedicada separado del estándar de productividad, exploración, y se completan las tareas de correo electrónico. Eliminación de productividad, exploración web y correo electrónico desde un dispositivo puede tener un impacto negativo en la productividad, pero esta protección es normalmente aceptable en escenarios donde las tareas del trabajo no requieren explícitamente y riesgo de un incidente de seguridad es alto.

> [!NOTE]
> Exploración de Web aquí hace referencia al acceso general a los sitios Web arbitrarios, que es un riesgo alto diferente de mediante un explorador web para tener acceso a un pequeño número de sitios Web de administración conocidos para servicios como Azure, Office 365, otros proveedores en la nube y SaaS aplicaciones.

Estrategias de contención proporcionan garantías de seguridad mayor aumentando el número y tipo de controles que un adversario debe superar para tener acceso a recursos sensibles. El modelo desarrollado aquí proporciona contención de privilegios administrativos a dispositivos concretos con un modelo de privilegio en niveles.

## <a name="supply-chain-management"></a>Administración de la cadena de suministro

Esencial para una estación de trabajo protegida es una solución de cadena de suministro que es de confianza, la estación de trabajo que utilice una raíz de confianza. Esta solución abordará la raíz de confianza utilizando la [Microsoft Autopilot](https://docs.microsoft.com/windows/deployment/windows-autopilot/windows-autopilot) tecnología. Para una estación de trabajo protegida Autopilot Microsoft proporciona la capacidad de aprovechar los dispositivos de Microsoft con optimización para OEM de Windows 10 que proporcionan un estado válido conocido del fabricante. Restableciendo imagen inicial de un dispositivo que es posible que no sea de confianza, en lugar de Microsoft Autopilot puede transformar un dispositivo de Windows en un estado "listo para el negocio", aplicando la configuración y las directivas, instalar aplicaciones, y aunque se cambie la edición de Windows 10 que se va a usar (por ejemplo, desde Windows 10 Pro a Windows 10 Enterprise, para admitir características avanzadas).

![Niveles de la estación de trabajo segura](./media/concept-azure-managed-workstation/supplychain.png)

## <a name="device-roles-and-profiles"></a>Funciones de dispositivo y perfiles

A lo largo de la guía, se resolverá varios perfiles de seguridad y roles para lograr una solución más segura para los usuarios, desarrolladores y personal de operaciones de TI. Estos perfiles se han alineado para admitir usuarios comunes en las organizaciones que pueden beneficiarse de una estación de trabajo mejorada o segura, al tiempo que equilibra la facilidad de uso y el riesgo. Las instrucciones proporcionará la configuración de valores según los estándares del sector aceptado. Esta guía sirve para ilustrar un método de protección de Windows 10 y reducir los riesgos asociados con el riesgo de dispositivo o usuario mediante una tecnología y directivas para ayudar a administrar los riesgos y las características de seguridad.
![Niveles de la estación de trabajo segura](./media/concept-azure-managed-workstation/seccon-levels.png)

* **Baja seguridad** : una estación de trabajo estándar administrado proporciona un buen punto de partida para su uso de más pequeñas y domésticas. Estos dispositivos están registrados en Azure AD y administrados por Intune. El perfil permite a los usuarios ejecutar cualquier aplicación y busque cualquier sitio Web. Al igual que una solución antimalware [Microsoft Defender](https://www.microsoft.com/windows/comprehensive-security) debe estar habilitada.
* **Seguridad mejorada** : es una solución de nivel de entrada protegido, bien para usuarios domésticos, usuarios de pequeñas empresas, así como los desarrolladores generales.
   * La estación de trabajo mejorado proporciona una directiva a partir de medios mejorar la seguridad del perfil de seguridad baja. Este perfil permite que un medio trabajar con los datos del cliente y podrá usar las herramientas de productividad como la comprobación de correo electrónico y exploración web seguro. Puede usarse una estación de trabajo mejorado para auditar el comportamiento del usuario y el uso de perfil de una estación de trabajo mediante la habilitación de directivas de auditoría y registro a Intune. En este perfil, la estación de trabajo habilitan los controles de seguridad y las directivas describen en el contenido e implementan en la estación de trabajo mejorado - Windows10 script (1809). La implementación también aprovecha las ventajas del uso de protección de malware avanzado [protección contra amenazas avanzada (ATP)](https://docs.microsoft.com/office365/securitycompliance/office-365-atp)
* **Alta seguridad** : es el medio más eficaz para reducir la superficie de ataque de una estación de trabajo quitar la capacidad de autoadministrar la estación de trabajo. Quitar derechos administrativos es un paso que mejora la seguridad y puede afectar a la productividad si implementa incorrectamente. El perfil de alta seguridad se basa en el perfil de seguridad mejorada con un cambio considerable, la eliminación del administrador local. Este perfil está diseñado para ayudar a los usuarios que quizás un usuario de alto perfil como un ejecutivo o usuarios que pueden tener que ponerse en contacto con los datos confidenciales, como nóminas o aprobación de servicios y procesos.
   * El perfil de usuario de alta seguridad exige un entorno controlado superior al tiempo que es capaz de realizar su actividad de productividad, como correo electrónico y manteniendo una fácil de usar la experiencia de exploración web. Los usuarios esperan características como las cookies, favoritos y otros métodos abreviados para operar. Sin embargo, estos usuarios no necesite la capacidad de modificar o depurar su dispositivo y no tendrá que instalar a los controladores. El perfil de seguridad alta se implementa mediante la seguridad alta - script Windows10 (1809).
* **Especializado** : los desarrolladores y los administradores de TI son un objetivo muy atractivo para los atacantes como estos roles pueden modificar los sistemas de interés para los atacantes. La estación de trabajo especializada toma el esfuerzo que se implementan en la estación de trabajo de alta seguridad y emphases más riesgo su seguridad mediante la administración de aplicaciones locales, limitar los sitios web de internet y restricción de las capacidades de productividad que son altos, como ActiveX, Java, complemento de explorador y otros controles conocidos de alto riesgo en un dispositivo de Windows. En este perfil, la estación de trabajo habilitan los controles de seguridad y las directivas describen en el contenido e implementan en el DeviceConfiguration_NCSC - Windows10 (1803) SecurityBaseline script.
* **Protege** : si un atacante puede poner en peligro una cuenta administrativa normalmente puede causar daños de negocio importantes mediante la interrupción del servicio, la modificación de datos o robo de datos. En este estado protegido, se habilitará la estación de trabajo todos los controles de seguridad y directivas que restrinjan el control directo de administración de aplicaciones locales y se quitan las herramientas de productividad. Como resultado, poner en peligro el dispositivo se realiza más difícil como correo y medios sociales están bloqueados que reflejan la manera más común que se pueden realizar correctamente los ataques de suplantación de identidad.  La estación de trabajo protegida puede implementarse con la estación de trabajo seguro - script SecurityBaseline Windows10 (1809).

   ![Estación de trabajo protegida](./media/concept-azure-managed-workstation/secure-workstation.png)

   Una estación de trabajo seguro proporciona a los administradores una estación de trabajo protegida que tiene el control de la aplicación no cifrado y protección de aplicaciones. La estación de trabajo usará credenciales, el dispositivo y exploit guard para proteger al host de comportamientos malintencionados. Locales además todos los discos se cifran con el cifrado de Bitlocker.

* **Aislado** : este escenario sin conexión personalizada representa el final de extremo del espectro (no hay scripts de instalación se proporcionan en este caso). Es posible que las organizaciones necesitan administrar una función crítica aislados de la empresa como una línea de producción de gran valor o sistemas de soporte técnico de vida que requiere sin/revisión no admite los sistemas operativos antiguos. Dado que la seguridad es importante y servicios en la nube no están disponibles, las organizaciones pueden manualmente administrar o actualizar estos equipos o usar una arquitectura aislada de bosque de Active Directory (como el mejorado seguridad Administrador de entorno (ESAE)) para administrarlos. En estas circunstancias quitar todo el acceso excepto basic Intune y ATP se debe considerar la comprobación de mantenimiento.
   * [Requisito de las comunicaciones de red de Intune](https://docs.microsoft.com/intune/network-bandwidth-use)
   * [Requisito de las comunicaciones de red de ATP](https://docs.microsoft.com/azure-advanced-threat-protection/configure-proxy)

## <a name="next-steps"></a>Pasos siguientes

[Implementación de una estación de trabajo segura administrada por Azure](howto-azure-managed-workstation.md)
