---
title: Protección contra amenazas avanzada de Windows Defender con Azure Security Center
description: Este documento presenta la integración entre Azure Security Center y la Protección contra amenazas avanzada de Windows Defender.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/27/2018
ms.author: v-mohabe
ms.openlocfilehash: 597670803514fa59faaafdad73786cdb745a0976
ms.sourcegitcommit: 36e9cbd767b3f12d3524fadc2b50b281458122dc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2019
ms.locfileid: "69640912"
---
# <a name="windows-defender-advanced-threat-protection-with-azure-security-center"></a>Protección contra amenazas avanzada de Windows Defender con Azure Security Center

Azure Security Center va a ampliar su oferta de plataformas de protección de cargas de trabajo en la nube mediante la integración con la [Protección contra amenazas avanzada de Windows Defender](https://www.microsoft.com/en-us/WindowsForBusiness/windows-atp) (ATP).
Este cambio ofrece funcionalidades completas de detección y respuesta (EDR) de puntos de conexión. Con la integración de ATP de Windows Defender, puede detectar anomalías. También puede detectar y responder a ataques avanzados en los puntos de conexión de servidor que Azure Security Center supervisa.

## <a name="windows-defender-atp-features-in-security-center"></a>Características de ATP de Windows Defender en Security Center

Cuando usa ATP de Windows Defender, obtiene lo siguiente:

- **Sensores de detección de brechas de próxima generación**: los sensores de ATP de Windows Defender para los servidores Windows recopilan una amplia variedad de señales de comportamiento.

- **Detección de brechas basadas en la nube y el análisis**: ATP de Windows Defender se adapta rápidamente a las cambiantes amenazas. Utiliza análisis avanzados y macrodatos. ATP de Windows Defender se amplía gracias a la eficacia de Intelligent Security Graph con señales a través de Windows Azure y Office para detectar amenazas desconocidas. Proporciona alertas que requieren intervención y le permite responder rápidamente.

- **Información sobre amenazas**: ATP de Windows Defender identifica las herramientas, las técnicas y los procedimientos de los atacantes. Cuando los detecta, genera alertas. Usa datos generados por los equipos de seguridad y los buscadores de amenazas de Microsoft, con la ayuda de mecanismos de inteligencia que aportan los partners.

Estas funcionalidades están disponibles ahora en Azure Security Center:

- **Incorporación automatizada**: el sensor de ATP de Windows Defender se habilita automáticamente para los servidores de Windows que se incorporan a Azure Security Center.

- **Panel único**: la consola de Azure Security Center muestra las alertas de ATP de Windows Defender.

- **Investigación detallada de las máquinas**: los clientes de Azure Security Center pueden acceder a la consola de ATP de Windows Defender para llevar a cabo una investigación detallada destinada a descubrir el ámbito de una brecha.

![Azure Security Center muestra una lista de alertas e información general sobre cada alerta](media/security-center-wdatp/image1.png)

Puede investigar más la alerta basándose en ATP de Windows Defender. Allí puede ver información adicional como el árbol de procesos de alerta y el grafo de incidentes. También puede ver una escala de tiempo de la máquina detallada que muestra cada comportamiento durante un período histórico de hasta seis meses.

![Página de ATP de Windows Defender con información detallada sobre una alerta](media/security-center-wdatp/image3.png)

## <a name="platform-support"></a>Compatibilidad con plataformas

ATP de Windows Defender en Security Center admite la detección en los sistemas operativos Windows Server 2016, 2012 R2 y 2008 R2 SP1 de una suscripción de servicio estándar.

> [!NOTE]
> Al usar Azure Security Center para supervisar los servidores, se crea automáticamente un inquilino de ATP de Windows Defender y los datos de este se almacenan en Europa de forma predeterminada. Si necesita mover los datos a otra ubicación, deberá ponerse en contacto con el Soporte técnico de Microsoft para restablecer el inquilino.

## <a name="onboarding-servers-to-security-center"></a>Incorporación de servidores a Security Center 

Para incorporar servidores a Security Center, haga clic en **Go to Azure Security Center to onboard servers** (Ir a Azure Security Center para incorporarlos servidores) desde la incorporación del servidor de Windows Defender ATP.

1. En la hoja **Incorporación**, seleccione o cree un área de trabajo en la que almacenar los datos. <br>
2. Si no puede ver todas las áreas de trabajo, puede deberse a una falta de permisos. Asegúrese de que el área de trabajo se establece en el nivel Estándar de Azure Security. Si quiere obtener más información, vea [Actualización al nivel Estándar de Security Center para mejorar la seguridad](security-center-pricing.md).
    
3. Seleccione **Agregar servidores** para ver instrucciones acerca de la instalación de Microsoft Monitoring Agent. 

4. Después de la incorporación, puede supervisar las máquinas en **Procesos y aplicaciones**.

   ![Incorporación de equipos](media/security-center-wdatp/onboard-computers.png)

## <a name="enable-windows-defender-atp-integration"></a>Habilitación de la integración con ATP de Windows Defender

Para comprobar si la integración de ATP de Windows Defender está habilitada, seleccione **Security Center** > **Pricing & settings** (Precios y configuración) > haga clic en su suscripción.

  ![Administración de directivas de Azure Security Center](media/security-center-wdatp/policy-management.png)

Aquí puede ver las integraciones habilitadas actualmente.

  ![Página de configuración de detección de amenazas de Azure Security Center con la integración de ATP de Windows Defender habilitada](media/security-center-wdatp/enable-integrations.png)

- Si ya ha incorporado los servidores al nivel estándar de Azure Security Center, no tiene que realizar ninguna otra acción. Azure Security Center incorporará automáticamente los servidores de ATP de Windows Defender. Este proceso puede tardar hasta 24 horas.

- Si nunca ha incorporado los servidores al nivel estándar de Azure Security Center, incorpórelos a Azure Security Center como de costumbre.

- Si ha incorporado los servidores mediante ATP de Windows Defender:
  - Consulte la documentación para obtener instrucciones sobre [cómo retirar las máquinas de servidor](https://go.microsoft.com/fwlink/p/?linkid=852906).
  - Incorpore estos servidores a Azure Security Center.

## <a name="access-to-the-windows-defender-atp-portal"></a>Acceso al portal de ATP de Windows Defender

Siga las instrucciones de [Asignar acceso de usuario al portal](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/assign-portal-access-windows-defender-advanced-threat-protection).

## <a name="set-the-firewall-configuration"></a>Establecimiento de la configuración del firewall

Si tiene un servidor proxy o firewall que bloquea el tráfico anónimo, como un sensor de ATP de Windows Defender que se conecta desde el contexto del sistema, asegúrese de que el tráfico anónimo se permite. Siga las instrucciones para [habilitar el acceso a direcciones URL del servicio de ATP de Windows Defender en el servidor proxy](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/configure-proxy-internet-windows-defender-advanced-threat-protection#enable-access-to-microsoft-defender-atp-service-urls-in-the-proxy-server).

## <a name="test-the-feature"></a>Prueba de la característica

Para generar una alerta de prueba inofensiva de ATP de Windows Defender:

1. Use Escritorio remoto para acceder a una máquina virtual de Windows Server 2012 R2 o a una máquina virtual de Windows Server 2016.  Abra una ventana de símbolo del sistema.

2. En el símbolo del sistema, copie el siguiente comando y ejecútelo. La ventana del símbolo del sistema se cerrará automáticamente.

    ```
    powershell.exe -NoExit -ExecutionPolicy Bypass -WindowStyle Hidden (New-Object System.Net.WebClient).DownloadFile('http://127.0.0.1/1.exe', 'C:\\test-WDATP-test\\invoice.exe'); Start-Process 'C:\\test-WDATP-test\\invoice.exe'
    ```

   ![Una ventana de símbolo del sistema con el comando anterior](media/security-center-wdatp/image4.jpeg)

3. Si el comando se ejecuta correctamente, verá una nueva alerta en el panel de Azure Security Center y el portal de ATP de Windows Defender. Esta alerta puede tardar unos minutos en aparecer.

4. Para revisar la alerta en Security Center, vaya a **Security Alerts (Alertas de seguridad)**  >  **Suspicious Powershell CommandLine (Línea de comandos de Powershell Sospechosa)** .

5. En la ventana de la investigación, seleccione el vínculo para ir al portal de ATP de Windows Defender.

## <a name="next-steps"></a>Pasos siguientes

- [Establecimiento de directivas de seguridad en Azure Security Center](tutorial-security-policy.md): aprenda a configurar directivas de seguridad para las suscripciones y los grupos de recursos de Azure.
- [Administración de recomendaciones de seguridad en el Centro de seguridad de Azure](security-center-recommendations.md): aprenda cómo las recomendaciones le ayudarán a proteger los recursos de Azure.
- [Supervisión del estado de seguridad en Azure Security Center](security-center-monitoring.md): Aprenda a supervisar el estado de los recursos de Azure.
