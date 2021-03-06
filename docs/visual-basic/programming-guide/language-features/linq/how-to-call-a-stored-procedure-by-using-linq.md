---
title: 如何：使用 LINQ 呼叫預存程式
ms.date: 07/20/2015
helpviewer_keywords:
- queries [LINQ in Visual Basic], stored procedure calls
- stored procedures sample [Visual Basic]
- stored procedures [LINQ to SQL]
- queries [LINQ in Visual Basic], how-to topics
ms.assetid: 6436d384-d1e0-40aa-8afd-451007477260
ms.openlocfilehash: f91ccda1842887b3785ce304fd41bdd020a55479
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/22/2019
ms.locfileid: "74345030"
---
# <a name="how-to-call-a-stored-procedure-by-using-linq-visual-basic"></a>如何：使用 LINQ 呼叫預存程序 (Visual Basic)
語言整合式查詢（LINQ）可讓您輕鬆地存取資料庫資訊，包括資料庫物件，例如預存程式。  
  
 下列範例示範如何建立在 SQL Server 資料庫中呼叫預存程式的應用程式。 此範例會示範如何在資料庫中呼叫兩個不同的預存程式。 每個程式都會傳回查詢的結果。 其中一個程式接受輸入參數，而另一個程式不接受參數。  
  
 本主題中的範例會使用 Northwind 範例資料庫。 如果您的開發電腦上沒有這個資料庫，則可以從 Microsoft 下載中心下載。 如需指示，請參閱[下載範例資料庫](../../../../framework/data/adonet/sql/linq/downloading-sample-databases.md)。  
  
[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]  
  
### <a name="to-create-a-connection-to-a-database"></a>建立資料庫的連接  
  
1. 在 Visual Studio 中，按一下 [**視圖**]**功能表上的**[**伺服器總管**/資料庫總管]，開啟**伺服器總管**/**資料庫總管**。  
  
2. 以滑鼠右鍵按一下**伺服器總管**/**資料庫總管**中的 [**資料連線**]，然後按一下 [**新增連接**]。  
  
3. 請指定與 Northwind 範例資料庫的有效連接。  
  
### <a name="to-add-a-project-that-contains-a-linq-to-sql-file"></a>若要加入包含 LINQ to SQL 檔案的專案  
  
1. 在 Visual Studio 的 [檔案] 功能表上，指向 [新增]，然後按一下 [專案]。 選取 [Visual Basic **Windows Forms 應用程式**] 做為 [專案類型]。  
  
2. 在 [專案] 功能表中，按一下 [加入新項目]。 選取 [ **LINQ to SQL 類別**] 專案範本。  
  
3. 將檔案命名為 `northwind.dbml`。 按一下 [加入]。 [物件關聯式設計工具（O/R 設計工具）] 會針對 northwind .dbml 檔案開啟。  
  
### <a name="to-add-stored-procedures-to-the-or-designer"></a>若要將預存程式加入至 O/R 設計工具  
  
1. 在**伺服器總管**/**資料庫總管**中，展開 Northwind 資料庫的連接。 展開 [**預存程式**] 資料夾。  
  
     如果您已經關閉 O/R 設計工具，可以按兩下先前加入的 northwind 檔案來重新開啟它。  
  
2. 按一下 [**依年份的銷售**] 預存程式，然後將它拖曳至設計工具的右窗格。 按一下**十個最昂貴的產品**預存程式，將其拖曳至設計工具的右窗格。  
  
3. 儲存您的變更並關閉設計工具。  
  
4. 儲存您的專案。  
  
### <a name="to-add-code-to-display-the-results-of-the-stored-procedures"></a>若要加入程式碼以顯示預存程式的結果  
  
1. 從 [**工具箱**] 中，將 [<xref:System.Windows.Forms.DataGridView>] 控制項拖曳至您的專案 [Form1] 的預設 Windows Form。  
  
2. 按兩下 [Form1]，將程式碼新增至其 `Load` 事件。  
  
3. 當您在 O/R 設計工具中加入預存程式時，設計工具會為您的專案加入 <xref:System.Data.Linq.DataContext> 物件。 此物件包含存取這些程式時必須具備的程式碼。 專案的 <xref:System.Data.Linq.DataContext> 物件是根據 .dbml 檔案的名稱來命名。 針對此專案，<xref:System.Data.Linq.DataContext> 物件的名稱為 `northwindDataContext`。  
  
     您可以在程式碼中建立 <xref:System.Data.Linq.DataContext> 的實例，並呼叫 O/R 設計工具所指定的預存程式方法。 若要系結至 <xref:System.Windows.Forms.DataGridView> 物件，您可能必須在預存程式的結果上呼叫 <xref:System.Linq.Enumerable.ToList%2A> 方法，以強制立即執行查詢。  
  
     將下列程式碼新增至 `Load` 事件，以呼叫公開為數據內容之方法的其中一個預存程式。  
  
     [!code-vb[VbLINQtoSQLHowTos#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQtoSQLHowTos/VB/Form3.vb#1)]  
    [!code-vb[VbLINQtoSQLHowTos#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQtoSQLHowTos/VB/Form3.vb#2)]  
  
4. 按 F5 執行您的專案並查看結果。  
  
## <a name="see-also"></a>另請參閱

- [LINQ](../../../../visual-basic/programming-guide/language-features/linq/index.md)
- [查詢](../../../../visual-basic/language-reference/queries/index.md)
- [LINQ to SQL](../../../../framework/data/adonet/sql/linq/index.md)
- [DataContext 方法 (O/R 設計工具)](/visualstudio/data-tools/datacontext-methods-o-r-designer)
- [如何：指派用來執行更新、插入和刪除的預存程序 (O/R 設計工具)](/visualstudio/data-tools/how-to-assign-stored-procedures-to-perform-updates-inserts-and-deletes-o-r-designer)
