---
title: Implantar um pacote MSIX com o MSIX Core
description: Descreve como implantar um pacote MSIX com o MSIX Core usando a ferramenta msixmgr. exe.
ms.date: 12/19/2019
ms.topic: article
keywords: Windows 10, Windows 7, Windows 8, Windows Server, UWP, msix, msixcore, 1709, 1703, 1607, 1511, 1507
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 98ca9fce99139608da8e59108d68756717c605ae
ms.sourcegitcommit: 0412ba69187ce791c16313d0109a5d896141d44c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/20/2019
ms.locfileid: "75303350"
---
# <a name="deploy-an-msix-package-with-msix-core"></a>Implantar um pacote MSIX com o MSIX Core

O [MSIX Core](msixcore.md) traz a implantação do MSIX para selecionar versões anteriores do Windows. Para começar, primeiro verifique se o MSIX Core está instalado no dispositivo de destino.

## <a name="msi-installation"></a>Instalação do MSI

É recomendável usar nossos instaladores de MSI fornecidos para instalar o MSIX Core, pois eles adicionam automaticamente o msixmgr. exe ao caminho de pesquisa e associam a extensão MSIX ao instalador.

Você pode baixar os seguintes instaladores de MSI específicos da arquitetura da seção **ativos** em nossa [página de lançamento](https://github.com/microsoft/msix-packaging/releases):

* **msixmgrSetup-x64. msi**
* **msixmgrSetup-86. msi**

> [!NOTE]
> certifique-se de escolher o instalador correto para a arquitetura do dispositivo. Isso afetará o local em que o instalador armazenará arquivos importantes. O nome do arquivo pode ser alterado com base na versão do instalador.

## <a name="installing-your-certificate"></a>Instalando seu certificado

Os pacotes MSIX devem ser assinados. Antes de instalar qualquer pacote MSIX, verifique se você instalou o certificado usado para assinar seus pacotes. Você pode fazer isso usando fluxos de trabalho normais para instalar o certificado de sua ferramenta de gerenciamento.

Se você quiser instalar um certificado manualmente, poderá executar esse comando em um prompt de comando elevado:

```PowerShell
certutil -addstore root <insert certificate.cert>
```

> [!NOTE]
> você deve adicionar seu certificado confiável na autoridade de certificação raiz confiável em todos os cenários.

## <a name="using-the-command-line"></a>Usando a linha de comando

Depois que a ferramenta msixmgr. exe for instalada, ela poderá ser usada para gerenciar seus pacotes do MSIX neste computador pesquisando, instalando e removendo. O utilitário de linha de comando msixmgr. exe destina-se a administradores de sistema. Ele é mais útil quando executado do prompt do administrador. Nem todos os comandos quando executados a partir de um prompt de comando regular serão exibidos para o console do. Veja abaixo para obter mais detalhes.

### <a name="install"></a>Instalar

Usando o prompt de comando ou o PowerShell, navegue até o diretório que contém msixmgr. exe e execute o comando a seguir para instalar o pacote MSIX. O parâmetro `-quietUX` também pode ser adicionado no final do comando para que os usuários não vejam a interface do usuário do instalador.

```PowerShell
msixmgr.exe -AddPackage C:\NotePadPlus\notepadplus.msix -quietUX
```

> [!NOTE]
> isso e os exemplos a seguir usam **notepadplus. msix**. Este é um dos nossos [pacotes de exemplo](https://github.com/microsoft/msix-packaging/tree/master/MsixCore/Tests).

### <a name="querying-for-a-specific-msix-package"></a>Consultando um pacote MSIX específico

Pesquisar um pacote específico é possível por **packageFullName**, **packageFamilyName** e/ou usar curingas também. Os curingas com suporte são * (corresponder a qualquer caractere) e? (corresponder um único caractere).

```PowerShell
msixmgr.exe -FindPackage notepadplus_0.0.0.1_???__8wekyb3d8bbwe
msixmgr.exe -FindPackage *padplus_0.0.*
msixmgr.exe -FindPackage *epadplus_8wekyb3d8bbw?
```

### <a name="uninstall"></a>Uninstall

Para desinstalar, use o seguinte comando:

```PowerShell
msixmgr.exe -RemovePackage notepadplus_0.0.0.1_x64__8wekyb3d8bbwe -quietUX
```