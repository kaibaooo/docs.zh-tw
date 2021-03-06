---
title: HOW TO：將 Windows Communication Foundation 服務設為使用連接埠共用
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 6400bc71-a858-4ac2-8d5a-caa72d3b5482
ms.openlocfilehash: cd8d76137ac195e452a7d66fb6ddbeda405a922f
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/12/2020
ms.locfileid: "79185094"
---
# <a name="how-to-configure-a-windows-communication-foundation-service-to-use-port-sharing"></a>HOW TO：將 Windows Communication Foundation 服務設為使用連接埠共用
在 Windows 通信基礎 （WCF） 應用程式中使用 net.tcp：// 埠共用的最簡單方法是使用 公開<xref:System.ServiceModel.NetTcpBinding>服務。  
  
 這項繫結會提供 <xref:System.ServiceModel.NetTcpBinding.PortSharingEnabled%2A> 屬性，以控制是否針對使用此繫結進行設定的服務啟用 net.tcp:// 連接埠共用。  
  
 下列程序將說明如何使用 <xref:System.ServiceModel.NetTcpBinding> 類別來開啟統一資源識別元 (URI) net.tcp://localhost/MyService 中的端點 (先使用程式碼，然後再使用組態項目)。  
  
### <a name="to-enable-nettcp-port-sharing-on-a-nettcpbinding-in-code"></a>若要使用程式碼啟用 NetTcpBinding 上的 net.tcp:// 連接埠共用  
  
1. 創建一個服務來實現稱為`IMyService`的協定並調用它`MyService`。  
  
     [!code-csharp[c_ConfigurePortSharing#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_configureportsharing/cs/source.cs#1)]
     [!code-vb[c_ConfigurePortSharing#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_configureportsharing/vb/source.vb#1)]  
  
2. 建立 <xref:System.ServiceModel.NetTcpBinding> 類別的執行個體，並將 <xref:System.ServiceModel.NetTcpBinding.PortSharingEnabled%2A> 屬性設定為 `true`。  
  
     [!code-csharp[c_ConfigurePortSharing#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_configureportsharing/cs/source.cs#2)]
     [!code-vb[c_ConfigurePortSharing#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_configureportsharing/vb/source.vb#2)]  
  
3. 建立 <xref:System.ServiceModel.ServiceHost>，並在其上為 `MyService` 新增服務端點，該服務會使用連接埠共用啟用之 <xref:System.ServiceModel.NetTcpBinding>，並在端點位址 URI "net.tcp://localhost/MyService" 上進行接聽。  
  
     [!code-csharp[c_ConfigurePortSharing#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_configureportsharing/cs/source.cs#3)]
     [!code-vb[c_ConfigurePortSharing#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_configureportsharing/vb/source.vb#3)]  
  
    > [!NOTE]
    > 此範例使用預設 TCP 連接埠號碼 808，因為端點位址 URI 並未指定不同的連接埠號碼。 由於已在傳輸繫結程序上明確啟用連接埠共用，此服務可與其他處理序中的其他服務共用連接埠 808。 如果不允許使用連接埠共用，而另一個應用程式已在使用連接埠 808，則此服務將在開啟時擲回 <xref:System.ServiceModel.AddressAlreadyInUseException>。  
  
### <a name="to-enable-nettcp-port-sharing-on-a-nettcpbinding-in-configuration"></a>若要使用組態啟用 NetTcpBinding 上的 net.tcp:// 連接埠共用  
  
1. 下列範例將說明如何使用組態項目來啟用連接埠共用並新增服務端點。  
  
```xml  
<system.serviceModel>  
  <bindings>  
    <netTcpBinding name="portSharingBinding"
                   portSharingEnabled="true" />  
  </bindings>  
  <services>  
    <service name="MyService">  
        <endpoint address="net.tcp://localhost/MyService"  
                  binding="netTcpBinding"  
                  contract="IMyService"  
                  bindingConfiguration="portSharingBinding" />  
    </service>  
  </services>  
</system.serviceModel>  
```  
  
## <a name="see-also"></a>另請參閱

- [Net.TCP 連接埠共用](../../../../docs/framework/wcf/feature-details/net-tcp-port-sharing.md)
- [如何：啟用 Net.TCP 連接埠共用服務](../../../../docs/framework/wcf/feature-details/how-to-enable-the-net-tcp-port-sharing-service.md)
