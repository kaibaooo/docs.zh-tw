---
title: 使用 DataReader 擷取資料
ms.date: 10/29/2018
dev_langs:
- csharp
- vb
ms.assetid: 97afc121-fb8b-465b-bab3-6d844420badb
ms.openlocfilehash: 88cd85ce343aaab08b944f81c9659918014da0a5
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/12/2020
ms.locfileid: "79149020"
---
# <a name="retrieve-data-using-a-datareader"></a>使用資料讀取器檢索資料
若要使用**DataReader**檢索資料，請創建**命令**物件的實例，然後通過調用**Command.ExecuteReader**從資料來源檢索行來創建**DataReader。** **DataReader**提供了一個未緩衝的資料流程，允許過程邏輯按順序有效地處理資料來源的結果。 當您檢索大量資料時 **，DataReader**是一個不錯的選擇，因為資料未緩存在記憶體中。

下面的示例演示了使用**DataReader**，`reader`其中表示有效的 DataReader`command`並表示有效的命令物件。  

```csharp
reader = command.ExecuteReader();  
```

```vb
reader = command.ExecuteReader()
```  

使用**DataReader.Read**方法從查詢結果中獲取行。 通過將列的名稱或單位編號傳遞給**DataReader，** 可以訪問返回行的每個列。 但是，為了獲得最佳性能 **，DataReader**提供了一系列方法，允許您訪問其本機資料類型中的列值（GetDateTime、GetDouble、GetGuid、GetInt32 等）。** ** **GetDouble** **GetGuid** **GetInt32** 有關特定于資料提供程式**的資料閱讀器**的鍵入訪問器方法的清單，請參閱<xref:System.Data.OleDb.OleDbDataReader>和<xref:System.Data.SqlClient.SqlDataReader>。 當您知道基礎資料類型時，使用鍵入的訪問器方法可減少檢索列值時所需的類型轉換量。  
  
 以下示例通過**DataReader**物件進行遍讀，並從每行返回兩列。  
  
 [!code-csharp[DataWorks SqlClient.HasRows#1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SqlClient.HasRows/CS/source.cs#1)]
 [!code-vb[DataWorks SqlClient.HasRows#1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SqlClient.HasRows/VB/source.vb#1)]  
  
## <a name="closing-the-datareader"></a>關閉 DataReader  
 使用完**DataReader**物件後，始終調用**Close**方法。  
  
 如果**命令**包含輸出參數或傳回值，則在**關閉 DataReader**之前，這些值不可用。  
  
 當**資料閱讀器**處於打開狀態時，**該連接**僅由該**資料閱讀器**使用。 在關閉原始**資料閱讀器**之前，您不能執行**連接**的任何命令，包括創建另一個**DataReader。**  
  
> [!NOTE]
> 不要調用**連接****、DataReader**或**Connection**類**的 Finalize**方法中的任何其他託管**物件。** 在完成項中，只需釋放類別直接擁有的 Unmanaged 資源。 如果類不擁有任何非託管資源，請不要在類定義中包括**Finalize**方法。 有關詳細資訊，請參閱[垃圾回收](../../../standard/garbage-collection/index.md)。  
  
## <a name="retrieving-multiple-result-sets-using-nextresult"></a>使用 NextResult 檢索多個結果集  
 如果**DataReader**返回多個結果集，請調用**NextResult**方法以按順序遍讀結果集。 下列範例顯示 <xref:System.Data.SqlClient.SqlDataReader> 使用 <xref:System.Data.SqlClient.SqlCommand.ExecuteReader%2A> 方法，處理兩個 SELECT 陳述式的結果。  
  
 [!code-csharp[DataWorks SqlClient.NextResult#1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SqlClient.NextResult/CS/source.cs#1)]
 [!code-vb[DataWorks SqlClient.NextResult#1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SqlClient.NextResult/VB/source.vb#1)]  
  
## <a name="getting-schema-information-from-the-datareader"></a>從資料閱讀器獲取架構資訊  
 打開**DataReader**時，可以使用**GetSchemaTable**方法檢索有關當前結果集的架構資訊。 **GetSchemaTable**返回<xref:System.Data.DataTable>一個包含當前結果集的架構資訊的行和列填充的物件。 **資料表**包含結果集每一列的一行。 架構表的每一列映射到結果集行中返回的列的屬性，其中 **"列名稱**"是屬性的名稱，列的值是屬性的值。 下面的示例為**DataReader**編寫架構資訊。  
  
 [!code-csharp[DataWorks SqlClient.GetSchemaTable#1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SqlClient.GetSchemaTable/CS/source.cs#1)]
 [!code-vb[DataWorks SqlClient.GetSchemaTable#1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SqlClient.GetSchemaTable/VB/source.vb#1)]  
  
## <a name="working-with-ole-db-chapters"></a>使用 OLE DB 章節  
 可以使用 檢索<xref:System.Data.OleDb.OleDbDataReader>層次結構行集或章節（OLE DB 類型**DBTYPE_HCHAPTER**ADO 類型**廣告章節**）。 當包含章節的查詢作為**DataReader**返回時，該章將作為該**資料閱讀器**中的列返回，並作為**DataReader**物件公開。  
  
 **DataSetADO.NET**也可用於使用表之間的父子關係來表示分層行集。 有關詳細資訊，請參閱[資料集、資料表和資料檢視](./dataset-datatable-dataview/index.md)。  
  
 下列程式碼範例使用 MSDataShape 提供者，替客戶清單中每位客戶的訂單產生章節資料行。  
  
```vb  
Using connection As OleDbConnection = New OleDbConnection(
    "Provider=MSDataShape;Data Provider=SQLOLEDB;" &
    "Data Source=localhost;Integrated Security=SSPI;Initial Catalog=northwind")

    Using custCMD As OleDbCommand = New OleDbCommand(
        "SHAPE {SELECT CustomerID, CompanyName FROM Customers} " &
        "APPEND ({SELECT CustomerID, OrderID FROM Orders} AS CustomerOrders " &
        "RELATE CustomerID TO CustomerID)", connection)

        connection.Open()

        Using custReader As OleDbDataReader = custCMD.ExecuteReader()

            Do While custReader.Read()
                Console.WriteLine("Orders for " & custReader.GetString(1))
                ' custReader.GetString(1) = CompanyName  

                Using orderReader As OleDbDataReader = custReader.GetValue(2)
                    ' custReader.GetValue(2) = Orders chapter as DataReader  

                    Do While orderReader.Read()
                        Console.WriteLine(vbTab & orderReader.GetInt32(1))
                        ' orderReader.GetInt32(1) = OrderID  
                    Loop
                    orderReader.Close()
                End Using
            Loop
            ' Make sure to always close readers and connections.  
            custReader.Close()
        End Using
    End Using
End Using
```  
  
```csharp  
using (OleDbConnection connection = new OleDbConnection(
    "Provider=MSDataShape;Data Provider=SQLOLEDB;" +
    "Data Source=localhost;Integrated Security=SSPI;Initial Catalog=northwind"))
{
    using (OleDbCommand custCMD = new OleDbCommand(
        "SHAPE {SELECT CustomerID, CompanyName FROM Customers} " +
        "APPEND ({SELECT CustomerID, OrderID FROM Orders} AS CustomerOrders " +
        "RELATE CustomerID TO CustomerID)", connection))
    {
        connection.Open();

        using (OleDbDataReader custReader = custCMD.ExecuteReader())
        {

            while (custReader.Read())
            {
                Console.WriteLine("Orders for " + custReader.GetString(1));
                // custReader.GetString(1) = CompanyName  

                using (OleDbDataReader orderReader = (OleDbDataReader)custReader.GetValue(2))
                {
                    // custReader.GetValue(2) = Orders chapter as DataReader  

                    while (orderReader.Read())
                        Console.WriteLine("\t" + orderReader.GetInt32(1));
                    // orderReader.GetInt32(1) = OrderID  
                    orderReader.Close();
                }
            }
            // Make sure to always close readers and connections.  
            custReader.Close();
        }
    }
}
```  
  
## <a name="returning-results-with-oracle-ref-cursors"></a>使用 Oracle REF CURSORs 返回結果  
 Oracle 的 .NET Framework 資料提供者支援使用 Oracle REF CURSOR 傳回查詢結果。 Oracle REF CURSOR 以 <xref:System.Data.OracleClient.OracleDataReader> 傳回。  
  
 可以使用<xref:System.Data.OracleClient.OracleCommand.ExecuteReader%2A>方法檢索<xref:System.Data.OracleClient.OracleDataReader>表示 Oracle REF CURSOR 的物件。 還可以指定 返回<xref:System.Data.OracleClient.OracleCommand>一個或多個 Oracle REF CURSOR 作為<xref:System.Data.OracleClient.OracleDataAdapter>用於填充 的<xref:System.Data.DataSet>**SelectCommand 的 指定命令**。  
  
 要訪問從 Oracle 資料來源返回的 REF CURSOR，<xref:System.Data.OracleClient.OracleCommand>請為查詢創建 一個，並添加一個輸出參數，<xref:System.Data.OracleClient.OracleCommand.Parameters>將 REF <xref:System.Data.OracleClient.OracleCommand>CURSOR 引用到 集合中。 參數的名稱必須符合查詢中 REF CURSOR 參數的名稱。 將參數的類型設置為<xref:System.Data.OracleClient.OracleType.Cursor?displayProperty=nameWithType>。 <xref:System.Data.OracleClient.OracleCommand>方法<xref:System.Data.OracleClient.OracleCommand.ExecuteReader?displayProperty=nameWithType><xref:System.Data.OracleClient.OracleDataReader>返回 REF CURSOR 的  
  
 如果<xref:System.Data.OracleClient.OracleCommand>返回多個 REF CURSORS，請添加多個輸出參數。 您可以通過調用<xref:System.Data.OracleClient.OracleCommand.ExecuteReader?displayProperty=nameWithType>方法訪問不同的 REF CURSOR。 調用 返回<xref:System.Data.OracleClient.OracleCommand.ExecuteReader>引用第<xref:System.Data.OracleClient.OracleDataReader>一個 REF CURSOR。 然後，<xref:System.Data.OracleClient.OracleDataReader.NextResult?displayProperty=nameWithType>您可以調用 方法以訪問後續的 REF CURSOR。 儘管<xref:System.Data.OracleClient.OracleCommand.Parameters?displayProperty=nameWithType>集合中的參數按名稱與 REF CURSOR 輸出參數匹配，但<xref:System.Data.OracleClient.OracleDataReader>訪問參數的順序是添加到集合中<xref:System.Data.OracleClient.OracleCommand.Parameters>的順序。  
  
 例如，請考慮下列的 Oracle Package 和 Package 內容。  
  
```sql
CREATE OR REPLACE PACKAGE CURSPKG AS
  TYPE T_CURSOR IS REF CURSOR;
  PROCEDURE OPEN_TWO_CURSORS (EMPCURSOR OUT T_CURSOR,
    DEPTCURSOR OUT T_CURSOR);
END CURSPKG;  
  
CREATE OR REPLACE PACKAGE BODY CURSPKG AS
  PROCEDURE OPEN_TWO_CURSORS (EMPCURSOR OUT T_CURSOR,
    DEPTCURSOR OUT T_CURSOR)
  IS
  BEGIN
    OPEN EMPCURSOR FOR SELECT * FROM DEMO.EMPLOYEE;
    OPEN DEPTCURSOR FOR SELECT * FROM DEMO.DEPARTMENT;
  END OPEN_TWO_CURSORS;
END CURSPKG;
```  
  
 以下代碼創建 一<xref:System.Data.OracleClient.OracleCommand>個，通過向<xref:System.Data.OracleClient.OracleType.Cursor?displayProperty=nameWithType><xref:System.Data.OracleClient.OracleCommand.Parameters?displayProperty=nameWithType>集合添加兩個類型的參數來返回上一個 Oracle 包中的 REF CURSOR。  
  
```vb  
Dim cursCmd As OracleCommand = New OracleCommand("CURSPKG.OPEN_TWO_CURSORS", oraConn)  
cursCmd.Parameters.Add("EMPCURSOR", OracleType.Cursor).Direction = ParameterDirection.Output  
cursCmd.Parameters.Add("DEPTCURSOR", OracleType.Cursor).Direction = ParameterDirection.Output  
```  
  
```csharp  
OracleCommand cursCmd = new OracleCommand("CURSPKG.OPEN_TWO_CURSORS", oraConn);  
cursCmd.Parameters.Add("EMPCURSOR", OracleType.Cursor).Direction = ParameterDirection.Output;  
cursCmd.Parameters.Add("DEPTCURSOR", OracleType.Cursor).Direction = ParameterDirection.Output;  
```  
  
 以下代碼使用 的<xref:System.Data.OracleClient.OracleDataReader.Read>和<xref:System.Data.OracleClient.OracleDataReader.NextResult>返回上一個命令的結果。 <xref:System.Data.OracleClient.OracleDataReader> REF CURSOR 參數會依順序傳回。  
  
```vb  
oraConn.Open()  
  
Dim cursCmd As OracleCommand = New OracleCommand("CURSPKG.OPEN_TWO_CURSORS", oraConn)  
cursCmd.CommandType = CommandType.StoredProcedure  
cursCmd.Parameters.Add("EMPCURSOR", OracleType.Cursor).Direction = ParameterDirection.Output  
cursCmd.Parameters.Add("DEPTCURSOR", OracleType.Cursor).Direction = ParameterDirection.Output  
  
Dim reader As OracleDataReader = cursCmd.ExecuteReader()  
  
Console.WriteLine(vbCrLf & "Emp ID" & vbTab & "Name")  
  
Do While reader.Read()  
  Console.WriteLine("{0}" & vbTab & "{1}, {2}", reader.GetOracleNumber(0), reader.GetString(1), reader.GetString(2))  
Loop  
  
reader.NextResult()  
  
Console.WriteLine(vbCrLf & "Dept ID" & vbTab & "Name")  
  
Do While reader.Read()  
  Console.WriteLine("{0}" & vbTab & "{1}", reader.GetOracleNumber(0), reader.GetString(1))  
Loop  
' Make sure to always close readers and connections.  
reader.Close()  
oraConn.Close()  
```  
  
```csharp  
oraConn.Open();  
  
OracleCommand cursCmd = new OracleCommand("CURSPKG.OPEN_TWO_CURSORS", oraConn);  
cursCmd.CommandType = CommandType.StoredProcedure;  
cursCmd.Parameters.Add("EMPCURSOR", OracleType.Cursor).Direction = ParameterDirection.Output;  
cursCmd.Parameters.Add("DEPTCURSOR", OracleType.Cursor).Direction = ParameterDirection.Output;  
  
OracleDataReader reader = cursCmd.ExecuteReader();  
  
Console.WriteLine("\nEmp ID\tName");  
  
while (reader.Read())  
  Console.WriteLine("{0}\t{1}, {2}", reader.GetOracleNumber(0), reader.GetString(1), reader.GetString(2));  
  
reader.NextResult();  
  
Console.WriteLine("\nDept ID\tName");  
  
while (reader.Read())  
  Console.WriteLine("{0}\t{1}", reader.GetOracleNumber(0), reader.GetString(1));  
// Make sure to always close readers and connections.  
reader.Close();  
oraConn.Close();  
```  
  
 下面的示例使用前面的命令用 Oracle 包<xref:System.Data.DataSet>的結果填充 。  
  
```vb  
Dim ds As DataSet = New DataSet()  
  
Dim adapter As OracleDataAdapter = New OracleDataAdapter(cursCmd)  
adapter.TableMappings.Add("Table", "Employees")  
adapter.TableMappings.Add("Table1", "Departments")  
  
adapter.Fill(ds)  
```  
  
```csharp  
DataSet ds = new DataSet();  
  
OracleDataAdapter adapter = new OracleDataAdapter(cursCmd);  
adapter.TableMappings.Add("Table", "Employees");  
adapter.TableMappings.Add("Table1", "Departments");  
  
adapter.Fill(ds);  
```

> [!NOTE]
> 為了避免**溢出異常**，我們建議您在將值存儲在 中之前，還要處理從 Oracle NUMBER 類型到有效的 .NET 框架類型的任何轉換<xref:System.Data.DataRow>。 您可以使用該<xref:System.Data.Common.DataAdapter.FillError>事件來確定是否發生了**溢出異常**。 有關事件的詳細資訊，<xref:System.Data.Common.DataAdapter.FillError>請參閱[處理資料配接器事件](handling-dataadapter-events.md)。  
  
## <a name="see-also"></a>另請參閱

- [DataAdapter 和 DataReader](dataadapters-and-datareaders.md)
- [命令和參數](commands-and-parameters.md)
- [擷取資料庫結構描述資訊](retrieving-database-schema-information.md)
- [ADO.NET 概觀](ado-net-overview.md) \(部分機器翻譯\)
