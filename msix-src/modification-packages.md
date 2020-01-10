---
title: Personalizar seus aplicativos empresariais com pacotes de modificação
description: Este artigo descreve como personalizar seus aplicativos empresariais usando os pacotes MSIX de modificação que armazenam as personalizações.
ms.date: 01/15/2019
ms.topic: article
keywords: Windows 10, UWP, MSIX
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 6db2a7aa009f0a4bce9a2aa8950e029473df63aa
ms.sourcegitcommit: 90eed7d23240aefa3761085955a193323f4661d4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75831458"
---
# <a name="customize-your-enterprise-apps-with-modification-packages"></a>Personalizar seus aplicativos empresariais com pacotes de modificação 

A capacidade de personalizar a experiência de um aplicativo é importante, especialmente para empresas. Falamos aos profissionais de ti e sabemos que a personalização de aplicativos para atender às necessidades do usuário é essencial para o esforço de migrar para o Windows 10. Ao personalizar aplicativos que são empacotados usando o MSI, é bem compreendido que os profissionais de ti devem adquirir o pacote dos desenvolvedores e reempacotar o instalador com a personalização para atender às suas necessidades. Esse é um esforço dispendioso para as empresas. Avançando, queremos desacoplar a personalização e o aplicativo principal para que o reempacotamento não seja mais necessário. Isso garante que as empresas obtenham as atualizações mais recentes dos desenvolvedores e, ao mesmo tempo, mantenham o controle de suas personalizações.

No Windows 10 versão 1809, apresentamos um novo tipo de pacote MSIX chamado de *pacote de modificação*. Os pacotes de modificação são pacotes MSIX que armazenam personalizações. Os pacotes de modificação também podem ser plug-ins/complementos que podem não ter um ponto de ativação. Os profissionais de ti podem usar esse recurso para alterar de maneira flexível os contêineres de MSIX para que os aplicativos sejam sobrepostos pelas personalizações da sua empresa. Estamos empolgados com esse recurso e estamos ansiosos para introduzir aprimoramentos em versões futuras. 

## <a name="how-it-works"></a>Como funciona

Os pacotes de modificação são projetados para empresas que não possuem o código do aplicativo e só têm o instalador. Você pode criar um pacote de modificação usando a versão mais recente da ferramenta de empacotamento MSIX (para Windows 10 versão 1809 ou posterior). Se você tiver o código para o aplicativo, você pode criar uma extensão de [aplicativo](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-an-extension)como alternativa. 

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

Essa é uma configuração simples se a relação entre o pacote de modificação e o pacote principal for um para um. As personalizações típicas geralmente exigem chaves do registro em HKEY_CURRENT_USER ou HKEY_CURRENT_USERCLASS. Dentro de nosso pacote MSIX temos os arquivos user. dat e userclass. dat para capturar as chaves do registro. Você precisará criar User. dat se precisar de chaves do registro em HKCU\Software\* (assim como o Registry. dat é usado para HKLM\Software\*). Use userclass. dat se você precisar de chaves em HKCU\Sofware\Classes\*. 

Aqui estão as maneiras típicas de criar um arquivo. dat: 

1. Use regedit para criar um arquivo. Crie um Hive no regedit e insira as chaves necessárias. Em vez de clicar com o botão direito, exportar e salvar como arquivo do hive. Certifique-se de nomear o arquivo como User. dat ou userclass. dat

2. Use uma API para criar os arquivos necessários. Você pode usar a função [ORSaveHive](https://docs.microsoft.com/windows/win32/devnotes/orsavehive) para salvar um arquivo. dat. Certifique-se de nomear o arquivo de ether User. dat ou userclass. dat

Depois de fazer as alterações necessárias, você pode criar o pacote de modificação como qualquer outro pacote MSIX. Em seguida, você pode implantar o pacote com a configuração de implantação atual. Ao reiniciar o aplicativo principal, você pode ver as alterações feitas pelo pacote de modificação. Se você optar por remover o pacote de modificação, seu aplicativo principal será revertido para um estado sem o pacote de modificação. 

## <a name="find-out-what-modification-packages-are-installed-on-your-device"></a>Descubra quais pacotes de modificação estão instalados em seu dispositivo
Usando o PowerShell, você pode ver os pacotes de modificação instalados usando: Get-AppPackage-PackageTypeFilter opcional

## <a name="see-also"></a>Veja também
- [Pacotes de modificação no Windows 10 versão 1809](modification-package-1809-update.md)
- [Pacotes de modificação no Windows 10 versão 1903](modification-package-1903.md)
