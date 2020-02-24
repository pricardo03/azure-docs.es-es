---
title: Detección avanzada de ataques de varias fases en Azure Sentinel
description: Use la tecnología de fusión de Azure Sentinel para reducir el exceso de alertas y crear incidentes útiles basados en la detección avanzada de ataques de varias fases.
services: sentinel
documentationcenter: na
author: rkarlin
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/12/2020
ms.author: rkarlin
ms.openlocfilehash: ada2ad67bc3634d8e6a31d3c8a69fc0c8b08a93a
ms.sourcegitcommit: f255f869c1dc451fd71e0cab340af629a1b5fb6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/16/2020
ms.locfileid: "77369693"
---
# <a name="advanced-multistage-attack-detection-in-azure-sentinel"></a>Detección avanzada de ataques de varias fases en Azure Sentinel

Mediante el uso de la tecnología de fusión basada en el aprendizaje automático, Azure Sentinel puede detectar automáticamente ataques de varias fases mediante la combinación de los comportamientos anómalos y las actividades sospechosas que se observan en varias fases de la cadena de eliminación. De esta forma, Azure Sentinel genera incidentes que, de otro modo, serían muy difíciles de detectar. Estos incidentes incluyen dos o más alertas o actividades. Por diseño, estos incidentes tienen poco volumen, alta fidelidad y alta gravedad.

Cuando se personaliza para adaptarla a su entorno, esta detección no solo reduce las tasas de falsos positivos, sino que también puede detectar ataques con información limitada o que falta.

## <a name="configuration-for-advanced-multistage-attack-detection"></a>Configuración de la detección avanzada de ataques de varias fases

Esta detección está habilitada de forma predeterminada en Azure Sentinel. Para comprobar el estado, o para deshabilitar la detección quizás porque está usando una solución alternativa para crear incidentes basados en varias alertas, siga estas instrucciones:

1. Si aún no lo ha hecho, inicie sesión en el [Azure Portal](https://portal.azure.com).

2. Vaya a **Azure Sentinel** > **Configuración** > **Análisis**.

3. Seleccione **Reglas activas** y busque **Detección avanzada de ataques de varias fases** en la columna **NOMBRE**. Compruebe la columna **ESTADO** para confirmar si esta detección está habilitada o deshabilitada.

4. Para cambiar el estado, seleccione esta entrada y, en la hoja **Detección avanzada de ataques de varias fases**, seleccione **Editar**.

5. En la hoja **Asistente para creación de reglas**, se selecciona automáticamente el cambio de estado, así que seleccione **Siguiente: revisar** y, a continuación, **Guardar**. 

Las plantillas de reglas no son aplicables para la detección avanzada de ataques de varias fases.

## <a name="fusion-using-palo-alto-networks-and-microsoft-defender-atp"></a>Fusión mediante Palo Alto Networks y ATP de Microsoft Defender

- Solicitud de red al servicio de anonimización TOR, seguido por el tráfico anómalo marcado por el firewall de Palo Alto Networks

- PowerShell estableció una conexión de red sospechosa, seguida por tráfico anómalo marcado por el firewall de Palo Alto Networks

- Una conexión saliente a IP con un historial de intentos de acceso no autorizados, seguida por tráfico anómalo marcado por el firewall de Palo Alto Networks



## <a name="fusion-using-identity-protection-and-microsoft-cloud-app-security"></a>Fusión mediante Identity Protection y Microsoft Cloud App Security

En la detección avanzada de ataques de varias fases, Azure Sentinel admite los siguientes escenarios que combinan eventos anómalos de Azure Active Directory Identity Protection y Microsoft Cloud App Security:

- [Viaje imposible a una ubicación inusual seguido de una actividad anómala de Office 365](#impossible-travel-to-atypical-location-followed-by-anomalous-office-365-activity)
- [Actividad de inicio de sesión para una ubicación desconocida seguida de una actividad anómala de Office 365](#sign-in-activity-for-unfamiliar-location-followed-by-anomalous-office-365-activity)
- [Actividad de inicio de sesión desde un dispositivo infectado seguida de una actividad anómala de Office 365](#sign-in-activity-from-infected-device-followed-by-anomalous-office-365-activity)
- [Actividad de inicio de sesión desde una dirección IP anónima seguida de una actividad anómala de Office 365](#sign-in-activity-from-anonymous-ip-address-followed-by-anomalous-office-365-activity)
- [Actividad de inicio de sesión de un usuario con credenciales filtradas seguida de una actividad anómala de Office 365](#sign-in-activity-from-user-with-leaked-credentials-followed-by-anomalous-office-365-activity)

Debe tener el [conector de datos de Azure AD Identity Protection](connect-azure-ad-identity-protection.md) y los conectores de [Cloud App Security](connect-cloud-app-security.md) configurados.

En las descripciones siguientes, Azure Sentinel mostrará el valor real de los datos que se representan en esta página como variables entre corchetes. Por ejemplo, el nombre para mostrar real de una cuenta en lugar de \<*nombreDeCuenta*> y el número real en lugar de \<*número*>.

### <a name="impossible-travel-to-atypical-location-followed-by-anomalous-office-365-activity"></a>Viaje imposible a una ubicación inusual seguido de una actividad anómala de Office 365

Hay siete posibles incidentes de Azure Sentinel que combinan alertas de viaje imposible a ubicaciones inusuales de Azure AD Identity Protection y alertas de actividades anómalas de Office 365 que genera Microsoft Cloud App Security:

- **Viaje imposible a ubicaciones inusuales que conducen a la filtración de los buzones de Office 365**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*account name*> desde un viaje imposible a \<*location*>, una ubicación inusual, seguido de una regla de reenvío sospechosa de la bandeja de entrada que se estableció en una bandeja de entrada del usuario.
    
    Esto puede indicar que la cuenta está en peligro y que el buzón se usa para la filtración de información de su organización. El usuario \<*nombreDeCuenta*> ha creado o actualizado una regla de reenvío de bandeja de entrada que reenvía el correo electrónico entrante a la dirección externa \<*direcciónCorreoElectrónico*>.

- **Viaje imposible a ubicaciones inusuales que conducen a una actividad de administración de aplicaciones en la nube sospechosa**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*account name*> desde un viaje imposible a \<*location*>, una ubicación inusual.
    
    A continuación, la cuenta \<*nombreDeCuenta*> realizó \<*número*> actividades de administración en una única salida.

- **Viaje imposible a ubicaciones inusuales que conducen a una eliminación de archivos masiva**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*nombreDeCuenta*> en \<*ubicación*>, una ubicación inusual. 
    
    A continuación, la cuenta \<*nombreDeCuenta*> eliminó \<*númeroDe*> archivos únicos en una sola sesión.

- **Viaje imposible a ubicaciones inusuales que conducen a una descarga de archivos masiva**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*account name*> desde un viaje imposible a \<*location*>, una ubicación inusual. 
    
    A continuación, la cuenta \<*nombreDeCuenta*> descargó \<*númeroDe*> archivos únicos en una sola sesión.

- **Viaje imposible a ubicaciones inusuales que conducen a la suplantación de Office 365**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*account name*> desde un viaje imposible a \<*location*>, una ubicación inusual. 
    
    A continuación, la cuenta \<*nombreDeCuenta*> realizó una cantidad inusual (\<*númeroDeActividades*>) de actividades de suplantación en una sola sesión.

- **Viaje imposible a ubicaciones inusuales que conducen a un uso compartido de archivos masivo**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*account name*> desde un viaje imposible a \<*location*>, una ubicación inusual. 
    
    A continuación, la cuenta \<*nombreDeCuenta*> compartió \<*númeroDe*> archivos únicos en una sola sesión.

- **Viaje imposible a ubicaciones inusuales que conducen a ransomware en las aplicaciones en la nube**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*account name*> desde un viaje imposible a \<*location*>, una ubicación inusual. 
    
    A continuación, la cuenta \<*nombreDeCuenta*> descargó \<*númeroDe*> archivos y eliminó un total de \<*númeroDe*> archivos. 
    
    Este patrón de actividad es indicativo de un posible ataque por ransomware.


### <a name="sign-in-activity-for-unfamiliar-location-followed-by-anomalous-office-365-activity"></a>Actividad de inicio de sesión para una ubicación desconocida seguida de una actividad anómala de Office 365

Hay siete posibles incidentes de Azure Sentinel que combinan alertas de actividades de inicio de sesión para ubicaciones desconocidas de Azure AD Identity Protection y alertas de actividades anómalas de Office 365 que genera Microsoft Cloud App Security.

- **Evento de inicio de sesión desde una ubicación desconocida que conduce a la filtración de buzones de Exchange Online**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*nombreDeCuenta*> desde \<*ubicación*>, una ubicación desconocida, seguido de una regla de reenvío sospechosa de la bandeja de entrada que se estableció en una bandeja de entrada del usuario.
    
    Esto puede indicar que la cuenta está en peligro y que el buzón se usa para la filtración de información de su organización. El usuario \<*nombreDeCuenta*> ha creado o actualizado una regla de reenvío de bandeja de entrada que reenvía el correo electrónico entrante a la dirección externa \<*direcciónCorreoElectrónico*>. 

- **Evento de inicio de sesión desde una ubicación desconocida que conduce a una actividad de administración de aplicaciones en la nube sospechosa**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*nombreDeCuenta*> desde \<*ubicación*>, una ubicación desconocida. 
    
    A continuación, la cuenta \<*nombreDeCuenta*> realizó \<*númeroDe*> actividades de administración en una única salida.

- **Evento de inicio de sesión desde una ubicación desconocida que conduce a una eliminación de archivos masiva**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*nombreDeCuenta*> desde \<*ubicación*>, una ubicación desconocida. 
    
    A continuación, la cuenta \<*nombreDeCuenta*> eliminó \<*númeroDe*> archivos únicos en una sola sesión.

- **Evento de inicio de sesión desde una ubicación desconocida que conduce a una descarga de archivos masiva**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*nombreDeCuenta*> desde \<*ubicación*>, una ubicación desconocida. 
    
    A continuación, la cuenta \<*nombreDeCuenta*> descargó \<*númeroDe*> archivos únicos en una sola sesión.

- **Evento de inicio de sesión desde una ubicación desconocida que conduce a una suplantación de Office 365**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*nombreDeCuenta*> desde \<*ubicación*>, una ubicación desconocida.
    
    A continuación, la cuenta \<*nombreDeCuenta*> suplantó \<*númeroDe*> cuentas diferentes en una sola sesión.

- **Evento de inicio de sesión desde una ubicación desconocida que conduce a un uso compartido de archivos masivo**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*nombreDeCuenta*> desde \<*ubicación*>, una ubicación desconocida. 
    
    A continuación, la cuenta \<*nombreDeCuenta*> compartió \<*númeroDe*> archivos únicos en una sola sesión.

- **Evento de inicio de sesión desde una ubicación desconocida que conducen a ransomware en las aplicaciones en la nube**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*nombreDeCuenta*> desde \<*ubicación*>, una ubicación desconocida. 
    
    A continuación, la cuenta \<*nombreDeCuenta*> descargó \<*númeroDe*> archivos y eliminó un total de \<*númeroDe*> archivos. 
    
    Este patrón de actividad es indicativo de un posible ataque por ransomware.

### <a name="sign-in-activity-from-infected-device-followed-by-anomalous-office-365-activity"></a>Actividad de inicio de sesión desde un dispositivo infectado seguida de una actividad anómala de Office 365

Hay siete posibles incidentes de Azure Sentinel que combinan alertas de actividades de inicio de sesión desde dispositivos infectados de Azure AD Identity Protection y alertas de actividades anómalas de Office 365 que genera Microsoft Cloud App Security:

- **Evento de inicio de sesión desde un dispositivo infectado que conduce a la filtración de buzones de Office 365**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*account name*> desde un dispositivo posiblemente infectado con malware, seguido de una regla de reenvío sospechosa de la bandeja de entrada que se estableció en una bandeja de entrada del usuario.
    
    Esto puede indicar que la cuenta está en peligro y que el buzón se usa para la filtración de información de su organización. El usuario \<*nombreDeCuenta*> ha creado o actualizado una regla de reenvío de bandeja de entrada que reenvía el correo electrónico entrante a la dirección externa \<*direcciónCorreoElectrónico*>. 

- **Evento de inicio de sesión desde un dispositivo infectado que conduce a una actividad de administración de aplicaciones en la nube sospechosa**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*nombreDeCuenta*> desde un dispositivo posiblemente infectado con malware.
    
    A continuación, la cuenta \<*nombreDeCuenta*> realizó \<*númeroDe*> actividades de administración en una única salida.

- **Evento de inicio de sesión desde un dispositivo infectado que conduce a una eliminación de archivos masiva**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*nombreDeCuenta*> desde un dispositivo posiblemente infectado con malware. 
    
    A continuación, la cuenta \<*nombreDeCuenta*> eliminó \<*númeroDe*> archivos únicos en una sola sesión.

- **Evento de inicio de sesión desde un dispositivo infectado que conduce a una descarga de archivos masiva**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*nombreDeCuenta*> desde un dispositivo posiblemente infectado con malware. 
    
    A continuación, la cuenta \<*nombreDeCuenta*> descargó \<*númeroDe*> archivos únicos en una sola sesión.

- **Evento de inicio de sesión desde un dispositivo infectado que conduce a una suplantación de Office 365**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*nombreDeCuenta*> desde un dispositivo posiblemente infectado con malware. 
    
    A continuación, la cuenta \<*nombreDeCuenta*> suplantó \<*númeroDe*> cuentas diferentes en una sola sesión.

- **Evento de inicio de sesión desde un dispositivo infectado que conduce a un uso compartido de archivos masivo**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*nombreDeCuenta*> desde un dispositivo posiblemente infectado con malware. 
    
    A continuación, la cuenta \<*nombreDeCuenta*> compartió \<*númeroDe*> archivos únicos en una sola sesión.

- **Evento de inicio de sesión desde un dispositivo infectado que conduce a ransomware en las aplicaciones en la nube**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*nombreDeCuenta*> desde un dispositivo posiblemente infectado con malware. 
    
    A continuación, la cuenta \<*nombreDeCuenta*> descargó \<*númeroDe*> archivos y eliminó un total de \<*númeroDe*> archivos. 
    
    Este patrón de actividad es indicativo de un posible ataque por ransomware.

### <a name="sign-in-activity-from-anonymous-ip-address-followed-by-anomalous-office-365-activity"></a>Actividad de inicio de sesión desde una dirección IP anónima seguida de una actividad anómala de Office 365

Hay siete posibles incidentes de Azure Sentinel que combinan alertas de actividades de inicio de sesión desde una dirección IP anónima de Azure AD Identity Protection y alertas de actividades anómalas de Office 365 que genera Microsoft Cloud App Security:

- **Evento de inicio de sesión desde una dirección IP anónima que conduce a la filtración de buzones de Office 365**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*nombreDeCuenta*> desde una dirección IP de proxy anónima \<*direcciónIP*>, seguido de una regla de reenvío sospechosa de la bandeja de entrada que se estableció en una bandeja de entrada del usuario.
    
    Esto puede indicar que la cuenta está en peligro y que el buzón se usa para la filtración de información de su organización. El usuario \<*nombreDeCuenta*> ha creado o actualizado una regla de reenvío de bandeja de entrada que reenvía el correo electrónico entrante a la dirección externa \<*direcciónCorreoElectrónico*>. 

- **Evento de inicio de sesión desde una dirección IP anónima que conduce a una actividad de administración de aplicaciones en la nube sospechosa**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*nombreDeCuenta*> desde una dirección IP de proxy anónima \<*direcciónIP*>. 
    
    A continuación, la cuenta \<*nombreDeCuenta*> realizó \<*númeroDe*> actividades de administración en una única salida.

- **Evento de inicio de sesión desde una dirección IP anónima que conduce a una eliminación de archivos masiva**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*nombreDeCuenta*> desde una dirección IP de proxy anónima \<*direcciónIP*>. 
    
    A continuación, la cuenta \<*nombreDeCuenta*> eliminó \<*númeroDe*> archivos únicos en una sola sesión.

- **Evento de inicio de sesión desde una dirección IP anónima que conduce a una descarga de archivos masiva**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*nombreDeCuenta*> desde una dirección IP de proxy anónima \<*direcciónIP*>. 
    
    A continuación, la cuenta \<*nombreDeCuenta*> descargó \<*númeroDe*> archivos únicos en una sola sesión.

- **Evento de inicio de sesión desde una dirección IP anónima que conduce a una suplantación de Office 365**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*nombreDeCuenta*> desde una dirección IP de proxy anónima \<*direcciónIP*>. 
    
    A continuación, la cuenta \<*nombreDeCuenta*> suplantó \<*númeroDe*> cuentas diferentes en una sola sesión.

- **Evento de inicio de sesión desde una dirección IP anónima que conduce a un uso compartido de archivos masivo**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*nombreDeCuenta*> desde una dirección IP de proxy anónima \<*direcciónIP*>. 
    
    A continuación, la cuenta \<*nombreDeCuenta*> compartió \<*númeroDe*> archivos únicos en una sola sesión.

- **Evento de inicio de sesión desde una dirección IP anónima que conduce a ransomware en las aplicaciones en la nube**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*nombreDeCuenta*> desde una dirección IP de proxy anónima \<*direcciónIP*>. 
    
    A continuación, la cuenta \<*nombreDeCuenta*> descargó \<*númeroDe*> archivos y eliminó un total de \<*númeroDe*> archivos. 
    
    Este patrón de actividad es indicativo de un posible ataque por ransomware.

### <a name="sign-in-activity-from-user-with-leaked-credentials-followed-by-anomalous-office-365-activity"></a>Actividad de inicio de sesión de un usuario con credenciales filtradas seguida de una actividad anómala de Office 365

Hay siete posibles incidentes de Azure Sentinel que combinan alertas de actividades de inicio de sesión de un usuario con credenciales filtradas de Azure AD Identity Protection y alertas de actividades anómalas de Office 365 que genera Microsoft Cloud App Security:

- **Evento de inicio de sesión desde un usuario con credenciales filtradas que conduce a la filtración de buzones de Office 365**
    
    Esta alerta constituye un indicador de un evento de inicio de sesión de \<*nombreDeCuenta*> desde un usuario con credenciales filtradas, seguido de una regla de reenvío sospechosa de la bandeja de entrada que se estableció en una bandeja de entrada del usuario. 
    
    Esto puede indicar que la cuenta está en peligro y que el buzón se usa para la filtración de información de su organización. El usuario \<*nombreDeCuenta*> ha creado o actualizado una regla de reenvío de bandeja de entrada que reenvía el correo electrónico entrante a la dirección externa \<*direcciónCorreoElectrónico*>. 

- **Evento de inicio de sesión desde un usuario con credenciales filtradas que conduce a una actividad de administración de aplicaciones en la nube sospechosa**
    
    Esta alerta constituye un indicador de que el evento de inicio de sesión de \<*nombreDeCuenta*> utilizó credenciales filtradas.
    
    A continuación, la cuenta \<*nombreDeCuenta*> realizó \<*númeroDe*> actividades de administración en una única salida.

- **Evento de inicio de sesión desde un usuario con credenciales filtradas que conduce a una eliminación de archivos masiva**
    
    Esta alerta constituye un indicador de que el evento de inicio de sesión de \<*nombreDeCuenta*> utilizó credenciales filtradas.
    
    A continuación, la cuenta \<*nombreDeCuenta*> eliminó \<*númeroDe*> archivos únicos en una sola sesión.

- **Evento de inicio de sesión desde un usuario con credenciales filtradas que conduce a una descarga de archivos masiva**
    
    Esta alerta constituye un indicador de que el evento de inicio de sesión de \<*nombreDeCuenta*> utilizó credenciales filtradas.
    
    A continuación, la cuenta \<*nombreDeCuenta*> descargó \<*númeroDe*> archivos únicos en una sola sesión.

- **Evento de inicio de sesión desde un usuario con credenciales filtradas que conduce a la suplantación de Office 365**
    
    Esta alerta constituye un indicador de que el evento de inicio de sesión de \<*nombreDeCuenta*> utilizó credenciales filtradas. 
    
    A continuación, la cuenta \<*nombreDeCuenta*> suplantó \<*númeroDe*> cuentas diferentes en una sola sesión.

- **Evento de inicio de sesión desde un usuario con credenciales filtradas que conduce a un uso compartido de archivos masivo**
    
    Esta alerta constituye un indicador de que el evento de inicio de sesión de \<*nombreDeCuenta*> utilizó credenciales filtradas.
    
    A continuación, la cuenta \<*nombreDeCuenta*> compartió \<*númeroDe*> archivos únicos en una sola sesión.

- **Evento de inicio de sesión desde un usuario con credenciales filtradas que conduce a ransomware en las aplicaciones en la nube**
    
    Esta alerta constituye un indicador de que el evento de inicio de sesión de \<*nombreDeCuenta*> utilizó credenciales filtradas. 
    
    A continuación, la cuenta \<*nombreDeCuenta*> descargó \<*númeroDe*> archivos y eliminó un total de \<*númeroDe*> archivos. 
    
    Este patrón de actividad es indicativo de un posible ataque por ransomware.

## <a name="next-steps"></a>Pasos siguientes

Ahora que conoce más detalles sobre la detección avanzada de ataques de varias fases, puede que le interese el siguiente inicio rápido para aprender a obtener visibilidad sobre sus datos y a detectar posibles amenazas: [Introducción a Azure Sentinel](quickstart-get-visibility.md).

Si está preparado para investigar los incidentes que se han creado, consulte el siguiente tutorial: [Investigación de incidentes con Azure Sentinel](tutorial-investigate-cases.md).

