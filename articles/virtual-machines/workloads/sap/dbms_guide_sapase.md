---
title: Развертывание СУБД на виртуальных машинах Azure в SAP ASE для рабочей нагрузки SAP | Документы Майкрософт
description: Развертывание СУБД на виртуальных машинах Azure в SAP ASE для рабочей нагрузки SAP
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/1/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3f50f013020c704ddc294a59f8c6c5dac24bbd5a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60835283"
---
# <a name="sap-ase-azure-virtual-machines-dbms-deployment-for-sap-workload"></a>Развертывание СУБД на виртуальных машинах Azure в SAP ASE для рабочей нагрузки SAP

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
[1558958;]: https://launchpad.support.sap.com/#/notes/1558958
[1585981.]: https://launchpad.support.sap.com/#/notes/1585981
[1588316]: https://launchpad.support.sap.com/#/notes/1588316
[1590719]: https://launchpad.support.sap.com/#/notes/1590719
[1597355]:https://launchpad.support.sap.com/#/notes/1597355
[1605680;]: https://launchpad.support.sap.com/#/notes/1605680
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
[1882376.]: https://launchpad.support.sap.com/#/notes/1882376
[1909114]:https://launchpad.support.sap.com/#/notes/1909114
[1922555]: https://launchpad.support.sap.com/#/notes/1922555
[1928533]: https://launchpad.support.sap.com/#/notes/1928533
[1941500]: https://launchpad.support.sap.com/#/notes/1941500
[1956005.]: https://launchpad.support.sap.com/#/notes/1956005
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
[azure-subscription-service-limits]:../../../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../../../azure-subscription-service-limits.md#subscription-limits

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
[resource-group-overview]:../../../azure-resource-manager/resource-group-overview.md
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
[storage-scalability-targets]:../../../storage/common/storage-scalability-targets.md
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



В этом документе рассматривается несколько аспектов, которые следует учитывать при развертывании SAP ASE в Azure IaaS. Перед чтением этого документа следует ознакомиться с документом [Вопросы развертывания СУБД для рабочей нагрузки SAP на виртуальных машинах Azure](dbms_guide_general.md), а также с другими руководствами в [документации по рабочей нагрузке SAP в Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started). 

## <a name="specifics-to-sap-ase-on-windows"></a>Особенности SAP ASE в Windows
Начиная с Microsoft Azure можно переносить существующие приложения SAP ASE на виртуальные машины Azure. SAP ASE на виртуальной машине Azure позволяет снизить общую стоимость владения для развертывания и обслуживания приложений уровня организации, а также управления ими благодаря переносу этих приложений в Microsoft Azure. С помощью SAP ASE на виртуальной машине Azure администраторы и разработчики могут продолжать использовать те же средства разработки и администрирования, которые доступны в локальной среде.

Соглашения об уровне обслуживания для виртуальных машин Azure можно найти здесь: <https://azure.microsoft.com/support/legal/sla/virtual-machines>

Microsoft Azure предоставляет различные типы виртуальных машин, которые позволяют запускать как небольшие системы и ландшафты SAP, так и крупные системы и ландшафты SAP с несколькими тысячами пользователей. Номера SAPS размеров SAP для различных номеров SKU виртуальных машин, сертифицированных SAP, предоставлены в примечании к SAP [1928533].

Инструкции и рекомендации по использованию службы хранилища Azure, развертыванию виртуальных машин SAP и мониторингу SAP, приведенные в разделе [Вопросы развертывания СУБД для рабочей нагрузки SAP на виртуальных машинах Azure](dbms_guide_general.md), также применяются к развертываниям SAP ASE.

### <a name="sap-ase-version-support"></a>Поддержка версий SAP ASE
SAP в настоящее время поддерживает версию SAP ASE 16.0 для использования с продуктами SAP Business Suite. Все обновления для сервера SAP ASE, а также драйверы JDBC и ODBC для продуктов SAP Business Suite доступны только в SAP Service Marketplace по адресу: <https://support.sap.com/swdc>.

Не следует загружать обновления для сервера SAP ASE или драйверы JDBC и ODBC непосредственно с веб-сайтов Sybase. Подробные сведения об исправлениях, которые поддерживаются для использования с продуктами SAP в локальной среде и на виртуальных машинах Azure, см. в следующих примечаниях SAP:

* [1590719]
* [1973241]

Общие сведения о запуске SAP Business Suite в SAP ASE можно найти на сайте [SCN](https://www.sap.com/community/topic/ase.html)

### <a name="sap-ase-configuration-guidelines-for-sap-related-sap-ase-installations-in-azure-vms"></a>Рекомендации по конфигурации SAP ASE для связанных с SAP установок SAP ASE на виртуальных машинах Azure
#### <a name="structure-of-the-sap-ase-deployment"></a>Структура развертывания SAP ASE
Исполняемые файлы SAP ASE должны быть расположены или установлены на системном диске ОС виртуальной машины (диск C:\)). Как правило, большинство баз данных системы и средств SAP ASE не испытывают больших рабочих нагрузок. Поэтому базы данных системы и средств (master, model, saptools, sybmgmtdb, sybsystemdb) могут оставаться на диске C:\. 

Исключением может быть временная база данных, для которой в случае некоторых рабочих нагрузок SAP ERP и любых рабочих нагрузок BW может потребоваться больший объем данных или объем операций ввода-вывода. Этот объем или число операций ввода-вывода в секунду не могут быть предоставлены с помощью диска ОС виртуальной машины (диска C:\).

В зависимости от версии SAPInst/SWPM, использованной для установки, пример конфигурации SAP ASE может выглядеть так:

* Отдельную базу tempdb SAP ASE, которая создается при установке SAP ASE.
* Базу tempdb SAP ASE, созданную при установке SAP ASE, и дополнительную базу saptempdb, созданную процедурой установки SAP.
* Базу данных tempdb SAP ASE, созданную при установке SAP ASE, и дополнительную базу данных tempdb, созданную вручную (например, согласно примечанию к SAP [1752266]) для соответствия особым требованиям tempdb к ERP/BW.

По причинам, связанным с производительностью, устройства tempdb для определенных рабочих нагрузок ERP или всех рабочих нагрузок BW можно размещать в дополнительно созданной базе данных tempdb на диске, отличном от C:\. При отсутствии дополнительной базы данных tempdb рекомендуется ее создать (примечание к SAP [1752266]).

Для таких систем необходимо выполнить следующие действия над дополнительно созданной базой данных tempdb:

* Переместите первое устройство tempdb на первое устройство базы данных SAP.
* Добавьте устройства tempdb на каждый диск, содержащий устройства базы данных SAP.

Эта конфигурация позволяет tempdb использовать больше пространства, чем может предоставить системный диск. Для справки можно проверить размеры устройств tempdb в существующих системах, работающих локально. Такая конфигурация также позволяет увеличить число операций ввода-вывода в секунду для базы данных tempdb, которое не может быть обеспечено при использовании системного диска. Системы, работающие в локальной среде, можно использовать для мониторинга рабочей нагрузки ввода-вывода на базу данных tempdb.

Никогда не помещайте устройства SAP ASE на диск D:\ виртуальной машины. Для SAP ASE это правило также применяется к tempdb, даже если содержащиеся в tempdb объекты являются временными.

Инструкции и рекомендации по развертыванию файлов данных и файлов журналов транзакций приведены в разделе [Вопросы развертывания СУБД для рабочей нагрузки SAP на виртуальных машинах Azure](dbms_guide_general.md). Для развертываний на основе Windows рекомендуется использовать дисковые пространства Windows для создания чередующихся наборов с достаточно большим числом операций ввода-вывода в секунду, высокой пропускной способностью и достаточным объемом.  

#### <a name="impact-of-database-compression"></a>Влияние сжатия базы данных
В конфигурациях, в которых пропускная способность ввода-вывода может стать ограничивающим фактором, каждая мера, позволяющая сократить объем операций ввода-вывода, помогает растянуть рабочую нагрузку, которую можно запускать в сценарии IaaS, например Azure. Таким образом, рекомендуем обязательно использовать сжатие SAP ASE перед отправкой имеющейся базы данных SAP в Azure.

Использовать сжатие перед отправкой в Azure рекомендуется по нескольким причинам:

* Объем отправляемых в Azure данных уменьшается.
* Длительность выполнения сжатия короче, при условии, что локально можно использовать более мощное оборудование с большим числом ЦП, более высокой пропускной способностью ввода-вывода или меньшими задержками ввода-вывода.
* Уменьшение размеров баз данных может сократить затраты на выделение места на дисках.

Сжатие данных и LOB работает на виртуальных машинах Azure так же, как и в локальной среде. Дополнительные сведения о том, как проверить, используется ли уже сжатие в существующей базе данных SAP ASE, см. в примечании к SAP [1750510].

#### <a name="using-dbacockpit-to-monitor-database-instances"></a>Использование DBACockpit для мониторинга экземпляров баз данных
Для систем SAP, в которых используется SAP ASE в качестве платформы баз данных, средство DBACockpit доступно в виде встроенных окон браузера в DBACockpit для транзакции или в виде Webdynpro. Однако полный набор функций для мониторинга и администрирования базы данных доступен только в реализации Webdynpro средства DBACockpit.

Как и для локальных систем, требуется выполнить ряд действий, чтобы включить все функциональные возможности SAP NetWeaver, используемые в реализации Webdynpro средства DBACockpit. Ознакомьтесь с информацией в примечании к SAP [1245200] , чтобы включить использование инструментов Webdynpro и создать те из них, которые необходимы. При соблюдении инструкций в упомянутых выше примечаниях будет также выполнена настройка Internet Communication Manager (icm) вместе с портами, используемыми для подключений http и https. Настройка по умолчанию для http выглядит следующим образом:

> icm/server_port_0 = PROT=HTTP,PORT=8000,PROCTIMEOUT=600,TIMEOUT=600
> 
> icm/server_port_1 = PROT=HTTPS,PORT=443$$,PROCTIMEOUT=600,TIMEOUT=600
> 
> 

Ссылки, создаваемые в DBACockpit для транзакции, выглядят так:

> https:\//\<fullyqualifiedhostname >: 44300 во/sap/bc/webdynpro/sap/dba_cockpit
> 
> http:\//\<fullyqualifiedhostname >: 8000/sap/bc/webdynpro/sap/dba_cockpit
> 
> 

В зависимости от того, как виртуальная машина Azure, на которой размещается система SAP, подключена к AD и DNS, необходимо убедиться в том, что ICM использует полное имя узла, которое может быть разрешено на компьютере, с которого вы открываете DBACockpit. Чтобы узнать, как ICM определяет полное имя узла в зависимости от параметров профиля, а также явным образом задать параметр icm/host_name_full (если необходимо), ознакомьтесь с примечанием к SAP [773830].

Если вы развернули виртуальную машину только в облачной среде (без подключения между локальной средой и Azure), вам необходимо определить общедоступный IP-адрес и метку домена. Формат общедоступного DNS-имени виртуальной машины будет выглядеть так:

> `<custom domainlabel`>.`<azure region`>.cloudapp.azure.com
> 
> 

Дополнительную информацию о DNS-имени см. [здесь][virtual-machines-azurerm-versus-azuresm].

Если для параметра профиля SAP icm/host_name_full задать DNS-имя виртуальной машины Azure, ссылка может выглядеть так:

> https:\//mydomainlabel.westeurope.cloudapp.net:44300/sap/bc/webdynpro/sap/dba_cockpit
> 
> http:\//mydomainlabel.westeurope.cloudapp.net:8000/sap/bc/webdynpro/sap/dba_cockpit

В таком случае необходимо:

* добавить правила для входящего трафика в группу безопасности сети на портале Azure для портов TCP/IP, используемых для связи с ICM;
* добавить правила для входящего трафика в конфигурацию брандмауэра Windows для портов TCP/IP, используемых для связи с ICM.

Для автоматического импорта всех доступных исправлений рекомендуется периодически применять примечания SAP, содержащие наборы исправлений, которые относятся к вашей версии SAP:

* [1558958;]
* [1619967]
* [1882376.]

Дополнительные сведения о панели DBA для SAP ASE можно найти в следующих примечаниях SAP:

* [1605680;]
* [1757924]
* [1757928]
* [1758182]
* [1758496]    
* [1814258]
* [1922555]
* [1956005.]

#### <a name="backuprecovery-considerations-for-sap-ase"></a>Рекомендации по резервному копированию и восстановлению для SAP ASE
При развертывании SAP ASE в Azure методику резервного копирования необходимо пересмотреть. Даже в средах, не являющихся рабочими, необходимо периодически проводить резервное копирование баз данных SAP. Так как в службе хранилища Azure хранятся три образа, резервное копирование может считаться менее важным для восстановления хранилища после сбоя. Основная причина соблюдения надлежащего плана резервного копирования и восстановления следующая. Благодаря возможности восстанавливать состояние на определенный момент времени вы можете исправлять логические и пользовательские ошибки. Следовательно, резервные копии используются для восстановления состояния базы данных на определенный момент времени или для создания другой системы в Azure путем копирования существующей базы данных. 

Резервное копирование и восстановление базы данных в Azure работает так же, как и в локальных системах. Ознакомьтесь с примечаниями к SAP:

* [1588316]
* [1585981.]

В них вы найдете дополнительные сведения о создании конфигураций дампов и планировании резервного копирования. В зависимости от стратегии и потребностей можно настроить запись дампов базы данных и журнала на диск на одном из существующих дисков или добавить дополнительный диск для резервных копий. Чтобы снизить риск потери данных в случае возникновения ошибки, мы рекомендуем использовать диск, на котором нет файлов баз данных.

Помимо сжатия данных и больших объектов система SAP ASE также предлагает сжатие резервных копий. Чтобы дампы базы данных и журнала занимали меньше места, рекомендуется использовать сжатие резервных копий. Дополнительные сведения см. в примечании SAP [1588316]. Сжатие резервных копий также позволяет существенно уменьшить объем данных при передаче. Это актуально, если вы планируете скачивать резервные копии или содержимое виртуальных дисков с дампами резервных копий с виртуальных машин Azure в локальную систему.

Не используйте диск D:\ в качестве назначения дампа базы данных или журнала.

#### <a name="performance-considerations-for-backupsrestores"></a>Рекомендации по ускорению резервного копирования и восстановления
Как и при развертывании на сервер без операционной системы, скорость резервного копирования и восстановления зависит от количества томов, которые могут считываться параллельно, а также от пропускной способности этих томов. Имейте в виду, что для сжатия резервных копий используются ресурсы ЦП. Загрузка ЦП при сжатии резервной копии может оказывать заметное влияние на работу виртуальных машин с небольшим числом потоков ЦП. Исходя из сказанного выше, можно сделать следующие выводы.

* Чем меньше дисков используется для хранения устройств базы данных, тем меньше общая пропускная способность при чтении.
* Чем меньше потоков ЦП на виртуальной машине, тем сильнее влияние сжатия резервной копии.
* Чем меньше целевых объектов (чередующиеся каталоги, диски), на которые записываются резервные копии, тем меньше пропускная способность.

Увеличить количество целевых объектов для записи можно двумя способами. Выбор того или иного варианта (и даже их сочетания) зависит от ваших потребностей.

* Распределить содержимое целевого тома, на котором хранятся резервные копии, между несколькими подключенными дисками, чтобы увеличить количество операций ввода-вывода в секунду на чередующемся томе.
* Создать конфигурацию дампов на уровне SAP ASE, в которой дампы записываются в несколько целевых каталогов.

Сведения о распределении тома диска между несколькими дисками см. в разделе [Вопросы развертывания СУБД для рабочей нагрузки SAP на виртуальных машинах Azure](dbms_guide_general.md). Дополнительные сведения об использовании нескольких каталогов в конфигурации дампов в SAP ASE см. в документации по хранимой процедуре sp_config_dump, которая используется для создания конфигурации дампов, в [справочном центре Sybase](http://infocenter.sybase.com/help/index.jsp).

### <a name="disaster-recovery-with-azure-vms"></a>Аварийное восстановление с помощью виртуальных машин Azure
#### <a name="data-replication-with-sap-sybase-replication-server"></a>Репликация данных с помощью сервера репликации SAP Sybase
Сервер репликации SAP Sybase (SRS) в SAP ASE — это "теплый" резервный сервер, используемый для асинхронной передачи транзакций базы данных в удаленное расположение. 

Установка и эксплуатация сервера SRS на виртуальной машине, размещенной в службах виртуальных машин Azure, функционально ничем не отличается от аналогичных операций в локальной среде.

Для решения SAP ASE HADR не требуется внутренней подсистемы балансировки нагрузки Azure. Это решение не зависит от кластеризации на уровне ОС. Оно работает на виртуальных машинах Azure в Windows и Linux. Дополнительные сведения о SAP ASE HADR см. в [этом руководстве пользователей](https://help.sap.com/viewer/efe56ad3cad0467d837c8ff1ac6ba75c/16.0.3.3/en-US/a6645e28bc2b1014b54b8815a64b87ba.html).

## <a name="specifics-to-sap-ase-on-linux"></a>Особенности SAP ASE на платформе Linux
Начиная с Microsoft Azure, можно легко переносить существующие приложения SAP ASE на виртуальные машины Azure. SAP ASE на виртуальной машине позволяет снизить общую стоимость владения для развертывания и обслуживания приложений уровня организации, а также управления ими благодаря переносу этих приложений в Microsoft Azure. С помощью SAP ASE на виртуальной машине Azure администраторы и разработчики могут продолжать использовать те же средства разработки и администрирования, которые доступны в локальной среде.

Для развертывания виртуальных машин Azure важно учитывать официальные соглашения об уровне обслуживания, которые можно найти по ссылке: <https://azure.microsoft.com/support/legal/sla>.

Сведения о размерах SAP и перечень сертифицированных SAP SKU виртуальных машин предоставлены в примечании к SAP [1928533]. Дополнительные документы о размерах SAP для виртуальных машин Azure можно найти здесь: <https://blogs.msdn.com/b/saponsqlserver/archive/2015/06/19/how-to-size-sap-systems-running-on-azure-vms.aspx> и <https://blogs.msdn.com/b/saponsqlserver/archive/2015/12/01/new-white-paper-on-sizing-sap-solutions-on-azure-public-cloud.aspx>.

Инструкции и рекомендации по использованию службы хранилища Azure, развертыванию виртуальных машин SAP или мониторингу SAP применяются к развертываниям SAP ASE в сочетании с приложениями SAP, как указано в первых четырех разделах этого документа.

Общие сведения об ASE на Linux и ASE в облаке содержатся в следующих двух примечаниях SAP:

* [2134316]
* [1941500]

### <a name="sap-ase-version-support"></a>Поддержка версий SAP ASE
SAP в настоящее время поддерживает версию SAP ASE 16.0 для использования с продуктами SAP Business Suite. Все обновления для сервера SAP ASE, а также драйверы JDBC и ODBC для продуктов SAP Business Suite доступны только в SAP Service Marketplace по адресу: <https://support.sap.com/swdc>.

Для установки в локальной среде не следует загружать обновления для сервера SAP ASE или драйверы JDBC и ODBC непосредственно с веб-сайтов Sybase. Подробные сведения об исправлениях, которые поддерживаются для использования с продуктами SAP Business Suite в локальной среде и на виртуальных машинах Azure, см. в следующих примечаниях SAP:

* [1590719]
* [1973241]

Общие сведения о запуске SAP Business Suite в SAP ASE можно найти на сайте [SCN](https://www.sap.com/community/topic/ase.html)

### <a name="sap-ase-configuration-guidelines-for-sap-related-sap-ase-installations-in-azure-vms"></a>Рекомендации по конфигурации SAP ASE для связанных с SAP установок SAP ASE на виртуальных машинах Azure
#### <a name="structure-of-the-sap-ase-deployment"></a>Структура развертывания SAP ASE
Исполняемые файлы SAP ASE должны быть расположены или установлены в корневой файловой системе виртуальной машины ( /sybase ). Как правило, большинство баз данных системы и средств SAP ASE не испытывают больших рабочих нагрузок. Поэтому базы данных системы и средств (master, model, saptools, sybmgmtdb, sybsystemdb) могут храниться в корневой файловой системе. 

Исключением может быть временная база данных, для которой в случае некоторых рабочих нагрузок SAP ERP и любых рабочих нагрузок BW может потребоваться больший объем данных или объем операций ввода-вывода. Этот объем или число операций ввода-вывода в секунду не могут быть предоставлены с помощью диска ОС виртуальной машины. 

В зависимости от версии SAPInst или SWPM, использованной для установки системы, база данных может содержать следующие компоненты.

* Отдельную базу tempdb SAP ASE, которая создается при установке SAP ASE.
* Базу tempdb SAP ASE, созданную при установке SAP ASE, и дополнительную базу saptempdb, созданную процедурой установки SAP.
* Базу данных tempdb SAP ASE, созданную при установке SAP ASE, и дополнительную базу данных tempdb, созданную вручную (например, согласно примечанию к SAP [1752266]) для соответствия особым требованиям tempdb к ERP/BW.

По причинам, связанным с производительностью, устройства tempdb для определенных рабочих нагрузок ERP или всех рабочих нагрузок BW можно размещать в дополнительно созданной (с помощью SWPM или вручную) базе данных tempdb в отдельной файловой системе, которая может быть представлена одним диском данных Azure или RAID-массивом Linux, включающим несколько дисков данных Azure. При отсутствии дополнительной базы данных tempdb рекомендуется ее создать (примечание к SAP [1752266]).

Для таких систем необходимо выполнить следующие действия над дополнительно созданной базой данных tempdb.

* Переместить первый каталог базы данных tempdb в первую файловую систему базы данных SAP.
* Добавить каталоги tempdb на каждый диск, содержащий файловую систему базы данных SAP.

Эта конфигурация позволяет tempdb использовать больше пространства, чем может предоставить системный диск. Для справки можно проверить размеры устройств tempdb в существующих системах, работающих локально. Такая конфигурация также позволяет увеличить число операций ввода-вывода в секунду для базы данных tempdb, которое не может быть обеспечено при использовании системного диска. Системы, работающие в локальной среде, можно использовать для мониторинга рабочей нагрузки ввода-вывода на базу данных tempdb.

Никогда не помещайте каталоги SAP ASE в каталог /mnt или /mnt/resource виртуальной машины. Для SAP ASE это правило также применяется к tempdb, даже если содержащиеся в tempdb объекты являются временными. Это связано с тем, что каталог /mnt или /mnt/resource по умолчанию используется в качестве временного каталога виртуальных машин Azure и не является постоянным. Дополнительные сведения о временном пространстве виртуальной машины Azure можно найти в [этой статье][virtual-machines-linux-how-to-attach-disk].

Инструкции и рекомендации по развертыванию файлов данных и файлов журналов транзакций приведены в разделе [Вопросы развертывания СУБД для рабочей нагрузки SAP на виртуальных машинах Azure](dbms_guide_general.md). Для развертываний на основе Linux рекомендуется использовать LVM или MDADM для создания чередующихся наборов с достаточно большим числом операций ввода-вывода в секунду, высокой пропускной способностью и достаточным объемом. 

#### <a name="impact-of-database-compression"></a>Влияние сжатия базы данных
В конфигурациях, в которых пропускная способность ввода-вывода может стать ограничивающим фактором, каждая мера, позволяющая сократить объем операций ввода-вывода, помогает растянуть рабочую нагрузку, которую можно запускать в сценарии IaaS, например Azure. Таким образом, рекомендуем обязательно использовать сжатие SAP ASE перед отправкой имеющейся базы данных SAP в Azure.

Использовать сжатие перед отправкой в Azure рекомендуется по нескольким причинам:

* Объем отправляемых в Azure данных уменьшается.
* Длительность выполнения сжатия короче, при условии, что локально можно использовать более мощное оборудование с большим числом ЦП, более высокой пропускной способностью ввода-вывода или меньшими задержками ввода-вывода.
* Уменьшение размеров баз данных может сократить затраты на выделение места на дисках.

Сжатие данных и LOB работает на виртуальных машинах Azure так же, как и в локальной среде. Дополнительные сведения о том, как проверить, используется ли уже сжатие в существующей базе данных SAP ASE, см. в примечании к SAP [1750510]. Дополнительные сведения о сжатии баз данных см. в примечании к SAP [2121797].

#### <a name="using-dbacockpit-to-monitor-database-instances"></a>Использование DBACockpit для мониторинга экземпляров баз данных
Для систем SAP, в которых используется SAP ASE в качестве платформы баз данных, средство DBACockpit доступно в виде встроенных окон браузера в DBACockpit для транзакции или в виде Webdynpro. Однако полный набор функций для мониторинга и администрирования базы данных доступен только в реализации Webdynpro средства DBACockpit.

Как и для локальных систем, требуется выполнить ряд действий, чтобы включить все функциональные возможности SAP NetWeaver, используемые в реализации Webdynpro средства DBACockpit. Ознакомьтесь с информацией в примечании к SAP [1245200] , чтобы включить использование инструментов Webdynpro и создать те из них, которые необходимы. При соблюдении инструкций в упомянутых выше примечаниях будет также выполнена настройка Internet Communication Manager (icm) вместе с портами, используемыми для подключений http и https. Настройка по умолчанию для http выглядит следующим образом:

> icm/server_port_0 = PROT=HTTP,PORT=8000,PROCTIMEOUT=600,TIMEOUT=600
> 
> icm/server_port_1 = PROT=HTTPS,PORT=443$$,PROCTIMEOUT=600,TIMEOUT=600
> 
> 

Ссылки, создаваемые в DBACockpit для транзакции, будут выглядеть примерно так:

> https:\//\<fullyqualifiedhostname >: 44300 во/sap/bc/webdynpro/sap/dba_cockpit
> 
> http:\//\<fullyqualifiedhostname >: 8000/sap/bc/webdynpro/sap/dba_cockpit
> 
> 

В зависимости от того, как виртуальная машина Azure, на которой размещается система SAP, подключена к AD и DNS, необходимо убедиться в том, что ICM использует полное имя узла, которое может быть разрешено на компьютере, с которого вы открываете DBACockpit. Чтобы узнать, как ICM определяет полное имя узла в зависимости от параметров профиля, а также явным образом задать параметр icm/host_name_full (если необходимо), ознакомьтесь с примечанием к SAP [773830].

Если вы развернули виртуальную машину только в облачной среде (без подключения между локальной средой и Azure), вам необходимо определить общедоступный IP-адрес и метку домена. Формат общедоступного DNS-имени виртуальной машины будет выглядеть так:

> `<custom domainlabel`>.`<azure region`>.cloudapp.azure.com
> 
> 

Дополнительную информацию о DNS-имени см. [здесь][virtual-machines-azurerm-versus-azuresm].

Если для параметра профиля SAP icm/host_name_full задать DNS-имя виртуальной машины Azure, ссылка может выглядеть так:

> https:\//mydomainlabel.westeurope.cloudapp.net:44300/sap/bc/webdynpro/sap/dba_cockpit
> 
> http:\//mydomainlabel.westeurope.cloudapp.net:8000/sap/bc/webdynpro/sap/dba_cockpit

В таком случае необходимо:

* добавить правила для входящего трафика в группу безопасности сети на портале Azure для портов TCP/IP, используемых для связи с ICM;
* добавить правила для входящего трафика в конфигурацию брандмауэра Windows для портов TCP/IP, используемых для связи с ICM.

Для автоматического импорта всех доступных исправлений рекомендуется периодически применять примечания SAP, содержащие наборы исправлений, которые относятся к вашей версии SAP:

* [1558958;]
* [1619967]
* [1882376.]

Дополнительные сведения о панели DBA для SAP ASE можно найти в следующих примечаниях SAP:

* [1605680;]
* [1757924]
* [1757928]
* [1758182]
* [1758496]    
* [1814258]
* [1922555]
* [1956005.]

#### <a name="backuprecovery-considerations-for-sap-ase"></a>Рекомендации по резервному копированию и восстановлению для SAP ASE
При развертывании SAP ASE в Azure методику резервного копирования необходимо пересмотреть. Даже в средах, не являющихся рабочими, необходимо периодически проводить резервное копирование баз данных SAP. Так как в службе хранилища Azure хранятся три образа, резервное копирование может считаться менее важным для восстановления хранилища после сбоя. Основная причина соблюдения надлежащего плана резервного копирования и восстановления следующая. Благодаря возможности восстанавливать состояние на определенный момент времени вы можете исправлять логические и пользовательские ошибки. Следовательно, резервные копии используются для восстановления состояния базы данных на определенный момент времени или для создания другой системы в Azure путем копирования существующей базы данных. 

Резервное копирование и восстановление базы данных в Azure работает так же, как и в локальных системах. Ознакомьтесь с примечаниями к SAP:

* [1588316]
* [1585981.]

В них вы найдете дополнительные сведения о создании конфигураций дампов и планировании резервного копирования. В зависимости от стратегии и потребностей можно настроить запись дампов базы данных и журнала на диск на одном из существующих дисков или добавить дополнительный диск для резервных копий. Чтобы снизить риск потери данных в случае возникновения ошибки, рекомендуем использовать диск, на котором нет каталога или файлов базы данных.

Помимо сжатия данных и больших объектов система SAP ASE также предлагает сжатие резервных копий. Чтобы дампы базы данных и журнала занимали меньше места, рекомендуется использовать сжатие резервных копий. Дополнительные сведения см. в примечании SAP [1588316]. Сжатие резервных копий также позволяет существенно уменьшить объем данных при передаче. Это актуально, если вы планируете скачивать резервные копии или содержимое виртуальных дисков с дампами резервных копий с виртуальных машин Azure в локальную систему.

Не сохраняйте дампы базы данных или журналов во временные пространства виртуальной машины Azure /mnt и /mnt/resource.

#### <a name="performance-considerations-for-backupsrestores"></a>Рекомендации по ускорению резервного копирования и восстановления
Как и при развертывании на сервер без операционной системы, скорость резервного копирования и восстановления зависит от количества томов, которые могут считываться параллельно, а также от пропускной способности этих томов. Имейте в виду, что для сжатия резервных копий используются ресурсы ЦП. Загрузка ЦП при сжатии резервной копии может оказывать заметное влияние на работу виртуальных машин с небольшим числом потоков ЦП.  Исходя из сказанного выше, можно сделать следующие выводы.

* Чем меньше дисков используется для хранения устройств базы данных, тем меньше общая пропускная способность при чтении.
* Чем меньше потоков ЦП на виртуальной машине, тем сильнее влияние сжатия резервной копии.
* Чем меньше целевых объектов (программные RAID-массивы Linux, диски), на которые записываются резервные копии, тем меньше пропускная способность.

Увеличить количество целевых объектов для записи можно двумя способами. Выбор того или иного варианта (и даже их сочетания) зависит от ваших потребностей.

* Распределить содержимое целевого тома, на котором хранятся резервные копии, между несколькими подключенными дисками, чтобы увеличить количество операций ввода-вывода в секунду на чередующемся томе.
* Создать конфигурацию дампов на уровне SAP ASE, в которой дампы записываются в несколько целевых каталогов.

Сведения о распределении тома диска между несколькими дисками см. в разделе [Вопросы развертывания СУБД для рабочей нагрузки SAP на виртуальных машинах Azure](dbms_guide_general.md). Дополнительные сведения об использовании нескольких каталогов в конфигурации дампов в SAP ASE см. в документации по хранимой процедуре sp_config_dump, которая используется для создания конфигурации дампов, в [справочном центре Sybase](http://infocenter.sybase.com/help/index.jsp).

### <a name="disaster-recovery-with-azure-vms"></a>Аварийное восстановление с помощью виртуальных машин Azure
#### <a name="data-replication-with-sap-sybase-replication-server"></a>Репликация данных с помощью сервера репликации SAP Sybase
Сервер репликации SAP Sybase (SRS) в SAP ASE — это "теплый" резервный сервер, используемый для асинхронной передачи транзакций базы данных в удаленное расположение. 

Установка и эксплуатация сервера SRS на виртуальной машине, размещенной в службах виртуальных машин Azure, функционально ничем не отличается от аналогичных операций в локальной среде.

Поддерживается использование ASE HADR через сервер репликации SAP. Для подобной настройки настоятельно рекомендуется использовать SAP ASE 16.03. Подробные инструкции по установке таких конфигураций можно найти в [этой](https://blogs.msdn.microsoft.com/saponsqlserver/2018/06/18/installation-procedure-for-sybase-16-3-patch-level-3-always-on-dr-on-suse-12-3-recent-customer-proof-of-concept/) статье блога.
