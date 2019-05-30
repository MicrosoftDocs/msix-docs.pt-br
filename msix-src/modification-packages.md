---
title: Personalizar seus aplicativos empresariais com pacotes de modificação
description: Saiba como personalizar seus aplicativos corporativos
author: mcleanbyron
ms.author: mcleans
ms.date: 01/15/2019
ms.topic: article
keywords: Windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 142fce0927d916376733a0380fab6a0dc026a55e
ms.sourcegitcommit: 5669d59a0979a9de1dead4949f44d1544fd45988
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65795410"
---
# <a name="customize-your-enterprise-apps-with-modification-packages"></a>Personalizar seus aplicativos empresariais com pacotes de modificação 

A capacidade de personalizar a experiência do aplicativo é importante, especialmente para empresas. Podemos ter falado para profissionais de TI e sabemos que a personalização de aplicativos para atender às suas necessidades de usuário é essencial para o esforço de mover para o Windows 10. Ao personalizar os aplicativos que são empacotados usando o MSI, é bem compreendida que os profissionais de TI devem adquirir o pacote dos desenvolvedores e empacotar novamente o instalador com a personalização para atender às suas necessidades. Isso é um esforço dispendioso para empresas. Mais adiante, queremos desacoplar a personalização e o aplicativo principal para que o empacotamento novamente não é mais necessário. Isso garante que as empresas obterem as atualizações mais recentes dos desenvolvedores e ainda manter o controle das suas personalizações.

No Windows 10 versão 1809, apresentamos um novo tipo de pacote MSIX chamado um *pacote de modificação*. Pacotes de modificação são pacotes MSIX que armazenam as personalizações. Pacotes de modificação também podem ser plug-ins/add-complementos que podem não ter um ponto de ativação. Os profissionais de TI podem usar esse recurso para alterar com flexibilidade contêineres MSIX, de modo que os aplicativos são sobrepostos por personalizações da sua empresa. Estamos empolgados sobre esse recurso e aparência encaminhar para introduzir os aprimoramentos no futuro libera. 

## <a name="how-it-works"></a>Como funciona

Pacotes de modificação são projetados para as empresas que não possuem o código do aplicativo e ter apenas o instalador. Você pode criar um pacote de modificação, usando a versão mais recente da ferramenta de empacotamento MSIX (Windows 10 versão 1809 ou posterior). Se você tiver o código do aplicativo, você também pode criar uma [extensão do aplicativo](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-an-extension). 

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">Obter a ferramenta de empacotamento MSIX</a></p></div>

Se você quiser criar um pacote de modificação que possui uma associação estrita para o aplicativo principal, você pode declarar o aplicativo principal como uma dependência no manifesto do pacote de modificação. 

``` xml
<Dependencies>
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.15063.0"/>
    <uap4:MainPackageDependency Name="Main.App"/>
</Dependencies>
```

O exemplo a seguir demonstra como especificar um certificado diferente ou um editor.

``` xml
<Dependencies>
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.15063.0"/>
    <uap4:MainPackageDependency Name="Main.App" Publisher="CN=Contoso, C=US" />
</Dependencies>

```

Isso é uma configuração simples, se a relação entre o pacote de modificação e o pacote principal é um para um. Personalizações típicas geralmente exigem chaves do registro em HKEY_CURRENT_USER ou HKEY_CURRENT_USERCLASS. Dentro de nosso pacote MSIX temos os arquivos DAT e Userclass.dat para capturar as chaves do registro. Você precisará criar User. dat, se você precisar de chaves do registro em HKCU\Software\* (assim como Registry.dat é usado para HKLM\Software\*). Use Userclass.dat se precisar de chaves em HKCU\Sofware\Classes\*. 

Aqui estão as maneiras comuns de criar um arquivo. dat: 

1.  Use Regedit para criar um arquivo. Criar um hive regedit e inserir as chaves necessárias. De clique à direita, exportar e salvar-como arquivo de hive. Certifique-se de nomear o arquivo qualquer dat ou Userclass.dat

2.  Use uma API para criar os arquivos necessários. Publicamos uma [API](https://msdn.microsoft.com/en-us/library/ee210773(v=vs.85).aspx) que você pode usar para salvar um arquivo. dat. Certifique-se de nomear o ether arquivo User. dat ou Userclass.dat

Após fazer as alterações necessárias, você pode criar o pacote de modificação, como qualquer outro pacote MSIX. Em seguida, você pode implantar o pacote com a configuração de implantação atual. Quando você reiniciar seu aplicativo principal, você pode ver as alterações que fez o pacote de modificação. Se você optar por remover o pacote de modificação, seu aplicativo principal será revertido para um estado sem o pacote de modificação. 

## <a name="see-also"></a>Consulte também
- [Pacotes de modificação no Windows 10 versão 1809](modification-package-1809-update.md)
- [Pacotes de modificação no Windows 10 versão 1903](modification-package-1903.md)
