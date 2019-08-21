---
title: No se puede iniciar sesión en Zeppelin en Azure HDInsight
description: No se puede iniciar sesión en Zeppelin en Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 08/12/2019
ms.openlocfilehash: 14f4cd7588405dbf351124b1e469fd96f9b38274
ms.sourcegitcommit: 62bd5acd62418518d5991b73a16dca61d7430634
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2019
ms.locfileid: "68976714"
---
# <a name="scenario-unable-to-sign-in-to-apache-zeppelin-in-azure-hdinsight"></a>Escenario: No se puede iniciar sesión en Apache Zeppelin en Azure HDInsight.

En este artículo se describen los pasos de solución de problemas y las posibles soluciones para los problemas que se producen al usar clústeres de Azure HDInsight.

## <a name="issue"></a>Problema

No se puede iniciar sesión en Apache Zeppelin después de cambiar la contraseña de ADDS en Active Directory.

## <a name="cause"></a>Causa

El usuario mencionado en `activeDirectoryRealm.systemUsername` del archivo `shiro_ini` ha cambiado la contraseña de Active Directory.

## <a name="resolution"></a>Resolución

1. Compruebe que la contraseña modificada es la causa principal mediante la inclusión de `activeDirectoryRealm.systemPassword = <new password>` en la configuración `shiro_ini` de Zeppelin en Ambari. Quite la configuración `activeDirectoryRealm.hadoopSecurityCredentialPath`. A continuación se muestra la ubicación de `shiro ini`.

    ![Shiro](./media/domain-joined-zeppelin-signin/shiro.png)

1. Si los usuarios ahora pueden iniciar sesión en Zeppelin después del paso 1, cree un nuevo archivo `jceks` con la nueva contraseña y reemplace `activeDirectoryRealm.hadoopSecurityCredentialPath` por el nuevo archivo.

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure mediante el [soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).

* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure, para mejorar la experiencia del cliente. Esta cuenta conecta a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.

* Si necesita más ayuda, puede enviar una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Seleccione **Soporte técnico** en la barra de menús o abra la central **Ayuda + soporte técnico**. Para información más detallada, revise [Creación de una solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). La suscripción a Microsoft Azure incluye acceso al soporte técnico para facturación y administración de suscripciones. El soporte técnico se proporciona a través de uno de los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).
