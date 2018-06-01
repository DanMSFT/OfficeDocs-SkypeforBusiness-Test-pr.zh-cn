﻿---
title: Lync Server 2013：发布更新的拓扑
TOCTitle: 发布更新的拓扑
ms:assetid: 59455dd1-6a9e-433f-a714-d3636c068100
ms:mtpsurl: https://technet.microsoft.com/zh-cn/library/JJ204910(v=OCS.15)
ms:contentKeyID: 49312939
ms.date: 05/19/2016
mtps_version: v=OCS.15
ms.translationtype: HT
---

# 在 Lync Server 2013 中发布更新的拓扑

 

_**上一次修改主题：** 2012-10-01_

更新了 拓扑生成器中的拓扑之后，必须在配置和使用 持久聊天服务器之前先将拓扑发布至 中央管理存储。数据的只读副本将复制到拓扑中的所有服务器，使所有服务器与拓扑和其他配置更改保持同步。

## 发布更新后的拓扑

在发布拓扑之前，请安装持久聊天服务器的数据库。使用拓扑生成器，通过选择“操作”和“安装数据库”来安装数据库。

1.  在运行 Lync Server 2013 或安装了 Lync Server 管理工具的计算机上，使用同时具有 **Domain Admins** 组成员身份和 **RTCUniversalServerAdmins** 组成员身份的帐户登录，且该帐户对要用于 持久聊天服务器文件存储的文件存储具有完全控制权限（即读取、写入和修改）（以使 拓扑生成器可以配置所需的随机访问控制列表 (DACL)），或者使用具有等效用户权限的帐户登录。

2.  启动拓扑生成器。选择“从现有部署下载拓扑”或“从本地文件打开拓扑”（如果在本地保存它）。

3.  在控制台树中，右键单击“Lync Server 2013”，然后单击“发布拓扑”。

4.  在“发布拓扑”页上，单击“下一步”。

5.  在“发布向导完成”页上，确认已成功发布拓扑，然后单击“完成”。
    
    <table>
    <thead>
    <tr class="header">
    <th><img src="images/Gg398794.important(OCS.15).gif" title="important" alt="important" />重要提示：</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>发布拓扑后，必须先配置 持久聊天服务器支持，然后才能存档内容。</td>
    </tr>
    </tbody>
    </table>
