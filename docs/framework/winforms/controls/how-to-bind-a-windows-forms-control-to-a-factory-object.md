---
title: 將控制項系結至 Factory 物件
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- controls [Windows Forms], binding
- factory objects [Windows Forms], binding to
- BindingSource component [Windows Forms], binding to a factory object
- BindingSource component [Windows Forms], examples
ms.assetid: 7d59af89-ff82-41d8-a48a-f1fbae788b0d
ms.openlocfilehash: 2b4d9aca3345a0cb1e7e995f66a8982dee983ca8
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2020
ms.locfileid: "76745089"
---
# <a name="how-to-bind-a-windows-forms-control-to-a-factory-object"></a>如何：將 Windows Forms 控制項繫結至 Factory 物件
當您在建立與資料互動的控制項時，有時會發現需要將控制項繫結程序至會產生其他物件的物件或方法。 這類物件或方法就叫做 Factory。 比方說，您的資料來源可能是從方法呼叫傳回的值，而不是記憶體或類型中的物件。 只要來源傳回集合，您就可以將控制項繫結至這種資料來源。  
  
 使用 <xref:System.Windows.Forms.BindingSource> 控制項可讓您輕鬆將控制項繫結至 Factory 物件。  
  
## <a name="example"></a>範例  
 下列範例示範如何使用 <xref:System.Windows.Forms.DataGridView> 控制項，將 <xref:System.Windows.Forms.BindingSource> 控制項繫結至 Factory 方法。 Factory 方法名為 `GetOrdersByCustomerId`，它會傳回 Northwind 資料庫中指定客戶的所有訂單。  
  
 [!code-cpp[System.Windows.Forms.DataConnector.BindToFactory#1](~/samples/snippets/cpp/VS_Snippets_Winforms/System.Windows.Forms.DataConnector.BindToFactory/CPP/form1.cpp#1)]
 [!code-csharp[System.Windows.Forms.DataConnector.BindToFactory#1](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataConnector.BindToFactory/CS/form1.cs#1)]
 [!code-vb[System.Windows.Forms.DataConnector.BindToFactory#1](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataConnector.BindToFactory/VB/form1.vb#1)]  
  
## <a name="compiling-the-code"></a>編譯程式碼  
 這個範例需要：  
  
- System、System.Data、System.Drawing 和 System.Windows.Forms 組件的參考。  
  
## <a name="see-also"></a>另請參閱

- <xref:System.Windows.Forms.BindingNavigator>
- <xref:System.Windows.Forms.DataGridView>
- <xref:System.Windows.Forms.BindingSource>
- [BindingSource 元件](bindingsource-component.md)
- [如何：將 Windows Forms 控制項繫結至型別](how-to-bind-a-windows-forms-control-to-a-type.md)
