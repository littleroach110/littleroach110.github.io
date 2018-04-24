---
layout: post
title: STIX 2.0 示例剖析
category: 技术
tags: 威胁情报
keywords:
description:
---

# 关于STIX 2.0

### 什么是STIX？

STIX【1】，Structured Threat Information Expression，结构化的威胁信息表达，是一种用于交换网络空间威胁情报的语言和序列化格式。STIX是开放源代码和免费的。

### 为什么你应该关心STIX

贡献和获取网络空间威胁情报变得更容易。借助STIX，可疑、攻陷和溯源的所有方面的内容都可以使用对象和描述性关系来清晰地表达。STIX信息可以直观地表示给分析人员，或者存储为JSON格式以便快速机器可读。STIX的开放性允许与现有工具和产品集成，或者用于特定分析师或网络需求。

### STIX 对象

STIX对象将每条信息与要填充的特定属性进行分类。通过关系连接多个对象可以简化或复杂地表示网络空间威胁情报。以下是可以通过STIX进行表达的列表。

STIX2 定义了十二个SDO（STIX字段对象）

![STIX Example 01]({{site.CDN_PATH}}/public/image/20180424-STIX-Examples01.png)

攻击模式，TTP（攻击方法）的一种，用于描述威胁主体尝试攻击目标的方法。

![STIX Example 02]({{site.CDN_PATH}}/public/image/20180424-STIX-Examples02.png)

攻击活动，一组敌对的行为，描述一系列针对特定目标的恶意行为或一段时间内发动的攻击。

![STIX Example 03]({{site.CDN_PATH}}/public/image/20180424-STIX-Examples03.png)

应对措施，用于阻止或响应攻击的措施。

![STIX Example 04]({{site.CDN_PATH}}/public/image/20180424-STIX-Examples04.png)

身份，个人、组织或团体，以及个人、组织或团体的类别。

![STIX Example 05]({{site.CDN_PATH}}/public/image/20180424-STIX-Examples05.png)

攻击指标，包含可用于检测可疑或恶意网络行为的模式。

![STIX Example 06]({{site.CDN_PATH}}/public/image/20180424-STIX-Examples06.png)

入侵集合，一个拥有共同属性的敌对行为和资源的分组集合，被认为是由单个威胁主体策划的。

![STIX Example 07]({{site.CDN_PATH}}/public/image/20180424-STIX-Examples07.png)

恶意软件，TTP（攻击方法）的一种类型，也称为恶意代码和恶意软件，用于攻陷受害者数据或系统的机密性、完整性或可用性。

![STIX Example 08]({{site.CDN_PATH}}/public/image/20180424-STIX-Examples08.png)

可观察数据，表达在系统或网络上观察到的信息（例如，IP地址）。

![STIX Example 09]({{site.CDN_PATH}}/public/image/20180424-STIX-Examples09.png)

报告，专注在一个或多个主题的威胁情报集合，例如，威胁主体、恶意软件或攻击技术的描述，包括上下文内容详细信息。

![STIX Example 10]({{site.CDN_PATH}}/public/image/20180424-STIX-Examples10.png)

威胁主体，被认为恶意操作的个人、团体或组织。

![STIX Example 11]({{site.CDN_PATH}}/public/image/20180424-STIX-Examples11.png)

工具，被威胁主体用于实施攻击的软件。

![STIX Example 12]({{site.CDN_PATH}}/public/image/20180424-STIX-Examples12.png)

脆弱性，软件中的一个错误，可以被黑客直接利用来访问系统或网络。

STIX定义了两个SRO（STIX关系对象）

![STIX Example 13]({{site.CDN_PATH}}/public/image/20180424-STIX-Examples13.png)

关系，用于连接两个SDO（STIX字段对象），来描述他们之间是如何与对方进行关联。

![STIX Example 14]({{site.CDN_PATH}}/public/image/20180424-STIX-Examples14.png)

瞄准，表示网络威胁情报元素（如攻击指标、恶意软件）被看到的情况。

### STIX 2.0的结构

STIX 2的对象用JSON表示，以下是STIX 2.0 的攻击活动对象基于JSON的示例：

```
{
    "type": "campaign",  
    "id": "campaign--8e2e2d2b-17d4-4cbf-938f-98ee46b3cd3f",  
    "created": "2016-04-06T20:03:00.000Z",  
    "name": "Green Group Attacks Against Finance",  
    "description": "Campaign by Green Group against targets in the financial services sector."  
}
```

![STIX Example 15]({{site.CDN_PATH}}/public/image/20180424-STIX-Examples15.png)

STIX 关系示例

# STIX 2.0 示例概览

以下示例演示了如何将STIX 2.0 概念用于常见用例【2】。它们可用于将多个概念连接在一起，并提供有关STIX对象和属性的更多详细信息

### 识别威胁主体简介

商业威胁情报提供者和资源充足的政府机构通常将恶意行为溯源到特定威胁主体或威胁主体集团【3】。

##### 场景

在这种场景下，对一个名为“Disco 团队”的威胁主体集团使用STIX威胁主体和身份对象进行建模。Disco 团队主要使用西班牙语来运营，他们以窃取信用卡信息来获取经济利益而闻名，他们公开使用电子邮件别名“disco-team@stealthemail.com”，或者称为“Equipo del Discoteca”

##### 数据模型

正如你所期望的那样，威胁主体识别是使用威胁主体的SDO（STIX字段对象）来表示的。可以在该对象中获取与威胁主体相关的信息，例如目标和动机。其他基本信息不是威胁主体特定的，如联系人信息，最好使用身份SDO来表示。身份对象也可以用于STIX中的威胁主体以外的其他信息。他们可以对组织、政府机构和信息来源等内容进行建模。

需要注意的是，在这种场景下，Disco 团队小组是作为一个威胁主体，而不是入侵集合。他们可能会支持入侵集合，但是这些信息是未知的。一个入侵集合最适合描述包含多个攻击活动和目的的整个攻击集合。在这种情况下，Disco 团队是一个有目的的威胁主体。

```name```（名称）和```labels```（属性）标签是威胁主体SDO所需的唯一必需属性。```labels```字段对于描述Disco 团队是什么类型的威胁主体是至关重要的。因为Disco 团队被认为是大型的、有组织的、利益驱动的窃取财务信息的犯罪团伙，所以，他们最好被标签为```crime-syndicate```（犯罪集团）。

威胁主体SDO还可以为构建更完整的威胁主体简介的可选属性进行建模。例如，```aliases```（别名）字段包含此威胁主体被称为其他名称的列表。威胁主体也可能有一个或多个```roles```（角色），以更多地描述他们的工作。例如，威胁主体可以赞助或指挥攻击，可以编写恶意软件或者运作恶意的基础设施。在Disco 团队案例中，他们以代理人的身份进行活动运作、发动攻击，进而窃取财务信息。

像大多数威胁主体一样，Disco团队在他们的攻击中有一个特定的目标。因此，```goals```（目标）列表描述了威胁主体想要做什么。在这种情况下，Disco 团队的唯一目标就是窃取信用卡凭证。威胁主体也具有不同程度的专业知识。因此，攻击者的```sophistication```级别（如果知道）可以描述攻击者的技能和知识。由于先进的攻击方法和熟练的工具或恶意代码，Disco 团队被标记为```expert```（专家级）。他们组织的```recource_level```字段表明他们与小规模的个人或团队相比，规模更庞大、资金更充裕。最后，威胁主体通常有一个或多个攻击背后的动机。```primary_motivation```字段描述了发动攻击的主要原因。一些威胁主体可能会寻求恶名或者支配地位，而另一些威胁主体则是为了报复或者个人满足感。对于Disco 团队来说，获取财务信息的动机被归入```personal-gain```动机。

威胁主体的基本身份信息可以使用身份SDO进行建模。对于Disco 团队，他们是一类```organization```（组织），通过```identity_class```字段进行获取。这是由于这种威胁主体更正式、更有组织性，而不是个人黑客或非正式的黑客群体。获取的另一个属性```contact_information```（如果已知身份），表示任何邮件地址或电话号码。对于Disco 团队而言，已经提供了一个邮件地址。

既然Disco 团队的信息是在威胁主体和身份SDO中进行表示，则通过关系SRO将这两个对象连接在一起。在本例中，```source_ref```威胁主体```id```是```attribute-to```到```target_ref```身份```id```。

下面的关系图说明了威胁主体和身份的SDO和关系SRO：

![STIX Example 16]({{site.CDN_PATH}}/public/image/20180424-STIX-Examples16.png)

##### 实现

JSON

```
{
  "type": "bundle",
  "id": "bundle--c9567f73-3803-415c-b06e-2b0622830e5d",
  "spec_version": "2.0",
  "objects": [
    {
      "type": "threat-actor",
      "id": "threat-actor--dfaa8d77-07e2-4e28-b2c8-92e9f7b04428",
      "created": "2014-11-19T23:39:03.893348Z",
      "modified": "2014-11-19T23:39:03.893348Z",
      "name": "Disco Team Threat Actor Group",
      "description": "This organized threat actor group operates to create profit from all types of crime.",
      "labels": [
        "crime-syndicate"
      ],
      "aliases": [
        "Equipo del Discoteca"
      ],
      "roles": [
        "agent"
      ],
      "goals": [
        "Steal Credit Card information"
      ],
      "sophistication": "expert",
      "resource_level": "organization",
      "primary_motivation": "personal-gain"
    },
    {
      "type": "identity",
      "id": "identity--733c5838-34d9-4fbf-949c-62aba761184c",
      "created": "2016-08-23T18:05:49.307000Z",
      "modified": "2016-08-23T18:05:49.307000Z",
      "name": "Disco Team",
      "description": "Disco Team is the name of an organized threat actor crime-syndicate.",
      "identity_class": "organization",
      "contact_information": "disco-team@stealthemail.com"
    },
    {
      "type": "relationship",
      "id": "relationship--966c5838-34d9-4fbf-949c-62aba7611837",
      "created": "2016-08-23T18:05:49.307000Z",
      "modified": "2016-08-23T18:05:49.307000Z",
      "relationship_type": "attributed-to",
      "source_ref": "threat-actor--dfaa8d77-07e2-4e28-b2c8-92e9f7b04428",
      "target_ref": "identity--733c5838-34d9-4fbf-949c-62aba761184c"
    }
  ]
}
```

Python 生产者

```
import stix2

threat_actor = stix2.ThreatActor(
    id="threat-actor--dfaa8d77-07e2-4e28-b2c8-92e9f7b04428",
    created="2014-11-19T23:39:03.893Z",
    modified="2014-11-19T23:39:03.893Z",
    name="Disco Team Threat Actor Group",
    description="This organized threat actor group operates to create profit from all types of crime.",
    labels=["crime-syndicate"],
    aliases=["Equipo del Discoteca"],
    roles=["agent"],
    goals=["Steal Credit Card Information"],
    sophistication="expert",
    resource_level="organization",
    primary_motivation="personal-gain"
)

identity = stix2.Identity(
    id="identity--733c5838-34d9-4fbf-949c-62aba761184c",
    created="2016-08-23T18:05:49.307Z",
    modified="2016-08-23T18:05:49.307Z",
    name="Disco Team",
    description="Disco Team is the name of an organized threat actor crime-syndicate.",
    identity_class="organisation",
    contact_information="disco-team@stealthemail.com"
)

relationship = stix2.Relationship(threat_actor, 'attributed-to', identity)

bundle = stix2.Bundle(objects=[threat_actor, identity, relationship])
```

Python 消费者

```
import stix2

for obj in bundle.objects:
    if obj == threat_actor:
        print("------------------")
        print("== THREAT ACTOR ==")
        print("------------------")
        print("ID: " + obj.id)
        print("Created: " + str(obj.created))
        print("Modified: " + str(obj.modified))
        print("Name: " + obj.name)
        print("Description: " + obj.description)
        print("Labels: " + obj.labels[0])
        print("Aliases: " + obj.aliases[0])
        print("Goals: " + obj.goals[0])
        print("Sophistication: " + obj.sophistication)
        print("Resource Level: " + obj.resource_level)
        print("Primary Motivation: " + obj.primary_motivation)

    elif obj == identity:
        print("------------------")
        print("== IDENTITY ==")
        print("------------------")
        print("ID: " + obj.id)
        print("Created: " + str(obj.created))
        print("Modified: " + str(obj.modified))
        print("Name: " + obj.name)
        print("Description: " + obj.description)
        print("Identity Class: " + obj.identity_class)
        print("Contact Information: " + obj.contact_information)

    elif obj == relationship:
        print("------------------")
        print("== RELATIONSHIP ==")
        print("------------------")
        print("ID: " + obj.id)
        print("Created: " + str(obj.created))
        print("Modified: " + str(obj.modified))
        print("Relationship Type: " + obj.relationship_type)
        print("Source Ref: " + obj.source_ref)
        print("Target Ref: " + obj.target_ref)
```

### 定义攻击活动 VS. 威胁主体 VS. 入侵集合

网络攻击经常被威胁主体使用，作为针对特定目标的协同攻击活动的一部分。这些攻击活动通常有一个目标或对象。有时，这些攻击活动是由来自国家级别的、犯罪集团级别的或其他邪恶组织级别的威胁主体策划的，并且包含相似的属性、行为和性质，以便在很长一段时间内实现多个目标，整个攻击包被称为入侵集合【4】。

##### 场景

该场景表示一种怀疑由国家“Franistan”资助的高级持续威胁（APT），攻击目标是BPP（Branistan人民党），BPP是国家Branistan的执政党之一。该入侵集合包括一系列针对BPP网站的复杂攻击活动和攻击模式。其中一项攻击活动的目的是将错误的信息插入到BPP的网页中；另一项攻击活动是针对BPP网络服务器的DDOS攻击。

##### 数据模型

首先，使用身份SDO来对Franistan和BPP相关信息进行建模。正如其它STIX示例中提到的（如，识别威胁主体简介），该对象专门用于表示关于Franistan和BPP的共同可识别信息。场景中的身份对象最适合用来帮助使用SRO（STIX 关系对象）建立与其它对象之间的关系。例如，Franistan被归属为一个威胁主体，而BPP是入侵集合和多个攻击活动的目标。

接下来，该例子中的高级持续威胁的细节内容在入侵集合SDO中表示。这个被标记```name```为APT BPP的入侵集合对象包含入侵集合尝试实现的任何动机和目标。```goals```（目标）属性列表中的APT BPP的一些对象是影响Branistan选举和扰乱BPP。因此，他们的动机是相似的，他们的```primary_motivation```是意识形态，他们的```secondary_motivations```是控制权。另外，由于他们被高度怀疑是由Franistan提供资金和资源的资助，他们的```resource_level```是```government```（政府），他们的动机和和资源级别的值来自于公开词汇攻击动机和攻击资源级别。

与许多入侵集合一样，可能有多个威胁主体（见威胁主体SDO）和攻击活动。在这种场景下，有一个威胁主体简称为假BPP，其目标也是影响Branistan选举。其动机和```resource_level```也同入侵集合SDO相同，因为该威胁主体已经与此APT关联，所以这是有意义的。假BPP被怀疑由Franistan提供资金资助，这意味着该威胁主体的```labels```标签是```nation-state```。其它相关信息可以在```roles```（角色）和```sophistication```（复杂度）属性中找到。在该案例中，假BPP是针对Branistan攻击的主导者，因此，其```roles```（角色）字段将会标记为```director```。由于他们被认为是资金充裕的先进国家级威胁主体，能够发动APT级别的攻击，假BPP的```sophistication```（复杂度）级别被认为是战略级。```roles```（角色）和```sophistication```（复杂度）的值可以在STIX 2.0规范的开放词汇列表中的威胁主体角色和威胁主体复杂度中找到。

作为入侵集合的一部分，一些不同的攻击活动已经与威胁主体联系在一起。这些详细信息可在2个攻击活动SDO中获取。第一个攻击活动称之为Operation Bran Flakes，是由假BPP策划的，目的是黑进BPP网站www.bpp.bn，并在网页中注入虚假信息。第二个报告的攻击活动，名为Operation Raisin Bran，在第一个攻击活动之后发生，尝试对BPP网站实施洪泛攻击，以拒绝合法用户访问该网站。

除了在攻击活动对象中对攻击细节进行建模以外，攻击模式SDO还使用CAPEC来对特定攻击进行分类。在这类对象中，你可以在```external_references```属性下找到对CAPEC ID的引用。例如，尝试插入虚假信息的第一个攻击活动归入```external_id``` CAPEC-148 或“内容欺骗”中。与拒绝服务活动相关的第二个攻击模式SDO引用为```external_id``` CAPEC-488或“HTTP洪泛攻击”。

现在我们已经介绍了本示例中的所有STIX字段对象，我们可以检查其中的关系或SRO（STIX关系对象）。在这种场景下，威胁主体、入侵集合和攻击活动使用攻击模式，因此创建多个SRO来表示这些关系。在所有这些关系中，```source_ref```会引用威胁主体、入侵集合或攻击活动的ID，```target_ref```将指向本实例中提到的任一攻击模式ID。```relationship_type```属性将会简单标记为```uses```。

下一个共同关系涉及到BPP的身份SDO。在该案例中，攻击活动、入侵集合和威胁主体都指向这个身份，所以```target_ref```字段将包括以```relationship_type```为目标的BPP的身份ID。除了这些关系外，威胁主体假BPP还与其他身份对象之间存在关系。由于假BPP与国家Franistan相关联，因此该威胁主体SDO与Franistan身份SDO通过```attributed-to```的```relationship_type```相关联。另外，在前面提到的另一种攻击中，假BPP试图接管真BPP的网站，并在网站发布冒充真BPP的内容，所以需要另一种关系来表示假BPP ```impersonates```（冒充）真BPP。

最后，还有更多关系将攻击活动、入侵集合和威胁主体联系在一起，两个攻击活动通过两个独立的```attributed-to```关系关联到了入侵集合和攻击主体SDO。另外，由于入侵集合表示该攻击主体主导了整个攻击包，入侵集合SDO也通过```attributed-to```关系关联到了威胁主体。

以下图表有助于在该场景中对SDO之间的关系进行可视化。下面的第一张图用来表示入侵集合、威胁主体和攻击活动对象之间的关联。

![STIX Example 17]({{site.CDN_PATH}}/public/image/20180424-STIX-Examples17.png)

第二张图对身份对象、入侵集合、威胁主体和攻击活动SDO之间的关联关系进行建模。

![STIX Example 18]({{site.CDN_PATH}}/public/image/20180424-STIX-Examples18.png)

最后，第三张图捕获了攻击模式SDO和入侵集合、威胁主体和攻击活动对象的关系。

![STIX Example 19]({{site.CDN_PATH}}/public/image/20180424-STIX-Examples19.png)

##### 实现

JSON

```

{
  "spec_version": "2.0",
  "type": "bundle",
  "id": "bundle--81810123-b298-40f6-a4e7-186efcd07670",
  "objects": [
    {
      "type": "identity",
      "id": "identity--8c6af861-7b20-41ef-9b59-6344fd872a8f",
      "created": "2016-08-08T15:50:10.983Z",
      "modified": "2016-08-08T15:50:10.983Z",
      "name": "Franistan Intelligence",
      "identity_class": "organisation"
    },
    {
      "type": "identity",
      "id": "identity--ddfe7140-2ba4-48e4-b19a-df069432103b",
      "created": "2016-08-08T15:50:10.983Z",
      "modified": "2016-08-08T15:50:10.983Z",
      "external_references": [
        {
          "source_name": "website",
          "url": "http://www.bpp.bn"
        }
      ],
      "name": "Branistan Peoples Party",
      "identity_class": "organisation"
    },
    {
      "type": "threat-actor",
      "id": "threat-actor--56f3f0db-b5d5-431c-ae56-c18f02caf500",
      "created": "2016-08-08T15:50:10.983Z",
      "modified": "2016-08-08T15:50:10.983Z",
      "labels": [
        "nation-state"
      ],
      "roles": [
        "director"
      ],
      "goals": [
        "Influence the election in Branistan"
      ],
      "resource_level": "government",
      "primary_motivation": "ideology",
      "secondary_motivations": [
        "dominance"
      ],
      "name": "Fake BPP (Branistan Peoples Party)",
      "sophistication": "strategic"
    },
    {
      "type": "campaign",
      "id": "campaign--e5268b6e-4931-42f1-b379-87f48eb41b1e",
      "created": "2016-08-08T15:50:10.983Z",
      "modified": "2016-08-08T15:50:10.983Z",
      "name": "Operation Bran Flakes",
      "description": "A concerted effort to insert false information into the BPP's web pages",
      "aliases": [
        "OBF"
      ],
      "first_seen": "2016-01-08T12:50:40.123Z",
      "objective": "Hack www.bpp.bn"
    },
    {
      "type": "campaign",
      "id": "campaign--1d8897a7-fdc2-4e59-afc9-becbe04df727",
      "created": "2016-08-08T15:50:10.983Z",
      "modified": "2016-08-08T15:50:10.983Z",
      "name": "Operation Raisin Bran",
      "description": "A DDOS campaign to flood BPP web servers",
      "aliases": [
        "ORB"
      ],
      "first_seen": "2016-02-07T19:45:32.126Z",
      "objective": "Flood www.bpp.bn"
    },
    {
      "type": "intrusion-set",
      "id": "intrusion-set--ed69450a-f067-4b51-9ba2-c4616b9a6713",
      "created": "2016-08-08T15:50:10.983Z",
      "modified": "2016-08-08T15:50:10.983Z",
      "name": "APT BPP",
      "description": "An advanced persistent threat that seeks to disrupt Branistan's election with multiple attacks",
      "first_seen": "2016-01-08T12:50:40.123Z",
      "resource_level": "government",
      "primary_motivation": "ideology",
      "goals": [
        "Influence the Branistan election",
        "Disrupt the BPP"
      ],
      "secondary_motivations": [
        "dominance"
      ],
      "aliases": [
        "Bran-teaser"
      ]
    },
    {
      "type": "attack-pattern",
      "id": "attack-pattern--19da6e1c-71ab-4c2f-886d-d620d09d3b5a",
      "created": "2016-08-08T15:50:10.983Z",
      "modified": "2017-01-30T21:15:04.127Z",
      "external_references": [
        {
          "external_id": "CAPEC-148",
          "source_name": "capec"
        }
      ],
      "name": "Content Spoofing"
    },
    {
      "type": "attack-pattern",
      "id": "attack-pattern--f6050ea6-a9a3-4524-93ed-c27858d6cb3c",
      "created": "2016-08-08T15:50:10.983Z",
      "modified": "2017-01-30T21:15:04.127Z",
      "external_references": [
        {
          "external_id": "CAPEC-488",
          "source_name": "capec"
        }
      ],
      "name": "HTTP Flood"
    },
    {
      "type": "relationship",
      "id": "relationship--3dcf59c3-30e3-4aa5-9c05-2cbffcee5922",
      "created": "2016-08-08T15:50:10.983Z",
      "modified": "2016-08-08T15:50:10.983Z",
      "relationship_type": "attributed-to",
      "source_ref": "campaign--e5268b6e-4931-42f1-b379-87f48eb41b1e",
      "target_ref": "threat-actor--56f3f0db-b5d5-431c-ae56-c18f02caf500"
    },
    {
      "type": "relationship",
      "id": "relationship--45cd8846-fec5-4e64-8271-3d807dc4ea3b",
      "created": "2016-08-08T15:50:10.983Z",
      "modified": "2016-08-08T15:50:10.983Z",
      "relationship_type": "attributed-to",
      "source_ref": "campaign--1d8897a7-fdc2-4e59-afc9-becbe04df727",
      "target_ref": "threat-actor--56f3f0db-b5d5-431c-ae56-c18f02caf500"
    },
    {
      "type": "relationship",
      "id": "relationship--9b35d9a0-87ae-4800-88fc-f1fc63246c18",
      "created": "2016-08-08T15:50:10.983Z",
      "modified": "2016-08-08T15:50:10.983Z",
      "relationship_type": "attributed-to",
      "source_ref": "campaign--e5268b6e-4931-42f1-b379-87f48eb41b1e",
      "target_ref": "intrusion-set--ed69450a-f067-4b51-9ba2-c4616b9a6713"
    },
    {
      "type": "relationship",
      "id": "relationship--50896dfd-d12f-4376-8b47-26ca4155ed52",
      "created": "2016-08-08T15:50:10.983Z",
      "modified": "2016-08-08T15:50:10.983Z",
      "relationship_type": "attributed-to",
      "source_ref": "campaign--1d8897a7-fdc2-4e59-afc9-becbe04df727",
      "target_ref": "intrusion-set--ed69450a-f067-4b51-9ba2-c4616b9a6713"
    },
    {
      "type": "relationship",
      "id": "relationship--8bd69586-33a6-4dab-99b1-e5728cc3dcd8",
      "created": "2016-08-08T15:50:10.983Z",
      "modified": "2016-08-08T15:50:10.983Z",
      "relationship_type": "attributed-to",
      "source_ref": "intrusion-set--ed69450a-f067-4b51-9ba2-c4616b9a6713",
      "target_ref": "threat-actor--56f3f0db-b5d5-431c-ae56-c18f02caf500"
    },
    {
      "type": "relationship",
      "id": "relationship--11290b55-63e2-40f7-be78-7c32a8c08e68",
      "created": "2016-08-08T15:50:10.983Z",
      "modified": "2016-08-08T15:50:10.983Z",
      "relationship_type": "targets",
      "source_ref": "intrusion-set--ed69450a-f067-4b51-9ba2-c4616b9a6713",
      "target_ref": "identity--ddfe7140-2ba4-48e4-b19a-df069432103b"
    },
    {
      "type": "relationship",
      "id": "relationship--af2a647c-c215-4dc1-af29-19b4aab94f96",
      "created": "2016-08-08T15:50:10.983Z",
      "modified": "2016-08-08T15:50:10.983Z",
      "relationship_type": "uses",
      "source_ref": "intrusion-set--ed69450a-f067-4b51-9ba2-c4616b9a6713",
      "target_ref": "attack-pattern--19da6e1c-71ab-4c2f-886d-d620d09d3b5a"
    },
    {
      "type": "relationship",
      "id": "relationship--98f8012d-a797-43f8-bd59-ed11c078fae0",
      "created": "2016-08-08T15:50:10.983Z",
      "modified": "2016-08-08T15:50:10.983Z",
      "relationship_type": "uses",
      "source_ref": "intrusion-set--ed69450a-f067-4b51-9ba2-c4616b9a6713",
      "target_ref": "attack-pattern--f6050ea6-a9a3-4524-93ed-c27858d6cb3c"
    },
    {
      "type": "relationship",
      "id": "relationship--6b6b524f-f115-4eeb-b488-045d62ddfb66",
      "created": "2016-08-08T15:50:10.983Z",
      "modified": "2016-08-08T15:50:10.983Z",
      "relationship_type": "targets",
      "source_ref": "campaign--e5268b6e-4931-42f1-b379-87f48eb41b1e",
      "target_ref": "identity--ddfe7140-2ba4-48e4-b19a-df069432103b"
    },
    {
      "type": "relationship",
      "id": "relationship--032fb0f6-c1ab-4caf-95aa-25cdd7fb0563",
      "created": "2016-08-08T15:50:10.983Z",
      "modified": "2016-08-08T15:50:10.983Z",
      "relationship_type": "targets",
      "source_ref": "campaign--1d8897a7-fdc2-4e59-afc9-becbe04df727",
      "target_ref": "identity--ddfe7140-2ba4-48e4-b19a-df069432103b"
    },
    {
      "type": "relationship",
      "id": "relationship--1f820ee7-bb30-4d0c-96c8-08e07e08b0ed",
      "created": "2016-08-08T15:50:10.983Z",
      "modified": "2016-08-08T15:50:10.983Z",
      "relationship_type": "uses",
      "source_ref": "campaign--e5268b6e-4931-42f1-b379-87f48eb41b1e",
      "target_ref": "attack-pattern--19da6e1c-71ab-4c2f-886d-d620d09d3b5a"
    },
    {
      "type": "relationship",
      "id": "relationship--addad2d4-f7f1-4d8d-95fb-a94fe084a433",
      "created": "2016-08-08T15:50:10.983Z",
      "modified": "2016-08-08T15:50:10.983Z",
      "relationship_type": "uses",
      "source_ref": "campaign--1d8897a7-fdc2-4e59-afc9-becbe04df727",
      "target_ref": "attack-pattern--f6050ea6-a9a3-4524-93ed-c27858d6cb3c"
    },
    {
      "type": "relationship",
      "id": "relationship--5b271699-d2ad-468c-903d-304ad7a17d71",
      "created": "2016-08-08T15:50:10.983Z",
      "modified": "2016-08-08T15:50:10.983Z",
      "relationship_type": "attributed-to",
      "source_ref": "threat-actor--56f3f0db-b5d5-431c-ae56-c18f02caf500",
      "target_ref": "identity--8c6af861-7b20-41ef-9b59-6344fd872a8f"
    },
    {
      "type": "relationship",
      "id": "relationship--f9d2f337-bf47-40d2-8afd-908d4e366572",
      "created": "2016-08-08T15:50:10.983Z",
      "modified": "2016-08-08T15:50:10.983Z",
      "relationship_type": "impersonates",
      "source_ref": "threat-actor--56f3f0db-b5d5-431c-ae56-c18f02caf500",
      "target_ref": "identity--ddfe7140-2ba4-48e4-b19a-df069432103b"
    },
    {
      "type": "relationship",
      "id": "relationship--51c9484f-e415-4156-a910-613e9f06ba98",
      "created": "2016-08-08T15:50:10.983Z",
      "modified": "2016-08-08T15:50:10.983Z",
      "relationship_type": "targets",
      "source_ref": "threat-actor--56f3f0db-b5d5-431c-ae56-c18f02caf500",
      "target_ref": "identity--ddfe7140-2ba4-48e4-b19a-df069432103b"
    },
    {
      "type": "relationship",
      "id": "relationship--f9d2f337-bf47-40d2-8afd-908d4e366572",
      "created": "2016-08-08T15:50:10.983Z",
      "modified": "2016-08-08T15:50:10.983Z",
      "relationship_type": "uses",
      "source_ref": "threat-actor--56f3f0db-b5d5-431c-ae56-c18f02caf500",
      "target_ref": "attack-pattern--19da6e1c-71ab-4c2f-886d-d620d09d3b5a"
    },
    {
      "type": "relationship",
      "id": "relationship--f9d2f337-bf47-40d2-8afd-908d4e366572",
      "created": "2016-08-08T15:50:10.983Z",
      "modified": "2016-08-08T15:50:10.983Z",
      "relationship_type": "uses",
      "source_ref": "threat-actor--56f3f0db-b5d5-431c-ae56-c18f02caf500",
      "target_ref": "attack-pattern--f6050ea6-a9a3-4524-93ed-c27858d6cb3c"
    }
  ]
}
```

Python生产者

```
import stix2

threat_actor = stix2.ThreatActor(
    id="threat-actor--56f3f0db-b5d5-431c-ae56-c18f02caf500",
    created="2016-08-08T15:50:10.983Z",
    modified="2016-08-08T15:50:10.983Z",
    name="Fake BPP (Branistan Peoples Party)",
    labels=["nation-state"],
    roles=["director"],
    goals=["Influence the election in Branistan"],
    resource_level="government"
    primary_motivation="ideology",
    secondary_motivations=["dominance"],
    sophistication="strategic"
)

identity1 = stix2.Identity(
    id="identity--8c6af861-7b20-41ef-9b59-6344fd872a8f",
    created="2016-08-08T15:50:10.983Z",
    modified="2016-08-08T15:50:10.983Z",
    name="Franistan Intelligence",
    identity_class="organisation"
)

ref_bpp = stix2.ExternalReference(
    source_name="website",
    url="http://www.bpp.bn"
)

identity2 = stix2.Identity(
    id="identity--ddfe7140-2ba4-48e4-b19a-df069432103b",
    created="2016-08-08T15:50:10.983Z",
    modified="2016-08-08T15:50:10.983Z",
    name="Branistan Peoples Party",
    identity_class="organisation"
    external_references= [ref_bpp]
)

ref_capec1 = stix2.ExternalReference(
    source_name="capec",
    url="https://capec.mitre.org/data/definitions/148.html",
    external_id="CAPEC-148"
)

ref_capec2 = stix2.ExternalReference(
    source_name="capec",
    url="https://capec.mitre.org/data/definitions/488.html",
    external_id="CAPEC-488"
)

attack_pattern1 = stix2.AttackPattern(
    id="attack-pattern--19da6e1c-71ab-4c2f-886d-d620d09d3b5a",
    created="2016-08-08T15:50:10.983Z",
    modified="2017-01-30T21:15:04.127Z",
    name="Content Spoofing",
    external_references=[ref_capec1]
)

attack_pattern2 = stix2.AttackPattern(
    id="attack-pattern--f6050ea6-a9a3-4524-93ed-c27858d6cb3c",
    created="2016-08-08T15:50:10.983Z",
    modified="2017-01-30T21:15:04.127Z",
    name="HTTP Flood",
    external_references=[ref_capec2]
)

campaign1 = stix2.Campaign(
    id="campaign--e5268b6e-4931-42f1-b379-87f48eb41b1e",
    created="2016-08-08T15:50:10.983Z",
    modified="2016-08-08T15:50:10.983Z",
    name="Operation Bran Flakes",
    description="A concerted effort to insert false information into the BPP's web pages.",
    aliases=["OBF"],
    first_seen="2016-01-08T12:50:40.123Z",
    objective="Hack www.bpp.bn"
)

campaign2 = stix2.Campaign(
    id="campaign--1d8897a7-fdc2-4e59-afc9-becbe04df727",
    created="2016-08-08T15:50:10.983Z",
    modified="2016-08-08T15:50:10.983Z",
    name="Operation Raisin Bran",
    description="A DDOS campaign to flood BPP web servers.",
    aliases=["ORB"],
    first_seen="2016-02-07T19:45:32.126Z",
    objective="Flood www.bpp.bn"
)

intrusionset = stix2.IntrusionSet(
    id="intrusion-set--ed69450a-f067-4b51-9ba2-c4616b9a6713",
    created="2016-08-08T15:50:10.983Z",
    modified="2016-08-08T15:50:10.983Z",
    name="APT BPP",
    description="An advanced persistent threat that seeks to disrupt Branistan's election with multiple attacks.",
    first_seen="2016-01-08T12:50:40.123Z",
    resource_level="government",
    primary_motivation="ideology",
    goals=["Influence the Branistan election", "Disrupt the BPP"],
    secondary_motivations=["dominance"],
    aliases=["Bran-teaser"]
)

relationship1 = stix2.Relationship(campaign1, 'attributed-to', threat_actor)
relationship2 = stix2.Relationship(campaign2, 'attributed-to', threat_actor)
relationship3 = stix2.Relationship(campaign1, 'attributed-to', intrusionset)
relationship4 = stix2.Relationship(campaign2, 'attributed-to', intrusionset)
relationship5 = stix2.Relationship(intrusionset, 'attributed-to', threat_actor)
relationship6 = stix2.Relationship(intrusionset, 'targets', identity2)
relationship7 = stix2.Relationship(intrusionset, 'uses', attack_pattern1)
relationship8 = stix2.Relationship(intrusionset, 'uses', attack_pattern2)
relationship9 = stix2.Relationship(campaign1, 'targets', identity2)
relationship10 = stix2.Relationship(campaign2, 'targets', identity2)
relationship11 = stix2.Relationship(campaign1, 'uses', attack_pattern1)
relationship12 = stix2.Relationship(campaign2, 'uses', attack_pattern2)
relationship13 = stix2.Relationship(threat_actor, 'impersonates', identity2)
relationship14 = stix2.Relationship(threat_actor, 'targets', identity2)
relationship15 = stix2.Relationship(threat_actor, 'attributed-to', identity1)
relationship16 = stix2.Relationship(campaign2, 'targets', identity2)
relationship17 = stix2.Relationship(threat_actor, 'uses', attack_pattern1)
relationship18 = stix2.Relationship(threat_actor, 'uses', attack_pattern2)

bundle = stix2.Bundle(objects=[threat_actor, identity1, identity2, attack_pattern1, attack_pattern2, campaign1, campaign2, intrusionset, relationship1, relationship2, relationship3, relationship4, relationship5, relationship6, relationship7, relationship8, relationship9, relationship10, relationship11, relationship12, relationship13, relationship14, relationship15, relationship16, relationship17, relationship18])
```

Python消费者

```
import stix2
import re

for obj in bundle.objects:
    if obj == threat_actor:
        print("------------------")
        print("== THREAT ACTOR ==")
        print("------------------")
        print("ID: " + obj.id)
        print("Created: " + str(obj.created))
        print("Modified: " + str(obj.modified))
        print("Name: " + obj.name)
        print("Labels: " + str(obj.labels))
        print("Roles: " + str(obj.roles))
        print("Goals: " + str(obj.goals))
        print("Resource Level: " + obj.resource_level)
        print("Primary Motivation: " + obj.primary_motivation)
        print("Secondary Motivations: " + str(obj.secondary_motivations))
        print("Sophistication: " + obj.sophistication)

    elif obj == identity1:
        print("------------------")
        print("== IDENTITY ==")
        print("------------------")
        print("ID: " + obj.id)
        print("Created: " + str(obj.created))
        print("Modified: " + str(obj.modified))
        print("Name: " + obj.name)
        print("Identity Class: " + obj.identity_class)

    elif obj == identity2:
        print("------------------")
        print("== IDENTITY ==")
        print("------------------")
        print("ID: " + obj.id)
        print("Created: " + str(obj.created))
        print("Modified: " + str(obj.modified))
        print("Name: " + obj.name)
        print("External References: " + str(obj.external_references))

    elif obj == attack_pattern1:
        print("------------------")
        print("== ATTACK PATTERN ==")
        print("------------------")
        print("ID: " + obj.id)
        print("Created: " + str(obj.created))
        print("Modified: " + str(obj.modified))
        print("Name: " + obj.name)
        print("External References: " + str(obj.external_references))

    elif obj == attack_pattern2:
        print("------------------")
        print("== ATTACK PATTERN ==")
        print("------------------")
        print("ID: " + obj.id)
        print("Created: " + str(obj.created))
        print("Modified: " + str(obj.modified))
        print("Name: " + obj.name)
        print("External References: " + str(obj.external_references))

    elif obj == campaign1:
        print("------------------")
        print("== CAMPAIGN ==")
        print("------------------")
        print("ID: " + obj.id)
        print("Created: " + str(obj.created))
        print("Modified: " + str(obj.modified))
        print("Name: " + obj.name)
        print("Aliases: " + str(obj.aliases))
        print("First Seen: " + str(obj.first_seen))
        print("Objective: " + obj.objective)

    elif obj == campaign2:
        print("------------------")
        print("== CAMPAIGN ==")
        print("------------------")
        print("ID: " + obj.id)
        print("Created: " + str(obj.created))
        print("Modified: " + str(obj.modified))
        print("Name: " + obj.name)
        print("Aliases: " + str(obj.aliases))
        print("First Seen: " + str(obj.first_seen))
        print("Objective: " + obj.objective)

    elif obj == intrusionset:
        print("------------------")
        print("== INTRUSION SET ==")
        print("------------------")
        print("ID: " + obj.id)
        print("Created: " + str(obj.created))
        print("Modified: " + str(obj.modified))
        print("Name: " + obj.name)
        print("Description: " + obj.name)
        print("Resource Level: " + obj.resource_level)
        print("First Seen: " + str(obj.first_seen))
        print("Primary Motivation: " + obj.primary_motivation)
        print("Secondary Motivations: " + str(obj.secondary_motivations))
        print("Aliases: " + str(obj.aliases))
        print("Goals: " + str(obj.goals))

    elif re.search('relationship*', str(obj)):
        print("------------------")
        print("== RELATIONSHIP ==")
        print("------------------")
        print("ID: " + obj.id)
        print("Created: " + str(obj.created))
        print("Modified: " + str(obj.modified))
        print("Relationship Type: " + obj.relationship_type)
        print("Source Ref: " + obj.source_ref)
        print("Target Ref: " + obj.target_ref)
```

### 恶意URL的攻击指标

将恶意软件投送到潜在目标的一种常见方法是将其托管在特定URL中，然后，目标通过网络钓鱼邮件或另一个网站的链接，被引导到该URL，并在访问URL时被利用。分享恶意URL列表可能是限制暴露恶意代码的一种有效而且便宜的方法【5】。

##### 场景

这种场景包括一个已知是恶意的URL http://x4z9arb.cn/4712/的攻击指标，以及一个与该URL相关的后门恶意软件。该网站已被证明托管这个后门恶意软件，并且已知恶意软件可以下载远程文件。

##### 数据模型

恶意URL值只是可以通过使用攻击指标SDO表示的众多攻击指标之一。这是通过使用基于STIX语言模式的攻击指标SDO的```pattern```（模式）属性来完成的。使用该语言，URL可以使用比对表达式进行构造：[url:value= 'http://x4z9arb.cn/4712/']

该攻击指标对象还必须包含一个```labels```（标签）属性，该属性提供有关URL的更多上下文内容信息。已知此场景中的URL是恶意的，因此该攻击指标的标签是```malicious-activity```，该值取自攻击指标标签开放词汇表，其中包含用于分类攻击指标的其它有用标签。

攻击指标对象的另一个必需字段```valid_from```表明此URL应该被视为有价值的情报。在这种情况下，URL从对象创建开始时就生效。

在该场景下，与URL相关的恶意软件是一种后门程序，可以使用STIX恶意软件SDO进行建模。像攻击指标对象一样，恶意软件对象可以使用```labels```（标签）属性进行分类，标签属性选自恶意软件标签开放词汇表。例如，一个恶意软件可能被分类为键盘记录器、间谍软件、蠕虫、病毒等。在这个例子中，附属在URL的恶意软件是一种```backdoor```（后门）和```remote-access-trojan```（远程访问木马）。

恶意软件SDO也可以用于捕捉有关恶意软件实例的杀伤链信息。据了解，这款恶意软件试图建立一个后门立足点并下载远程文件。因此，恶意软件对象使用```kill_chain_phases```列表来表示，该列表既包含所使用的杀伤链的名称，有包含该杀伤链所处的阶段。在这种情况下，Mandiant攻击生命周期模型被用作杀伤链，并包含```phase_name```的```establish-foothold```建立点。也可使用其他杀伤链，例如洛克马丁公司或其他组织。

最后，关系SDO可以用于连接攻击指标和恶意软件对象。URL攻击指标表明后门恶意软件对象。在该关系中，攻击指标```id```是```source_ref```，恶意软件```id```是```target_ref```。

以下关系图表示了攻击指标和恶意软件SDO，以及它们之间关系SRO

![STIX Example 20]({{site.CDN_PATH}}/public/image/20180424-STIX-Examples20.png)

##### 实现

JSON

```
{
  "type": "bundle",
  "id": "bundle--44af6c39-c09b-49c5-9de2-394224b04982",
  "spec_version": "2.0",
  "objects": [
    {
      "type": "indicator",
      "id": "indicator--d81f86b9-975b-bc0b-775e-810c5ad45a4f",
      "created": "2014-06-29T13:49:37.079000Z",
      "modified": "2014-06-29T13:49:37.079000Z",
      "labels": [
        "malicious-activity"
      ],
      "name": "Malicious site hosting downloader",
      "pattern": "[url:value = 'http://x4z9arb.cn/4712/']",
      "valid_from": "2014-06-29T13:49:37.079000Z"
    },
    {
      "type": "malware",
      "id": "malware--162d917e-766f-4611-b5d6-652791454fca",
      "created": "2014-06-30T09:15:17.182Z",
      "modified": "2014-06-30T09:15:17.182Z",
      "name": "x4z9arb backdoor",
      "labels": [
        "backdoor",
        "remote-access-trojan"
      ],
      "description": "This malware attempts to download remote files after establishing a foothold as a backdoor.",
      "kill_chain_phases": [
        {
          "kill_chain_name": "mandiant-attack-lifecycle-model",
          "phase_name": "establish-foothold"
        }
      ]
    },
    {
      "type": "relationship",
      "id": "relationship--6ce78886-1027-4800-9301-40c274fd472f",
      "created": "2014-06-30T09:15:17.182Z",
      "modified": "2014-06-30T09:15:17.182Z",
      "relationship_type": "indicates",
      "source_ref": "indicator--d81f86b9-975b-bc0b-775e-810c5ad45a4f",
      "target_ref": "malware--162d917e-766f-4611-b5d6-652791454fca"
    }
  ]
}
```

Python 生产者

```
import stix2

indicator = stix2.Indicator(
    id="indicator--d81f86b9-975b-bc0b-775e-810c5ad45a4f",
    created="2014-06-29T13:49:37.079Z",
    modified="2014-06-29T13:49:37.079Z",
    name="Malicious site hosting downloader",
    description="This organized threat actor group operates to create profit from all types of crime.",
    labels=["malicious-activity"],
    pattern="[url:value = 'http://x4z9arb.cn/4712/']",
    valid_from="2014-06-29T13:49:37.079000Z"
)

foothold = stix2.KillChainPhase(
    kill_chain_name="mandiant-attack-lifecycle-model",
    phase_name="establish-foothold"
)

malware = stix2.Malware(
    id="malware--162d917e-766f-4611-b5d6-652791454fca",
    created="2014-06-30T09:15:17.182Z",
    modified="2014-06-30T09:15:17.182Z",
    name="x4z9arb backdoor",
    labels=["backdoor", "remote-access-trojan"],
    description="This malware attempts to download remote files after establishing a foothold as a backdoor.",
    kill_chain_phases=[foothold]
)

relationship = stix2.Relationship(indicator, 'indicates', malware)

bundle = stix2.Bundle(objects=[indicator, malware, relationship])
```

Python 消费者

```
import stix2

for obj in bundle.objects:
    if obj == malware:
        print("------------------")
        print("== MALWARE ==")
        print("------------------")
        print("ID: " + obj.id)
        print("Created: " + str(obj.created))
        print("Modified: " + str(obj.modified))
        print("Name: " + obj.name)
        print("Description: " + obj.description)
        print("Labels: " + obj.labels[0] + ", " + obj.labels[1])
        print("Kill Chain: " + str(obj.kill_chain_phases))

    elif obj == indicator:
        print("------------------")
        print("== INDICATOR ==")
        print("------------------")
        print("ID: " + obj.id)
        print("Created: " + str(obj.created))
        print("Modified: " + str(obj.modified))
        print("Name: " + obj.name)
        print("Description: " + obj.description)
        print("Labels: " + obj.labels[0])
        print("Pattern: " + obj.pattern)
        print("Valid From: " + str(obj.valid_from))

    elif obj == relationship:
        print("------------------")
        print("== RELATIONSHIP ==")
        print("------------------")
        print("ID: " + obj.id)
        print("Created: " + str(obj.created))
        print("Modified: " + str(obj.modified))
        print("Relationship Type: " + obj.relationship_type)
        print("Source Ref: " + obj.source_ref)
        print("Target Ref: " + obj.target_ref)
```

### 文件哈希的恶意软件攻击指标

常用的威胁情报共享方式是共享基于主机的恶意代码攻击指标，这些恶意代码通常是文件名和哈希值。本示例描述了文件哈希攻击指标，恶意代码的名字和类型【6】。

##### 场景

该场景由一个简单的攻击指标的描述组成，该攻击指标表示一个给定哈希文件和上下文内容信息的模式，如果发现具有该哈希值的文件，这可能表示可能存在Poison Ivy的样本。

##### 数据模型

攻击指标SDO用于对表达式模式进行建模，例如本示例中的Posion Ivy文件哈希。该哈希表示使用基于STIX模式语言的攻击指标对象的```pattern```（模式）属性，通过该语言，SHA-256哈希的比较表达式如下：[file:hashes.'SHA-256'= 'ef537f25c895bfa782526529a9b63d97aa631564d5d789c2b765448c8635fb6c']。如果已知，则可以表示其他文件属性，例如，名称或路径。另外，虽然本示例只涉及文件哈希，许多其他网络可观测数据对象和它们的属性可以使用攻击指标模式进行建模。例如，邮件信息、域名、IP地址和进程等。

攻击指标对象还需要一个```labels```（标签）属性，它有助于定义正在表示的攻击指标类型。在该场景中，哈希值与Poison Ivy（一种已知恶意类型的恶意样本）相关联，所以该攻击指标被标记为```malicious-activity```（恶意活动）。该值取自攻击指标标签开放词汇表，其中提供了用于分类指标的其他有用标签。

接下来，Poison Ivy恶意软件的详细信息可以使用STIX恶意软件对象进行捕获。STIX中恶意软件对象还包含特定恶意软件类型所需的```labels```（标签）属性。在这种情况下，Poison Ivy是一个```remote-access-trojan```。该词汇选自恶意软件标签开放词汇表，其中包含多种常见类型的恶意软件类别，例如病毒、后门、间谍软件等。

这些SDO通过关系SRO耦合在一起。此关系将```source_ref```攻击指标和```target_ref```恶意软件通过指示```relationship_typ```e进行关联。

以下关系图表示了攻击指标和恶意软件SDO以及关系SRO之间的关联关系：

![STIX Example 21]({{site.CDN_PATH}}/public/image/20180424-STIX-Examples21.png)

##### 实现

JSON

```
{
  "type": "bundle",
  "id": "bundle--44af6c39-c09b-49c5-9de2-394224b04982",
  "spec_version": "2.0",
  "objects": [
    {
      "type": "indicator",
      "id": "indicator--a932fcc6-e032-176c-126f-cb970a5a1ade",
      "created": "2014-02-20T09:16:08.989000Z",
      "modified": "2014-02-20T09:16:08.989000Z",
      "name": "File hash for Poison Ivy variant",
      "description": "This file hash indicates that a sample of Poison Ivy is present.",
      "labels": [
        "malicious-activity"
      ],
      "pattern": "[file:hashes.'SHA-256' = 'ef537f25c895bfa782526529a9b63d97aa631564d5d789c2b765448c8635fb6c']",
      "valid_from": "2014-02-20T09:00:00.000000Z"
    },
    {
      "type": "malware",
      "id": "malware--fdd60b30-b67c-11e3-b0b9-f01faf20d111",
      "created": "2014-02-20T09:16:08.989000Z",
      "modified": "2014-02-20T09:16:08.989000Z",
      "name": "Poison Ivy",
      "labels": [
        "remote-access-trojan"
      ]
    },
    {
      "type": "relationship",
      "id": "relationship--f191e70e-1736-47c3-b0f9-fdfe01387eb1",
      "created": "2014-02-20T09:16:08.989000Z",
      "modified": "2014-02-20T09:16:08.989000Z",
      "relationship_type": "indicates",
      "source_ref": "indicator--a932fcc6-e032-176c-126f-cb970a5a1ade",
      "target_ref": "malware--fdd60b30-b67c-11e3-b0b9-f01faf20d111"
    }
  ]
}

```

Python 生产者

```
import stix2

indicator = stix2.Indicator(
    id="indicator--a932fcc6-e032-176c-126f-cb970a5a1ade",
    created="2014-02-20T09:16:08.989Z",
    modified="2014-02-20T09:16:08.989Z",
    name="File hash for Poison Ivy variant",
    description="This file hash indicates that a sample of Poison Ivy is present.",
    labels=["malicious-activity"],
    pattern="[file:hashes.'SHA-256' = 'ef537f25c895bfa782526529a9b63d97aa631564d5d789c2b765448c8635fb6c']",
    valid_from="2014-02-20T09:00:00.000000Z"
)

malware = stix2.Malware(
    id="malware--fdd60b30-b67c-11e3-b0b9-f01faf20d111",
    created="2014-02-20T09:16:08.989Z",
    modified="2014-02-20T09:16:08.989Z",
    name="Poison Ivy",
    labels=["remote-access-trojan"]
)

relationship = stix2.Relationship(indicator, 'indicates', malware)

bundle = stix2.Bundle(objects=[indicator, malware, relationship])

```

Python 消费者

```
import stix2

for obj in bundle.objects:
    if obj == malware:
        print("------------------")
        print("== MALWARE ==")
        print("------------------")
        print("ID: " + obj.id)
        print("Created: " + str(obj.created))
        print("Modified: " + str(obj.modified))
        print("Name: " + obj.name)
        print("Labels: " + obj.labels[0])

    elif obj == indicator:
        print("------------------")
        print("== INDICATOR ==")
        print("------------------")
        print("ID: " + obj.id)
        print("Created: " + str(obj.created))
        print("Modified: " + str(obj.modified))
        print("Name: " + obj.name)
        print("Description: " + obj.description)
        print("Labels: " + obj.labels[0])
        print("Pattern: " + obj.pattern)
        print("Valid From: " + str(obj.valid_from))

    elif obj == relationship:
        print("------------------")
        print("== RELATIONSHIP ==")
        print("------------------")
        print("ID: " + obj.id)
        print("Created: " + str(obj.created))
        print("Modified: " + str(obj.modified))
        print("Relationship Type: " + obj.relationship_type)
        print("Source Ref: " + obj.source_ref)
        print("Target Ref: " + obj.target_ref)

```

### 攻击指标的瞄准

网络威胁情报共享和协作的一个主要好处是能够在该攻击指标出现在系统或网络之前，提前给其它公司和机构发出告警。这为解决网络威胁问题提供了更主动的方法。在许多情况下，一个攻击指标会在多个网络中出现。可以分享这个在其它地方看到的特定攻击指标，对于其它也使用此类威胁情报的组织至关重要【7】。

##### 场景

该场景包括两个威胁共享威胁情报的网络安全公司Alpha和Beta。在Alpha的网络上看到一个恶意URL，并且生成了一个攻击指标来捕获这些信息。Alpha公司然后与B公司分享这些信息，Beta公司后来在他们的系统中发现了这个攻击指标，然后，Beta公司会创建并分享该攻击指标的瞄准，表示该攻击指标已经被发现。

##### 数据模型

在该案例中，有两个身份SDO用于两个公司：Alpha和Bwta。身份对象记录关于两个组织的相关信息，例如他们是哪个行业、他们的工作以及联系方式。两个组织都是STIX情报的生产者和消费者，所以他们的```id```可以在使用```created_by_ref```属性的对象中引用，以表明他们是生成STIX对象的创建人。值得注意的是，身份SDO也可以用来表示个人、攻击对象、政府机构、团体等。

身份对象至少需要一些必需的属性：```name```和```identity_class```。```identity_class```字段对于Alpha和Beta代表的身份类型的分类很重要，在他们的案例中，该字段被值```organization```填充。该术语来自于身份类型开放词汇表，其中包含了标签身份的建议值。

身份SDO的其它字段是可选的，有助于帮助构建完整的身份简介。例如，了解个人或团体的角色列表可能很有用，该列表使用```labels```（标签）属性进行捕捉。由于这个场景中两家公司都使用处置网络威胁，因此将他们标记为```cyber security```（网络安全）是有意义的。如果你了解身份可能属于```sectors```（行业）列表或任何相关的```contact_information```，这也可以为这些对象提供此信息。例如，知道某些STIX对象创建者在金融行业，则可能会提供更多的背景知识，来说明为什么他们发现某些攻击指标或成为特定威胁主体的目标。例子中的两个公司都在技术行业，这些词汇来自与行业开放词汇表。

接下来，Alpha公司使用攻击指标SDO来捕获他们在他们网络中发现的恶意URL信息。使用STIX模式语言，Alpha在```pattern```（模式）属性中将URL表示为比较表达式：[url:value = 'http://paypa1.banking.com']。由于Alpha知道该URL是恶意的，他们在```labels```字段将其标记为```malicious-activity```，该字段来自于攻击指标标签开放词汇表。

Beta公司从Alpha公司接收到这个攻击指标情报，并在自己的网络上部署它，用于发现该特定URL。一旦发现它们，他们就会生成瞄准SRO，这是一种与常规关系SRO不同的特殊类型的关系对象。例如，瞄准包含的```count```、```first_seen```和```last_seen```等唯一字段，用于表示在特定时间内看到SDO的时间，以及此SDO出现的次数。另外，一个标准关系SRO仅用于将两个SDO连接在一起，而不提供相同情报类型的认定。

在该案例中，Beta的瞄准对象捕获了关于A的攻击指标相关信息，这些信息被B在他们的网络中发现。由于在这个案例中，他们是该对象的创建者和受害者，B的身份ID在```created_by_ref```和```where_sighted_refs```属性中进行表示。值得一提的是，```where_sighted_refs```字段是一个列表，所以它可以列出发现的其它身份SDO ID的位置。另一个引用中，```sighting_of_ref```包含瞄准SDO的ID，在这种情况下是攻击指标对象。这是一个必需的属性，因为没有一个“瞄准”对象的话，就没有瞄准属性。

在某些情况下，像URL的攻击指标可以在一段时间内在网络上发现多次。但是，对于这种情况，Beta只发现一次URL，最后```count```（计数）字段标记为整数“1”。因为只看到了它一次，```first_seen```和```last_seen```属性描述了相同的时间戳。

以下关系图表示表示身份、身份SDO和瞄准SRO：

![STIX Example 22]({{site.CDN_PATH}}/public/image/20180424-STIX-Examples22.png)

##### 实现

JSON

```
{
  "type": "bundle",
  "id": "bundle--c6a895f2-849c-4d1b-aba4-4b45c2800374",
  "spec_version": "2.0",
  "objects": [
    {
      "type": "identity",
      "id": "identity--39012926-a052-44c4-ae48-caaf4a10ee6e",
      "created": "2017-02-24T15:50:10.564Z",
      "modified": "2017-08-24T15:50:10.564Z",
      "name": "Alpha Threat Analysis Org.",
      "identity_class": "organization",
      "labels": [
        "Cyber Security"
      ],
      "sectors": [
        "technology"
      ],
      "contact_information": "info@alpha.org"
    },
    {
      "type": "identity",
      "id": "identity--5206ba14-478f-4b0b-9a48-395f690c20a2",
      "created": "2017-02-26T17:55:10.442Z",
      "modified": "2017-02-26T17:55:10.442Z",
      "name": "Beta Cyber Intelligence Company",
      "identity_class": "organization",
      "labels": [
        "Cyber Security"
      ],
      "sectors": [
        "technology"
      ],
      "contact_information": "info@beta.com"
    },
    {
      "type": "indicator",
      "id": "indicator--9299f726-ce06-492e-8472-2b52ccb53191",
      "created_by_ref": "identity--39012926-a052-44c4-ae48-caaf4a10ee6e",
      "created": "2017-02-27T13:57:10.515Z",
      "modified": "2017-02-27T13:57:10.515Z",
      "name": "Malicious URL",
      "description": "This URL is potentially associated with malicious activity and is listed on several blacklist sites.",
      "pattern": "[url:value = 'http://paypa1.banking.com']",
      "valid_from": "2015-06-29T09:10:15.915Z",
      "labels": [
        "malicious-activity"
      ]
    },
    {
      "type": "sighting",
      "id": "sighting--8356e820-8080-4692-aa91-ecbe94006833",
      "created_by_ref": "identity--5206ba14-478f-4b0b-9a48-395f690c20a2",
      "created": "2017-02-28T19:37:11.213Z",
      "modified": "2017-02-28T19:37:11.213Z",
      "first_seen": "2017-02-27T21:37:11.213Z",
      "last_seen": "2017-02-27T21:37:11.213Z",
      "count": 1,
      "sighting_of_ref": "indicator--9299f726-ce06-492e-8472-2b52ccb53191",
      "where_sighted_refs": [
        "identity--5206ba14-478f-4b0b-9a48-395f690c20a2"
      ]
    }
  ]
}
```

Python生产者

```
import stix2

identityAlpha = stix2.Identity(
    id="identity--39012926-a052-44c4-ae48-caaf4a10ee6e",
    created="2017-02-24T15:50:10.564Z",
    modified="2017-02-24T15:50:10.564Z",
    name="Alpha Threat Analysis Org.",
    identity_class="organisation",
    contact_information="info@alpha.org",
    labels=["Cyber Security"],
    sectors=["technology"]
)

identityBeta = stix2.Identity(
    id="identity--5206ba14-478f-4b0b-9a48-395f690c20a2",
    created="2017-02-26T17:55:10.442Z",
    modified="2017-02-26T17:55:10.442Z",
    name="Beta Cyber Intelligence Company",
    identity_class="organisation",
    contact_information="info@beta.com",
    labels=["Cyber Security"],
    sectors=["technology"]
)

indicator = stix2.Indicator(
    id="indicator--9299f726-ce06-492e-8472-2b52ccb53191",
    created_by_ref="identity--39012926-a052-44c4-ae48-caaf4a10ee6e",
    created="2017-02-27T13:57:10.515Z",
    modified="2017-02-27T13:57:10.515Z",
    name="Malicious URL",
    description="This URL is potentially associated with malicious activity and is listed on several blacklist sites.",
    labels=["malicious-activity"],
    pattern="[url:value = 'http://paypa1.banking.com']",
    valid_from="2015-06-29T09:10:15.915Z"
)

sighting = stix2.Sighting(
    id="sighting--8356e820-8080-4692-aa91-ecbe94006833",
    created_by_ref="identity--5206ba14-478f-4b0b-9a48-395f690c20a2",
    created="2017-02-28T19:37:11.213Z",
    modified="2017-02-28T19:37:11.213Z",
    first_seen="2017-02-27T21:37:11.213Z",
    last_seen="2017-02-27T21:37:11.213Z",
    count=1,
    sighting_of_ref="indicator--9299f726-ce06-492e-8472-2b52ccb53191",
    where_sighted_refs=["identity--5206ba14-478f-4b0b-9a48-395f690c20a2"]
)

bundle = stix2.Bundle(objects=[indicator, identityAlpha, identityBeta, sighting])
```

Python消费者

```
import stix2

for obj in bundle.objects:
    if obj == identityAlpha:
        print("------------------")
        print("== IDENTITY ==")
        print("------------------")
        print("ID: " + obj.id)
        print("Created: " + str(obj.created))
        print("Modified: " + str(obj.modified))
        print("Name: " + obj.name)
        print("Identity Class: " + obj.identity_class)
        print("Contact Information: " + obj.contact_information)
        print("Labels: " + obj.labels[0])
        print("Sectors: " + obj.sectors[0])

    elif obj == identityBeta:
        print("------------------")
        print("== IDENTITY ==")
        print("------------------")
        print("ID: " + obj.id)
        print("Created: " + str(obj.created))
        print("Modified: " + str(obj.modified))
        print("Name: " + obj.name)
        print("Identity Class: " + obj.identity_class)
        print("Contact Information: " + obj.contact_information)
        print("Labels: " + obj.labels[0])
        print("Sectors: " + obj.sectors[0])

    elif obj == indicator:
        print("------------------")
        print("== INDICATOR ==")
        print("------------------")
        print("ID: " + obj.id)
        print("Created: " + str(obj.created))
        print("Modified: " + str(obj.modified))
        print("Created by Ref: " + obj.created_by_ref)
        print("Name: " + obj.name)
        print("Description: " + obj.description)
        print("Labels: " + obj.labels[0])
        print("Pattern: " + obj.pattern)
        print("Valid From: " + str(obj.valid_from))

    elif obj == sighting:
        print("------------------")
        print("== SIGHTING ==")
        print("------------------")
        print("ID: " + obj.id)
        print("Created: " + str(obj.created))
        print("Modified: " + str(obj.modified))
        print("Created by Ref: " + obj.created_by_ref)
        print("First Seen: " + str(obj.first_seen))
        print("Last Seen: " + str(obj.last_seen))
        print("Count: " + str(obj.count))
        print("Sighting of Ref: " + obj.sighting_of_ref)
        print("Where Sighted Refs: " + obj.where_sighted_refs[0])
```

### 可观测数据的瞄准

虽然攻陷的攻击指标表示攻击背后的情报认定，但原始可观测数据有助于构建该威胁情报背后的基础。在许多情况下，分享可观测数据对组织来说是有好处的。与攻击指标类似，瞄准可以包括可观测数据对象的引用：在其它组织的网络中观察到该可观测数据，就可以表示有关某种恶意软件的信息。这可能潜在地允许基于这个瞄准的原始数据来对情报做更深层次的认定【8】。

##### 场景

该场景由两个网络威胁公司Pym和Oscorp组成，他们彼此共享威胁情报。Pym公司最早与Oscorp公司共享恶意软件SDO。Oscorp公司后来认为，他们已经根据一些捕获到的可观测数据，在他们自己的网络上发现了恶意软件对象。这些数据包含了匹配恶意软件哈希和恶意软件创建的注册表键值。为了表示这一点，Oscorp发布了一个瞄准SRO，该对象包含这些可观测数据的引用，基于此可以成为一个该特定恶意软件的瞄准。

##### 数据模型

在该案例中，两个公司使用两个身份SDO：Pym和Oscorp。身份对象记录了这两个组织的相关信息，例如他们在那个行业以及相关的联系信息。这两个组织都是STIX情报的生产者和消费者，所以他们的```id```可以在使用```created_by_ref```属性的对象中引用，以表明他们是他们生成的STIX对象的创建者。值得注意的是，身份SDO也可以用来表示个人、攻击目标、政府机构和团体等等。

身份对象至少需要一些必需的属性：```name```和```identity_class```。```identity_class```字段对于将Pym和Oscorp标识的类型进行分类很重要。在该案例中标识为```organization```（组织）。此值来自于身份类别开放词汇表，其中包含标识身份的建议值。

Pym首先创建了恶意软件SDO，用来表示该场景中有关恶意软件类型的详细信息。这种特定恶意软件类型被标记为```remote-access-trojan```，并且伪装成可以创建多个注册表键值的pdf文件。Pym将该情报向Oscorp进行了分享。

拥有该恶意软件对象的Oscorp，相信已经在他的网络中发现了该恶意软件，并且已经创建了一个瞄准对象来表示。瞄准SRO是一种特殊类型的STIX关系，其中包含所见对象的属性，如恶意软件SDO的```id```（具有```sighting_of_ref```）、表示该恶意软件被发现次数的```count```（计数值）属性，以及首次发现和最后发现的时间戳。此外，还包括可观测数据列表，用以表达任何可能支持该恶意软件瞄准相关的必要信息。

可观测数据SDO包含在系统和网络中捕获的网络可观测数据信息，包括IP地址、文件和URL。在该场景中，Oscorp观察到文件和注册表键值的信息，他们使用两个不同的可观测数据对象进行建模。尽管你可以在可观测数据案例中包含多个网络可观测数据对象，在这种情况下，文件和注册表键值数据不直接相关。因此，它们包含在单独的可观测数据中。你可以分别阅读STIX 2.0 规范第3部分和第4部分涵盖的STIX网络可观测数据的概念和对象内容。

除了所有对象的通用属性外，可观测数据的属性都是必需的。因此，对于每一个对象，Oscorp必须提供每个案例的```first_observed```、```last_observed```以及```number_observed```（发现的次数）。另外，他们需要在```objects```（对象属性）中提供实际的网络可观测对象。本示例中的第一个可观测数据（下图中的Observed Data 1）包含有关所看到的文件的信息。因此，使用包含哈希列表、文件名称和大小等数据来表示文件。在第二个可观测数据对象中，Oscorp对Windows注册表键值进行建模，例如可疑的恶意软件创建的注册表键值。

最后，值得一提的是，在这种情况下，没有一个对象使用通常将一个对象和另一个对象进行关联的标准关系SRO。瞄准SRO用于恶意软件对象的瞄准，下图中的所有其它关系都被嵌套在这些对象中，例如，瞄准对象包含几个嵌入式关系，包括观测到的内容、谁创建的对象以及在哪里看到的。

下图描述了这种场景：

![STIX Example 23]({{site.CDN_PATH}}/public/image/20180424-STIX-Examples23.png)

##### 实现

JSON

```
{
  "type": "bundle",
  "id": "bundle--a836f05a-f235-4b4b-b523-bd87e40478a1",
  "spec_version": "2.0",
  "objects": [
    {
      "type": "identity",
      "id": "identity--987eeee1-413a-44ac-96cc-0a8acdcc2f2c",
      "created": "2017-04-14T13:07:49.812Z",
      "modified": "2017-04-14T13:07:49.812Z",
      "name": "Oscorp Industries",
      "identity_class": "organization",
      "contact_information": "norman@oscorp.com",
      "sectors": [
        "technology"
      ]
    },
    {
      "type": "identity",
      "id": "identity--7865b6d2-a4af-45c5-b582-afe5ec376c33",
      "created": "2017-04-14T13:07:49.812Z",
      "modified": "2017-04-14T13:07:49.812Z",
      "name": "Pym Technologies",
      "identity_class": "organization",
      "contact_information": "hank@pymtech.com",
      "sectors": [
        "technology"
      ]
    },
    {
      "type": "malware",
      "id": "malware--ae560258-a5cb-4be8-8f05-013d6712295f",
      "created_by_ref": "identity--7865b6d2-a4af-45c5-b582-afe5ec376c33",
      "created": "2014-02-20T09:16:08.989000Z",
      "modified": "2014-02-20T09:16:08.989000Z",
      "name": "Online Job Site Trojan",
      "description": "Trojan that is disguised as the executable file resume.pdf., it also creates a registry key.",
      "labels": [
        "remote-access-trojan"
      ]
    },
    {
      "type": "sighting",
      "id": "sighting--779c4ae8-e134-4180-baa4-03141095d971",
      "created_by_ref": "identity--987eeee1-413a-44ac-96cc-0a8acdcc2f2c",
      "created": "2017-02-28T19:37:11.213Z",
      "modified": "2017-02-28T19:37:11.213Z",
      "first_seen": "2017-02-28T19:07:24.856Z",
      "last_seen": "2017-02-28T19:07:24.856Z",
      "count": 1,
      "sighting_of_ref": "malware--ae560258-a5cb-4be8-8f05-013d6712295f",
      "where_sighted_refs": [
        "identity--987eeee1-413a-44ac-96cc-0a8acdcc2f2c"
      ],
      "observed_data_refs": [
        "observed-data--cf8eaa41-6f4c-482e-89b9-9cd2d6a83cb1",
        "observed-data--a0d34360-66ad-4977-b255-d9e1080421c4"
      ]
    },
    {
      "type": "observed-data",
      "id": "observed-data--cf8eaa41-6f4c-482e-89b9-9cd2d6a83cb1",
      "created_by_ref": "identity--987eeee1-413a-44ac-96cc-0a8acdcc2f2c",
      "created": "2017-02-28T19:37:11.213Z",
      "modified": "2017-02-28T19:37:11.213Z",
      "first_observed": "2017-02-27T21:37:11.213Z",
      "last_observed": "2017-02-27T21:37:11.213Z",
      "number_observed": 1,
      "objects": {
        "0": {
          "type": "file",
          "hashes": {
            "MD5": "1717b7fff97d37a1e1a0029d83492de1",
            "SHA-1": "c79a326f8411e9488bdc3779753e1e3489aaedea"
          },
          "name": "resume.pdf",
          "size": 83968
        }
      }
    },
    {
      "type": "observed-data",
      "id": "observed-data--a0d34360-66ad-4977-b255-d9e1080421c4",
      "created_by_ref": "identity--987eeee1-413a-44ac-96cc-0a8acdcc2f2c",
      "created": "2017-02-28T19:37:11.213Z",
      "modified": "2017-02-28T19:37:11.213Z",
      "first_observed": "2017-02-27T21:37:11.213Z",
      "last_observed": "2017-02-27T21:37:11.213Z",
      "number_observed": 1,
      "objects": {
        "0": {
          "type": "windows-registry-key",
          "key": "HKEY_LOCAL_MACHINE\\SYSTEM\\ControlSet001\\Services\\WSALG2"
        }
      }
    }
  ]
}
```

Python生产者

```
import stix2

identityOscorp = stix2.Identity(
    id="identity--987eeee1-413a-44ac-96cc-0a8acdcc2f2c",
    created="2017-04-14T13:07:49.812Z",
    modified="2017-04-14T13:07:49.812Z",
    name="Oscorp Industries",
    identity_class="organisation",
    contact_information="norman@oscorp.com",
    sectors=["technology"]
)

identityPym = stix2.Identity(
    id="identity--7865b6d2-a4af-45c5-b582-afe5ec376c33",
    created="2017-04-14T13:07:49.812Z",
    modified="2017-04-14T13:07:49.812Z",
    name="Pym Technologies",
    identity_class="organisation",
    contact_information="hank@pymtech.com",
    sectors=["technology"]
)

malware = stix2.Malware(
    id="malware--ae560258-a5cb-4be8-8f05-013d6712295f",
    created="2014-02-20T09:16:08.989Z",
    modified="2014-02-20T09:16:08.989Z",
    created_by_ref="identity--7865b6d2-a4af-45c5-b582-afe5ec376c33",
    name="Online Job Site Trojan",
    description="Trojan that is disguised as the executable file resume.pdf., it also creates a registry key.",
    labels=["remote-access-trojan"]
)

observedDataFile = stix2.ObservedData(
    id="observed-data--cf8eaa41-6f4c-482e-89b9-9cd2d6a83cb1",
    created="2017-02-28T19:37:11.213Z",
    modified="2017-02-28T19:37:11.213Z",
    first_observed="2017-02-27T21:37:11.213Z",
    last_observed="2017-02-27T21:37:11.213Z",
    number_observed=1,
    created_by_ref="identity--7865b6d2-a4af-45c5-b582-afe5ec376c33",
    objects={
        "0": {
            "type": "file",
            "hashes": {
                "MD5": "1717b7fff97d37a1e1a0029d83492de1",
                "SHA-1": "c79a326f8411e9488bdc3779753e1e3489aaedea"
            },
            "name": "resume.pdf",
            "size": 83968
        }
    }
)

observedDataRegKey = stix2.ObservedData(
    id="observed-data--a0d34360-66ad-4977-b255-d9e1080421c4",
    created="2017-02-28T19:37:11.213Z",
    modified="2017-02-28T19:37:11.213Z",
    first_observed="2017-02-27T21:37:11.213Z",
    last_observed="2017-02-27T21:37:11.213Z",
    number_observed=1,
    created_by_ref="identity--7865b6d2-a4af-45c5-b582-afe5ec376c33",
    objects={
        "0": {
            "type": "windows-registry-key",
            "key": "HKEY_LOCAL_MACHINE\\SYSTEM\\ControlSet001\\Services\\WSALG2"
        }
    }
)

sighting = stix2.Sighting(
    id="sighting--779c4ae8-e134-4180-baa4-03141095d971",
    created_by_ref="identity--987eeee1-413a-44ac-96cc-0a8acdcc2f2c",
    created="2017-02-28T19:37:11.213Z",
    modified="2017-02-28T19:37:11.213Z",
    first_seen="2017-02-28T19:07:24.856Z",
    last_seen="2017-02-28T19:07:24.856Z",
    count=1,
    sighting_of_ref="malware--ae560258-a5cb-4be8-8f05-013d6712295f",
    where_sighted_refs=["identity--987eeee1-413a-44ac-96cc-0a8acdcc2f2c"],
    observed_data_refs=["observed-data--cf8eaa41-6f4c-482e-89b9-9cd2d6a83cb1", "observed-data--a0d34360-66ad-4977-b255-d9e1080421c4"]
)

bundle = stix2.Bundle(objects=[identityPym, identityOscorp, malware, observedDataFile, observedDataRegKey, sighting])
```

Python消费者

```
import stix2

for obj in bundle.objects:
    if obj == identityPym:
        print("------------------")
        print("== IDENTITY ==")
        print("------------------")
        print("ID: " + obj.id)
        print("Created: " + str(obj.created))
        print("Modified: " + str(obj.modified))
        print("Name: " + obj.name)
        print("Identity Class: " + obj.identity_class)
        print("Contact Information: " + obj.contact_information)
        print("Sectors: " + obj.sectors[0])

    elif obj == identityOscorp:
        print("------------------")
        print("== IDENTITY ==")
        print("------------------")
        print("ID: " + obj.id)
        print("Created: " + str(obj.created))
        print("Modified: " + str(obj.modified))
        print("Name: " + obj.name)
        print("Identity Class: " + obj.identity_class)
        print("Contact Information: " + obj.contact_information)
        print("Sectors: " + obj.sectors[0])

    elif obj == malware:
        print("------------------")
        print("== MALWARE ==")
        print("------------------")
        print("ID: " + obj.id)
        print("Created: " + str(obj.created))
        print("Modified: " + str(obj.modified))
        print("Created by Ref: " + obj.created_by_ref)
        print("Name: " + obj.name)
        print("Description: " + obj.description)
        print("Labels: " + obj.labels[0])

    elif obj == observedDataFile:
        print("------------------")
        print("== OBSERVED DATA ==")
        print("------------------")
        print("ID: " + obj.id)
        print("Created: " + str(obj.created))
        print("Modified: " + str(obj.modified))
        print("Created by Ref: " + obj.created_by_ref)
        print("First Observed: " + str(obj.first_observed))
        print("Last Observed: " + str(obj.last_observed))
        print("Number Observed: " + str(obj.number_observed))
        print("Objects: " + str(obj.objects))

    elif obj == observedDataRegKey:
        print("------------------")
        print("== OBSERVED DATA ==")
        print("------------------")
        print("ID: " + obj.id)
        print("Created: " + str(obj.created))
        print("Modified: " + str(obj.modified))
        print("Created by Ref: " + obj.created_by_ref)
        print("First Observed: " + str(obj.first_observed))
        print("Last Observed: " + str(obj.last_observed))
        print("Number Observed: " + str(obj.number_observed))
        print("Objects: " + str(obj.objects))

    elif obj == sighting:
        print("------------------")
        print("== SIGHTING ==")
        print("------------------")
        print("ID: " + obj.id)
        print("Created: " + str(obj.created))
        print("Modified: " + str(obj.modified))
        print("Created by Ref: " + obj.created_by_ref)
        print("First Seen: " + str(obj.first_seen))
        print("Last Seen: " + str(obj.last_seen))
        print("Count: " + str(obj.count))
        print("Sighting of Ref: " + obj.sighting_of_ref)
        print("Where Sighted Refs: " + obj.where_sighted_refs[0])
```

### 威胁主体利用攻击模式和恶意软件

对威胁主体进行溯源和关联的很大一部分原因是为了更好地理解敌方行为，以确定应对措施并防御这类攻击。在很多情况下，敌方行为可以通过他们使用的攻击模式的类型来表征。例如，作为恶意软件的一种投递机制，使用鱼叉式网络钓鱼是一种攻击模式。在其它情况下，可以根据敌方通常使用的恶意软件来描述行为【9】。

##### 场景

这个场景表示一个被为“Adversary Bravo”的威胁主体，已知敌方Adversary Bravo使用钓鱼攻击来向目标投递远程访问的恶意软件。他们通常使用的是Poison Ivy恶意软件的变种。

##### 数据模型

敌方Bravo的任何已知特征和属性都可以使用威胁主体SDO进行建模。该对象捕捉威胁主体的特定信息，例如其它别名、攻击动机、攻击中他们可能扮演的角色。有时候，这些信息并不完全清楚，敌方Bravo就是这种情况。因此，你只需要指定威胁主体对象的必需属性，包含```name```（名称）和```labels```（标签）列表。```labels```（标签）字段根据开放词汇```threat-actor-label-ov```对威胁主体的类型进行分类。在这种情况下，我们可以推断，通过使用Poison Ivy恶意软件建立远程后门，敌方Bravo可能会执行恶意活动或间谍活动，从而会产生```criminal```（犯罪）和```spy```（间谍）标签。

有关敌方Bravo的其它基本标识信息是通过身份SDO获取的。在该场景下，此对象用于威胁主体的身份，但它也可以代表组织、政府和其它主体。这对于获取此身份可能所属的行业以及相关的联系信息很有用。在敌方Bravo案例中，关于这个身份的信息很少，所以基于身份类别开放列表所需的```identity_class```属性可能是未知的。身份SDO可以通过使用关系对象与威胁主体SDO进行连接。这两个对象之间的关系类型（由```relationship_type```字段表示）将包含```attributed-to```值，这意味着该威胁主体归属于此身份。

该场景中的恶意软件是Poison Ivy的变种d1c6，可以使用一个恶意软件SDO来表示。每个恶意软件对象都需要包含一系列标签来描述恶意软件。由于这是一种远程木马，因此```labels```（标签）属性将包含来自恶意软件标签开放词汇表的值```remote-access-trojan```。另外，你可以使用多个值标记恶意软件对象，因为有些恶意软件可能具有多个功能。例如，特定类型的恶意软件可能同时是键盘记录器和间谍软件。另一种关系可能在恶意软件SDO和威胁主体SDO之间建立。在这种情况下，两个对象之间使用```relationship_type```，威胁主体使用这个恶意软件。

敌方Bravo使用网络钓鱼作为恶意软件Poison Ivy的一种投递机制，这可以使用攻击模式SDO来表示。除了提供更多关于攻击主体想要做什么的上下文信息外，攻击模式对象对于表示分类很有用，例如CAPEC的```external_references```字段。CAPEC是潜在攻击模式的字典，所以在这种情况下，通过查看该字典，生产者可以看到“CAPEC-98”是网络钓鱼的ID，并且可以标记为```external_id```。关系SRO再次使用```relationship_type```将威胁主体连接到此次攻击模式对象。

在恶意软件和攻击模式对象中可以看到的另一个有用的概念是获取杀伤链信息的能力。例如，一旦系统受到攻击，某些攻击模式、恶意软件和工具可能用于建立立足点或横向移动。在这个例子中，由于攻击主体试图用Poison Ivy变种建立一个初级后门，Poison Ivy恶意软件和网络钓鱼攻击模式与杀伤链阶段```initial-compromis```e相关。此阶段来自于Mandiant攻击生命周期模型，但你不限于使用任何指定类型的杀伤链。

下图显示了此场景中使用的对象：

![STIX Example 24]({{site.CDN_PATH}}/public/image/20180424-STIX-Examples24.png)

##### 实现

JSON

```
{
  "type": "bundle",
  "id": "bundle--44af6c39-c09b-49c5-9de2-394224b04982",
  "spec_version": "2.0",
  "objects": [
    {
      "type": "attack-pattern",
      "id": "attack-pattern--8ac90ff3-ecf8-4835-95b8-6aea6a623df5",
      "created": "2015-05-07T14:22:14.760144Z",
      "modified": "2015-05-07T14:22:14.760144Z",
      "name": "Phishing",
      "description": "Spear phishing used as a delivery mechanism for malware.",
      "external_references": [
        {
          "source_name": "capec",
          "description": "phishing",
          "url": "https://capec.mitre.org/data/definitions/98.html",
          "external_id": "CAPEC-98"
        }
      ],
      "kill_chain_phases": [
        {
          "kill_chain_name": "mandiant-attack-lifecycle-model",
          "phase_name": "initial-compromise"
        }
      ]
    },
    {
      "type": "identity",
      "id": "identity--1621d4d4-b67d-11e3-9670-f01faf20d111",
      "created": "2015-05-10T16:27:17.760123Z",
      "modified": "2015-05-10T16:27:17.760123Z",
      "name": "Adversary Bravo",
      "description": "Adversary Bravo is a threat actor that utilizes phishing attacks",
      "identity_class": "unknown"
    },
    {
      "type": "threat-actor",
      "id": "threat-actor--9a8a0d25-7636-429b-a99e-b2a73cd0f11f",
      "created": "2015-05-07T14:22:14.760144Z",
      "modified": "2015-05-07T14:22:14.760144Z",
      "name": "Adversary Bravo",
      "description": "Adversary Bravo is known to use phishing attacks to deliver remote access malware to the targets.",
      "labels": [
        "spy",
        "criminal"
      ]
    },
    {
      "type": "malware",
      "id": "malware--d1c612bc-146f-4b65-b7b0-9a54a14150a4",
      "created": "2015-04-23T11:12:34.760122Z",
      "modified": "2015-04-23T11:12:34.760122Z",
      "name": "Poison Ivy Variant d1c6",
      "labels": [
        "remote-access-trojan"
      ],
      "kill_chain_phases": [
        {
          "kill_chain_name": "mandiant-attack-lifecycle-model",
          "phase_name": "initial-compromise"
        }
      ]
    },
    {
      "type": "relationship",
      "id": "relationship--ad4bccee-1ed3-44f5-9a56-8085584d3360",
      "created": "2015-05-07T14:22:14.760144Z",
      "modified": "2015-05-07T14:22:14.760144Z",
      "relationship_type": "uses",
      "source_ref": "threat-actor--9a8a0d25-7636-429b-a99e-b2a73cd0f11f",
      "target_ref": "malware--d1c612bc-146f-4b65-b7b0-9a54a14150a4"
    },
    {
      "type": "relationship",
      "id": "relationship--e05a50c3-a557-4d5f-ac19-e3f0859171cc",
      "created": "2015-05-07T14:22:14.760144Z",
      "modified": "2015-05-07T14:22:14.760144Z",
      "relationship_type": "uses",
      "source_ref": "threat-actor--9a8a0d25-7636-429b-a99e-b2a73cd0f11f",
      "target_ref": "attack-pattern--8ac90ff3-ecf8-4835-95b8-6aea6a623df5"
    },
    {
      "type": "relationship",
      "id": "relationship--bdcef81d-9dfa-4f5d-a7e5-7ab13b695495",
      "created": "2015-05-07T14:22:14.760144Z",
      "modified": "2015-05-07T14:22:14.760144Z",
      "relationship_type": "attributed-to",
      "source_ref": "threat-actor--9a8a0d25-7636-429b-a99e-b2a73cd0f11f",
      "target_ref": "identity--1621d4d4-b67d-11e3-9670-f01faf20d111"
    }
  ]
}
```

Python 生产者

```
import stix2

threat_actor = stix2.ThreatActor(
    id="threat-actor--9a8a0d25-7636-429b-a99e-b2a73cd0f11f",
    created="2015-05-07T14:22:14.760Z",
    modified="2015-05-07T14:22:14.760Z",
    name="Adversary Bravo",
    description="Adversary Bravo is known to use phishing attacks to deliver remote access malware to the targets.",
    labels=["spy", "criminal"]
)

identity = stix2.Identity(
    id="identity--1621d4d4-b67d-11e3-9670-f01faf20d111",
    created="2015-05-10T16:27:17.760Z",
    modified="2015-05-10T16:27:17.760Z",
    name="Adversary Bravo",
    description="Adversary Bravo is a threat actor that utilizes phishing attacks.",
    identity_class="unknown"
)

init_comp = stix2.KillChainPhase(
    kill_chain_name="mandiant-attack-lifecycle-model",
    phase_name="initial-compromise"
)

malware = stix2.Malware(
    id="malware--d1c612bc-146f-4b65-b7b0-9a54a14150a4",
    created="2015-04-23T11:12:34.760Z",
    modified="2015-04-23T11:12:34.760Z",
    name="Poison Ivy Variant d1c6",
    labels=["remote-access-trojan"],
    kill_chain_phases=[init_comp]
)

ref = stix2.ExternalReference(
    source_name="capec",
    description="phishing",
    url="https://capec.mitre.org/data/definitions/98.html",
    external_id="CAPEC-98"
)

attack_pattern = stix2.AttackPattern(
    id="attack-pattern--8ac90ff3-ecf8-4835-95b8-6aea6a623df5",
    created="2015-05-07T14:22:14.760Z",
    modified="2015-05-07T14:22:14.760Z",
    name="Phishing",
    description="Spear phishing used as a delivery mechanism for malware.",
    kill_chain_phases=[init_comp],
    external_references=[ref]
)

relationship1 = stix2.Relationship(threat_actor, 'uses', malware)
relationship2 = stix2.Relationship(threat_actor, 'uses', attack_pattern)
relationship3 = stix2.Relationship(threat_actor, 'attributed-to', identity)

bundle = stix2.Bundle(objects=[threat_actor, malware, attack_pattern, identity, relationship1, relationship2, relationship3])
```

Python 消费者

```
import stix2

for obj in bundle.objects:
    if obj == threat_actor:
        print("------------------")
        print("== THREAT ACTOR ==")
        print("------------------")
        print("ID: " + obj.id)
        print("Created: " + str(obj.created))
        print("Modified: " + str(obj.modified))
        print("Name: " + obj.name)
        print("Description: " + obj.description)
        print("Labels: " + obj.labels[0] + ", " + obj.labels[1])

    elif obj == identity:
        print("------------------")
        print("== IDENTITY ==")
        print("------------------")
        print("ID: " + obj.id)
        print("Created: " + str(obj.created))
        print("Modified: " + str(obj.modified))
        print("Name: " + obj.name)
        print("Description: " + obj.description)
        print("Identity Class: " + obj.identity_class)

    elif obj == malware:
        print("------------------")
        print("== MALWARE ==")
        print("------------------")
        print("ID: " + obj.id)
        print("Created: " + str(obj.created))
        print("Modified: " + str(obj.modified))
        print("Name: " + obj.name)
        print("Labels: " + obj.labels[0])
        print("Kill Chain: " + str(obj.kill_chain_phases))

    elif obj == attack_pattern:
        print("------------------")
        print("== ATTACK PATTERN ==")
        print("------------------")
        print("ID: " + obj.id)
        print("Created: " + str(obj.created))
        print("Modified: " + str(obj.modified))
        print("Name: " + obj.name)
        print("Description: " + obj.description)
        print("Kill Chain: " + str(obj.kill_chain_phases))
        print("External References: " + str(obj.external_references))

    elif obj == relationship1:
        print("------------------")
        print("== RELATIONSHIP ==")
        print("------------------")
        print("ID: " + obj.id)
        print("Created: " + str(obj.created))
        print("Modified: " + str(obj.modified))
        print("Relationship Type: " + obj.relationship_type)
        print("Source Ref: " + obj.source_ref)
        print("Target Ref: " + obj.target_ref)

    elif obj == relationship2:
        print("------------------")
        print("== RELATIONSHIP ==")
        print("------------------")
        print("ID: " + obj.id)
        print("Created: " + str(obj.created))
        print("Modified: " + str(obj.modified))
        print("Relationship Type: " + obj.relationship_type)
        print("Source Ref: " + obj.source_ref)
        print("Target Ref: " + obj.target_ref)

    elif obj == relationship3:
        print("------------------")
        print("== RELATIONSHIP ==")
        print("------------------")
        print("ID: " + obj.id)
        print("Created: " + str(obj.created))
        print("Modified: " + str(obj.modified))
        print("Relationship Type: " + obj.relationship_type)
        print("Source Ref: " + obj.source_ref)
        print("Target Ref: " + obj.target_ref)
```

### 使用标记定义

能够通过使用数据标记来构建数据处理，对于共享网络威胁情报（CTI）的组织至关重要。这种方法的好处是允许STIX生产者限制对象的访问，并传达使用条款和版权信息【10】。

##### 场景

此场景重点关注STIX生产者“Stark Industries”，其在攻击指标对象上添加对象标记。在共享此指标之前，Stark会创建一个“声明”标记定义并选择“交通灯协议”（TLP）标记定义。这些标记包含版权信息，另外，基于其TLP标记类型可限制攻击指标的使用。

##### 数据模型

首先，我们从这个场景的STIX内容生产者Stark Industries开始。与该公司有关的信息可以使用身份SDO来表示。与所有STIX对象一样，```id```属性唯一标识Stark Industries，并且可以在生成的所有对象中使用```created_by_ref```属性来引用他们。虽然```created_by_ref```是可选的，但这有助于将创建的标签直接归属于Stark。身份对象也可用于列出有关Stark的其它相关详细信息，例如，```contact_information```和它们```identity_class```字段的身份类型。

接下来，Stark使用两个STIX标记定义对象来限制对攻击指标对象的处理，并且包含版权信息。首先，Stark选择TLP标记对象类型来为该攻击指标传达适当的限制。对于此标记定义对象，```definition_type```必须是```tlp```，并且```definition```（定义）字段必须包含TLP的四种类型之一。在本例中，TLP限制类型为```amber```，这仅向又需要知道的合适接收者提供有限的披露。要了解此限制和其他类型的TLP，请查看US-CERT的TLP定义和使用。

由Stark Industries创建的称为Statement的第二种标记类型，用于表示其版权信息并应用于它们生成的所有对象。这与TLP标记定义对象的格式类似，除了这种情况下```definition_type```必须是```statement```，并且存在```created_by_ref```字段，因为TLP已经在STIX 2.0规范中预先定义。```definition```（定义）字段包含你想传达的任何类型的版权信息。对于这个组织，它只是声明版权@ Stark Industries 2017。这个属性还可以传达任何使用条款，或者因为Statement允许多种标记类型，所以你可以同时使用这两个条款。

值得注意的一点是，标记定义对象不能像其他STIX对象那样进行版本化。例如，如果Stark Industries希望更新他们的```Statement```信息，或者将条款添加到标记定义中，他们不得不生成一个新的标志定义对象，并更新攻击指标SDO以指向这个新的定义。他们无法添加或更改当前的语句标记，只是像修改其他对象一样更新```modified```（修改）的属性，因为标记定义对象没有必需的```modified```（修改）属性。要了解有关版本控制对象的更多信息，请查看这个有关如何在STIX 2中使用版本控制的教程视频。

最后，Stark可以将这些标记定义应用于包含他们在网络上发现的恶意IP地址攻击指标SDO。这些对象标记嵌入在攻击指标对象的```object_marking_refs```属性中，并引用Statement和TLP的标记定义对象的ID。一旦引用，这些标记就应用到攻击指标对象。值得一提的是，此属性和之前介绍的```created_by_ref```仅代表STIX 2.0中的几个嵌入关系之一。在大多数情况下，为了在STIX中建立对象之间的关系，比如在攻击指标和威胁主体SDO之间，你可以创建关系SRO。

除了对象标记引用外，攻击指标对象的其余部分还包含有关IP地址的详细信息的属性。例如，```pattern```（模式）属性基于STIX模式语言，并将IPv4地址表示为比较表达式：[ipv4-addr:value = '10.0.0.0']。Stark也知道这个是一个恶意IP，并将这些信息与```labels```（标签）属性进行关联，表明该IP与```malicious-activity```（恶意行为）相关。由于这是网络上存在的已知不良IP，因此Stark能够将该攻击指标使用适当的TLP标记定义是有利的。

下图介绍了身份和攻击指标SDO以及标记定义对象：

![STIX Example 25]({{site.CDN_PATH}}/public/image/20180424-STIX-Examples25.png)

##### 实现

JSON

```
{
  "type": "bundle",
  "id": "bundle--b56c1e2e-a40c-44ca-83dd-09e25936d273",
  "spec_version": "2.0",
  "objects": [
    {
      "type": "identity",
      "id": "identity--611d9d41-dba5-4e13-9b29-e22488058ffc",
      "created": "2017-04-14T13:07:49.812Z",
      "modified": "2017-04-14T13:07:49.812Z",
      "name": "Stark Industries",
      "identity_class": "organization",
      "contact_information": "info@stark.com",
      "sectors": [
        "defence"
      ]
    },
    {
      "type": "marking-definition",
      "id": "marking-definition--f88d31f6-486f-44da-b317-01333bde0b82",
      "created": "2017-01-20T00:00:00.000Z",
      "definition_type": "tlp",
      "definition": {
        "tlp": "amber"
      }
    },
    {
      "type": "marking-definition",
      "id": "marking-definition--d771aceb-3148-4315-b4b4-130b888533d0",
      "created": "2017-04-14T13:07:49.812Z",
      "created_by_ref": "identity--611d9d41-dba5-4e13-9b29-e22488058ffc",
      "definition_type": "statement",
      "definition": {
        "statement": "Copyright © Stark Industries 2017."
      }
    },
    {
      "type": "indicator",
      "id": "indicator--33fe3b22-0201-47cf-85d0-97c02164528d",
      "created": "2017-04-14T13:07:49.812Z",
      "modified": "2017-04-14T13:07:49.812Z",
      "created_by_ref": "identity--611d9d41-dba5-4e13-9b29-e22488058ffc",
      "name": "Known malicious IP Address",
      "labels": [
        "malicious-activity"
      ],
      "pattern": "[ipv4addr:value = '10.0.0.0']",
      "valid_from": "2017-04-14T13:07:49.812Z",
      "object_marking_refs": [
        "marking-definition--f88d31f6-486f-44da-b317-01333bde0b82",
        "marking-definition--d771aceb-3148-4315-b4b4-130b888533d0"
      ]
    }
  ]
}
```

Python 生产者

```
import stix2

identity = stix2.Identity(
    id="identity--611d9d41-dba5-4e13-9b29-e22488058ffc",
    created="2017-04-14T13:07:49.812Z",
    modified="2017-04-14T13:07:49.812Z",
    name="Stark Industries",
    contact_information="info@stark.com",
    identity_class="organisation",
    sectors=["defence"]
)

marking_def_amber = stix2.MarkingDefinition(
    id="marking-definition--f88d31f6-486f-44da-b317-01333bde0b82",
    created="2017-01-20T00:00:00.000Z",
    definition_type="tlp",
    definition={
        "tlp": "amber"
    }
)

marking_def_statement = stix2.MarkingDefinition(
    id="marking-definition--d81f86b9-975b-bc0b-775e-810c5ad45a4f",
    created="2017-04-14T13:07:49.812Z",
    definition_type="statement",
    definition=stix2.StatementMarking("Copyright (c) Stark Industries 2017.")
)

indicator = stix2.Indicator(
    id="indicator--33fe3b22-0201-47cf-85d0-97c02164528d",
    created="2017-04-14T13:07:49.812Z",
    modified="2017-04-14T13:07:49.812Z",
    created_by_ref="identity--611d9d41-dba5-4e13-9b29-e22488058ffc",
    name="Known malicious IP Address",
    labels=["malicious-activity"],
    pattern="[ipv4-addr:value = '10.0.0.0']",
    valid_from="2017-04-14T13:07:49.812Z",
    object_marking_refs=[marking_def_amber, marking_def_statement]
)

bundle = stix2.Bundle(objects=[identity, indicator, marking_def_amber, marking_def_statement])
```


Python 消费者

```
import stix2

for obj in bundle.objects:
    if obj == identity:
        print("------------------")
        print("== IDENTITY ==")
        print("------------------")
        print("ID: " + obj.id)
        print("Created: " + str(obj.created))
        print("Modified: " + str(obj.modified))
        print("Name: " + obj.name)
        print("Identity Class: " + obj.identity_class)
        print("Contact Information: " + obj.contact_information)
        print("Sectors: " + str(obj.sectors))

    elif obj == indicator:
        print("------------------")
        print("== INDICATOR ==")
        print("------------------")
        print("ID: " + obj.id)
        print("Created: " + str(obj.created))
        print("Modified: " + str(obj.modified))
        print("Created by Ref: " + obj.created_by_ref)
        print("Name: " + obj.name)
        print("Labels: " + obj.labels[0])
        print("Pattern: " + obj.pattern)
        print("Valid From: " + str(obj.valid_from))
        print("Object Marking Refs: " + str(obj.object_marking_refs))

    elif obj == marking_def_amber:
        print("------------------")
        print("== MARKING DEFINITION ==")
        print("------------------")
        print("ID: " + obj.id)
        print("Created: " + str(obj.created))
        print("Definition Type: " + obj.definition_type)
        print("Definition: " + str(obj.definition))

    elif obj == marking_def_statement:
        print("------------------")
        print("== MARKING DEFINITION ==")
        print("------------------")
        print("ID: " + obj.id)
        print("Created: " + str(obj.created))
        print("Definition Type: " + obj.definition_type)
        print("Definition: " + str(obj.definition))
```

### 使用粒度标记

对分享哪些网络威胁情报进行更细致、更细化的限制，这对于那些不愿共享某些特定信息的组织是有益的。这种控制要素允许STIX生产者将特定数据的可访问性在其共享情报的组织中进行限制【11】。

##### 场景

此场景重点关注STIX生产者“Gotham National Bank”，其在攻击指标对象中施加了粒度标记。在分享这个指标之前，Gotham选择一些适用于该攻击指标的TLP标记定义。基于TLP标记类型的这些标记定义有助于限制攻击指标的某些属性的使用。

##### 数据模型

本场景中的STIX对象生产者 Gotham National Bank，可以使用身份SDO表示。与所有STIX对象一样，```id```属性唯一标识了Gotham National，并且可以在他们生成的所有对象中使用```created_by_ref```属性进行利用。虽然```created_by_ref```是可选的，但它有助于将攻击指标SDO直接归属到Gotham，并允许任何消费者查看谁将TLP标记应用于攻击指标。身份对象也可用于列出有关Gotham的其它相关信息，例如```contact_information```和他们与```identity_class```字段的身份类型。

为了强制对攻击指标对象的特定属性进行限制，Gotham决定使用TLP标记定义对象。这种特殊的标记定义类型可以在TLP标记对象类型下的STIX 2.0规范中看到，有助于指定他们要施加的限制类型。例如，在这种情况下，他们需要使用三个定义的TLP标记定义，每个都有不同的限制类型。对于所有这些对象，```definition_type```是必须的，并且必须是TLP。另外，```definition```（定义）属性也是必需的，并且必需包含四种类型TLP中的一种。Gotham需要TLP定义的四种类型中的三种：```green```，```amber```和 ```red```。要了解四种TLP类型中的每一种，以及它们指定的限制条件，请查看US-CERT的TLP定义和用法。了解这些类型对于您希望对对象和对象属性提供的限制级别很有用。

值得注意的一点是，必须使用STIX 2.0规范中定义的TLP标记对象类型来表示TLP标记。Gotham或任何其他生产者不能创建自己的TLP标记，但可以创建```organization-specifi```的```Statement```标记对象类型。这两种类型```TLP```和```Statement```都不能像其他STIX对象那样版本化，这就是为什么这些类型没有```modified```（修改）属性的原因。要了解有关版本控制对象的更多信息，请查看这篇关于如何在STIX 2中使用版本控制的视频教程。

现在Gotham已经选择了合适的TLP标记对象类型，它们可以应用于其它STIX对象或对象的属性。在这个场景的第一部分，它们被附加到Gotham生成的攻击指标SDO的属性上。创建该攻击指标是为了表示一个虚假电子邮件地址，该电子邮件地址向银行会员索取其凭证。由于该攻击指标中包含一些敏感的信息，他们使用```granular_markings```属性将不同的TLP标记应用于此对象的各个部分。该属性是一个列表，其中包含具有```marking_ref```字段的标记定义对象```ID```的引用和一个```selector```（选择器）属性，该属性指定应使用此标记定义的标记内容。例如，Gotham认为需要在```description```（描述）字段应用严格的TLP：```Red```标记，因为这会在此场景中提供有关威胁主体的某些敏感信息。为了进一步说明，在攻击指标的```granular_markings```字段中，```marking_ref```属性将包含TLP标记定义ID：```Red```和```selectors```（选择器）列表将包含值描述。

对于其他攻击指标属性，Gotham使用的限制性更少的标记：```Red```。一个必需的攻击指标属性是一个```labels```（标签）列表，它提供了更多有关建模指标类型的上下文内容。在此属性列表中，Gotham将此攻击指标标记为```malicious-activity```和```attribution```。列表中的两个标签都来自STIX 2.0规范的攻击指标开放词汇表。Gotham决定应用较低限制级的TLP：```Green```标记应用于```malicious-activity```，并认为TLP更具限制性；```attribution```需要Amber标记。为了在```labels```（标签）属性中传达两个不同的标记，列表中的第一个标签被表示为```labels[0]```，第二个标签被表示为```labels[1]```。为了说明这一点，下面的JSON示例显示了如果仅仅标记```labels```（标签）字段，这些标记将如何标记（注意：标记以“f88…”开头的定义ID为TLP：```Amber``` ，以“340…”开头的ID为TLP：```Green```）

```
{
"granular_markings": [  
  {
    "marking_ref": "marking-definition--f88d31f6-486f-44da-b317-01333bde0b82",
    "selectors": [    
      "labels.[1]"
    ]
  },
  {
    "marking_ref": "marking-definition--34098fce-860f-48ae-8e50-ebd3cc5e41da",
    "selectors": [
      "labels.[0]",      
    ]
  }
]}

```

除了这些攻击指标```labels```（标签），Gotham选择将属性```name```（名称）和```pattern```（模式）标记为TLP： ```Green```，他们可以标记他们想要的任何属性，但不能标记无效属性，如```labels[3]```或```kill_chain_phases[0]```，因为它们目前不存在于此攻击指标SDO中。

Gotham还创建了一个威胁主体SDO来捕获此攻击指标指示的威胁主体的信息。在这个例子中，名为Joker的威胁主体被归属到假邮件攻击指标。除了名称以外，该对象还有助于构建关于Joker的其他信息，例如```aliases```（别名）、```roles```（角色）和```primary_motivation```。由于所有这些情报都被认为对Gotham National敏感，因此他们使用对象标记，而不是粒度标记，将整个对象标记为TLP： ```Red```。这是通过所有SDO和SRO中固有的属性（```object_marking_refs```）完成的。该属性列出了适用于此对象的所有标记定义ID。与适用于威胁主体中不同字段的```granular_markings```属性不同，```object_marking_refs```适用于整个威胁主体SDO。

这种情况下，最后一个情报是一个关系SRO，它将攻击指标和威胁主体SDO连接在一起。在此关系中，```relationship_type```属性指定此攻击指标指示威胁主体。由于这个关系对象连接到一个TLP：```Red```标记的对象，Gotham也使用关系内的```object_marking_refs```字段将其标记为TLP：```Red```。

完整的JSON表示形式可以在本示例的最后看到，下面说明该场景的示意图：

![STIX Example 26]({{site.CDN_PATH}}/public/image/20180424-STIX-Examples26.png)

##### 实现

JSON

```
{
  "type": "bundle",
  "id": "bundle--963410f2-fd7d-4d80-937c-8ad3aed5f432",
  "spec_version": "2.0",
  "objects": [
    {
      "type": "identity",
      "id": "identity--b38dfe21-7477-40d1-aa90-5c8671ce51ca",
      "created": "2017-04-27T16:18:24.318Z",
      "modified": "2017-04-27T16:18:24.318Z",
      "name": "Gotham National Bank",
      "identity_class": "organization",
      "contact_information": "contact@gothamnational.com",
      "sectors": [
        "financial-services"
      ]
    },
    {
      "type": "threat-actor",
      "id": "threat-actor--8b6297fe-cae7-47c6-9256-5584b417849c",
      "created": "2017-04-27T16:18:24.318Z",
      "modified": "2017-04-27T16:18:24.318Z",
      "created_by_ref": "identity--b38dfe21-7477-40d1-aa90-5c8671ce51ca",
      "name": "The Joker",
      "labels": [
        "criminal",
        "terrorist"
      ],
      "aliases": [
        "Joe Kerr",
        "The Clown Prince of Crime"
      ],
      "roles": [
        "director"
      ],
      "resource_level": "team",
      "primary_motivation": "personal-satisfaction",
      "object_marking_refs": [
        "marking-definition--5e57c739-391a-4eb3-b6be-7d15ca92d5ed"
      ]
    },
    {
      "type": "marking-definition",
      "id": "marking-definition--34098fce-860f-48ae-8e50-ebd3cc5e41da",
      "created": "2017-01-20T00:00:00.000Z",
      "definition_type": "tlp",
      "definition": {
        "tlp": "green"
      }
    },
    {
      "type": "marking-definition",
      "id": "marking-definition--f88d31f6-486f-44da-b317-01333bde0b82",
      "created": "2017-01-20T00:00:00.000Z",
      "definition_type": "tlp",
      "definition": {
        "tlp": "amber"
      }
    },
    {
      "type": "marking-definition",
      "id": "marking-definition--5e57c739-391a-4eb3-b6be-7d15ca92d5ed",
      "created": "2017-01-20T00:00:00.000Z",
      "definition_type": "tlp",
      "definition": {
        "tlp": "red"
      }
    },
    {
      "type": "indicator",
      "id": "indicator--1ed8caa7-a708-4706-b651-f1186ede6ca1",
      "created": "2017-04-27T16:18:24.318Z",
      "modified": "2017-04-27T16:18:24.318Z",
      "created_by_ref": "identity--b38dfe21-7477-40d1-aa90-5c8671ce51ca",
      "name": "Fake email address",
      "description": "Known to be used by The Joker.",
      "labels": [
        "malicious-activity",
        "attribution"
      ],
      "pattern": "[email-message:from_ref.value MATCHES '.+\\\\banking@g0thamnatl\\\\.com$']",
      "valid_from": "2017-04-27T16:18:24.318Z",
      "granular_markings": [
        {
          "marking_ref": "marking-definition--5e57c739-391a-4eb3-b6be-7d15ca92d5ed",
          "selectors": [
            "description"
          ]
        },
        {
          "marking_ref": "marking-definition--f88d31f6-486f-44da-b317-01333bde0b82",
          "selectors": [
            "labels.[1]"
          ]
        },
        {
          "marking_ref": "marking-definition--34098fce-860f-48ae-8e50-ebd3cc5e41da",
          "selectors": [
            "labels.[0]",
            "name",
            "pattern"
          ]
        }
      ]
    },
    {
      "type": "relationship",
      "id": "relationship--3d1dd3cc-eb47-4704-9c77-ceff2971b95c",
      "created": "2017-04-27T16:18:24.318Z",
      "modified": "2017-04-27T16:18:24.318Z",
      "relationship_type": "indicates",
      "object_marking_refs": [
        "marking-definition--5e57c739-391a-4eb3-b6be-7d15ca92d5ed"
      ],
      "source_ref": "indicator--1ed8caa7-a708-4706-b651-f1186ede6ca1",
      "target_ref": "threat-actor--8b6297fe-cae7-47c6-9256-5584b417849c"
    }
  ]
}
```

Python 生产者

```
import stix2

granular_red = stix2.GranularMarking(
        marking_ref=stix2.TLP_RED.id,
        selectors=["description"]
)

granular_amber = stix2.GranularMarking(
        marking_ref=stix2.TLP_AMBER.id,
        selectors=["labels.[1]"]
)

granular_green = stix2.GranularMarking(
        marking_ref=stix2.TLP_GREEN.id,
        selectors=["labels.[0]", "name", "pattern"]
)

identity = stix2.Identity(
    id="identity--b38dfe21-7477-40d1-aa90-5c8671ce51ca",
    created="2017-04-27T16:18:24.318Z",
    modified="2017-04-27T16:18:24.318Z",
    name="Gotham National Bank",
    contact_information="contact@gothamnational.com",
    identity_class="organisation",
    sectors=["financial-services"]
)

threat_actor = stix2.ThreatActor(
    id="threat-actor--8b6297fe-cae7-47c6-9256-5584b417849c",
    created="2017-04-27T16:18:24.318Z",
    modified="2017-04-27T16:18:24.318Z",
    created_by_ref="identity--b38dfe21-7477-40d1-aa90-5c8671ce51ca",
    name="The Joker",
    labels=["terrorist", "criminal"],
    aliases=["Joe Kerr", "The Clown Prince of Crime"],
    roles=["director"],
    resource_level="team",
    primary_motivation="personal-satisfaction",
    object_marking_refs=[stix2.TLP_RED]
)

indicator = stix2.Indicator(
    id="indicator--1ed8caa7-a708-4706-b651-f1186ede6ca1",
    created="2017-04-27T16:18:24.318Z",
    modified="2017-04-27T16:18:24.318Z",
    created_by_ref="identity--b38dfe21-7477-40d1-aa90-5c8671ce51ca",
    name="Fake email address",
    description="Known to be used by The Joker.",
    labels=["malicious-activity", "attribution"],
    pattern="[email-message:from_ref.value MATCHES '.+\\\\banking@g0thamnatl\\\\.com$']",
    valid_from="2017-04-27T16:18:24.318Z",
    granular_markings=[granular_red, granular_amber, granular_green]
)

rel = stix2.Relationship(
    id="relationship--3d1dd3cc-eb47-4704-9c77-ceff2971b95c",
    created="2017-04-27T16:18:24.318Z",
    modified="2017-04-27T16:18:24.318Z",
    relationship_type='indicates',
    source_ref="indicator--1ed8caa7-a708-4706-b651-f1186ede6ca1",
    target_ref="threat-actor--8b6297fe-cae7-47c6-9256-5584b417849c",
    object_marking_refs=[stix2.TLP_RED]
)

bundle = stix2.Bundle(objects=[identity, indicator, threat_actor, rel])
```

Python 消费者

```
import stix2

for obj in bundle.objects:
    if obj == threat_actor:
        print("------------------")
        print("== THREAT ACTOR ==")
        print("------------------")
        print("ID: " + obj.id)
        print("Created: " + str(obj.created))
        print("Modified: " + str(obj.modified))
        print("Created by Ref:" + obj.created_by_ref)
        print("Name: " + obj.name)
        print("Labels: " + obj.labels[0] + ", " + obj.labels[1])
        print("Aliases: " + obj.aliases[0] + ", " + obj.aliases[1])
        print("Roles: " + str(obj.roles))
        print("Resource Level: " + obj.resource_level)
        print("Primary Motivation: " + obj.primary_motivation)
        print("Object Marking Refs: " + str(obj.object_marking_refs))

    elif obj == identity:
        print("------------------")
        print("== IDENTITY ==")
        print("------------------")
        print("ID: " + obj.id)
        print("Created: " + str(obj.created))
        print("Modified: " + str(obj.modified))
        print("Name: " + obj.name)
        print("Identity Class: " + obj.identity_class)
        print("Contact Information: " + obj.contact_information)
        print("Sectors: " + str(obj.sectors))

    elif obj == indicator:
        print("------------------")
        print("== INDICATOR ==")
        print("------------------")
        print("ID: " + obj.id)
        print("Created: " + str(obj.created))
        print("Modified: " + str(obj.modified))
        print("Created by Ref:" + obj.created_by_ref)
        print("Name: " + obj.name)
        print("Description: " + obj.description)
        print("Labels: " + obj.labels[0])
        print("Pattern: " + obj.pattern)
        print("Valid From: " + str(obj.valid_from))
        print("Granular Markings: " + str(obj.granular_markings))

    elif obj == rel:
        print("------------------")
        print("== RELATIONSHIP ==")
        print("------------------")
        print("ID: " + obj.id)
        print("Created: " + str(obj.created))
        print("Modified: " + str(obj.modified))
        print("Relationship Type: " + obj.relationship_type)
        print("Source Ref: " + obj.source_ref)
        print("Target Ref: " + obj.target_ref)
        print("Object Marking Refs: " + str(obj.object_marking_refs))
```

### 可视化的STIX字段对象关系

本示例变了所有SDO，并直观地表示每个SDO和另一个SDO之间的可能关系。一图胜千言，所有的图形均使用Graphviz创建【12】。

##### 所有关系概述

下图显示了每个SDO如何适应整个SDO生态系统的可能关系。

注意：报告和可观测数据没有任何关系

![STIX Example 27]({{site.CDN_PATH}}/public/image/20180424-STIX-Examples27.png)

##### SDO关系

攻击模式

![STIX Example 28]({{site.CDN_PATH}}/public/image/20180424-STIX-Examples28.png)

攻击活动

![STIX Example 29]({{site.CDN_PATH}}/public/image/20180424-STIX-Examples29.png)

应对措施

![STIX Example 30]({{site.CDN_PATH}}/public/image/20180424-STIX-Examples30.png)

身份

![STIX Example 31]({{site.CDN_PATH}}/public/image/20180424-STIX-Examples31.png)

攻击指标

![STIX Example 32]({{site.CDN_PATH}}/public/image/20180424-STIX-Examples32.png)

入侵集合

![STIX Example 33]({{site.CDN_PATH}}/public/image/20180424-STIX-Examples33.png)

恶意软件

![STIX Example 34]({{site.CDN_PATH}}/public/image/20180424-STIX-Examples34.png)

威胁主体

![STIX Example 35]({{site.CDN_PATH}}/public/image/20180424-STIX-Examples35.png)

工具

![STIX Example 36]({{site.CDN_PATH}}/public/image/20180424-STIX-Examples36.png)

脆弱性

![STIX Example 37]({{site.CDN_PATH}}/public/image/20180424-STIX-Examples37.png)

# 关键词翻译

* CTI Cyber Threat Intelligence 网络空间威胁情报

* Campaigns 攻击活动

* Indicator 攻击指标

* Sighting 瞄准

* Observed-data 可观测数据

* Course of Action 应对措施

* Indentity 身份

* Indicator 攻击指标

* Instrusion Set 入侵集合

* Malware 恶意软件

* Observed Data 可观测数据

* Report 报告

* Threat Actor 威胁主体

* Tool 工具

* Vulnerability 脆弱性

* SDOs STIX Domain Objects STIX字段对象

* SROs STIX Relationship Objects STIX关系对象

* TTP Tactics, Techniques, and Procedures 攻击方法

* APT Advanced Persistent Threat 高级可持续威胁

* CAPEC Common Attack Pattern Enumeration and Classification 通用攻击模式枚举和分类

# 参考

【1】Introduction to STIX，https://oasis-open.github.io/cti-documentation/stix/intro

【2】STIX 2.0 Examples，https://oasis-open.github.io/cti-documentation/stix/examples

【3】Identifying a Threat Actor Profile，https://oasis-open.github.io/cti-documentation/examples/identifying-a-threat-actor-profile

【4】Defining Campaigns vs. Threat Actors vs. Intrusion Sets，https://oasis-open.github.io/cti-documentation/examples/defining-campaign-ta-is

【5】Indicator for Malicious URL，https://oasis-open.github.io/cti-documentation/examples/indicator-for-malicious-url

【6】Malware Indicator for File Hash，https://oasis-open.github.io/cti-documentation/examples/malware-indicator-for-file-hash

【7】Sighting of an Indicator，https://oasis-open.github.io/cti-documentation/examples/sighting-of-an-indicator

【8】Sighting of Observed-data，https://oasis-open.github.io/cti-documentation/examples/sighting-of-observed-data

【9】Threat Actor Leveraging Attack Patterns and Malware，https://oasis-open.github.io/cti-documentation/examples/threat-actor-leveraging-attack-patterns-and-malware

【10】Using Marking Definitions，https://oasis-open.github.io/cti-documentation/examples/using-marking-definitions

【11】Using Granular Markings，https://oasis-open.github.io/cti-documentation/examples/using-granular-markings

【12】Visualized SDO Relationships，https://oasis-open.github.io/cti-documentation/examples/visualized-sdo-relationships