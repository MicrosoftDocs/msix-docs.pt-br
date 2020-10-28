---
description: Este artigo descreve problemas conhecidos e dicas de solução de problemas para a ferramenta de assinatura
title: Problemas conhecidos e solução de problemas com SignTool
ms.date: 02/05/2020
ms.topic: article
author: dianmsft
keywords: Windows 10, UWP, MSIX
ms.localizationpriority: medium
ms.openlocfilehash: fe664b00541f405181862b440530937f1752677d
ms.sourcegitcommit: 4ecff6f1386c6239cfd79ddf82265f4194302bbb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2020
ms.locfileid: "92686853"
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

| **ID do evento** | **Cadeia de caracteres de evento de exemplo** | **Sugerir** |
|--------------|--------------------------|----------------|
| 150          | Erro 0x8007000B: O nome do editor de manifesto de aplicativo (CN = Contoso) deve coincidir com o nome do requerente do certificado de autenticação (CN = Contoso, C = US). | O nome do editor de manifesto de aplicativo deve corresponder exatamente ao nome do assunto após a assinatura.               |
| 151          | Erro 0x8007000B: O método de hash de assinatura especificado (SHA512) deve coincidir com o método de hash usado no mapa de blocos do pacote de aplicativos (SHA256).     | O hashAlgorithm especificado no parâmetro /fd está incorreto. Execute o **SignTool** usando o hashAlgorithm que corresponda ao mapa de blocos do pacote de aplicativos (usado para criar o pacote de aplicativos)  |
| 152          | Erro 0x8007000B: O conteúdo do pacote de aplicativos deve ser validado em relação ao mapa de blocos.                                                           | O pacote de aplicativos está corrompido e precisa ser recompilado para gerar um novo mapa de blocos. Para saber mais sobre como criar um pacote de aplicativos, consulte [Criar um pacote de aplicativos com a ferramenta MakeAppx.exe](create-app-package-with-makeappx-tool.md). |

Outro erro comum é 0x80080057. Você pode enfrentar os seguintes problemas ao tentar assinar um arquivo executável portátil (PE) usando a ferramenta SignTool no Windows:

- Falha ao assinar um arquivo PE que é de 4 gigabytes (GB) ou maior. Ao tentar assinar, você recebe uma mensagem de erro "parâmetro inválido (0x80080057)".
- Para arquivos com mais de 4 GB, o hash gerado pode não ser preciso mesmo que o arquivo de SignTool possa, de outra forma, ser assinado com êxito.

   > [!NOTE]
   > Isso é especialmente verdadeiro para arquivos. Cat.

Esse problema ocorre para arquivos PE como. exe,. sys e assim por diante. Esse problema ocorre devido a uma variável ULONG no cabeçalho PE que especifica o tamanho da imagem. (O tamanho da imagem é de 2 GB para sistemas operacionais de nível inferior, como vista e versões anteriores.) Essa é uma limitação de design desde 1996. O limite máximo para esse valor é de 4 GB para arquivos PE, como. exe e. sys. Embora arquivos. Cat normalmente sejam conectáveis, o hash interno gerado pode não ser preciso.

Para contornar esse problema, certifique-se de que qualquer arquivo PE que você tentar assinar seja menor que 4 GB. Devido a riscos de compatibilidade com versões anteriores, atualmente, nem as portas invertidas nem uma correção permanente são possíveis. No entanto, esse problema está sendo investigado.

> [!NOTE]
> Esse problema não é específico de SignTool. O design do cabeçalho PE é limitado a 4 GB para o Windows 7 e versões posteriores do Windows, independentemente de qual ferramenta é usada.

## <a name="frequently-asked-questions-faq"></a>Perguntas frequentes (FAQ)

Q1: Qual é o limite de tamanho de arquivo oficial e atual para uma assinatura digital (e assinatura de contador de carimbo de data/hora) no Windows?

A1: para arquivos PE, como. exe e. sys, o tamanho máximo do arquivo para assinatura é de 4 GB.

Q2: existe uma versão específica do Windows, como o Windows Server 2016, que tem a capacidade de assinar arquivos grandes?

A2: não, o problema afeta todas as versões do Windows.

Q3: a versão de 64 bits de SignTool tem melhor suporte para essa funcionalidade do que a versão de 32 bits?

R: não, a versão de 64 bits de SignTool usa os mesmos valores que a versão de 32 bits. Portanto, o problema permanece em 64 bits.

Q4: os clientes que estiverem usando uma versão de 32 bits do Windows potencialmente terão problemas se tentarem usar arquivos que foram assinados usando a versão de 64 bits de SignTool?

R: Não. No entanto, as limitações permanecerão independentemente de qual versão de SignTool é usada.

P5: devemos usar uma ferramenta ou um método de assinatura diferente?

R: no momento, não temos nenhum método alternativo para assinatura digital.





