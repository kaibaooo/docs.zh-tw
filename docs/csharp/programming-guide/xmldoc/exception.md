---
title: <exception>- C# 程式設計指南
ms.date: 07/20/2015
f1_keywords:
- exception
- <exception>
helpviewer_keywords:
- <exception> C# XML tag
- exception C# XML tag
ms.assetid: dd73aac5-3c74-4fcf-9498-f11bff3a2f3c
ms.openlocfilehash: 14318ac0b0cdf781d0488eecaf934879017d91f6
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/14/2020
ms.locfileid: "76789796"
---
# <a name="exception-c-programming-guide"></a>\<異常>（C# 程式設計指南）

## <a name="syntax"></a>語法

```xml
<exception cref="member">description</exception>
```

## <a name="parameters"></a>參數

- cref = " `member`"

  可從目前編譯環境取得之例外狀況的參考。 編譯器會檢查指定的例外狀況是否存在，並將 `member` 轉譯為輸出 XML 中的標準項目名稱。 `member` 必須出現在雙引號內 (" ")。

  如需有關如何設定 `member` 格式以參考泛型型別的詳細資訊，請參閱[處理 XML 檔案](processing-the-xml-file.md)。

- `description`

  例外狀況的描述。

## <a name="remarks"></a>備註

\<exception> 標記可讓您指定可以擲回的例外狀況。 這個標記可以套用至方法、屬性、事件和索引子的定義。

使用[-doc](../../language-reference/compiler-options/doc-compiler-option.md)編譯，以處理檔的文檔注釋。

如需例外狀況處理的詳細資訊，請參閱[例外狀況和例外狀況處理](../exceptions/index.md)。

## <a name="example"></a>範例

[!code-csharp[csProgGuideDocComments#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#4)]

## <a name="see-also"></a>另請參閱

- [C# 程式設計指南](../index.md)
- [建議使用的文件註解標籤](recommended-tags-for-documentation-comments.md)
