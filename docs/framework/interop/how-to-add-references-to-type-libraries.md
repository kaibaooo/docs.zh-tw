---
title: 如何：將參考加入至類型程式庫
ms.date: 03/30/2017
helpviewer_keywords:
- importing type library
- interop assemblies, generating
- type libraries
- COM interop, importing type library
ms.assetid: f5cfa6ba-cc25-4017-82cd-ba7391859113
ms.openlocfilehash: 1e82a499b77cc6d1d49eaf13e243201bbdc4c5fe
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/12/2020
ms.locfileid: "79181437"
---
# <a name="how-to-add-references-to-type-libraries"></a>如何：將參考加入至類型程式庫
Visual Studio 會產生內含新增類型程式庫參考時所產生之中繼資料的 interop 組件。 如有提供主要的 Interop 組件，Visual Studio 便會先使用現有的組件，然後再產生新的 Interop 組件。  
  
### <a name="to-add-a-reference-to-a-type-library-in-visual-studio"></a>新增 Visual Studio 中之類型程式庫的參考  
  
1. 若 Windows Setup.exe 未在您的電腦上安裝 COM DLL 或 EXE 檔案，請自行安裝。  
  
2. 依序選擇 [專案]**** 及 [新增參考]****。  
  
3. 在 [參考管理員] 中，選擇 [COM]****。  
  
4. 從清單中選取類型程式庫，或瀏覽至 .tlb 檔案。  
  
5. 選擇 **"確定**"。  
  
6. 在方案總管中，開啟所新增之參考的捷徑功能表，然後選擇 [屬性]****。  
  
7. 在 [屬性]**** 視窗中，確定**內嵌 Interop 類型**屬性已設為 [True]****。 這可讓 Visual Studio 將 COM 類型的類型資訊嵌入您的可執行檔中，免除將主要 Interop 組件部署到應用程式的動作。  
  
> [!NOTE]
> 功能表與對話方塊可能會隨您所使用的 Visual Studio 版本而不同。  
  
### <a name="to-add-a-reference-to-a-type-library-for-command-line-compilation"></a>新增類型程式庫參考供命令列編譯時使用  
  
1. 產生[如何：從型別程式庫產生 Interop 組件](how-to-generate-interop-assemblies-from-type-libraries.md)中所述的 Interop 組件。  
  
2. 將[-link（C# 編譯器選項）](../../csharp/language-reference/compiler-options/link-compiler-option.md)或[-link（可視基本）](../../visual-basic/reference/command-line-compiler/link.md)編譯器選項與內部程式集名稱一起嵌入到可執行檔中的 COM 類型類型資訊。  
  
## <a name="see-also"></a>另請參閱

- [匯入類型程式庫做為組件](importing-a-type-library-as-an-assembly.md)
- [將 COM 元件公開給 .NET Framework](exposing-com-components.md)
- [逐步解說：在 Visual Studio 中內嵌來自 Managed 組件的型別](../../standard/assembly/embed-types-visual-studio.md)
- [-link (C# 編譯器選項)](../../csharp/language-reference/compiler-options/link-compiler-option.md)
- [-連結（視覺基礎）](../../visual-basic/reference/command-line-compiler/link.md)
