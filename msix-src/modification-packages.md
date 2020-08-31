---
title: Personalizar seus aplicativos empresariais com pacotes de modificação
description: Este artigo descreve como personalizar seus aplicativos empresariais usando os pacotes MSIX de modificação que armazenam as personalizações.
ms.date: 01/15/2019
ms.topic: article
keywords: Windows 10, UWP, MSIX
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: d890ff472baa343c5b87873f85fce1b435d37b62
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/29/2020
ms.locfileid: "89090324"
---
# <a name="customize-your-enterprise-apps-with-modification-packages"></a>Personalizar seus aplicativos empresariais com pacotes de modificação

A capacidade de personalizar a experiência de um aplicativo é importante, especialmente para empresas. Falamos aos profissionais de ti e sabemos que a personalização de aplicativos para atender às necessidades do usuário é essencial para o esforço de migrar para o Windows 10. Ao personalizar aplicativos que são empacotados usando o MSI, é bem compreendido que os profissionais de ti devem adquirir o pacote dos desenvolvedores e reempacotar o instalador com a personalização para atender às suas necessidades. Esse é um esforço dispendioso para as empresas. Avançando, queremos desacoplar a personalização e o aplicativo principal para que o reempacotamento não seja mais necessário. Isso garante que as empresas obtenham as atualizações mais recentes dos desenvolvedores e, ao mesmo tempo, mantenham o controle de suas personalizações.

No Windows 10, versão 1809, apresentamos um novo tipo de pacote MSIX chamado de *pacote de modificação*. Os pacotes de modificação são pacotes MSIX que armazenam personalizações. Os pacotes de modificação também podem ser plug-ins/complementos que podem não ter um ponto de ativação. Os profissionais de ti podem usar esse recurso para alterar de maneira flexível os contêineres de MSIX para que os aplicativos sejam sobrepostos pelas personalizações da sua empresa.

## <a name="how-it-works"></a>Como ele funciona

Os pacotes de modificação são projetados para empresas que não possuem o código do aplicativo e só têm o instalador. Você pode criar um pacote de modificação usando a versão mais recente da ferramenta de empacotamento MSIX (para Windows 10 versão 1809 ou posterior). Se você tiver o código para o aplicativo, você pode criar uma extensão de [aplicativo](/windows/uwp/launch-resume/how-to-create-an-extension)como alternativa. 

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">Obter a Ferramenta de Empacotamento MSIX</a></p></div>

Se desejar criar um pacote de modificação que tenha uma ligação estrita ao aplicativo principal, você poderá declarar o aplicativo principal como uma dependência no manifesto do pacote de modificação. 

``` xml
<Dependencies>
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.15063.0"/>
    <uap4:MainPackageDependency Name="Main.App"/>
</Dependencies>
```

O exemplo a seguir demonstra como especificar um certificado ou Publicador diferente.

``` xml
<Dependencies>
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.15063.0"/>
    <uap4:MainPackageDependency Name="Main.App" Publisher="CN=Contoso, C=US" />
</Dependencies>

```

Essa é uma configuração simples se a relação entre o pacote de modificação e o pacote principal for um para um. As personalizações típicas geralmente exigem chaves do registro em HKEY_CURRENT_USER ou HKEY_CURRENT_USERCLASS. Dentro de nosso pacote MSIX temos os arquivos user. dat e userclass. dat para capturar as chaves do registro. Você precisará criar User. dat se precisar de chaves do registro em HKCU\Software \* (assim como Registry. dat é usado para HKLM\Software \* ). Use userclass. dat se você precisar de chaves em HKCU\Sofware\Classes \* . 

Aqui estão as maneiras típicas de criar um arquivo. dat:

* Use o regedit para criar um arquivo. Crie um Hive no regedit e insira as chaves necessárias. Em vez de clicar com o botão direito, exportar e salvar como arquivo do hive. Certifique-se de nomear o arquivo como User. dat ou userclass. dat

* Use uma API para criar os arquivos necessários. Você pode usar a função [ORSaveHive](/windows/win32/devnotes/orsavehive) para salvar um arquivo. dat. Certifique-se de nomear o arquivo de ether User. dat ou userclass. dat

Depois de fazer as alterações necessárias, você pode criar o pacote de modificação como qualquer outro pacote MSIX. Em seguida, você pode implantar o pacote com a configuração de implantação atual. Ao reiniciar o aplicativo principal, você pode ver as alterações feitas pelo pacote de modificação. Se você optar por remover o pacote de modificação, seu aplicativo principal será revertido para um estado sem o pacote de modificação. 

## <a name="find-out-what-modification-packages-are-installed-on-your-device"></a>Descubra quais pacotes de modificação estão instalados em seu dispositivo

Usando o PowerShell, você pode ver os pacotes de modificação instalados usando o comando a seguir.

```powershell
Get-AppPackage -PackageTypeFilter Optional
```

## <a name="modification-packages-on-windows-10-version-1809"></a>Pacotes de modificação no Windows 10, versão 1809

No Windows 10, versão 1809, os pacotes de modificação podem incluir configurações necessárias para serem definidas no registro, de modo que o pacote principal será executado conforme o esperado. O que significa que seu aplicativo principal utiliza o registro para exibir se existe um plug-in. Quando você implantar o pacote principal e o pacote de modificação, em runtime, o aplicativo exibirá o VREG (Registro virtual) do pacote principal e do pacote de modificação.

Observe que o pacote principal pode estar usando o VREG para fazer o seguinte:

* Exibindo onde carregar o arquivo (a DLL) do plug-in. Se esse for o caso, verifique se o arquivo faz parte do pacote. Ao fazer isso, o pacote principal pode acessar o arquivo em runtime.
* Exibir em que local o valor das chaves do VREG pode ser visto. O pacote principal pode estar procurando um valor que existe no VREG. Ao criar o pacote de modificação manualmente ou usando nossa [ferramenta](https://www.microsoft.com/p/msix-packaging-tool/9n5lw3jbcxkf), verifique se o valor está correto.

## <a name="modification-packages-on-windows-10-version-1903-and-later"></a>Pacotes de modificação no Windows 10, versão 1903 e posterior

Os recursos a seguir foram adicionados ao Windows 10, versão 1903.

### <a name="manifest-update"></a>Atualização do manifesto

Adicionamos suporte no elemento a seguir ao manifesto do pacote de modificação MSIX.

```xml
<Properties>
   <rescap6:ModificationPackage>true</rescap6:ModificationPackage>
</Properties>
```

Para garantir que os pacotes de modificação funcionem na versão 1903 ou posterior, o manifesto do pacote de modificação precisarão incluir esse elemento. Isso será feito para você se empacotar o pacote de modificação MSIX usando a versão de janeiro da Ferramenta de Empacotamento MSIX. Se você converter um pacote usando nossa ferramenta anterior à versão, poderá editar o pacote existente na ferramenta para adicionar esse novo elemento. Além disso, se os usuários instalarem o pacote de modificação, eles serão alertados de que o pacote poderá modificar o aplicativo principal.

Se você estiver usando um pacote de modificação que foi criado antes da versão 1903, será necessário editar o manifesto do pacote para atualizar o `MaxVersionTested` atributo para 10.0.18362.0.

```xml
<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.17701.0" MaxVersionTested="10.0.18362.0" />
```

### <a name="create-a-modification-package-using-the-msix-packaging-tool"></a>Criar um pacote de modificação usando a Ferramenta de Empacotamento MSIX

Crie um pacote de modificação com a Ferramenta de Empacotamento MSIX:

* Especifique o pacote principal. Verifique se você tem a versão do MSIX do pacote principal disponível no computador no qual está sendo feita a conversão. Caos contrário, solicitamos que você forneça manualmente as informações do fornecedor e do aplicativo principal. Além disso, uma personalização exige a instalação do aplicativo principal no computador.
![MPT do pacote de modificação](images/MPT-mod-page.png)

* Modifique o pacote depois que ele passar pela conversão usando o editor de pacote. Poderá haver casos em que o pacote principal exija que o pacote de modificação tenha certos valores no VREG. É nesse momento que você pode acessar e editar o pacote de forma adequada.

### <a name="create-a-modification-package-using-makeappxexe"></a>Criar um pacote de modificação usando o MakeAppx.exe

Você pode criar um pacote de modificação manualmente usando a ferramenta de [MakeAppX.exe](package/create-app-package-with-makeappx-tool.md) que está incluída no SDK do Windows 10.

* No manifesto, especifique o pacote principal. Inclua o fornecedor e o nome do pacote principal.

    ```xml
    <Dependencies>
      <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.17701.0" MaxVersionTested="12.0.0.0"/>
      <uap4:MainPackageDependency Name="HeadTrax" Publisher="CN=Contoso Software, O=Contoso Corporation, C=US" />
    </Dependencies>
    ```

* Crie Registry.dat, User.dat e Userclass.dat para criar as chaves do Registro necessárias para carregar o pacote de modificação. Isso só será necessário se você precisar que o aplicativo principal exiba chaves do Registro personalizadas. Lembre-se de que, como tudo está sendo executado dentro de um contêiner, em runtime, o Registro virtual do pacote principal e do pacote de modificação será mesclado, de modo que o pacote principal possa exibir o Registro virtual dos pacotes de modificação.  

Esse processo também dá suporte aos plug-ins e às personalizações do sistema de arquivos, desde que o executável do aplicativo principal não esteja em um VFS (sistema de arquivos virtual). Isso serve para garantir que o pacote principal obtenha todo o VFS do pacote principal e do pacote de modificação.