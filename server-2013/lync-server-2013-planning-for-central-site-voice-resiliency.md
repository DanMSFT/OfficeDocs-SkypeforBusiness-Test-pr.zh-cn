﻿---
title: Lync Server 2013：规划中央站点语音恢复能力
TOCTitle: 规划中央站点语音恢复能力
ms:assetid: 52dd0c3e-cd3c-44cf-bef5-8c49ff5e4c7a
ms:mtpsurl: https://technet.microsoft.com/zh-cn/library/Gg398347(v=OCS.15)
ms:contentKeyID: 49312854
ms.date: 05/19/2016
mtps_version: v=OCS.15
ms.translationtype: HT
---

# 在 Lync Server 2013 中规划中央站点语音恢复能力

 

_**上一次修改主题：** 2015-03-09_

企业在全球拥有多个站点的情况日益普遍。维护紧急服务、访问技术支持以及在中央站点停用时执行重要业务任务的能力对任何 企业语音恢复能力解决方案都是至关重要的。当中央站点不可用时，必须满足以下条件：

  - 必须提供语音故障转移。

  - 通常在中央站点的 前端池中注册的用户必须能够在备用 前端池中注册。可以通过创建多个 DNS SRV 记录来完成此操作，每个记录解析为每个中央站点中的 控制器池或 前端池。您可以调整 SRV 记录的优先级和权重，以便该中央站点所服务的用户将相应的 控制器和 前端池置于其他 SRV 记录中的控制器池和前端池之前。

  - 必须将其他站点的用户发送和接收的呼叫重新路由到 PSTN。

本主题介绍可用于保护中央站点语音恢复能力的安全的推荐解决方案。

## 体系结构和拓扑

规划中央站点的语音恢复能力需要对 Lync Server 2013 注册器在启用语音故障转移中担任的重要角色有基本的了解。 Lync Server 注册器是一个启用了客户端注册和身份验证并提供路由服务的服务器角色。它和其他组件一起驻留在 Standard Edition Server、 前端服务器、 控制器或 Survivable Branch Appliance 中。注册器池由驻留在 前端池上运行并位于同一站点的注册器服务组成。 前端池必须负载平衡。建议使用 DNS 负载平衡，但也支持使用硬件负载平衡。 Lync 客户端可通过以下发现机制发现 前端池：

1.  DNS SRV 记录

2.  自动发现 Web 服务（ Lync Server 2013 中的新增功能）

3.  DHCP 选项 120

在 Lync 客户端连接到 前端池之后，负载平衡器会将其定向到池中的某个 前端服务器。反过来， 前端服务器会将客户端重定向到池中的首选注册器。

已启用 企业语音的每个用户都会分配到特定的注册器池，该注册器池将成为该用户的主注册器池。在给定的站点上，通常成百上千个用户共享一个主注册器池。要说明其状态、会议或故障转移依赖于中央站点的任何分支站点用户使用中央站点资源的情况，建议您将每个分支站点用户视作已在中央站点注册的用户。当前对分支站点用户（包括在 Survivable Branch Appliance 中注册的用户）的数量没有限制。

为确保中央站点具有发生故障时的语音恢复能力，主注册器池必须有一个单独指定的备份注册器池，该注册器池位于另一个站点。可以使用 拓扑生成器恢复能力设置来配置备份。假定两个站点之间有一个可恢复的 WAN 链路，其主注册器池不再可用的用户将自动定向到备份注册器池。

以下步骤介绍了客户端发现和注册过程：

1.  客户端可通过 DNS SRV 记录发现 Lync Server。在 Lync Server 2013 中，可以将 DNS SRV 记录配置为向 DNS SRV 查询返回多个 FQDN。例如，如果企业 Contoso 具有三个中央站点（北美、欧洲和亚太），且每个中央站点有一个控制器池，则 DNS SRV 记录可以指向每个位置中的控制器池 FQDN。只要其中一个位置上的控制器池可用，客户端就可以连接到第一个跃点 Lync Server。
    
    <table>
    <thead>
    <tr class="header">
    <th><img src="images/Dn783119.note(OCS.15).gif" title="note" alt="note" />注意：</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>可以选择使用 控制器池。可以改用 前端池。</td>
    </tr>
    </tbody>
    </table>


2.  控制器池会通知 Lync 客户端有关用户的主注册器池和备份注册器池的情况。

3.  Lync 客户端首先尝试连接到用户的主注册器池。如果主注册器池可用，则该注册器接受注册。如果主注册器池不可用， Lync 客户端将尝试连接到备份注册器池。如果备份注册器池可用且已确定用户的主注册器池不可用（检测出指定故障转移间隔期间检测信号不足），则备份注册器池将接受用户的注册。备份注册器检测到主注册器再次可用后，备份注册器池会将故障转移 Lync 客户端重定向到其主池。

下图显示了可确保中央站点恢复能力的推荐拓扑。这两个站点由可恢复的 WAN 链路相连。如果中央站点变得不可用，则已分配给该池的用户会定向到备份站点以进行注册。

**中央站点语音恢复能力的推荐拓扑**

![中央站点语音恢复能力的拓扑](images/Gg398347.19ea3e74-8a5c-488c-a34e-fc180ab9a50a(OCS.15).jpg "中央站点语音恢复能力的拓扑")

## 要求与建议

以下有关实现中央站点语音恢复能力的要求和建议适用于大多数组织：

  - 主注册器池和备份注册器池所在的站点应由可恢复的 WAN 链路相连。

  - 每个中央站点必须包含由一个或多个注册器组成的注册器池。

  - 必须使用 DNS 负载平衡和/或硬件负载平衡使每个注册器池负载平衡。有关规划负载平衡配置的详细信息，请参阅[Lync Server 2013 的负载平衡要求](lync-server-2013-load-balancing-requirements.md)。

  - 必须使用 Lync Server 命令行管理程序**set-CsUser** cmdlet 或 Lync Server 控制面板将每个用户分配给主注册器池。

  - 主注册器池必须具有单个备份注册器池，该注册器池位于不同的中央站点。

  - 必须将主注册器池配置为可故障转移到备份注册器池。默认情况下，将主注册器设置为每隔 300 秒故障转移到备份注册器池。您可以使用 Lync Server 2013 拓扑生成器来更改此间隔。

  - 配置故障转移路由，如规划文档中的“[在 Lync Server 2013 中配置故障转移路由](lync-server-2013-configuring-a-failover-route.md)”主题中所述。配置该路由后，指定网关，该网关的站点与在主路由中指定的网关的站点不同。

  - 如果中央站点包含主管理服务器且该站点可能还要再关闭一段时间，则需要在备份站点重新安装管理工具，否则将不能更改任何管理设置。

## 依赖项

Lync Server 依赖于以下基础结构和软件组件，以确保语音恢复能力：


<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>组件</strong></p></td>
<td><p><strong>功能</strong></p></td>
</tr>
<tr class="even">
<td><p>DNS</p></td>
<td><p>解析有关服务器到服务器和服务器到客户端连接的 SRV 记录和 A 记录</p></td>
</tr>
<tr class="odd">
<td><p>Exchange 和 Exchange Web 服务 (EWS)</p></td>
<td><p>联系人存储；日历数据</p></td>
</tr>
<tr class="even">
<td><p>Exchange 统一消息和 Exchange Web 服务</p></td>
<td><p>呼叫日志、语音邮件列表、语音邮件</p></td>
</tr>
<tr class="odd">
<td><p>DHCP 选项 120</p></td>
<td><p>如果 DNS SRV 不可用，则客户端将尝试使用 DHCP 选项 120 来发现注册器。为此，必须配置 DHCP 服务器或启用 Lync Server 2013 DHCP。有关详细信息，请参阅<a href="lync-server-2013-branch-site-resiliency-requirements.md">Lync Server 2013 的分支站点恢复能力要求</a>一节中的“分支站点恢复能力的软硬件要求”。</p></td>
</tr>
</tbody>
</table>


## Survivable 语音功能

如果上述要求和建议已付诸实行，则备份注册器池将提供以下语音功能：

  - 出站 PSTN 呼叫

  - 入站 PSTN 呼叫，前提是电话服务提供商支持故障转移到备份站点的功能

  - 同一站点的用户间和两个不同站点的用户间的企业呼叫

  - 基本呼叫处理功能（包括呼叫保持、取回和转接）

  - 双方即时消息和在同一站点的用户间共享音频和视频

  - 呼叫转接、终结点同时响铃、呼叫委派和团队呼叫服务，但前提是在同一站点配置呼叫委派的双方或所有团队成员。

  - 现有电话和客户端继续工作。

  - 呼叫详细信息记录 (CDR)

  - 身份验证和授权

在主中央站点停用时，以下语音功能可能会工作，也可能不会工作，具体取决于配置方式：

  - 语音邮件处理和检索
    
    如果要使 Exchange UM 在主中央站点停用时可用，必须执行以下操作之一：
    
      - 更改 DNS SRV 记录，使中央站点的 Exchange UM 服务器指向其他站点的备份 Exchange UM 服务器。
    
      - 将每个用户的 Exchange UM 拨号计划配置为包含中央站点和备份站点的 Exchange UM 服务器，但将备份 Exchange UM 服务器指定为禁用。如果主站点变得不可用，则 Exchange 管理员必须将备份站点的 Exchange UM 服务器标记为启用。
    
    如果上述解决方案均不可用，则在中央站点变得不可用时， Exchange UM 将不可用。

  - 所有类型的会议
    
    故障转移到备份站点的用户，可以加入到由其池可用但不能在自己的主池（不再可用）上创建或承载会议的组织者创建或承载的会议。同样，其他用户也不能加入在受影响的用户的主池上承载的会议。

在主中央站点停用时，以下语音功能不会工作：

  - 会议自动助理

  - 基于状态和 DND 的路由

  - 更新呼叫转接设置

  - 响应组服务和呼叫寄存

  - 设置新的电话和客户端

  - 通讯簿 Web 搜索

## 另请参阅

#### 其他资源

[在 Lync Server 2013 中规划分支站点语音恢复能力](lync-server-2013-planning-for-branch-site-voice-resiliency.md)

