---
title: dotnet remove package 命令
description: dotnet remove package 命令提供方便的選項，以移除專案的 NuGet 套件參考。
ms.date: 02/14/2020
ms.openlocfilehash: 8eaa311748c5627351ef149012dc4dddd2ab2793
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/14/2020
ms.locfileid: "77503641"
---
# <a name="dotnet-remove-package"></a>dotnet remove package

**本文適用于：✔️** .NET Core 2.x SDK 和更高版本

## <a name="name"></a>名稱

`dotnet remove package` - 從專案檔中移除套件參考。

## <a name="synopsis"></a>概要

```dotnetcli
dotnet remove [<PROJECT>] package <PACKAGE_NAME> [-h|--help]
```

## <a name="description"></a>描述

`dotnet remove package` 命令提供方便的選項，以從專案中移除 NuGet 套件參考。

## <a name="arguments"></a>引數

`PROJECT`

指定專案檔。 如果未指定，命令會在目前的目錄中搜尋一個專案檔。

`PACKAGE_NAME`

要移除的套件參考。

## <a name="options"></a>選項。

- **`-h|--help`**

  印出命令的簡短說明。

## <a name="examples"></a>範例

- 從`Newtonsoft.Json`目前的目錄中的專案中刪除 NuGet 包：

  ```dotnetcli
  dotnet remove package Newtonsoft.Json
  ```
