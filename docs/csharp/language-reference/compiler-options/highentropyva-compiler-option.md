---
title: -highentropyva (C# 編譯器選項)
ms.date: 07/20/2015
f1_keywords:
- /highentropyva
helpviewer_keywords:
- /highentropyva compiler option [C#]
- -highentropyva compiler option [C#]
- highentropyva compiler option [C#]
ms.assetid: eaf409b3-384e-49dd-9417-62453658f421
ms.openlocfilehash: b710bb829f6a7591159d2f2e6bacc670d21c42d1
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/14/2020
ms.locfileid: "69606853"
---
# <a name="-highentropyva-c-compiler-options"></a>-highentropyva (C# 編譯器選項)
**-highentropyva** 編譯器選項會通知 Windows 核心某一特定可執行檔是否支援高熵位址空間配置隨機載入 (ASLR)。  
  
## <a name="syntax"></a>語法  
  
```console  
-highentropyva[+ | -]  
```  
  
## <a name="arguments"></a>引數  
 `+` &#124; `-`  
 此選項會指定，64 位元可執行檔或 [-platform:anycpu](./platform-compiler-option.md) 編譯器選項所標記的可執行檔支援高熵虛擬位址空間。 此選項預設為停用。 使用 **-highentropyva+** 或 **-highentropyva** 予以啟用。  
  
## <a name="remarks"></a>備註  
 當隨機化處理序的位址空間配置作為 ASLR 一部分時，**-highentropyva** 選項可讓相容的 Windows 核心版本使用較高程度的高熵。 使用較高程度的高熵表示可將大量位址配置到記憶體區域，例如堆疊和堆積， 因此更難猜測特定記憶體區域的位置。  
  
 指定 **-highentropyva** 編譯器選項時，目標可執行檔以及作為其依據的任何模組在作為 64 位元處理序執行時，都必須能夠處理大於 4 GB 的指標值。
