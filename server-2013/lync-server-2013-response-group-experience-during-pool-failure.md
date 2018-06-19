﻿---
title: 池故障期间的 Lync Server 2013 响应组体验
TOCTitle: 池故障期间的响应组体验
ms:assetid: 4e00fb38-64b1-4fd9-903d-7639177bc303
ms:mtpsurl: https://technet.microsoft.com/zh-cn/library/JJ204886(v=OCS.15)
ms:contentKeyID: 49312801
ms.date: 05/19/2016
mtps_version: v=OCS.15
ms.translationtype: HT
---

# 池故障期间 Lync Server 2013 中的响应组体验

 

_**上一次修改主题：** 2015-03-09_

本节详细描述了在以下各个阶段响应组活动如何受到影响：

  - 主要池发生中断，但尚未启动故障转移。

  - 服务故障转移到备份池。

  - 服务故障回复到主要池。

## 发生中断时的用户体验

当池或网站发生中断，但管理员尚未启动故障转移时，按下表所述处理响应组活动。

<table>
<thead>
<tr class="header">
<th><img src="images/Dn783119.note(OCS.15).gif" title="note" alt="note" />注意：</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>在灾难恢复期间，根据恢复期间主要池响应组是否已导入备份池，呼叫行为会有所不同。在下表中，对导入的响应组的引用意味着主要池响应组在灾难恢复模式期间已导入到该备份池。</td>
</tr>
</tbody>
</table>


### 发生中断

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>呼叫或用户操作类型</th>
<th>中断期间</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>连接到代理的呼叫</p></td>
<td><ul>
<li><p>常规呼叫保持连接。</p></li>
<li><p>匿名呼叫将中断。</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p>正在进行但尚未连接到代理的呼叫</p></td>
<td><p>呼叫将中断。</p></td>
</tr>
<tr class="odd">
<td><p>新呼叫</p></td>
<td><ul>
<li><p>呼叫将中断。</p></li>
<li><p>如果已导入响应组，则呼叫将连接到备份池，但驻留在主要池中的代理将无法访问。</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p>代理代表响应组进行呼叫</p></td>
<td><p>此阶段功能被禁用。</p></td>
</tr>
<tr class="odd">
<td><p>代理登录和代理信息</p></td>
<td><ul>
<li><p>可以在代理控制台上看到主要池拥有的代理组，但是代理无法登录。</p></li>
<li><p>可以在代理控制台上看到备份池拥有的代理组，而且代理可以登录。</p></li>
<li><p>代理控制台上未显示已导入的代理组。</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p>响应组配置</p></td>
<td><ul>
<li><p>根据主要池的后端数据库的可用性，可以查看由主要池拥有的响应组，但无法对其进行修改。</p></li>
<li><p>可以查看和修改备份池拥有的响应组。</p></li>
<li><p>已导入的响应组无法使用 Lync Server 控制面板或 响应组配置工具进行查看，但可使用 Lync Server 命令行管理程序 cmdlet 进行配置。</p></li>
</ul></td>
</tr>
</tbody>
</table>


## 故障转移期间的用户体验

当管理员调用至备份池的故障转移时，将在故障转移期间和故障转移之后按下表中所述处理响应组活动。第一列介绍可能发生的活动的类型。中间一列介绍在故障转移到备份池的很短的时间内如何处理每个活动。最后一列介绍在故障转移期间及该过程完成后如何处理活动，备份池将驻留在主池中。

<table>
<thead>
<tr class="header">
<th><img src="images/Dn783119.note(OCS.15).gif" title="note" alt="note" />注意：</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>在灾难恢复期间，根据恢复期间主要池响应组是否已导入备份池，呼叫行为会有所不同。在下表中，对导入的响应组的引用意味着主要池响应组在灾难恢复模式期间已导入到该备份池。</td>
</tr>
</tbody>
</table>


### 启动故障转移

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>呼叫或用户操作类型</th>
<th>故障转移期间</th>
<th>故障转移完成后</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>连接到代理的呼叫</p></td>
<td><ul>
<li><p>常规呼叫保持连接。</p></li>
<li><p>匿名呼叫将中断。</p></li>
</ul></td>
<td><ul>
<li><p>常规呼叫保持连接。</p></li>
<li><p>对于已导入的响应组，已到达备份池的匿名呼叫保持连接状态。</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p>正在进行但尚未连接到代理的呼叫</p></td>
<td><p>呼叫将中断。</p></td>
<td><ul>
<li><p>如果未导入响应组，则没有任何呼叫处于此状态。</p></li>
<li><p>对于已导入的响应组，已到达备份池的呼叫保持连接状态。</p></li>
</ul></td>
</tr>
<tr class="odd">
<td><p>新呼叫</p></td>
<td><ul>
<li><p>呼叫将中断。</p></li>
<li><p>对于已导入的响应组，呼叫将连接到备份池，但驻留在主要池中的代理将无法访问。</p></li>
</ul></td>
<td><ul>
<li><p>如果无法导入响应组，则将断开呼叫。</p></li>
<li><p>对于已导入的响应组，呼叫将连接到备份池。</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p>代理代表响应组进行呼叫</p></td>
<td><p>此阶段功能被禁用。</p></td>
<td><ul>
<li><p>如果未导入响应组，则呼叫失败。</p></li>
<li><p>对于已导入的响应组，呼叫成功。</p></li>
</ul></td>
</tr>
<tr class="odd">
<td><p>代理登录和代理信息</p></td>
<td><ul>
<li><p>可以在代理控制台上看到主要池拥有的代理组，但是代理无法登录。</p></li>
<li><p>可以在代理控制台上看到备份池拥有的代理组，而且代理可以登录。</p></li>
<li><p>在代理控制台上显示已导入的代理组，而且代理可以登录。</p></li>
</ul></td>
<td><ul>
<li><p>可以在代理控制台上看到主要池拥有的代理组，但是代理无法登录。</p></li>
<li><p>可以在代理控制台上看到备份池拥有的代理组，而且代理可以登录。</p></li>
<li><p>在代理控制台上显示已导入的代理组，而且代理可以登录。</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p>响应组配置</p></td>
<td><ul>
<li><p>根据主要池的后端数据库的可用性，可以查看由主要池拥有的响应组，但无法对其进行修改。</p></li>
<li><p>可以查看和修改备份池拥有的响应组。</p></li>
<li><p>已导入的响应组无法使用 Lync Server 控制面板或 响应组配置工具进行查看，但可使用 Lync Server 命令行管理程序 cmdlet 进行配置。</p></li>
</ul></td>
<td><ul>
<li><p>根据后端数据库的可用性，可以查看由主要池拥有的响应组，但无法对其进行修改。</p></li>
<li><p>可以查看和修改备份池拥有的响应组。</p></li>
<li><p>已导入的响应组无法使用 Lync Server 控制面板或 响应组配置工具进行查看，但可使用 Lync Server 命令行管理程序 cmdlet 进行配置。</p></li>
</ul></td>
</tr>
</tbody>
</table>


## 故障回复期间的用户体验

在管理员对主要池调用故障回复时，按下表所述在故障回复期间以及故障回复之后对响应组活动进行处理。

<table>
<thead>
<tr class="header">
<th><img src="images/Dn783119.note(OCS.15).gif" title="note" alt="note" />注意：</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>在灾难恢复期间，根据恢复期间主要池响应组是否已导入备份池，呼叫行为会有所不同。在下表中，对导入的响应组的引用意味着主要池响应组在灾难恢复模式期间已导入到该备份池。</td>
</tr>
</tbody>
</table>


### 故障回复中的呼叫处理

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>呼叫或用户操作类型</th>
<th>故障回复期间</th>
<th>故障回复完成后</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>连接到代理的呼叫</p></td>
<td><ul>
<li><p>常规呼叫保持连接。</p></li>
<li><p>如果未导入响应组，则没有任何匿名呼叫处于此状态。</p></li>
<li><p>对于已导入的响应组，匿名呼叫保持连接。</p></li>
</ul></td>
<td><ul>
<li><p>常规呼叫保持连接。</p></li>
<li><p>如果未导入响应组，则没有任何匿名呼叫处于此状态。</p></li>
<li><p>对于已导入的响应组，匿名呼叫保持连接。</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p>正在进行但尚未连接到代理的呼叫</p></td>
<td><ul>
<li><p>如果未导入响应组，则没有任何呼叫处于此状态。</p></li>
<li><p>对于已导入的响应组，呼叫将中断。</p></li>
</ul></td>
<td><ul>
<li><p>如果未导入响应组，则没有任何呼叫处于此状态。</p></li>
<li><p>对于已导入的响应组，呼叫将中断。</p></li>
</ul></td>
</tr>
<tr class="odd">
<td><p>新呼叫</p></td>
<td><p>呼叫连接到主要池，但驻留在主要池中的代理将无法访问。</p></td>
<td><p>呼叫连接到主要池。</p></td>
</tr>
<tr class="even">
<td><p>代理代表响应组进行呼叫</p></td>
<td><p>此阶段功能被禁用。</p></td>
<td><p>呼叫成功。</p></td>
</tr>
<tr class="odd">
<td><p>代理登录和代理信息</p></td>
<td><ul>
<li><p>可以在代理控制台上看到主要池拥有的代理组，但是代理无法登录。</p></li>
<li><p>可以在代理控制台上看到备份池拥有的代理组，而且代理可以登录。</p></li>
<li><p>在代理控制台上显示已导入的代理组，而且代理可以登录。</p></li>
</ul></td>
<td><ul>
<li><p>可以在代理控制台上看到主要池拥有的代理组，而且代理可以登录。</p></li>
<li><p>可以在代理控制台上看到备份池拥有的代理组，而且代理可以登录。</p></li>
<li><p>代理控制台上未显示已导入的代理组。</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p>响应组配置</p></td>
<td><ul>
<li><p>根据主要池的后端数据库的可用性，可以查看由主要池拥有的响应组，但无法对其进行修改。</p></li>
<li><p>可以查看和修改备份池拥有的响应组。</p></li>
<li><p>已导入的响应组无法使用 Lync Server 控制面板或 响应组配置工具进行查看，但可使用 Lync Server 命令行管理程序 cmdlet 进行配置。</p></li>
</ul></td>
<td><ul>
<li><p>可以查看和修改主要池拥有的响应组。</p></li>
<li><p>可以查看和修改备份池拥有的响应组。</p></li>
<li><p>已导入的响应组无法使用 Lync Server 控制面板或 响应组配置工具进行查看，但可使用 Lync Server 命令行管理程序 cmdlet 进行配置。</p></li>
</ul></td>
</tr>
</tbody>
</table>
