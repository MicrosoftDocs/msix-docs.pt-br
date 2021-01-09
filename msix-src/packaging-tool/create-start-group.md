---
title: Agrupar aplicativos em uma pasta no menu iniciar
description: Descreve como habilitar vários aplicativos MSIX empacotados para serem agrupados em uma única pasta no menu iniciar
ms.date: 12/02/2020
ms.topic: article
keywords: Windows 10, UWP, msix, Appx
ms.localizationpriority: medium
ms.openlocfilehash: eb7202b5cef5a50f8dbe52c61be4bb581d8a8cce
ms.sourcegitcommit: 3a9aae783331833bbf244091544c48848768137e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/08/2021
ms.locfileid: "98043279"
---
# <a name="group-applications-under-a-folder-in-the-start-menu"></a>Agrupar aplicativos em uma pasta no menu iniciar

> [!IMPORTANT]
> Esse recurso está disponível no momento em compilações preliminares do Windows 10 que são distribuídas por meio do anel de desenvolvimento do [programa Windows Insider](https://insider.windows.com/en/). Será necessário pelo menos o Build 20257 para habilitar esse recurso.

O manifesto de um aplicativo empacotado MSIX contém uma ou mais `<Application>` entradas, que são os pontos de entrada disponíveis. Cada um deles se tornará um ícone no menu iniciar.

Um pacote MSIX pode conter vários aplicativos. Como alternativa, uma empresa pode criar vários aplicativos, que são empacotados como pacotes MSIX separados, mas todos pertencem ao mesmo conjunto.
Em ambos os cenários, talvez você queira agrupar todas as entradas no menu iniciar em uma única pasta, para que, para o usuário, seja mais fácil localizar todos os aplicativos no mesmo local.

Essa meta pode ser obtida usando a `VisualGroup` Propriedade do `VisualElements` Item.
Estas são as etapas para implementar essa alteração:

1) Abra o arquivo de manifesto do seu aplicativo com um editor de texto escolhido. Como alternativa, se você estiver usando a ferramenta de empacotamento MSIX, poderá pressionar o botão *abrir manifesto* no editor de pacote.
2) Verifique se o `uap3` namespace está declarado no `<Package>` nó do manifesto:

    ```xml
    <Package ...
         xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"  
         IgnorableNamespaces="... uap3">
        ...
   </Package>
    ```

3) Localize a seção `Applications`. Dentro de você encontrará uma ou mais `Application` entradas, uma para cada ícone que será criado no menu iniciar. É assim que se parecerá com:

    ```xml
      <Applications>
          <Application>
              <VisualElements DisplayName="App1" 
                              Square150x150Logo="images/150x150.png"
                              Square44x44Logo="images/44x44.png"
                              Description="App1"
                              BackgroundColor="#777777"
                              AppListEntry="default">  
                  <uap:SplashScreen BackgroundColor="#777777"
                                    Image="images/splash.png"/>  
              </VisualElements>  
          </Application>
          <Application>
              ...
          </Application>
      </Applications>
    ```

4) Adicione o `uap3` prefixo à `VisualElements` seção. Lembre-se de adicioná-lo tanto às marcas de abertura quanto de término:

    ```xml
      <Applications>
          <Application>
              <uap3:VisualElements DisplayName="App1"
                                   Square150x150Logo="images/150x150.png"
                                   Square44x44Logo="images/44x44.png"
                                   Description="App1"
                                   BackgroundColor="#777777"
                                   AppListEntry="default">  
                  <uap:SplashScreen BackgroundColor="#777777"
                                    Image="images/splash.png"/>  
              </uap3:VisualElements>  
          </Application>
          <Application>
              ...
          </Application>
      </Applications>
    ```

5) Por fim, adicione o `VisualGroup` atributo ao `VisualElements` Item. Como valor, defina o nome que você deseja dar à pasta que será criada no menu iniciar.

    ```xml
      <Applications>
          <Application>
              <uap3:VisualElements DisplayName="App1"
                                   Square150x150Logo="images/150x150.png"
                                   Square44x44Logo="images/44x44.png"
                                   Description="App1"
                                   BackgroundColor="#777777"
                                   AppListEntry="default"
                                   VisualGroup="MyFolder">  
                  <uap:SplashScreen BackgroundColor="#777777"
                                    Image="images/splash.png"/>  
              </uap3:VisualElements>  
          </Application>
          <Application>
              ...
          </Application>
      </Applications>
    ```

Agora você pode repetir o processo para todas as outras `<Application>` entradas que deseja incluir na mesma pasta. Opcionalmente, você também pode fazer o mesmo com outros aplicativos, simplesmente editando o arquivo de manifesto incluído em seu pacote MSIX da mesma maneira e usando o mesmo valor para o `VisualGroup` atributo.
