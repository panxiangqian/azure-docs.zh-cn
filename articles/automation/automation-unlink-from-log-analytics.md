---
title: "从 Log Analytics 取消链接 Azure 自动化帐户 | Microsoft 文档"
description: "本文概述了如何从 OMS 工作区取消链接 Azure 自动化帐户。"
services: automation
documentationcenter: 
author: georgewallace
manager: carmonm
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to-article
ms.date: 09/29/2017
ms.author: magoedte
ms.openlocfilehash: 7deac9800471065d2bd5b921143b4ed08ee408da
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2017
---
# <a name="how-to-unlink-your-automation-account-from-a-log-analytics-workspace"></a>如何从 Log Analytics 工作区取消链接自动化帐户

Azure 自动化与 Log Analytics 集成，以便不仅支持对所有自动化帐户中的 Runbook 作业进行监视，而且在导入以下依赖于 Log Analytics 的解决方案时也必需：

* [更新管理](../operations-management-suite/oms-solution-update-management.md)
* [更改跟踪](../log-analytics/log-analytics-change-tracking.md)
* [在非工作时间启动/停止 VM](automation-solution-vm-management.md)
 
如果决定不再想要会自动化帐户与 Log Analytics 集成，可以直接从 Azure 门户取消链接帐户。  在继续之前，首先需要删除前面所述的解决方案，否则此过程将无法继续。  查看已导入的特定解决方案的主题，了解删除该解决方案所需的步骤。  

删除这些解决方案后，可以执行以下步骤取消链接自动化帐户。

## <a name="unlink-workspace"></a>取消链接工作区

1. 从 Azure 门户中打开自动化帐户，并在“自动化帐户”页左侧的“相关资源”部分下，选择“取消链接工作区”。<br><br> ![取消链接工作区选项](media/automation-unlink-from-log-analytics/automation-unlink-workspace-option.png)<br><br>  
2. 在“取消链接工作区”页上，单击“取消链接工作区”。<br><br> ![“取消链接工作区”页](media/automation-unlink-from-log-analytics/automation-unlink-workspace-blade.png)。<br><br>  系统会提示用户确认是否要继续。<br><br>
3. 当 Azure 自动化尝试从 Log Analytics 工作区中取消链接该帐户时，可以在菜单中的“通知”下跟踪进度。

如果使用了“更新管理”解决方案，可能会选择要删除在删除该解决方案后不再需要的以下项。

* 管理计划。  （每个计划都将具有与所创建的更新部署匹配的名称）

* 为解决方案创建的混合辅助角色组。  （每个混合辅助角色组的命名将类似于 machine1.contoso.com_9ceb8108-26c9-4051-b6b3-227600d715c8）。

如果使用了“在非工作时间启动/停止 VM”解决方案，可能会选择要删除在删除该解决方案后不再需要的以下项。

* 启动和停止 VM Runbook 计划 
* 启动和停止 VM Runbook
* 变量   

## <a name="next-steps"></a>后续步骤

要会自动化帐户重新配置为与 OMS Log Analytics 集成，请参阅[将作业状态和作业流从自动化转发到 Log Analytics (OMS)](automation-manage-send-joblogs-log-analytics.md)。 