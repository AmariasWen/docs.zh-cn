---
title: ADO.NET 实体框架中的连接字符串
ms.date: 10/15/2018
ms.assetid: 78d516bc-c99f-4865-8ff1-d856bc1a01c0
ms.openlocfilehash: 99b6b1b7a38477dc17d3960ee5bc0b63ec0cb819
ms.sourcegitcommit: e42d09e5966dd9fd02847d3e7eeb4ec0877069f8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2018
ms.locfileid: "49372481"
---
# <a name="connection-strings-in-the-adonet-entity-framework"></a>ADO.NET 实体框架中的连接字符串
连接字符串包含作为参数从数据提供程序传递到数据源的初始化信息。 其语法取决于数据提供程序，并且会在试图打开连接的过程中对连接字符串进行分析。 实体框架使用的连接字符串包含用于连接到支持实体框架的基础 ADO.NET 数据提供程序的信息。 它们还包含有关所需的模型和映射文件的信息。  
  
 在访问模型，映射元数据以及连接到数据源时，EntityClient 提供程序将使用该连接字符串。 连接字符串可通过 <xref:System.Data.EntityClient.EntityConnection.ConnectionString%2A> 的 <xref:System.Data.EntityClient.EntityConnection> 属性访问或设置。 <xref:System.Data.EntityClient.EntityConnectionStringBuilder> 类可用于以编程方式构造或访问连接字符串中的参数。 有关详细信息，请参阅[如何： 生成 EntityConnection 连接字符串](../../../../../docs/framework/data/adonet/ef/how-to-build-an-entityconnection-connection-string.md)。  
  
 [Entity Data Model 工具](https://msdn.microsoft.com/library/91076853-0881-421b-837a-f582f36be527)生成应用程序的配置文件中存储的连接字符串。 在创建对象查询时，<xref:System.Data.Objects.ObjectContext> 将自动检索此连接信息。 可通过 <xref:System.Data.EntityClient.EntityConnection> 属性访问 <xref:System.Data.Objects.ObjectContext> 实例所使用的 <xref:System.Data.Objects.ObjectContext.Connection%2A>。 有关详细信息，请参阅[管理连接和事务](https://msdn.microsoft.com/library/b6659d2a-9a45-4e98-acaa-d7a8029e5b99)。  

## <a name="connection-string-syntax"></a>连接字符串语法

若要了解有关连接字符串的常规语法，请参阅[连接字符串语法 |在 ADO.NET 中的连接字符串](../connection-strings.md#connection-string-syntax)。

## <a name="connection-string-parameters"></a>连接字符串参数  

下表列出了 <xref:System.Data.EntityClient.EntityConnection.ConnectionString%2A> 中的关键字值的有效名称。  
  
|关键字|描述|  
|-------------|-----------------|  
|`Provider`|此关键字在未指定 `Name` 关键字时是必需的。 提供程序名称，用于检索基础提供程序的 <xref:System.Data.Common.DbProviderFactory> 对象。 该值为常量。<br /><br /> 如果实体连接字符串中未包含 `Name` 关键字，则需要一个非空 `Provider` 关键字值。 此关键字与 `Name` 关键字互斥。|  
|`Provider Connection String`|可选。 指定要传递给基础数据源的提供程序特定的连接字符串。 此连接字符串包含有效的关键字/值对的数据提供程序。 如果 `Provider Connection String` 无效，则当数据源计算此字符串时，将导致运行时错误。<br /><br /> 此关键字与 `Name` 关键字互斥。<br /><br /> 请确保进行转义的值根据常规语法来[ADO.NET 连接字符串](../../../../../docs/framework/data/adonet/connection-strings.md)。 例如，考虑下面的连接字符串： `Server=serverName; User ID = userID`。 因为它包含分号，则必须进行转义。 因为它不包含双引号引起来，它们可用于进行转义：<br /><br /> `Provider Connection String ="Server=serverName; User ID = userID";`|  
|`Metadata`|此关键字在未指定 `Name` 关键字时是必需的。 一个由竖线分隔的目录、文件和资源位置的列表，供查找元数据和映射信息使用。 下面是一个示例：<br /><br /> `Metadata=`<br /><br /> `c:\model &#124; c:\model\sql\mapping.msl;`<br /><br /> 竖线分隔符两侧的空格将被忽略。<br /><br /> 此关键字与 `Name` 关键字互斥。|  
|`Name`|应用程序可以选择在应用程序配置文件中指定连接名称，以用于提供所需的关键字/值连接字符串值。 在此情况下，无法在连接字符串中直接提供这些值。 配置文件中不允许出现 `Name` 关键字。<br /><br /> 如果连接字符串中未包含 `Name` 关键字，则需要一个非空的 Provider 关键字值。<br /><br /> 此关键字与所有其他连接字符串关键字互斥。|  
  
 以下是针对的连接字符串的示例[AdventureWorks 销售模型](https://msdn.microsoft.com/library/f16cd988-673f-4376-b034-129ca93c7832)存储在应用程序配置文件：  
  
  
  
## <a name="model-and-mapping-file-locations"></a>模型和映射文件位置  
 `Metadata` 参数包含一个位置列表，`EntityClient` 提供程序可使用该列表来搜索模型和映射文件。 模型和映射文件通常与应用程序可执行文件部署在同一目录中。 这些文件也可以部署在特定位置或作为嵌入资源包含于应用程序中。  
  
 嵌入资源按如下方式指定：  
  
```  
Metadata=res://<assemblyFullName>/<resourceName>.   
```  
  
 下列选项用于定义嵌入资源的位置：  
  
|选项|描述|  
|-|-|  
|`assemblyFullName`|包含嵌入资源的程序集的完整名称。 该名称包含简单名称、版本名称、支持的区域性以及公钥，如下所示：<br /><br /> `ResourceLib, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null`<br /><br /> 资源可嵌入到应用程序能够访问的任何程序集中。<br /><br /> 如果指定通配符 (\*) 为`assemblyFullName`，Entity Framework 运行时将搜索以下位置，按此顺序中的资源：<br /><br /> 1.调用程序集。<br />2.引用的程序集。<br />3.应用程序的 bin 目录中的程序集。<br /><br /> 如果文件不在这些位置，则引发异常。 **注意：** 时使用通配符 （*），实体框架必须仔细查看具有正确名称的资源的所有程序集。 若要提高性能，请指定程序集名称而不使用通配符。|  
|`resourceName`|包含的资源的名称，如 AdvendtureWorksModel.csdl。 元数据服务仅查找具有以下任一扩展名的文件或资源：.csdl、.ssdl、或 .msl。 如果未指定 `resourceName`，则将加载所有元数据资源。 资源应在程序集中具有唯一的名称。 如果在程序集中的不同目录中为多个文件定义了相同的名称，则 `resourceName` 必须在资源名称前面包含文件夹结构，例如 FolderName.FileName.csdl。<br /><br /> 如果为 `resourceName` 指定通配符 (*)，则无需指定 `assemblyFullName`。|  
  
> [!NOTE]
>  若要提高性能，请在调用程序集中嵌入资源，调用程序集中未引用基础映射和元数据文件的非 Web 方案除外。  
  
 下面的示例加载应用程序 bin 目录中的调用程序集、引用程序集、以及其他程序集中的所有模型和映射文件。  
  
```  
Metadata=res://*/  
```  
  
 下面的示例从 AdventureWorks 程序集加载 model.csdl 文件，并从正在运行的应用程序的默认目录加载 model.ssdl 和 model.msl 文件。  
  
```  
Metadata=res://AdventureWorks, 1.0.0.0, neutral, a14f3033def15840/model.csdl|model.ssdl|model.msl  
```  
  
 下面的示例从特定程序集加载三个指定的资源。  
  
```  
Metadata=res://AdventureWorks, 1.0.0.0, neutral, a14f3033def15840/model.csdl|   
res://AdventureWorks, 1.0.0.0, neutral, a14f3033def15840/model.ssdl|   
res://AdventureWorks, 1.0.0.0, neutral, a14f3033def15840/model.msl  
```  
  
 下面的示例从程序集中加载所有扩展名为 .csdl、.msl 和 .ssdl 的嵌入资源。  
  
```  
Metadata=res://AdventureWorks, 1.0.0.0, neutral, a14f3033def15840/  
```  
  
 下面的示例加载加号的相对文件路径中的所有资源"datadir\metadata\\"从加载的程序集位置。  
  
```  
Metadata=datadir\metadata\  
```  
  
 下面的示例从已加载程序集的位置加载相对文件路径中的所有资源。  
  
```  
Metadata=.\  
```  
  
## <a name="support-for-the-124datadirectory124-substitution-string-and-the-web-application-root-operator-"></a>支持&#124;DataDirectory&#124;替代字符串和 Web 应用程序根目录运算符 （~）  
 `DataDirectory` 和 ~ 运算符中使用<xref:System.Data.EntityClient.EntityConnection.ConnectionString%2A>作为的一部分`Metadata`和`Provider Connection String`关键字。 <xref:System.Data.EntityClient.EntityConnection> 将 `DataDirectory` 和 ~ 运算符分别转发给 <xref:System.Data.Metadata.Edm.MetadataWorkspace> 和存储提供程序。  
  
|术语|描述|  
|----------|-----------------|  
|`&#124;DataDirectory&#124;`|解析为映射和元数据文件的相对路径。 这是通过 `AppDomain.SetData("DataDirectory", objValue)` 方法设置的值。 `DataDirectory`替代字符串必须由竖线字符括起来并不能有任何空白区域，其名称与竖线字符之间。 `DataDirectory` 名称不区分大小写。<br /><br /> 如果名为 DataDirectory 的物理目录必须作为元数据路径列表的成员传递，添加一个或两个方面的名称的空白区域。 例如：`Metadata="DataDirectory1 &#124; DataDirectory &#124; DataDirectory2"`。 ASP.NET 应用程序解析&#124;DataDirectory&#124;到"\<应用程序根目录 > / app_data"文件夹。|  
|~|解析为 Web 应用程序根目录。 位于开头的 ~ 字符总是被解释为 Web 应用程序根目录运算符 (~)，不过它也可以表示有效的本地子目录。 若要引用这样的本地子目录，用户应显示传递 `./~`。|  
  
 `DataDirectory` 和 ~ 运算符仅应指定在路径的开头，放在任何其他位置将得不到解析。 实体框架将尝试解析 `~/data`，但会将 `/data/~` 视为物理路径。  
  
 以 `DataDirectory` 或 ~ 运算符开头的路径不能解析为 `DataDirectory` 和 ~ 运算符分支之外的物理路径。 例如，下面的路径将获得解析：`~`、`~/data`、`~/bin/Model/SqlServer`。 下面的路径无法解析：`~/..`、`~/../other`。  
  
 `DataDirectory` 和 ~ 运算符可扩展为包含子目录，如下所示：`|DataDirectory|\Model`、`~/bin/Model`  
  
 对 `DataDirectory` 替代字符串和 ~ 运算符的解析不是递归进行的。 例如，如果 `DataDirectory` 包含 `~` 字符，则引发异常。 这样可以避免无穷递归。  
  
## <a name="see-also"></a>请参阅  
 [使用数据提供程序](../../../../../docs/framework/data/adonet/ef/working-with-data-providers.md)  
 [部署注意事项](../../../../../docs/framework/data/adonet/ef/deployment-considerations.md)  
 [管理连接和事务](https://msdn.microsoft.com/library/b6659d2a-9a45-4e98-acaa-d7a8029e5b99)  
 [连接字符串](../../../../../docs/framework/data/adonet/connection-strings.md)
