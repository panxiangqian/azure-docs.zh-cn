---
title: "Azure Stack 中的诊断"
description: "如何收集诊断 Azure 堆栈中的日志文件"
services: azure-stack
author: jeffgilb
manager: femila
cloud: azure-stack
ms.service: azure-stack
ms.topic: article
ms.date: 12/15/2017
ms.author: jeffgilb
ms.reviewer: adshar
ms.openlocfilehash: fdbf9b1b77c2c64b3ebfcdbc5463916f317e4881
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/16/2017
---
# <a name="azure-stack-diagnostics-tools"></a>Azure 堆栈诊断工具

*适用范围： Azure 堆栈集成系统和 Azure 堆栈开发工具包*
 
Azure 堆栈为协同工作，并且彼此交互的组件的大型集合。 所有这些组件都会生成其自己唯一的日志。 这会使诊断问题具有挑战性的任务，尤其是对于来自多个交互 Azure 堆栈组件的错误。 

我们的诊断工具帮助确保日志回收机制很容易也很有效。 下图显示如何在 Azure 堆栈工作中日志收集工具：

![Azure 堆栈诊断工具](media/azure-stack-diagnostics/get-azslogs.png)
 
 
## <a name="trace-collector"></a>跟踪收集器
 
跟踪收集器默认启用的并在后台以从 Azure 堆栈组件服务收集所有事件跟踪的 Windows (ETW) 日志持续运行。 ETW 日志存储在具有五个天期限限制的常见本地共享。 一旦达到此限制，如创建新的被删除最旧的文件。 每个文件允许的默认最大大小为 200 MB。 大小检查发生每 2 分钟，并且当前文件是 > = 200 MB 时，将其保存并生成一个新文件。 生成每个事件会话的总文件大小上也没有 8 GB 的限制。 

## <a name="log-collection-tool"></a>日志收集工具
 
PowerShell cmdlet **Get AzureStackLog**可以用于从 Azure 堆栈环境中的所有组件中收集日志。 它将它们保存在用户定义的位置中的 zip 文件中。 如果 Azure 堆栈技术支持团队需要你日志来帮助解决问题，它们可能会要求你运行此工具。

> [!CAUTION]
> 这些日志文件可能包含个人身份信息 (PII)。 考虑到这种之前你公开发布的任何日志文件。
 
收集某些示例日志类型如下：
*   **Azure 堆栈部署日志**
*   **Windows 事件日志**
*   **Panther 日志**
*   **群集日志**
*   **存储诊断日志**
*   **ETW 日志**

这些文件收集，并在共享中保存的跟踪收集器。 **Get AzureStackLog**然后可以使用 PowerShell cmdlet 收集它们在必要时。
 
### <a name="to-run-get-azurestacklog-on-an-azure-stack-development-kit-asdk-system"></a>若要在 Azure 堆栈开发工具包 (ASDK) 的系统上运行 Get AzureStackLog
1. 以登录**AzureStack\CloudAdmin**主机上。
2. 以管理员身份打开 PowerShell 窗口。
3. 运行**Get AzureStackLog** PowerShell cmdlet。

**示例：**

  收集所有角色的所有日志：

  ```powershell
  Get-AzureStackLog -OutputPath C:\AzureStackLogs
  ```

  从 VirtualMachines 和 BareMetal 角色收集的日志：

  ```powershell
  Get-AzureStackLog -OutputPath C:\AzureStackLogs -FilterByRole VirtualMachines,BareMetal
  ```

  与过去 8 小时内筛选日志文件的日期，从 VirtualMachines 和 BareMetal 角色中收集日志：
    
  ```powershell
  Get-AzureStackLog -OutputPath C:\AzureStackLogs -FilterByRole VirtualMachines,BareMetal -FromDate (Get-Date).AddHours(-8)
  ```

  收集从 VirtualMachines 和 BareMetal 角色，日期为 8 小时前到 2 小时前之间的时间段筛选日志文件日志：

  ```powershell
  Get-AzureStackLog -OutputPath C:\AzureStackLogs -FilterByRole VirtualMachines,BareMetal -FromDate (Get-Date).AddHours(-8) -ToDate (Get-Date).AddHours(-2)
  ```

### <a name="to-run-get-azurestacklog-on-an-azure-stack-integrated-system"></a>若要在 Azure 堆栈上运行 Get AzureStackLog 集成系统

若要在一个集成的系统上运行日志收集工具，你需要能够访问到特权终结点 (PEP)。 下面是你可以运行使用 PEP 以在集成的系统上收集的日志的一个示例脚本：

```powershell
$ip = "<IP ADDRESS OF THE PEP VM>" # You can also use the machine name instead of IP here.
 
$pwd= ConvertTo-SecureString "<CLOUD ADMIN PASSWORD>" -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ("<DOMAIN NAME>\CloudAdmin", $pwd)
 
$shareCred = Get-Credential
 
$s = New-PSSession -ComputerName $ip -ConfigurationName PrivilegedEndpoint -Credential $cred

$fromDate = (Get-Date).AddHours(-8)
$toDate = (Get-Date).AddHours(-2)  #provide the time that includes the period for your issue
 
Invoke-Command -Session $s {    Get-AzureStackLog -OutputPath "\\<HLH MACHINE ADDRESS>\c$\logs" -OutputSharePath "<EXTERNAL SHARE ADDRESS>" -OutputShareCredential $using:shareCred  -FilterByRole Storage -FromDate $using:fromDate -ToDate $using:toDate}

if($s)
{
    Remove-PSSession $s
}
```

- 当从 PEP 收集日志时，请指定**OutputPath**参数是硬件生命周期主机 (HLH) 计算机上的位置。 另外，请确保位置进行加密。
- 参数**OutputSharePath**和**OutputShareCredential**是可选的将日志上载到外部的共享文件夹时使用。 使用这些参数*此外*到**OutputPath**。 如果**OutputPath**未指定，则日志收集工具用于存储 PEP VM 的系统驱动器。 这可能会导致脚本失败，因为驱动器空间有限制。
- 在前面的示例所示**FromDate**和**ToDate**参数可以用于在特定时间段内收集日志。 这可以上用场的各种方案，例如在集成的系统上应用的更新包后收集日志。

### <a name="parameter-considerations-for-both-asdk-and-integrated-systems"></a>参数注意事项 ASDK 和集成的系统

- 如果**FromDate**和**ToDate**不指定参数，默认情况下过去的四个小时内收集日志。
- 你可以使用**TimeOutInMinutes**参数来设置日志收集的超时。 它是默认设置为 150 （2.5 小时数）。

- 目前，你可以使用**FilterByRole**参数的以下角色的筛选器日志收集：

   |   |   |   |
   | - | - | - |
   | ACSMigrationService     | ACSMonitoringService   | ACSSettingsService |
   | ACS                     | ACSFabric              | ACSFrontEnd        |
   | ACSTableMaster          | ACSTableServer         | ACSWac             |
   | ADFS                    | ASAppGateway           | BareMetal          |
   | BRP                     | CA                     | CPI                |
   | CRP                     | DeploymentMachine      | DHCP               |
   | 域                  | ECE                    | ECESeedRing        | 
   | FabricRing              | FabricRingServices     | FRP                |
   | 网关                 | HealthMonitoring       | HRP                |   
   | IBC                     | InfraServiceController | KeyVaultAdminResourceProvider|
   | KeyVaultControlPlane    | KeyVaultDataPlane      | NC                 |   
   | NonPrivilegedAppGateway | NRP                    | SeedRing           |
   | SeedRingServices        | SLB                    | SQL                |   
   | SRP                     | 存储                | StorageController  |
   | URP                     | UsageBridge            | virtualMachines    |  
   | 已                     | WASPUBLIC              | WDS                |


### <a name="collect-logs-using-a-graphical-user-interface"></a>收集日志使用图形用户界面
而不是提供 Get AzureStackLog cmdlet 来检索 Azure 堆栈日志所需的参数，你还可以利用位于主 Azure 堆栈工具 GitHub 工具存储库在 http://aka.ms/AzureStackTools 可用的开放源代码 Azure 堆栈工具。

**ERCS_AzureStackLogs.ps1** PowerShell 脚本存储在 GitHub 工具存储库，并定期更新。 若要确保你有可用的最新版本，您应直接从 http://aka.ms/ERCS 下载它。 从管理的 PowerShell 会话中启动，此脚本连接到特权终结点，并使用提供的参数运行 Get AzureStackLog。 如果未不提供任何参数，该脚本将默认为提示输入通过图形用户界面的参数。

若要了解有关 ERCS_AzureStackLogs.ps1 PowerShell 脚本的详细信息，你可以观看[简短视频](https://www.youtube.com/watch?v=Utt7pLsXEBc)或查看脚本的[自述文件](https://github.com/Azure/AzureStack-Tools/blob/master/Support/ERCS_Logs/ReadMe.md)位于 Azure 堆栈工具 GitHub 存储库。 

### <a name="additional-considerations"></a>其他注意事项

* 该命令需要一些时间，以基于收集日志的角色运行。 相关因素还包括日志收集和 Azure 堆栈环境中的节点数目为指定的持续时间。
* 日志收集完成后，检查中创建的新文件夹**OutputPath**命令中指定的参数。
* 每个角色具有在单个 zip 文件内的其日志。 根据收集的日志的大小，一个角色可拥有其拆分为多个 zip 文件中的日志。 对于此类角色，如果你想要解压缩到的单个文件夹中的所有日志文件，使用一种工具，可以将解压缩成批 （如 7zip)。 选择用于角色中，所有的压缩的文件，然后选择**此处提取**。 这会解压缩单个合并文件夹中的角色的所有日志的文件。
* 文件调用**Get AzureStackLog_Output.log**也在包含压缩的日志文件的文件夹中创建。 此文件是命令输出中，可以用于日志收集过程中的疑难解答问题的日志。
* 若要调查特定的失败，日志可能需要从多个组件。
    -   系统和基础结构的所有 Vm 的事件日志中收集*VirtualMachines*角色。
    -   系统和所有主机的事件日志中收集*BareMetal*角色。
    -   故障转移群集和 HYPER-V 事件日志中收集*存储*角色。
    -   在收集 ACS 日志*存储*和*ACS*角色。

> [!NOTE]
> 请务必确保高效利用你的存储空间，以确保它不获取淹没日志将会根据收集的日志会强制实施大小和保留时间限制。 但是，在诊断问题时你有时需要可能不再存在由于这些限制的日志。 因此，它是**强烈建议**卸载你的日志传输到的外部存储空间 （Azure 中的存储帐户，一个其他本地存储设备等） 每隔 8 至 12 个小时，并使其存在 1-3 个月，具体取决于你要求。 此外，确保此存储位置进行加密。

## <a name="next-steps"></a>后续步骤
[Microsoft Azure Stack 故障排除](azure-stack-troubleshooting.md)
