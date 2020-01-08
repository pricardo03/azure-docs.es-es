---
title: Implementación de DBMS de Azure Virtual Machines de SAP ASE para la carga de trabajo de SAP | Microsoft Docs
description: Implementación de DBMS de Azure Virtual Machines de SAP ASE para la carga de trabajo de SAP
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/1/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 78085924e0d4c77fef09814827231235c80d051b
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/03/2020
ms.locfileid: "75645827"
---
# <a name="sap-ase-azure-virtual-machines-dbms-deployment-for-sap-workload"></a>Implementación de DBMS de Azure Virtual Machines de SAP ASE para la carga de trabajo de SAP

[767598]:https://launchpad.support.sap.com/#/notes/767598
[773830]: https://launchpad.support.sap.com/#/notes/773830
[826037]:https://launchpad.support.sap.com/#/notes/826037
[965908]:https://launchpad.support.sap.com/#/notes/965908
[1031096]:https://launchpad.support.sap.com/#/notes/1031096
[1114181]:https://launchpad.support.sap.com/#/notes/1114181
[1139904]:https://launchpad.support.sap.com/#/notes/1139904
[1173395]:https://launchpad.support.sap.com/#/notes/1173395
[1245200]: https://launchpad.support.sap.com/#/notes/1245200
[1409604]:https://launchpad.support.sap.com/#/notes/1409604
[1558958]: https://launchpad.support.sap.com/#/notes/1558958
[1585981]: https://launchpad.support.sap.com/#/notes/1585981
[1588316]: https://launchpad.support.sap.com/#/notes/1588316
[1590719]: https://launchpad.support.sap.com/#/notes/1590719
[1597355]:https://launchpad.support.sap.com/#/notes/1597355
[1605680]: https://launchpad.support.sap.com/#/notes/1605680
[1619720]:https://launchpad.support.sap.com/#/notes/1619720
[1619726]:https://launchpad.support.sap.com/#/notes/1619726
[1619967]: https://launchpad.support.sap.com/#/notes/1619967
[1750510]: https://launchpad.support.sap.com/#/notes/1750510
[1752266]: https://launchpad.support.sap.com/#/notes/1752266
[1757924]: https://launchpad.support.sap.com/#/notes/1757924
[1757928]: https://launchpad.support.sap.com/#/notes/1757928
[1758182]: https://launchpad.support.sap.com/#/notes/1758182
[1758496]: https://launchpad.support.sap.com/#/notes/1758496
[1772688]:https://launchpad.support.sap.com/#/notes/1772688
[1814258]: https://launchpad.support.sap.com/#/notes/1814258
[1882376]: https://launchpad.support.sap.com/#/notes/1882376
[1909114]:https://launchpad.support.sap.com/#/notes/1909114
[1922555]: https://launchpad.support.sap.com/#/notes/1922555
[1928533]: https://launchpad.support.sap.com/#/notes/1928533
[1941500]: https://launchpad.support.sap.com/#/notes/1941500
[1956005]: https://launchpad.support.sap.com/#/notes/1956005
[1973241]: https://launchpad.support.sap.com/#/notes/1973241
[1984787]:https://launchpad.support.sap.com/#/notes/1984787
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2002167]:https://launchpad.support.sap.com/#/notes/2002167
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2039619]:https://launchpad.support.sap.com/#/notes/2039619
[2069760]:https://launchpad.support.sap.com/#/notes/2069760
[2121797]: https://launchpad.support.sap.com/#/notes/2121797
[2134316]: https://launchpad.support.sap.com/#/notes/2134316
[2171857]:https://launchpad.support.sap.com/#/notes/2171857
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2233094]:https://launchpad.support.sap.com/#/notes/2233094
[2243692]:https://launchpad.support.sap.com/#/notes/2243692

[azure-cli]:../../../cli-install-nodejs.md
[azure-portal]:https://portal.azure.com
[azure-ps]:/powershell/azureps-cmdlets-docs
[azure-quickstart-templates-github]:https://github.com/Azure/azure-quickstart-templates
[azure-script-ps]:https://go.microsoft.com/fwlink/p/?LinkID=395017
[azure-resource-manager/management/azure-subscription-service-limits]:../../../azure-resource-manager/management/azure-subscription-service-limits.md
[azure-resource-manager/management/azure-subscription-service-limits-subscription]:../../../azure-resource-manager/management/azure-subscription-service-limits.md#subscription-limits

[dbms-guide]:dbms-guide.md 
[dbms-guide-2.1]:dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f 
[dbms-guide-2.2]:dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91 
[dbms-guide-2.3]:dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152 
[dbms-guide-2]:dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64 
[dbms-guide-3]:dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3 
[dbms-guide-5.5.1]:dbms-guide.md#0fef0e79-d3fe-4ae2-85af-73666a6f7268 
[dbms-guide-5.5.2]:dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b 
[dbms-guide-5.6]:dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8 
[dbms-guide-5.8]:dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30 
[dbms-guide-5]:dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737 
[dbms-guide-8.4.1]:dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573 
[dbms-guide-8.4.2]:dbms-guide.md#23c78d3b-ca5a-4e72-8a24-645d141a3f5d 
[dbms-guide-8.4.3]:dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c 
[dbms-guide-8.4.4]:dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818 
[dbms-guide-900-sap-cache-server-on-premises]:dbms-guide.md#642f746c-e4d4-489d-bf63-73e80177a0a8
[dbms-guide-managed-disks]:dbms-guide.md#f42c6cb5-d563-484d-9667-b07ae51bce29

[dbms-guide-figure-100]:media/virtual-machines-shared-sap-dbms-guide/100_storage_account_types.png
[dbms-guide-figure-200]:media/virtual-machines-shared-sap-dbms-guide/200-ha-set-for-dbms-ha.png
[dbms-guide-figure-300]:media/virtual-machines-shared-sap-dbms-guide/300-reference-config-iaas.png
[dbms-guide-figure-400]:media/virtual-machines-shared-sap-dbms-guide/400-sql-2012-backup-to-blob-storage.png
[dbms-guide-figure-500]:media/virtual-machines-shared-sap-dbms-guide/500-sql-2012-backup-to-blob-storage-different-containers.png
[dbms-guide-figure-600]:media/virtual-machines-shared-sap-dbms-guide/600-iaas-maxdb.png
[dbms-guide-figure-700]:media/virtual-machines-shared-sap-dbms-guide/700-livecach-prod.png
[dbms-guide-figure-800]:media/virtual-machines-shared-sap-dbms-guide/800-azure-vm-sap-content-server.png
[dbms-guide-figure-900]:media/virtual-machines-shared-sap-dbms-guide/900-sap-cache-server-on-premises.png

[deployment-guide]:deployment-guide.md 
[deployment-guide-2.2]:deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94 
[deployment-guide-3.1.2]:deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab 
[deployment-guide-3.2]:deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281 
[deployment-guide-3.3]:deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2 
[deployment-guide-3.4]:deployment-guide.md#a9a60133-a763-4de8-8986-ac0fa33aa8c1 
[deployment-guide-3]:deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e 
[deployment-guide-4.1]:deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7 
[deployment-guide-4.2]:deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e 
[deployment-guide-4.3]:deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc 
[deployment-guide-4.4.2]:deployment-guide.md#6889ff12-eaaf-4f3c-97e1-7c9edc7f7542 
[deployment-guide-4.4]:deployment-guide.md#c7cbb0dc-52a4-49db-8e03-83e7edc2927d 
[deployment-guide-4.5.1]:deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4 
[deployment-guide-4.5.2]:deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f 
[deployment-guide-4.5]:deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca 
[deployment-guide-5.1]:deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2 
[deployment-guide-5.2]:deployment-guide.md#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1
[deployment-guide-5.3]:deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8 

[deployment-guide-configure-monitoring-scenario-1]:deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b 
[deployment-guide-configure-proxy]:deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d 
[deployment-guide-figure-100]:media/virtual-machines-shared-sap-deployment-guide/100-deploy-vm-image.png
[deployment-guide-figure-1000]:media/virtual-machines-shared-sap-deployment-guide/1000-service-properties.png
[deployment-guide-figure-11]:deployment-guide.md#figure-11
[deployment-guide-figure-1100]:media/virtual-machines-shared-sap-deployment-guide/1100-azperflib.png
[deployment-guide-figure-1200]:media/virtual-machines-shared-sap-deployment-guide/1200-cmd-test-login.png
[deployment-guide-figure-1300]:media/virtual-machines-shared-sap-deployment-guide/1300-cmd-test-executed.png
[deployment-guide-figure-14]:deployment-guide.md#figure-14
[deployment-guide-figure-1400]:media/virtual-machines-shared-sap-deployment-guide/1400-azperflib-error-servicenotstarted.png
[deployment-guide-figure-300]:media/virtual-machines-shared-sap-deployment-guide/300-deploy-private-image.png
[deployment-guide-figure-400]:media/virtual-machines-shared-sap-deployment-guide/400-deploy-using-disk.png
[deployment-guide-figure-5]:deployment-guide.md#figure-5
[deployment-guide-figure-50]:media/virtual-machines-shared-sap-deployment-guide/50-forced-tunneling-suse.png
[deployment-guide-figure-500]:media/virtual-machines-shared-sap-deployment-guide/500-install-powershell.png
[deployment-guide-figure-6]:deployment-guide.md#figure-6
[deployment-guide-figure-600]:media/virtual-machines-shared-sap-deployment-guide/600-powershell-version.png
[deployment-guide-figure-7]:deployment-guide.md#figure-7
[deployment-guide-figure-700]:media/virtual-machines-shared-sap-deployment-guide/700-install-powershell-installed.png
[deployment-guide-figure-760]:media/virtual-machines-shared-sap-deployment-guide/760-azure-cli-version.png
[deployment-guide-figure-900]:media/virtual-machines-shared-sap-deployment-guide/900-cmd-update-executed.png
[deployment-guide-figure-azure-cli-installed]:deployment-guide.md#402488e5-f9bb-4b29-8063-1c5f52a892d0
[deployment-guide-figure-azure-cli-version]:deployment-guide.md#0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda
[deployment-guide-install-vm-agent-windows]:deployment-guide.md#b2db5c9a-a076-42c6-9835-16945868e866
[deployment-guide-troubleshooting-chapter]:deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b

[deploy-template-cli]:../../../resource-group-template-deploy-cli.md
[deploy-template-portal]:../../../resource-group-template-deploy-portal.md
[deploy-template-powershell]:../../../resource-group-template-deploy.md

[dr-guide-classic]:https://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:get-started.md
[getting-started-dbms]:get-started.md#1343ffe1-8021-4ce6-a08d-3a1553a4db82
[getting-started-deployment]:get-started.md#6aadadd2-76b5-46d8-8713-e8d63630e955
[getting-started-planning]:get-started.md#3da0389e-708b-4e82-b2a2-e92f132df89c

[getting-started-windows-classic]:../../virtual-machines-windows-classic-sap-get-started.md
[getting-started-windows-classic-dbms]:../../virtual-machines-windows-classic-sap-get-started.md#c5b77a14-f6b4-44e9-acab-4d28ff72a930
[getting-started-windows-classic-deployment]:../../virtual-machines-windows-classic-sap-get-started.md#f84ea6ce-bbb4-41f7-9965-34d31b0098ea
[getting-started-windows-classic-dr]:../../virtual-machines-windows-classic-sap-get-started.md#cff10b4a-01a5-4dc3-94b6-afb8e55757d3
[getting-started-windows-classic-ha-sios]:../../virtual-machines-windows-classic-sap-get-started.md#4bb7512c-0fa0-4227-9853-4004281b1037
[getting-started-windows-classic-planning]:../../virtual-machines-windows-classic-sap-get-started.md#f2a5e9d8-49e4-419e-9900-af783173481c

[ha-guide-classic]:https://go.microsoft.com/fwlink/?LinkId=613056

[install-extension-cli]:virtual-machines-linux-enable-aem.md

[Logo_Linux]:media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:media/virtual-machines-shared-sap-shared/Windows.png

[msdn-set-azurermvmaemextension]:https://msdn.microsoft.com/library/azure/mt670598.aspx

[planning-guide]:planning-guide.md 
[planning-guide-1.2]:planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff 
[planning-guide-11]:planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058 
[planning-guide-11.4.1]:planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77 
[planning-guide-11.5]:planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f 
[planning-guide-2.1]:planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803 
[planning-guide-2.2]:planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10 
[planning-guide-3.1]:planning-guide.md#be80d1b9-a463-4845-bd35-f4cebdb5424a 
[planning-guide-3.2.1]:planning-guide.md#df49dc09-141b-4f34-a4a2-990913b30358 
[planning-guide-3.2.2]:planning-guide.md#fc1ac8b2-e54a-487c-8581-d3cc6625e560 
[planning-guide-3.2.3]:planning-guide.md#18810088-f9be-4c97-958a-27996255c665 
[planning-guide-3.2]:planning-guide.md#8d8ad4b8-6093-4b91-ac36-ea56d80dbf77 
[planning-guide-3.3.2]:planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 
[planning-guide-5.1.1]:planning-guide.md#4d175f1b-7353-4137-9d2f-817683c26e53 
[planning-guide-5.1.2]:planning-guide.md#e18f7839-c0e2-4385-b1e6-4538453a285c 
[planning-guide-5.2.1]:planning-guide.md#1b287330-944b-495d-9ea7-94b83aff73ef 
[planning-guide-5.2.2]:planning-guide.md#57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3 
[planning-guide-5.2]:planning-guide.md#6ffb9f41-a292-40bf-9e70-8204448559e7 
[planning-guide-5.3.1]:planning-guide.md#6e835de8-40b1-4b71-9f18-d45b20959b79 
[planning-guide-5.3.2]:planning-guide.md#a43e40e6-1acc-4633-9816-8f095d5a7b6a 
[planning-guide-5.4.2]:planning-guide.md#9789b076-2011-4afa-b2fe-b07a8aba58a1 
[planning-guide-5.5.1]:planning-guide.md#4efec401-91e0-40c0-8e64-f2dceadff646 
[planning-guide-5.5.3]:planning-guide.md#17e0d543-7e8c-4160-a7da-dd7117a1ad9d 
[planning-guide-7.1]:planning-guide.md#3e9c3690-da67-421a-bc3f-12c520d99a30 
[planning-guide-7]:planning-guide.md#96a77628-a05e-475d-9df3-fb82217e8f14 
[planning-guide-9.1]:planning-guide.md#6f0a47f3-a289-4090-a053-2521618a28c3 
[planning-guide-azure-premium-storage]:planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 

[planning-guide-figure-100]:media/virtual-machines-shared-sap-planning-guide/100-single-vm-in-azure.png
[planning-guide-figure-1300]:media/virtual-machines-shared-sap-planning-guide/1300-ref-config-iaas-for-sap.png
[planning-guide-figure-1400]:media/virtual-machines-shared-sap-planning-guide/1400-attach-detach-disks.png
[planning-guide-figure-1600]:media/virtual-machines-shared-sap-planning-guide/1600-firewall-port-rule.png
[planning-guide-figure-1700]:media/virtual-machines-shared-sap-planning-guide/1700-single-vm-demo.png
[planning-guide-figure-1900]:media/virtual-machines-shared-sap-planning-guide/1900-vm-set-vnet.png
[planning-guide-figure-200]:media/virtual-machines-shared-sap-planning-guide/200-multiple-vms-in-azure.png
[planning-guide-figure-2100]:media/virtual-machines-shared-sap-planning-guide/2100-s2s.png
[planning-guide-figure-2200]:media/virtual-machines-shared-sap-planning-guide/2200-network-printing.png
[planning-guide-figure-2300]:media/virtual-machines-shared-sap-planning-guide/2300-sapgui-stms.png
[planning-guide-figure-2400]:media/virtual-machines-shared-sap-planning-guide/2400-vm-extension-overview.png
[planning-guide-figure-2500]:media/virtual-machines-shared-sap-planning-guide/2500-vm-extension-details.png
[planning-guide-figure-2600]:media/virtual-machines-shared-sap-planning-guide/2600-sap-router-connection.png
[planning-guide-figure-2700]:media/virtual-machines-shared-sap-planning-guide/2700-exposed-sap-portal.png
[planning-guide-figure-2800]:media/virtual-machines-shared-sap-planning-guide/2800-endpoint-config.png
[planning-guide-figure-2900]:media/virtual-machines-shared-sap-planning-guide/2900-azure-ha-sap-ha.png
[planning-guide-figure-300]:media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png
[planning-guide-figure-3000]:media/virtual-machines-shared-sap-planning-guide/3000-sap-ha-on-azure.png
[planning-guide-figure-3200]:media/virtual-machines-shared-sap-planning-guide/3200-sap-ha-with-sql.png
[planning-guide-figure-400]:media/virtual-machines-shared-sap-planning-guide/400-vm-services.png
[planning-guide-figure-600]:media/virtual-machines-shared-sap-planning-guide/600-s2s-details.png
[planning-guide-figure-700]:media/virtual-machines-shared-sap-planning-guide/700-decision-tree-deploy-to-azure.png
[planning-guide-figure-800]:media/virtual-machines-shared-sap-planning-guide/800-portal-vm-overview.png
[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd 
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f 

[resource-group-authoring-templates]:../../../resource-group-authoring-templates.md
[resource-group-overview]:../../../azure-resource-manager/management/overview.md
[resource-groups-networking]:../../../networking/networking-overview.md
[sap-pam]:https://support.sap.com/pam 
[sap-templates-2-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-2-tier-os-disk]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk%2Fazuredeploy.json
[sap-templates-2-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-3-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image%2Fazuredeploy.json
[storage-azure-cli]:../../../storage/common/storage-azure-cli.md
[storage-azure-cli-copy-blobs]:../../../storage/common/storage-azure-cli.md#copy-blobs
[storage-introduction]:../../../storage/common/storage-introduction.md
[storage-powershell-guide-full-copy-vhd]:../../../storage/common/storage-powershell-guide-full.md#how-to-copy-blobs-from-one-storage-container-to-another
[storage-premium-storage-preview-portal]:../../windows/disks-types.md
[storage-redundancy]:../../../storage/common/storage-redundancy.md
[storage-scalability-targets]:../../../storage/common/scalability-targets-standard-accounts.md
[storage-use-azcopy]:../../../storage/common/storage-use-azcopy.md
[template-201-vm-from-specialized-vhd]:https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd
[templates-101-simple-windows-vm]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-simple-windows-vm
[templates-101-vm-from-user-image]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image
[virtual-machines-linux-attach-disk-portal]:../../linux/attach-disk-portal.md
[virtual-machines-azure-resource-manager-architecture]:../../../resource-manager-deployment-model.md
[virtual-machines-azurerm-versus-azuresm]:../../../resource-manager-deployment-model.md
[virtual-machines-windows-classic-configure-oracle-data-guard]:../../virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]:../../linux/cli-deploy-templates.md 
[virtual-machines-deploy-rmtemplates-powershell]:../../virtual-machines-windows-ps-manage.md 
[virtual-machines-linux-agent-user-guide]:../../linux/agent-user-guide.md
[virtual-machines-linux-agent-user-guide-command-line-options]:../../linux/agent-user-guide.md#command-line-options
[virtual-machines-linux-capture-image]:../../linux/capture-image.md
[virtual-machines-linux-capture-image-resource-manager]:../../linux/capture-image.md
[virtual-machines-linux-capture-image-resource-manager-capture]:../../linux/capture-image.md#step-2-capture-the-vm
[virtual-machines-linux-configure-raid]:../../linux/configure-raid.md
[virtual-machines-linux-configure-lvm]:../../linux/configure-lvm.md
[virtual-machines-linux-classic-create-upload-vhd-step-1]:../../virtual-machines-linux-classic-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[virtual-machines-linux-create-upload-vhd-suse]:../../linux/suse-create-upload-vhd.md
[virtual-machines-linux-redhat-create-upload-vhd]:../../linux/redhat-create-upload-vhd.md
[virtual-machines-linux-how-to-attach-disk]:../../linux/add-disk.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]:../../linux/add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk
[virtual-machines-linux-tutorial]:../../linux/quick-create-cli.md
[virtual-machines-linux-update-agent]:../../linux/update-agent.md
[virtual-machines-manage-availability-linux]:../../linux/manage-availability.md
[virtual-machines-manage-availability-windows]:../../windows/manage-availability.md
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]:virtual-machines-windows-create-powershell.md
[virtual-machines-sizes-linux]:../../linux/sizes.md
[virtual-machines-sizes-windows]:../../windows/sizes.md
[virtual-machines-windows-classic-ps-sql-alwayson-availability-groups]:./../../windows/sqlclassic/virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md
[virtual-machines-windows-classic-ps-sql-int-listener]:./../../windows/sqlclassic/virtual-machines-windows-classic-ps-sql-int-listener.md
[virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions]:./../../windows/sql/virtual-machines-windows-sql-high-availability-dr.md
[virtual-machines-sql-server-infrastructure-services]:./../../windows/sql/virtual-machines-windows-sql-server-iaas-overview.md
[virtual-machines-sql-server-performance-best-practices]:./../../windows/sql/virtual-machines-windows-sql-performance.md
[virtual-machines-upload-image-windows-resource-manager]:../../virtual-machines-windows-upload-image.md
[virtual-machines-windows-tutorial]:../../virtual-machines-windows-hero-tutorial.md
[virtual-machines-workload-template-sql-alwayson]:https://azure.microsoft.com/resources/templates/sql-server-2014-alwayson-existing-vnet-and-ad/
[virtual-network-deploy-multinic-arm-cli]:../linux/multiple-nics.md
[virtual-network-deploy-multinic-arm-ps]:../windows/multiple-nics.md
[virtual-network-deploy-multinic-arm-template]:../../../virtual-network/template-samples.md
[virtual-networks-configure-vnet-to-vnet-connection]:../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md
[virtual-networks-create-vnet-arm-pportal]:../../../virtual-network/manage-virtual-network.md#create-a-virtual-network
[virtual-networks-manage-dns-in-vnet]:../../../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md
[virtual-networks-multiple-nics]:../../../virtual-network/virtual-network-deploy-multinic-classic-ps.md
[virtual-networks-nsg]:../../../virtual-network/security-overview.md
[virtual-networks-reserved-private-ip]:../../../virtual-network/virtual-networks-static-private-ip-arm-ps.md
[virtual-networks-static-private-ip-arm-pportal]:../../../virtual-network/virtual-networks-static-private-ip-arm-pportal.md
[virtual-networks-udr-overview]:../../../virtual-network/virtual-networks-udr-overview.md
[vpn-gateway-about-vpn-devices]:../../../vpn-gateway/vpn-gateway-about-vpn-devices.md
[vpn-gateway-create-site-to-site-rm-powershell]:../../../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md
[vpn-gateway-cross-premises-options]:../../../vpn-gateway/vpn-gateway-plan-design.md
[vpn-gateway-site-to-site-create]:../../../vpn-gateway/vpn-gateway-site-to-site-create.md
[vpn-gateway-vpn-faq]:../../../vpn-gateway/vpn-gateway-vpn-faq.md
[xplat-cli]:../../../cli-install-nodejs.md
[xplat-cli-azure-resource-manager]:../../../xplat-cli-azure-resource-manager.md



En este documento se describen las diferentes áreas que se deben tener en cuenta al implementar SAP ASE en IaaS de Azure. Como condición previa a este documento, debe haber leído el documento [Consideraciones para la implementación de DBMS de Azure Virtual Machines para la carga de trabajo de SAP](dbms_guide_general.md) y otras guías de la [documentación de carga de trabajo de SAP en Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started). 

## <a name="specifics-to-sap-ase-on-windows"></a>Características específicas de SAP ASE en Windows
Desde Microsoft Azure, se pueden migrar las aplicaciones de SAP ASE a Azure Virtual Machines. SAP ASE en una máquina virtual de Azure permite reducir el costo total de propiedad de implementación, administración y mantenimiento de las aplicaciones empresariales al migrarlas fácilmente a Microsoft Azure. Con SAP ASE en una máquina virtual de Azure, los administradores y los desarrolladores pueden seguir usando las mismas herramientas de desarrollo y administración que se encuentran disponibles de forma local.

Los SLA para Azure Virtual Machines se pueden encontrar aquí: <https://azure.microsoft.com/support/legal/sla/virtual-machines>

Microsoft Azure ofrece numerosos tipos de máquinas virtuales diferentes que permiten ejecutar desde los sistemas e infraestructuras SAP más pequeños hasta otros de gran tamaño con miles de usuarios. Los números de SAPS de tamaño de SAP de las distintas SKU para máquinas virtuales certificadas por SAP se proporcionan en la nota de SAP [1928533].

Las instrucciones y recomendaciones sobre el uso de Azure Storage, la implementación de máquinas virtuales de SAP o la supervisión de SAP realizadas en [Consideraciones para la implementación de DBMS de Azure Virtual Machines para la carga de trabajo de SAP](dbms_guide_general.md) también se aplican a las implementaciones de SAP ASE.

### <a name="sap-ase-version-support"></a>Compatibilidad de versiones de SAP ASE
Actualmente, SAP admite la versión 16.0 de SAP ASE para poder utilizarse con productos de SAP Business Suite. Todas las actualizaciones para el servidor SAP ASE o los controladores ODBC y JDBC que se utilizarán con productos de SAP Business Suite se proporcionan únicamente a través de SAP Service Marketplace en: <https://support.sap.com/swdc>.

No descargue las actualizaciones del servidor de SAP ASE ni de los controladores JDBC y ODBC directamente desde los sitios web de Sybase. Para obtener más información sobre las revisiones que se pueden usar con los productos de SAP en el entorno local y en Azure Virtual Machines, vea las notas de SAP siguientes:

* [1590719]
* [1973241]

Encontrará información general sobre la ejecución de SAP Business Suite en SAP ASE en el [SCN](https://www.sap.com/community/topic/ase.html)

### <a name="sap-ase-configuration-guidelines-for-sap-related-sap-ase-installations-in-azure-vms"></a>Instrucciones de configuración de SAP ASE para SAP relacionadas con las instalaciones de SAP ASE en máquinas virtuales de Azure
#### <a name="structure-of-the-sap-ase-deployment"></a>Estructura de la implementación de SAP ASE
Los archivos ejecutables de SAP ASE se deben ubicar o instalar en la unidad del sistema del disco de sistema operativo de la máquina virtual (unidad C:\). Normalmente, la mayoría de las bases de datos de herramientas y sistemas SAP ASE no experimentan una carga de trabajo elevada. Por tanto, las bases de datos de herramientas y sistema (master, model, saptools, sybmgmtdb, sybsystemdb) pueden permanecer en la unidad C:\. 

Una excepción podría ser la base de datos temporal que, en el caso de algunas cargas de trabajo de SAP ERP y de todas las de BW, es posible que requiera un volumen de datos o de operaciones de E/S mayor. El disco del sistema operativo de la máquina virtual (unidad C:\) no puede proporcionar los volúmenes ni IOPS.

En función de la versión de SAPInst/SWPM que se use para la instalación, la configuración de la instancia de SAP ASE podría ser similar a:

* Una única tempdb de SAP ASE que se crea al instalar SAP ASE
* Una tempdb de SAP ASE creada al instalar SAP ASE y un saptempdb adicional creado en la rutina de instalación de SAP
* Una tempdb de SAP ASE creada al instalar SAP ASE y una tempdb adicional que se creó manualmente (por ejemplo, según la nota de SAP [1752266]) para cumplir los requisitos de tempdb específicos de ERP/BW

Debido a motivos de rendimiento para cargas de trabajo específicas de ERP o todas las de BW, tiene sentido almacenar los dispositivos de tempdb de la tempdb que se creó de forma adicional en una unidad distinta de C:\. Si no existe ninguna tempdb adicional, se recomienda crear una (nota de SAP [1752266]).

En estos sistemas, se deben seguir los pasos siguientes para la tempdb que se creó de forma adicional:

* Mueva el primer dispositivo de tempdb al primero de la base de datos de SAP.
* Agregue dispositivos de tempdb a cada uno de los VHD que contengan un dispositivo de la base de datos de SAP

Esta configuración permite que tempdb consuma más espacio que el que la unidad del sistema puede proporcionar. Como referencia, se pueden comprobar los tamaños de los dispositivos de tempdb en los sistemas existentes que se ejecutan de forma local. O bien, esa configuración admite los números de IOPS en la tempdb, que no se pueden proporcionar con la unidad del sistema. Los sistemas que se ejecutan de forma local se pueden usar para supervisar la carga de trabajo de E/S en tempdb.

Nunca coloque dispositivos de SAP ASE en la unidad D:\ de la máquina virtual. Para SAP ASE, este consejo también se aplica a tempdb, aunque los objetos que se conservan en ella solo son temporales.

Para las implementaciones de archivos de datos y de registro de transacciones, las instrucciones y sugerencias realizadas en [Consideraciones para la implementación de DBMS de Azure Virtual Machines para la carga de trabajo de SAP](dbms_guide_general.md). En el caso de las implementaciones basadas en Windows, se recomienda el uso de espacios de almacenamiento de Windows para crear espacios seccionados con IOPS, rendimiento y volumen suficientes.  

#### <a name="impact-of-database-compression"></a>Impacto de la compresión de las bases de datos
En configuraciones donde el ancho de banda de E/S puede convertirse en un factor de limitación, todas las medidas que reduzcan el número de IOPS podrían ayudar a ajustar la carga de trabajo que se puede ejecutar en un escenario de IaaS como Azure. Por lo tanto, se recomienda asegurarse de emplear la compresión de SAP ASE antes de cargar una base de datos existente de SAP en Azure.

La recomendación de aplicar la compresión antes de cargar en Azure viene motivada por varias razones:

* La cantidad de datos que se cargarán en Azure es menor.
* La duración de la ejecución de la compresión es menor, suponiendo que se pueda utilizar un hardware más eficaz con más capacidad de CPU, un mayor ancho de banda de E/S o una latencia de E/S menor en el entorno local.
* Si se utilizan tamaños de base de datos más pequeños, se incurriría en menos costes de asignación de disco.

La compresión de datos y de LOB funciona en una máquina virtual hospedada en Azure Virtual Machines del mismo modo a como lo hace en un entorno local. Para más información sobre cómo comprobar si ya se está usando compresión en una base de datos de SAP ASE existente, consulte la nota de SAP [1750510].

#### <a name="using-dbacockpit-to-monitor-database-instances"></a>Uso de DBACockpit para supervisar instancias de base de datos
En el caso de sistemas SAP que usan SAP ASE como plataforma de bases de datos, se puede acceder a DBACockpit mediante ventanas integradas en exploradores en transacciones de DBACockpit o de Webdynpro. Sin embargo, la funcionalidad completa de las actividades de supervisión y administración de bases de datos únicamente está disponible en la implementación de Webdynpro de DBACockpit.

Al igual que en el caso de los sistemas locales, es preciso seguir varios pasos para habilitar toda la funcionalidad de SAP NetWeaver que usa la implementación de Webdynpro de DBACockpit. Siga las indicaciones de la nota de SAP [1245200] para habilitar el uso de los recursos de Webdynpro y generar los necesarios. Las instrucciones de las notas anteriores también le ayudarán a configurar el administrador de comunicación de Internet (ICM) y los puertos que se utilizarán para las conexiones http y https. La configuración de http predeterminada tiene este aspecto:

> icm/server_port_0 = PROT=HTTP,PORT=8000,PROCTIMEOUT=600,TIMEOUT=600
> 
> icm/server_port_1 = PROT=HTTPS,PORT=443$$,PROCTIMEOUT=600,TIMEOUT=600
> 
> 

y los vínculos generados en la transacción de DBACockpit tienen un aspecto similar a este:

> https:\//\<fullyqualifiedhostname>:44300/sap/bc/webdynpro/sap/dba_cockpit
> 
> http:\//\<fullyqualifiedhostname>:8000/sap/bc/webdynpro/sap/dba_cockpit
> 
> 

En función de cómo se conecta la máquina virtual de Azure que hospeda el sistema SAP a AD y DNS, debe asegurarse de que ICM usa un nombre de host completo que se puede resolver en el equipo desde el que se abre DBACockpit. Vea la nota de SAP [773830] para obtener información sobre cómo ICM determina el nombre de host completo según los parámetros de perfil y, si es necesario, establezca el parámetro icm/host_name_full de forma explícita.

Si implementó la máquina virtual en un escenario solo de nube sin conectividad en varios entornos entre locales y Azure, debe definir una dirección IP pública y un valor de domainlabel. El formato del nombre DNS público de la máquina virtual tiene este aspecto:

> `<custom domainlabel`&gt;.`<azure region`&gt;.cloudapp.azure.com
> 
> 

Encontrará más detalles relacionados con el nombre DNS [aquí][virtual-machines-azurerm-versus-azuresm].

El ajuste del parámetro de perfil de SAP icm/host_name_full con el nombre DNS del vínculo de la máquina virtual de Azure puede tener este aspecto:

> https:\//mydomainlabel.westeurope.cloudapp.net:44300/sap/bc/webdynpro/sap/dba_cockpit
> 
> http:\//mydomainlabel.westeurope.cloudapp.net:8000/sap/bc/webdynpro/sap/dba_cockpit

En ese caso, debe asegurarse de lo siguiente:

* Agregación de reglas de entrada al grupo de seguridad de red en Azure Portal para los puertos TCP/IP que se utilizan en la comunicación con el ICM
* Agregar reglas de entrada a la configuración del Firewall de Windows para los puertos TCP/IP que se utilizan en la comunicación con el ICM

Para importar de forma automatizada todas las correcciones disponibles, se recomienda aplicar periódicamente la colección de correcciones de la nota de SAP correspondiente a su versión de SAP:

* [1558958]
* [1619967]
* [1882376]

Encontrará más información sobre DBA Cockpit para SAP ASE en las siguientes notas de SAP:

* [1605680]
* [1757924]
* [1757928]
* [1758182]
* [1758496]    
* [1814258]
* [1922555]
* [1956005]

#### <a name="backuprecovery-considerations-for-sap-ase"></a>Consideraciones de copia de seguridad y recuperación para SAP ASE
Al implementar SAP ASE en Azure, debe revisar la metodología de copia de seguridad. Incluso para los sistemas que no son de producción, se deben realizar copias de seguridad periódicas de las bases de datos de SAP. Como Azure Storage mantiene tres imágenes, es posible que una copia de seguridad sea menos importante para compensar un bloqueo de almacenamiento. La razón principal para el mantenimiento de un plan de copia de seguridad y restauración correcto es que puede compensar errores lógicos o manuales gracias a la capacidad de recuperación a un momento dado. El objetivo es usar las copias de seguridad para restaurar la base de datos a un momento dado o utilizar las copias de seguridad de Azure para propagar otro sistema mediante la copia de la base de datos existente. 

La realización de copias de seguridad y restauraciones de bases de datos de Azure funciona del mismo modo que en entornos locales. Consulte las notas de SAP:

* [1588316]
* [1585981]

para obtener más información sobre la creación de configuraciones de volcado y la programación de copias de seguridad. En función de la estrategia y las necesidades, puede configurar los volcados de la base de datos y del registro en disco en uno de los discos existentes, o bien agregar un disco adicional para la copia de seguridad. Para reducir el riesgo de pérdida de datos si se produce un error, se recomienda usar un disco en el que no haya archivos de base de datos.

Además de la compresión de datos y de LOB, SAP ASE también ofrece compresión de copias de seguridad. Para que los volcados de bases de datos y registros consuman menos espacio, se recomienda usar la compresión de copias de seguridad. Para más información, consulte la nota de SAP [1588316]. Este tipo de compresión también es fundamental para reducir la cantidad de datos que se transfieren si planea descargar copias de seguridad o VHD que contengan volcados de copias de seguridad de la máquina virtual de Azure en el entorno local.

No utilice la unidad D:\ como destino de los volcados de bases de datos o registros.

#### <a name="performance-considerations-for-backupsrestores"></a>Consideraciones de rendimiento sobre copias de seguridad / restauraciones
Como en las implementaciones sin sistema operativo, el rendimiento de las copias de seguridad y las restauraciones depende de cuántos volúmenes puedan leerse en paralelo y el rendimiento que estos volúmenes puedan tener. Tenga en cuenta que la compresión de copias de seguridad consume recursos de CPU. Este consumo de CPU de la compresión de copias de seguridad puede desempeñar un papel importante en las máquinas virtuales con un número reducido de subprocesos de CPU. Por lo tanto, puede asumir los siguientes puntos:

* Cuantos menos discos se utilicen para almacenar los dispositivos de las bases de datos, menor será el rendimiento general de lectura
* Cuantos menos subprocesos de CPU haya en la máquina virtual, mayor será el impacto en la compresión de copias de seguridad.
* Cuantos menos destinos (directorios de sección, discos) en los que escribir la copia de seguridad haya, menor será el rendimiento

Para aumentar la cantidad de destinos en los que escribir, existen dos opciones que se pueden usar o combinar según sus necesidades:

* Seccionar el volumen de destino de la copia de seguridad en varios discos montados para mejorar el rendimiento de IOPS en ese volumen seccionado
* Crear una configuración de volcado en el nivel de SAP ASE que utilice más de un directorio de destino en el que escribir el volcado

Seccionar un volumen de disco en varios discos montados se ha explicado anteriormente en [Consideraciones para la implementación de DBMS de Azure Virtual Machines para la carga de trabajo de SAP](dbms_guide_general.md). Para más información sobre el uso de varios directorios en la configuración de volcado de SAP ASE, consulte la documentación sobre el procedimiento almacenado sp_config_dump que se utiliza para crear la configuración de volcado en [Sybase Infocenter](http://infocenter.sybase.com/help/index.jsp).

### <a name="disaster-recovery-with-azure-vms"></a>Recuperación ante desastres con máquinas virtuales de Azure
#### <a name="data-replication-with-sap-sybase-replication-server"></a>Replicación de datos con el servidor de replicación SAP Sybase
Con el servidor de replicación SAP Sybase (SRS), SAP ASE proporciona una solución de espera semiactiva para transferir las transacciones de base de datos a una ubicación distante de forma asincrónica. 

La instalación y la actividad de SRS se desarrollan funcionalmente igual de bien en una máquina virtual hospedada en servicios de máquina virtual de Azure que en el entorno local.

SAP ASE HADR no necesita un equilibrador de carga interno de Azure y no tiene dependencias de agrupación en clústeres de nivel de sistema operativo. Funciona en máquinas virtuales Linux y Windows de Azure. Para más información SAP ASE HADR, lea el [manual del usuario de SAP ASE HADR](https://help.sap.com/viewer/efe56ad3cad0467d837c8ff1ac6ba75c/16.0.3.3/en-US/a6645e28bc2b1014b54b8815a64b87ba.html).

## <a name="specifics-to-sap-ase-on-linux"></a>Detalles de SAP ASE en Linux
Desde Microsoft Azure puede migrar fácilmente sus aplicaciones de SAP ASE a Azure Virtual Machines. Un entorno ASE de SAP en una máquina virtual permite reducir el costo total de propiedad de implementación, administración y mantenimiento de aplicaciones empresariales al migrarlas fácilmente a Microsoft Azure. Con SAP ASE en una máquina virtual de Azure, los administradores y los desarrolladores pueden seguir usando las mismas herramientas de desarrollo y administración que se encuentran disponibles de forma local.

Para implementar máquinas virtuales de Azure es importante conocer los Acuerdos de Nivel de Servicio oficiales que puede encontrar aquí: <https://azure.microsoft.com/support/legal/sla>

En la nota de SAP [1928533], se proporciona información de tamaño de SAP y una lista de SKU para máquinas virtuales certificada por SAP. Los documentos adicionales sobre ajuste del tamaño de SAP para máquinas virtuales de Azure se pueden encontrar aquí <https://blogs.msdn.com/b/saponsqlserver/archive/2015/06/19/how-to-size-sap-systems-running-on-azure-vms.aspx> y aquí <https://blogs.msdn.com/b/saponsqlserver/archive/2015/12/01/new-white-paper-on-sizing-sap-solutions-on-azure-public-cloud.aspx>

Las instrucciones y recomendaciones sobre el uso de Azure Storage, la implementación de máquinas virtuales de SAP o la supervisión de SAP se aplica a las implementaciones de SAP ASE junto con las aplicaciones de SAP, como se indica en los cuatro primeros capítulos de este documento.

Las dos siguientes notas de SAP incluyen información general acerca de ASE en Linux y ASE en la nube:

* [2134316]
* [1941500]

### <a name="sap-ase-version-support"></a>Compatibilidad de versiones de SAP ASE
Actualmente, SAP admite la versión 16.0 de SAP ASE para poder utilizarse con productos de SAP Business Suite. Todas las actualizaciones para el servidor SAP ASE o los controladores ODBC y JDBC que se utilizarán con productos de SAP Business Suite se proporcionan únicamente a través de SAP Service Marketplace en: <https://support.sap.com/swdc>.

En el caso de las instalaciones realizadas de forma local, no descargue las actualizaciones del servidor de SAP ASE ni de los controladores JDBC y ODBC directamente desde los sitios web de Sybase. Para más información sobre las revisiones que pueden utilizarse con los productos de SAP Business Suite en el entorno local y en Azure Virtual Machines, consulte las siguientes notas de SAP:

* [1590719]
* [1973241]

Encontrará información general sobre la ejecución de SAP Business Suite en SAP ASE en el [SCN](https://www.sap.com/community/topic/ase.html)

### <a name="sap-ase-configuration-guidelines-for-sap-related-sap-ase-installations-in-azure-vms"></a>Instrucciones de configuración de SAP ASE para SAP relacionadas con las instalaciones de SAP ASE en máquinas virtuales de Azure
#### <a name="structure-of-the-sap-ase-deployment"></a>Estructura de la implementación de SAP ASE
Los archivos ejecutables de SAP ASE se deben ubicar o instalar en el sistema de archivos raíz de la máquina virtual (/sybase). Normalmente, la mayoría de las bases de datos de herramientas y sistemas SAP ASE no experimentan una carga de trabajo elevada. Por tanto, las bases de datos de herramientas y sistemas (master, model, saptools, sybmgmtdb, sybsystemdb) se pueden almacenar en el sistema de archivos raíz. 

Una excepción podría ser la base de datos temporal que, en el caso de algunas cargas de trabajo de SAP ERP y de todas las de BW, es posible que requiera un volumen de datos o de operaciones de E/S mayor. El disco del sistema operativo de la máquina virtual no puede proporcionar los volúmenes ni IOPS. 

Según la versión de SAPInst/SWPM utilizada para instalar el sistema, la base de datos puede contener los siguientes elementos:

* Una única tempdb de SAP ASE que se crea al instalar SAP ASE
* Una tempdb de SAP ASE creada al instalar SAP ASE y un saptempdb adicional creado en la rutina de instalación de SAP
* Una tempdb de SAP ASE creada al instalar SAP ASE y una tempdb adicional que se creó manualmente (por ejemplo, según la nota de SAP [1752266]) para cumplir los requisitos de tempdb específicos de ERP/BW

Por motivos de rendimiento para cargas de trabajo específicas de ERP o para todas las de BW, puede tener sentido almacenar los dispositivos de tempdb de la tempdb que se creó de forma adicional (mediante SWPM o manualmente) en un sistema de archivos distinto, que se puede representar mediante un disco de datos de Azure único o un RAID de Linux que abarque varios discos de datos de Azure. Si no existe ninguna tempdb adicional, se recomienda crear una (nota de SAP [1752266]).

En estos sistemas, se deben seguir estos pasos para la tempdb que se creó de forma adicional:

* Traslade el primer directorio de tempdb al primer sistema de archivos de la base de datos de SAP.
* Agregue directorios de tempdb a cada uno de los discos que contengan un sistema de archivos de la base de datos de SAP

Esta configuración permite que tempdb consuma más espacio que el que la unidad del sistema puede proporcionar. Como referencia, se pueden comprobar los tamaños de los dispositivos de tempdb en los sistemas existentes que se ejecutan de forma local. O bien, esa configuración admite los números de IOPS en la tempdb, que no se pueden proporcionar con la unidad del sistema. Los sistemas que se ejecutan de forma local se pueden usar para supervisar la carga de trabajo de E/S en tempdb.

Nunca coloque ningún directorio de SAP ASE en las rutas /mnt ni /mnt/resource de la máquina virtual. Para SAP ASE, este consejo también se aplica a tempdb, aunque los objetos que se conservan en ella solo son temporales. Dado que /mnt o /mnt/resource es un espacio temporal de máquina virtual de Azure predeterminado, no es persistente. Encontrará más información sobre el espacio temporal de máquina virtual de Azure en [este artículo][virtual-machines-linux-how-to-attach-disk].

Para las implementaciones de archivos de datos y de registro de transacciones, las instrucciones y sugerencias realizadas en [Consideraciones para la implementación de DBMS de Azure Virtual Machines para la carga de trabajo de SAP](dbms_guide_general.md). En el caso de las implementaciones basadas en Linux, se recomienda el uso de espacios de LVM o MDADM para crear espacios seccionados con IOPS, rendimiento y volumen suficientes. 

#### <a name="impact-of-database-compression"></a>Impacto de la compresión de las bases de datos
En configuraciones donde el ancho de banda de E/S puede convertirse en un factor de limitación, todas las medidas que reduzcan el número de IOPS podrían ayudar a ajustar la carga de trabajo que se puede ejecutar en un escenario de IaaS como Azure. Por lo tanto, se recomienda asegurarse de emplear la compresión de SAP ASE antes de cargar una base de datos existente de SAP en Azure.

La recomendación de aplicar la compresión antes de cargar en Azure viene motivada por varias razones:

* La cantidad de datos que se cargarán en Azure es menor.
* La duración de la ejecución de la compresión es menor, suponiendo que se pueda utilizar un hardware más eficaz con más capacidad de CPU, un mayor ancho de banda de E/S o una latencia de E/S menor en el entorno local.
* Si se utilizan tamaños de base de datos más pequeños, se incurriría en menos costes de asignación de disco.

La compresión de datos y de LOB funciona en una máquina virtual hospedada en Azure Virtual Machines del mismo modo a como lo hace en un entorno local. Para más información sobre cómo comprobar si ya se está usando compresión en una base de datos de SAP ASE existente, consulte la nota de SAP [1750510]. Para obtener más información sobre la compresión de bases de datos, vea la nota de SAP [2121797].

#### <a name="using-dbacockpit-to-monitor-database-instances"></a>Uso de DBACockpit para supervisar instancias de base de datos
En el caso de sistemas SAP que usan SAP ASE como plataforma de bases de datos, se puede acceder a DBACockpit mediante ventanas integradas en exploradores en transacciones de DBACockpit o de Webdynpro. Sin embargo, la funcionalidad completa de las actividades de supervisión y administración de bases de datos únicamente está disponible en la implementación de Webdynpro de DBACockpit.

Al igual que en el caso de los sistemas locales, es preciso seguir varios pasos para habilitar toda la funcionalidad de SAP NetWeaver que usa la implementación de Webdynpro de DBACockpit. Siga las indicaciones de la nota de SAP [1245200] para habilitar el uso de los recursos de Webdynpro y generar los necesarios. Las instrucciones de las notas anteriores también le ayudarán a configurar el administrador de comunicación de Internet (ICM) y los puertos que se utilizarán para las conexiones http y https. La configuración de http predeterminada tiene el siguiente aspecto:

> icm/server_port_0 = PROT=HTTP,PORT=8000,PROCTIMEOUT=600,TIMEOUT=600
> 
> icm/server_port_1 = PROT=HTTPS,PORT=443$$,PROCTIMEOUT=600,TIMEOUT=600
> 
> 

y los vínculos generados en la transacción de DBACockpit tendrán un aspecto similar al siguiente:

> https:\//\<fullyqualifiedhostname>:44300/sap/bc/webdynpro/sap/dba_cockpit
> 
> http:\//\<fullyqualifiedhostname>:8000/sap/bc/webdynpro/sap/dba_cockpit
> 
> 

En función de cómo se conecta la máquina virtual de Azure que hospeda el sistema SAP a AD y DNS, debe asegurarse de que ICM usa un nombre de host completo que se puede resolver en el equipo desde el que se abre DBACockpit. Vea la nota de SAP [773830] para obtener información sobre cómo ICM determina el nombre de host completo según los parámetros de perfil y, si es necesario, establezca el parámetro icm/host_name_full de forma explícita.

Si implementó la máquina virtual en un escenario solo de nube sin conectividad en varios entornos entre locales y Azure, debe definir una dirección IP pública y un valor de domainlabel. El formato del nombre DNS público de la máquina virtual tiene este aspecto:

> `<custom domainlabel`&gt;.`<azure region`&gt;.cloudapp.azure.com
> 
> 

Encontrará más detalles relacionados con el nombre DNS [aquí][virtual-machines-azurerm-versus-azuresm].

El ajuste del parámetro de perfil de SAP icm/host_name_full con el nombre DNS del vínculo de la máquina virtual de Azure puede tener este aspecto:

> https:\//mydomainlabel.westeurope.cloudapp.net:44300/sap/bc/webdynpro/sap/dba_cockpit
> 
> http:\//mydomainlabel.westeurope.cloudapp.net:8000/sap/bc/webdynpro/sap/dba_cockpit

En ese caso, debe asegurarse de lo siguiente:

* Agregación de reglas de entrada al grupo de seguridad de red en Azure Portal para los puertos TCP/IP que se utilizan en la comunicación con el ICM
* Agregar reglas de entrada a la configuración del Firewall de Windows para los puertos TCP/IP que se utilizan en la comunicación con el ICM

Para importar de forma automatizada todas las correcciones disponibles, se recomienda aplicar periódicamente la colección de correcciones de la nota de SAP correspondiente a su versión de SAP:

* [1558958]
* [1619967]
* [1882376]

Encontrará más información sobre DBA Cockpit para SAP ASE en las siguientes notas de SAP:

* [1605680]
* [1757924]
* [1757928]
* [1758182]
* [1758496]    
* [1814258]
* [1922555]
* [1956005]

#### <a name="backuprecovery-considerations-for-sap-ase"></a>Consideraciones de copia de seguridad y recuperación para SAP ASE
Al implementar SAP ASE en Azure, debe revisar la metodología de copia de seguridad. Incluso para los sistemas que no son de producción, se deben realizar copias de seguridad periódicas de las bases de datos de SAP. Como Azure Storage mantiene tres imágenes, es posible que una copia de seguridad sea menos importante para compensar un bloqueo de almacenamiento. La razón principal para el mantenimiento de un plan de copia de seguridad y restauración correcto es que puede compensar errores lógicos o manuales gracias a la capacidad de recuperación a un momento dado. El objetivo es usar las copias de seguridad para restaurar la base de datos a un momento dado o utilizar las copias de seguridad de Azure para propagar otro sistema mediante la copia de la base de datos existente. 

La realización de copias de seguridad y restauraciones de bases de datos de Azure funciona del mismo modo que en entornos locales. Consulte las notas de SAP:

* [1588316]
* [1585981]

para obtener más información sobre la creación de configuraciones de volcado y la programación de copias de seguridad. En función de la estrategia y las necesidades, puede configurar los volcados de la base de datos y del registro en disco en uno de los discos existentes, o bien agregar un disco adicional para la copia de seguridad. Para reducir el riesgo de pérdidas de datos si se produce un error, se recomienda usar un disco en el que no haya directorios ni archivos de bases de datos.

Además de la compresión de datos y de LOB, SAP ASE también ofrece compresión de copias de seguridad. Para que los volcados de bases de datos y registros consuman menos espacio, se recomienda usar la compresión de copias de seguridad. Para más información, consulte la nota de SAP [1588316]. Este tipo de compresión también es fundamental para reducir la cantidad de datos que se transfieren si planea descargar copias de seguridad o VHD que contengan volcados de copias de seguridad de la máquina virtual de Azure en el entorno local.

No use el espacio temporal /mnt ni /mnt/Resource de máquina virtual de Azure como destino de volcados de registro o de bases de datos.

#### <a name="performance-considerations-for-backupsrestores"></a>Consideraciones de rendimiento sobre copias de seguridad / restauraciones
Como en las implementaciones sin sistema operativo, el rendimiento de las copias de seguridad y las restauraciones depende de cuántos volúmenes puedan leerse en paralelo y el rendimiento que estos volúmenes puedan tener. Tenga en cuenta que la compresión de copias de seguridad consume recursos de CPU. Este consumo de CPU de la compresión de copias de seguridad puede desempeñar un papel importante en las máquinas virtuales con un número reducido de subprocesos de CPU.  Por lo tanto, puede asumir los siguientes puntos:

* Cuantos menos discos se utilicen para almacenar los dispositivos de las bases de datos, menor será el rendimiento general de lectura
* Cuantos menos subprocesos de CPU haya en la máquina virtual, mayor será el impacto en la compresión de copias de seguridad.
* Cuantos menos destinos (RAID por software Linux, discos) en los que escribir la copia de seguridad haya, menor será el rendimiento

Para aumentar la cantidad de destinos en los que escribir, existen dos opciones que se pueden usar o combinar según sus necesidades:

* Seccionar el volumen de destino de la copia de seguridad en varios discos montados para mejorar el rendimiento de IOPS en ese volumen seccionado
* Crear una configuración de volcado en el nivel de SAP ASE que utilice más de un directorio de destino en el que escribir el volcado

Seccionar un volumen de disco en varios discos montados se ha explicado anteriormente en [Consideraciones para la implementación de DBMS de Azure Virtual Machines para la carga de trabajo de SAP](dbms_guide_general.md). Para más información sobre el uso de varios directorios en la configuración de volcado de SAP ASE, consulte la documentación sobre el procedimiento almacenado sp_config_dump que se utiliza para crear la configuración de volcado en [Sybase Infocenter](http://infocenter.sybase.com/help/index.jsp).

### <a name="disaster-recovery-with-azure-vms"></a>Recuperación ante desastres con máquinas virtuales de Azure
#### <a name="data-replication-with-sap-sybase-replication-server"></a>Replicación de datos con el servidor de replicación SAP Sybase
Con el servidor de replicación SAP Sybase (SRS), SAP ASE proporciona una solución de espera semiactiva para transferir las transacciones de base de datos a una ubicación distante de forma asincrónica. 

La instalación y la actividad de SRS se desarrollan funcionalmente igual de bien en una máquina virtual hospedada en servicios de máquina virtual de Azure que en el entorno local.

Se admite ASE HADR a través de SAP Replication Server. Se recomienda encarecidamente usar SAP ASE 16.03 para intentar esa configuración. En este [blog](https://blogs.msdn.microsoft.com/saponsqlserver/2018/06/18/installation-procedure-for-sybase-16-3-patch-level-3-always-on-dr-on-suse-12-3-recent-customer-proof-of-concept/) encontrará instrucciones más detalladas para instalar esas configuraciones.
