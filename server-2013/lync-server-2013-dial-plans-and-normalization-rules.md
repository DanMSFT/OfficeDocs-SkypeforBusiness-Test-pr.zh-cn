﻿---
title: Lync Server 2013：拨号计划和规范化规则
TOCTitle: 拨号计划和规范化规则
ms:assetid: fde45195-6eb4-403c-9094-57df7fc0bd2a
ms:mtpsurl: https://technet.microsoft.com/zh-cn/library/Gg413082(v=OCS.15)
ms:contentKeyID: 49314857
ms.date: 12/10/2016
mtps_version: v=OCS.15
ms.translationtype: HT
---

# Lync Server 2013 中的拨号计划和规范化规则

 

_**上一次修改主题：** 2016-12-08_

拨号计划是一组指定的规范化规则，可将指定位置、单个用户或联系人对象的电话号码转换为统一标准 (E.164) 格式，以进行电话授权和呼叫路由。

规范化规则定义如何针对每个指定的位置、用户或联系人对象来路由以不同格式表示的电话号码。同一拨号串的解析和转换可能不同，具体取决于拨打该号码的位置，以及发出呼叫的人员或联系人对象。

## 拨号计划作用域

拨号计划的 *范围* 决定了拨号计划的应用层级。在 Lync Server 中，会为用户分配一个特定的每用户拨号计划。如果没有分配用户拨号计划，则会应用注册器池拨号计划。如果没有注册器池拨号计划，则会应用站点拨号计划。最后，如果没有其他适用于该用户的拨号计划，则会应用全局拨号计划。

客户端通过用户登录 Lync Server 时所提供的带内设置获取拨号计划作用域级别。管理员可以使用 Lync Server 控制面板管理和分配拨号计划作用域级别。

<table>
<thead>
<tr class="header">
<th><img src="images/Dn783119.note(OCS.15).gif" title="note" alt="note" />注意：</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>服务级别公用电话交换网 (PSTN) 网关拨号计划应用于来自特定网关的传入呼叫。</td>
</tr>
</tbody>
</table>


拨号计划作用域级别定义如下：

  - **用户拨号计划 ：** 可分配给各个用户、组或联系人对象。当接收到其为用户默认的电话上下文设置的呼叫时，语音应用程序可查找每用户拨号计划。为便于分配拨号计划，将联系人对象视为单个用户。

  - **池拨号计划 ：** 可在服务级别为拓扑中的任何 PSTN 网关或注册器创建。要定义池拨号计划，必须指定要应用拨号计划的特定服务（PSTN 网关或注册器池）。

  - **站点拨号计划 ：** 可以为整个站点创建，但已经分配了池拨号计划或用户拨号计划的用户、组或联系人对象除外。要定义站点拨号计划，必须指定要应用拨号计划的站点。

  - **全局拨号计划 ：** 随产品一起安装的默认拨号计划。可以编辑全局拨号计划，但无法将其删除。除非配置和分配具有更具体的作用域的拨号计划，否则全局拨号计划将应用于部署中的所有 企业语音用户、组和联系人对象。

## 规划拨号计划

要规划拨号计划，请执行下列步骤：

  - 列出贵组织拥有办事处的全部区域设置。
    
    该列表必须是最新且完整的。随着公司的发展，将需要对该列表进行修订。在拥有许多小型分支机构的大型跨国公司中，这可能会是一个非常耗时的任务。

  - 标识每个站点的有效号码模式。
    
    在规划拨号计划时，最耗时的任务就是确定每个站点的有效号码模式。在某些情况下，尤其是当相应站点位于同一个国家/地区甚至同一个大陆上时，可以将为一个拨号计划编写的规范化规则复制到其他拨号计划中。在另一些情况下，只需对一个拨号计划中的号码进行微小的更改即可将这些号码用于其他拨号计划。

  - 制定组织级别的方案来命名拨号计划。
    
    实行标准命名方案可确保组织内的一致性，并使维护和更新更加容易。

  - 确定单个位置是否需要多个拨号计划。
    
    如果贵组织在多个位置维护一个拨号计划，仍可能需要为从专用交换机 (PBX) 迁移且需要保留现有分机的 企业语音用户创建一个单独的拨号计划。

  - 确定是否需要每用户拨号计划。例如，是否有分支站点的用户注册到中央站点，或是否有用户注册到 Survivable Branch Appliance，可以使用每用户拨号计划和规范化规则来为这样的用户考虑特殊的拨号方案。有关详细信息，请参阅 [Lync Server 2013 的分支站点恢复能力要求](lync-server-2013-branch-site-resiliency-requirements.md)。

  - 确定拨号计划作用域（如本主题上文所述）。

要创建拨号计划，请根据需要使用 Lync Server 控制面板或 Lync Server 命令行管理程序在以下字段中指定值。

## 名称和简单名称

对于用户拨号计划，应指定描述性名称，以标识要为其分配拨号计划的用户、组或联系人对象。对于站点拨号计划，“名称”字段会使用站点名称预先填充，且不能更改。对于池拨号计划，“名称”字段会使用 PSTN 网关或前端池完全限定域名 (FQDN) 预先填充，且不能更改。

拨号计划“简单名称”会使用派生自拨号计划名称的字符串预先填充。“简单名称”字段是可编辑的，从而使您可以为拨号计划创建更具描述性的命名约定。“简单名称”值不能为空，且必须是唯一的。最佳实践是为整个组织制定一个命名约定，然后在所有站点和用户中统一使用此约定。

## 说明

建议您键入一个要应用相应的拨号计划的通用可辨识地理位置名称。例如，如果拨号计划的名称是 London.Contoso.com，则建议的说明将为 London。

## 电话拨入式会议区域

如果要部署电话拨入式会议，则需要指定电话拨入式会议区域，以将电话拨入式会议访问号码与拨号计划关联起来。

## 外部访问前缀

如果用户需要拨打一个或多个额外的前导数字（例如，9）以拨通外线，则可以指定一个最多包含四个字符（\#、\* 和 0-9）的外部访问前缀。

<table>
<thead>
<tr class="header">
<th><img src="images/Dn783119.note(OCS.15).gif" title="note" alt="note" />注意：</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>如果指定了外部访问前缀，则不需要另外创建规范化规则来满足前缀。</td>
</tr>
</tbody>
</table>


## 规范化规则

规范化规则定义如何针对命名的位置来路由以不同格式表示的电话号码。同一号码字符串的解析和转换方式可能不同，具体取决于拨打该号码的场所。规范化规则是呼叫路由所必需的，因为用户在其联系人列表中输入电话号码时，可以而且确实会使用各种格式。

对用户提供的电话号码进行规范化可以提供一致的格式，从而便于以下任务：

  - 将所拨打的号码与预期收件人的 SIP-URI 相匹配。

  - 对呼叫者应用拨号授权规则。

规范化规则可能需要考虑下列号码字段：

  - 拨号计划

  - 国家/地区代码

  - 区号

  - 分机号长度

  - 站点前缀

## 创建规范化规则

规范化规则使用 .NET Framework 正则表达式指定数字匹配模式，服务器使用该模式将拨号串转换为 E.164 格式，以便执行反向号码查找。在 Lync Server 控制面板中，可以通过手动输入表达式，或通过输入要匹配的拨号串的开头数字和长度，并使 Lync Server 控制面板生成相应的正则表达式来创建规范化规则。无论采用何种方式，完成操作后，都可以输入一个测试号码来验证规范化规则能否按预期工作。

有关使用 .NET Framework 正则表达式的详细信息，请参阅“.NET Framework 正则表达式”，网址为 [http://go.microsoft.com/fwlink/p/?linkId=140927](http://go.microsoft.com/fwlink/p/?linkid=140927)。

## 规范化规则示例

下表显示以 .NET Framework 正则表达式编写的规范化规则示例。这些仅为示例，不应作为创建自己的规范化规则时的规定性参考。

### 表 1. 使用 .NET Framework 正则表达式的规范化规则

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<thead>
<tr class="header">
<th>规则名称</th>
<th>说明</th>
<th>号码模式</th>
<th>转换</th>
<th>示例</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>4digitExtension</p></td>
<td><p>转换 4 位分机号</p></td>
<td><p>^(\d{4})$</p></td>
<td><p>+1425555$1</p></td>
<td><p>将 0100 转换为 +14255550100</p></td>
</tr>
<tr class="even">
<td><p>5digitExtension</p></td>
<td><p>转换 5 位分机号</p></td>
<td><p>^5(\d{4})$</p></td>
<td><p>+1425555$1</p></td>
<td><p>将 50100 转换为 +14255550100</p></td>
</tr>
<tr class="odd">
<td><p>7digitcallingRedmond</p></td>
<td><p>将 7 位号码转换为雷德蒙德本地号码</p></td>
<td><p>^(\d{7})$</p></td>
<td><p>+1425$1</p></td>
<td><p>将 5550100 转换为 +14255550100</p></td>
</tr>
<tr class="even">
<td><p>7digitcallingDallas</p></td>
<td><p>将 7 位号码转换为达拉斯本地号码</p></td>
<td><p>^(\d{7})$</p></td>
<td><p>+1972$1</p></td>
<td><p>将 5550100 转换为 +19725550100</p></td>
</tr>
<tr class="odd">
<td><p>10digitcallingUS</p></td>
<td><p>转换美国的 10 位号码</p></td>
<td><p>^(\d{10})$</p></td>
<td><p>+1$1</p></td>
<td><p>将 2065550100 转换为 +12065550100</p></td>
</tr>
<tr class="even">
<td><p>LDCallingUS</p></td>
<td><p>用美国的长途前缀转换号码</p></td>
<td><p>^1(\d{10})$</p></td>
<td><p>+$1</p></td>
<td><p>将 12145550100 转换为 +2145550100</p></td>
</tr>
<tr class="odd">
<td><p>IntlCallingUS</p></td>
<td><p>用美国的国际前缀转换号码</p></td>
<td><p>^011(\d*)$</p></td>
<td><p>+$1</p></td>
<td><p>将 01191445550100 转换为 +91445550100</p></td>
</tr>
<tr class="even">
<td><p>RedmondOperator</p></td>
<td><p>将 0 转换为雷德蒙德号码</p></td>
<td><p>^0$</p></td>
<td><p>+14255550100</p></td>
<td><p>将 0 转换为 +14255550100</p></td>
</tr>
<tr class="odd">
<td><p>RedmondSitePrefix</p></td>
<td><p>用网内前缀 (6) 和雷德蒙德站点代码 (222) 转换号码</p></td>
<td><p>^6222(\d{4})$</p></td>
<td><p>+1425555$1</p></td>
<td><p>将 62220100 转换为 +14255550100</p></td>
</tr>
<tr class="even">
<td><p>NYSitePrefix</p></td>
<td><p>用网内前缀 (6) 和纽约站点代码 (333) 转换号码</p></td>
<td><p>^6333(\d{4})$</p></td>
<td><p>+1202555$1</p></td>
<td><p>将 63330100 转换为 +12025550100</p></td>
</tr>
<tr class="odd">
<td><p>DallasSitePrefix</p></td>
<td><p>用网内前缀 (6) 和达拉斯站点代码 (444) 转换号码</p></td>
<td><p>^6444(\d{4})$</p></td>
<td><p>+1972555$1</p></td>
<td><p>将 64440100 转换为 +19725550100</p></td>
</tr>
</tbody>
</table>


下表基于上表中显示的规范化规则，说明了美国华盛顿雷德蒙德的示例拨号计划。

### 表 2. 基于表 1 中所示规范化规则的雷德蒙德拨号计划

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Redmond.forestFQDN</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>5digitExtension</p></td>
</tr>
<tr class="even">
<td><p>7digitcallingRedmond</p></td>
</tr>
<tr class="odd">
<td><p>10digitcallingUS</p></td>
</tr>
<tr class="even">
<td><p>IntlCallingUS</p></td>
</tr>
<tr class="odd">
<td><p>RedmondSitePrefix</p></td>
</tr>
<tr class="even">
<td><p>NYSitePrefix</p></td>
</tr>
<tr class="odd">
<td><p>DallasSitePrefix</p></td>
</tr>
<tr class="even">
<td><p>RedmondOperator</p></td>
</tr>
</tbody>
</table>


<table>
<thead>
<tr class="header">
<th><img src="images/Dn783119.note(OCS.15).gif" title="note" alt="note" />注意：</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>上表中显示的规范化规则名称不包括空格，但是您可以选择包括空格。例如，表中的第一个名称可以写作“5 digit extension”或“5-digit Extension”，这些名称仍然有效。</td>
</tr>
</tbody>
</table>

