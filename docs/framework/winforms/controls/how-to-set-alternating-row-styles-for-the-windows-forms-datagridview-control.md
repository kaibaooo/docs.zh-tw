---
title: 設定 DataGridView 控制項的替代資料列樣式
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- DataGridView control [Windows Forms], row styles
- data grids [Windows Forms], row styles
- rows [Windows Forms], data grids
ms.assetid: 699ef759-458c-426d-ac87-7c7e71b018ae
ms.openlocfilehash: b87ad50ef7e5118a95998949bc62299955856b27
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2020
ms.locfileid: "76747130"
---
# <a name="how-to-set-alternating-row-styles-for-the-windows-forms-datagridview-control"></a>如何：設定 Windows Form DataGridView 控制項的替代資料列樣式
表格式資料通常會以類似 Ledger 的格式呈現給使用者，在這種格式中，交替資料列會有不同的背景色彩。 這種格式可讓使用者輕鬆地指出每個資料列中有哪些儲存格，特別是具有許多資料行的寬資料表。  
  
 利用 <xref:System.Windows.Forms.DataGridView> 控制項，您可以指定替代資料列的完整樣式資訊。 這個控制項可讓您使用背景色彩以外的樣式特性 (例如前景色彩和字型)，來區分替代資料列。  
  
 在 Visual Studio 中會支援這項工作。  另請參閱[如何：使用設計工具設定 Windows Forms DataGridView 控制項的替代資料列樣式](set-alternating-row-styles-for-the-datagrid-using-the-designer.md)。  
  
### <a name="to-set-alternating-row-styles-programmatically"></a>以程式設計方式設定替代資料列樣式  
  
- 設定由 <xref:System.Windows.Forms.DataGridViewCellStyle> 的 <xref:System.Windows.Forms.DataGridView.RowsDefaultCellStyle%2A> 和 <xref:System.Windows.Forms.DataGridView.AlternatingRowsDefaultCellStyle%2A> 屬性所傳回之 <xref:System.Windows.Forms.DataGridView> 物件的屬性。  
  
     [!code-csharp[System.Windows.Forms.DataGridViewMisc#068](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMisc/CS/datagridviewmisc.cs#068)]
     [!code-vb[System.Windows.Forms.DataGridViewMisc#068](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMisc/VB/datagridviewmisc.vb#068)]  
  
    > [!NOTE]
    > 使用 <xref:System.Windows.Forms.DataGridView.RowsDefaultCellStyle%2A> 和 <xref:System.Windows.Forms.DataGridView.AlternatingRowsDefaultCellStyle%2A> 屬性所指定的樣式，會覆寫在資料行和 <xref:System.Windows.Forms.DataGridView> 層級上所指定的樣式，但會由在個別資料列和儲存格層級設定的樣式所覆寫。 如需詳細資訊，請參閱[Windows Forms DataGridView 控制項中的儲存格樣式](cell-styles-in-the-windows-forms-datagridview-control.md)。  
  
## <a name="compiling-the-code"></a>編譯程式碼  
 這個範例需要：  
  
- 名為 <xref:System.Windows.Forms.DataGridView> 的 `dataGridView1` 控制項。  
  
- <xref:System?displayProperty=nameWithType>、<xref:System.Drawing?displayProperty=nameWithType> 和 <xref:System.Windows.Forms?displayProperty=nameWithType> 組件的參考。  
  
## <a name="robust-programming"></a>最佳化程式設計  
 若要達到最大延展性，您應該在使用相同樣式的多個資料列、資料行或儲存格之間共用 <xref:System.Windows.Forms.DataGridViewCellStyle> 物件，而不是分別設定每個項目的樣式屬性。 如需詳細資訊，請參閱 [縮放 Windows Form DataGridView 控制項的最佳做法](best-practices-for-scaling-the-windows-forms-datagridview-control.md)。  
  
## <a name="see-also"></a>另請參閱

- <xref:System.Windows.Forms.DataGridView.AlternatingRowsDefaultCellStyle%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridView.RowsDefaultCellStyle%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridView>
- <xref:System.Windows.Forms.DataGridViewCellStyle>
- [Windows Forms DataGridView 控制項中的基本格式化和樣式設定](basic-formatting-and-styling-in-the-windows-forms-datagridview-control.md)
- [Windows Forms DataGridView 控制項中的儲存格樣式](cell-styles-in-the-windows-forms-datagridview-control.md)
- [縮放 Windows Forms DataGridView 控制項的最佳作法](best-practices-for-scaling-the-windows-forms-datagridview-control.md)
- [操作說明：設定 Windows Forms DataGridView 控制項的字型和色彩樣式](how-to-set-font-and-color-styles-in-the-windows-forms-datagridview-control.md)
