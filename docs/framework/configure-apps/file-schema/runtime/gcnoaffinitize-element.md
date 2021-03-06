---
title: GCNoAffinitize 元素
ms.date: 11/08/2019
helpviewer_keywords:
- gcNoAffinitize element
- <gcNoAffinitize> element
ms.openlocfilehash: 4031ff19131c905072696837d1622dbb6e54ae61
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/12/2019
ms.locfileid: "73978372"
---
# <a name="gcnoaffinitize-element"></a>\<GCNoAffinitize > 元素

指定是否要使用 Cpu 將相似化為伺服器 GC 執行緒。

\<設定 > \
&nbsp;&nbsp;\<執行時間 > \
&nbsp;&nbsp;&nbsp;&nbsp;\<GCNoAffinitize >

## <a name="syntax"></a>語法

```xml
<GCNoAffinitize
   enabled="true|false"/>
```

## <a name="attributes-and-elements"></a>屬性和元素

下列各節描述屬性、子項目和父項目。

### <a name="attributes"></a>屬性

|屬性|描述|
|---------------|-----------------|
|`enabled`|必要屬性。<br /><br />指定是否要使用機器上可用的處理器來相似化為伺服器 GC 執行緒/堆積。|

#### <a name="enabled-attribute"></a>enabled 屬性

|值|描述|
|-----------|-----------------|
|`false`|具有 Cpu 的如何伺服器 GC 執行緒。 這是預設值。|
|`true`|不會將相似化為伺服器 GC 執行緒與 Cpu。|

### <a name="child-elements"></a>子元素

無。

### <a name="parent-elements"></a>父元素

|項目|描述|
|-------------|-----------------|
|`configuration`|通用語言執行平台和 .NET Framework 應用程式所使用之每個組態檔中的根項目。|
|`runtime`|包含有關組件繫結和記憶體回收的資訊。|

## <a name="remarks"></a>備註

根據預設，伺服器 GC 執行緒會以其各自的 Cpu 進行硬式相似化為。 系統的每個可用處理器都有自己的 GC 堆積和執行緒。 這通常是慣用的設定，因為它會優化快取使用量。 從 .NET Framework 4.6.2 開始，藉由將**GCNoAffinitize**專案的 `enabled` 屬性設定為 `false`，您可以指定伺服器 GC 執行緒和 cpu 不應緊密結合。

您可以單獨指定**GCNoAffinitize**設定元素，而不是使用 cpu 將相似化為伺服器 GC 執行緒。 您也可以將它與[GCHeapCount](gcheapcount-element.md)元素搭配使用，以控制應用程式所使用的 GC 堆積和執行緒數目。

如果**GCNoAffinitize**專案的 `enabled` 屬性是 `false` （其預設值），您也可以使用[GCHeapCount](gcheapcount-element.md)專案來指定 gc 執行緒和堆積的數目，以及[GCHeapAffinitizeMask](gcheapaffinitizemask-element.md)元素來指定要相似化為 gc 執行緒和堆積的處理器。

## <a name="example"></a>範例

下列範例不會硬式將相似化為伺服器 GC 執行緒：

```xml
<configuration>
   <runtime>
      <gcServer enabled="true"/>
      <GCNoAffinitize enabled="true"/>
   </runtime>
</configuration>
```

下列範例不會將相似化為伺服器 GC 執行緒，並將 GC 堆積/執行緒的數目限制為10個：

```xml
<configuration>
   <runtime>
      <gcServer enabled="true"/>
      <GCHeapCount enabled="10"/>
      <GCNoAffinitize enabled="true"/>
   </runtime>
</configuration>
```

## <a name="see-also"></a>請參閱

- <xref:System.Runtime.GCSettings.IsServerGC%2A?displayProperty=nameWithType>
- [GCHeapAffinitizeMask 元素](gcheapaffinitizemask-element.md)
- [GCHeapCount 元素](gcheapcount-element.md)
- [記憶體回收的基本概念](../../../../standard/garbage-collection/fundamentals.md)
- [執行階段設定結構描述](index.md)
- [組態檔結構描述](../index.md)
