---
description: Este artigo descreve problemas conhecidos e dicas de solução de problemas para a ferramenta de assinatura
title: Problemas conhecidos e solução de problemas com SignTool
ms.date: 02/05/2020
ms.topic: article
author: dianmsft
keywords: Windows 10, UWP, MSIX
ms.localizationpriority: medium
ms.openlocfilehash: e010332612988673f9339917a3d0c0867a1cc818
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77073692"
---
# <a name="known-issues-and-troubleshooting-for-signtool"></a>Problemas conhecidos e solução de problemas para SignTool
Os tipos mais comuns de erros ao usar o **SignTool** são internos e normalmente se parecem com isso:

```syntax
SignTool Error: An unexpected internal error has occurred.
Error information: "Error: SignerSign() failed." (-2147024885 / 0x8007000B) 
```

Se o código de erro começa com 0x8008, como 0x80080206 (APPX_E_CORRUPT_CONTENT), o pacote que está sendo assinado não é válido. Se você receber esse tipo de erro, você deve recriar o pacote e executar o **SignTool** novamente.

O **SignTool** tem uma opção de depuração disponível para mostrar erros de certificado e filtragem. Para usar o recurso de depuração, coloque a opção `/debug` diretamente após `sign`, seguida pelo comando **SignTool** completo.

```syntax
SignTool sign /debug [options]
``` 

Um erro mais comum é o 0x8007000B. Para esse tipo de erro, você pode encontrar mais informações no log de eventos.
 
Para obter mais informações no log de eventos:
- Execute Eventvwr.msc
- Abra o log de eventos: Visualizador de Eventos (Local) -> Aplicativos e Logs de Serviços -> Microsoft -> Windows -> AppxPackagingOM -> Microsoft-Windows-AppxPackaging/Operational
- Localize o evento de erro mais recente

O erro interno 0x8007000B geralmente corresponde a um destes valores:

| **ID do evento** | **Exemplo de cadeia de caracteres de evento** | **Sugerir** |
|--------------|--------------------------|----------------|
| 150          | Erro 0x8007000B: O nome do editor de manifesto de aplicativo (CN = Contoso) deve coincidir com o nome do requerente do certificado de autenticação (CN = Contoso, C = US). | O nome do editor de manifesto de aplicativo deve corresponder exatamente ao nome do assunto após a assinatura.               |
| 151          | Erro 0x8007000B: O método de hash de assinatura especificado (SHA512) deve coincidir com o método de hash usado no mapa de blocos do pacote de aplicativos (SHA256).     | O hashAlgorithm especificado no parâmetro /fd está incorreto. Execute o **SignTool** usando o hashAlgorithm que corresponda ao mapa de blocos do pacote de aplicativos (usado para criar o pacote de aplicativos)  |
| 152          | Erro 0x8007000B: O conteúdo do pacote de aplicativos deve ser validado em relação ao mapa de blocos.                                                           | O pacote de aplicativos está corrompido e precisa ser recompilado para gerar um novo mapa de blocos. Para saber mais sobre como criar um pacote de aplicativos, consulte [Criar um pacote de aplicativos com a ferramenta MakeAppx.exe](create-app-package-with-makeappx-tool.md). |