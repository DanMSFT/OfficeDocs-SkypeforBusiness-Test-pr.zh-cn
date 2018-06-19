﻿---
title: 使用最佳做法分析器扫描您部署中的潜在问题
TOCTitle: 使用最佳做法分析器扫描您部署中的潜在问题
ms:assetid: 09c84509-dc91-4e7b-882b-3c467b6b026d
ms:mtpsurl: https://technet.microsoft.com/zh-cn/library/Gg591343(v=OCS.15)
ms:contentKeyID: 49311938
ms.date: 05/19/2016
mtps_version: v=OCS.15
ms.translationtype: HT
---

# 使用最佳做法分析器扫描您部署中的潜在问题

 

_**上一次修改主题：** 2012-10-21_

要运行最佳做法分析器扫描，您必须指定以下项：

  - **凭据**   要运行扫描，必须使用属于本地 Administrators 组成员的帐户登录安装有最佳做法分析器的计算机。此外，您还需要使用具有运行适当扫描所需的用户权限的用户帐户登录，否则在运行最佳做法分析器时，必须指定具有适当用户权限的凭据。有关详细信息，请参阅[最佳做法分析器的组成员身份和用户权限要求](lync-server-2013-group-memberships-and-user-rights-requirements-for-best-practices-analyzer.md)。

  - **扫描范围**   若要指定扫描的范围，请选择要扫描的类别和服务器。可以选择您的 Lync Server 环境中的所有类别、一个或多个类别或者某个特定类别中的一个或多个服务器。

  - **扫描类型**   状况检查扫描是目前唯一可用的扫描类型（默认情况下选中）。状况检查扫描生成的报告中包含有关范围中指定的所有服务器的错误、警告和其他信息。

  - **网络速度**   网络速度选项包括快速局域网（100 Mbps 或更快）、局域网 (10 Mbps)、快速广域网 (1.5 Mbps) 或广域网 (64 kbps)。估计完成扫描的时间基于此设置。此设置还用于设置超时期限。扫描期间，最佳做法分析器在指定时间内等待来自服务器的响应。如果没有在指定超时期限内收到响应，它将移到扫描中的下一个服务器。在速度较慢的网络上，考虑到网络延迟更长，所以此指定超时期限更长。我们建议您为此参数选择拓扑中最慢的链接，以便该工具不会超时太快。

## 扫描 Lync Server 2013 部署

1.  使用属于本地 Administrators 组成员并拥有其他所需用户权限的帐户登录安装有最佳做法分析器的计算机。

2.  单击“开始”，指向“所有程序”，单击“Microsoft Lync Server 2013”，然后单击“最佳做法分析器”。

3.  在“欢迎”屏幕上，单击“为新扫描选择选项”。

4.  在“连接到 Active Directory”页面上，验证在 **Active Directory 服务器**中指定的名称，然后执行下列操作之一：
    
      - 若要使用您用来登录计算机的凭据运行扫描，请单击“连接到 Active Directory 服务器”。
    
      - 若要指定要用于 Active Directory 域服务、边缘服务器或 Exchange Server 的不同凭据，请单击“显示高级登录选项”，选中需要单独凭据的每个复选框，为每个选中的复选框指定凭据，然后单击“连接到 Active Directory 服务器”。
    
    <table>
    <thead>
    <tr class="header">
    <th><img src="images/Dn783119.note(OCS.15).gif" title="note" alt="note" />注意：</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>在开始扫描之前，最佳做法分析器会执行网络和权限检查，以确保指定帐户凭据有效并且最佳做法分析器可以连接到 Active Directory 域服务。如果该工具在工作组服务器上运行，则该工具还验证它能否连接到外围网络中的边缘服务器（即，它们是否包括在扫描中）。</td>
    </tr>
    </tbody>
    </table>


5.  在“启动新的最佳实践扫描”页面上，选择您要包括在扫描中的选项，指定网络速度，然后单击“开始扫描”。

6.  在“扫描已完成”页面上，单击“查看该最佳实践扫描的报告”。

7.  在“查看最佳实践报告”页面上，执行下列操作之一：
    
      - 若要查看按服务器组件组织的列表中的报告，请单击“列表报告”，然后单击“所有问题”选项卡或“信息项”选项卡。
    
      - 若要以按照结果类型组织的分层列表形式查看报告，请单击“目录树报告”, 然后单击“详细视图”选项卡或“摘要视图”选项卡。
    
      - 若要查看其他报告，请单击“其他报告”。
    
    <table>
    <thead>
    <tr class="header">
    <th><img src="images/Dn783119.note(OCS.15).gif" title="note" alt="note" />注意：</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>有关最佳做法分析器报告以及它们所标识的问题的详细信息，请参阅<a href="lync-server-2013-viewing-and-working-with-reports-created-by-best-practices-analyzer.md">查看和使用由最佳做法分析器创建的报告</a>和<a href="lync-server-2013-analyzing-and-resolving-issues-identified-by-best-practices-analyzer.md">分析和解决由最佳做法分析器标识的问题</a>。</td>
    </tr>
    </tbody>
    </table>
