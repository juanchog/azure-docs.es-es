---
title: Archivos multimedia de Hyperlapse con Azure Media Hyperlapse | Microsoft Docs
description: Azure Media Hyperlapse crea vídeos fluidos con la técnica time-lapse a partir de contenido generado en primera persona o con una cámara de acción. En este tema se muestra cómo usar el Indizador multimedia.
services: media-services
documentationcenter: ''
author: asolanki
manager: johndeu
editor: ''
ms.assetid: 37d54db6-9cf3-4ae9-b3c6-0d29c744e965
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/28/2018
ms.author: adsolank
ms.openlocfilehash: ed64a616538ed4699abc03225a2dcf27d164521f
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2018
---
# <a name="hyperlapse-media-files-with-azure-media-hyperlapse"></a>Archivos multimedia de Hyperlapse con Azure Media Hyperlapse
Azure Media Hyperlapse es un procesador de multimedia (MP) que crea vídeos fluidos con la técnica time-lapse a partir de contenido generado en primera persona o con una cámara de acción.  Microsoft Hyperlapse para Azure Media Services, el producto para la nube de [Hyperlapse Pro (para equipos de escritorio) y Hyperlapse Mobile (para móviles) de Microsoft Research](http://aka.ms/hyperlapse), usa la escalación masiva de la plataforma de procesamiento de medios de Azure Media Services para escalar horizontalmente y establecer paralelismos en el procesamiento masivo de Hyperlapse.

> [!IMPORTANT]
> Microsoft Hyperlapse está diseñado para funcionar mejor con los contenidos grabados en primera persona con una cámara móvil. Aunque la grabación con cámara fija también puede funcionar, no se garantiza el rendimiento y la calidad del procesador multimedia Azure Media Hyperlapse para otros tipos de contenido.
> 
> 

Un trabajo de Azure Media Hyperlapse toma como entrada un archivo de recurso en formato MP4, MOV o WMV, junto con un archivo de configuración que especifica a qué fotogramas de vídeo se debe aplicar el time-lapse y a qué velocidad (por ejemplo, los primeros 10.000 fotogramas a 2x).  El resultado es una representación estabilizada y en time-lapse del vídeo de entrada.

## <a name="hyperlapse-an-asset"></a>Aplicar Hyperlapse a un recurso
Primero debe cargar el archivo de entrada deseado en Azure Media Services.  Para obtener más información acerca de los conceptos relacionados con la carga y la administración de contenido, consulte el [artículo de administración de contenido](media-services-portal-vod-get-started.md).

### <a id="configuration"></a>Preestablecimiento de configuración para Hyperlapse
Una vez que el contenido esté en su cuenta de Media Services, deberá generar la configuración preestablecida.  En la tabla siguiente se describen los campos especificados por el usuario:

| Campo | DESCRIPCIÓN |
| --- | --- |
| StartFrame |El fotograma en el que debe comenzar el procesamiento de Microsoft Hyperlapse. |
| NumFrames |El número de fotogramas para procesar. |
| Velocidad |El factor por el que se acelera el vídeo de entrada. |

El siguiente es un ejemplo de un archivo de configuración compatible en JSON y XML:

**Valor preestablecido XML:**
```xml
    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
        <Sources>
            <Source StartFrame="0" NumFrames="10000" />
        </Sources>
        <Options>
            <Speed>12</Speed>
        </Options>
    </Preset>
```

**Valor preestablecido JSON:**
```json
    {
        "Version":1.0,
        "Sources": [
            {
                "StartFrame":0,
                "NumFrames":2147483647
            }
        ],
        "Options": {
            "Speed":1,
            "Stabilize":false
        }
    }
```

### <a id="sample_code"></a> Microsoft Hyperlapse con el SDK de .NET de AMS
El método siguiente carga un archivo multimedia como un recurso y crea un trabajo con el procesador de multimedia de Azure Media Hyperlapse.

> [!NOTE]
> Ya debe tener un CloudMediaContext con el nombre "context" para que este código funcione.  Para obtener más información al respecto, lea el [artículo sobre administración de contenido](media-services-dotnet-get-started.md).
> 
> [!NOTE]
> El argumento de cadena "hyperConfig" debe ser una configuración preestablecida compatible en JSON o XML, como se describió anteriormente.
> 
> 

```csharp
        static bool RunHyperlapseJob(string input, string output, string hyperConfig)
        {
            // create asset with input file
            IAsset asset = context
            .Assets
            .CreateAssetAndUploadSingleFile(input, "My Hyperlapse Input", AssetCreationOptions.None);

            // grab instances of Azure Media Hyperlapse MP
            IMediaProcessor mp = context
            .MediaProcessors
            .GetLatestMediaProcessorByName("Azure Media Hyperlapse");

            // create Job with Hyperlapse task
            IJob job = context
            .Jobs
            .Create(String.Format("Hyperlapse {0}", input));

            if (String.IsNullOrEmpty(hyperConfig))
            {
            // config cannot be empty
            return false;
            }

            hyperConfig = File.ReadAllText(hyperConfig);

            ITask hyperlapseTask = job.Tasks.AddNew("Hyperlapse task",
            mp,
            hyperConfig,
            TaskOptions.None);
            hyperlapseTask.InputAssets.Add(asset);
            hyperlapseTask.OutputAssets.AddNew("Hyperlapse output",
            AssetCreationOptions.None);

            job.Submit();

            // Create progress printing and querying tasks
            Task progressPrintTask = new Task(() =>
            {

            IJob jobQuery = null;
            do
            {
                var progressContext = context;
                jobQuery = progressContext.Jobs
                .Where(j => j.Id == job.Id)
                .First();
                Console.WriteLine(string.Format("{0}\t{1}\t{2}",
                DateTime.Now,
                jobQuery.State,
                jobQuery.Tasks[0].Progress));
                Thread.Sleep(10000);
            }
            while (jobQuery.State != JobState.Finished &&
                                   jobQuery.State != JobState.Error &&
                                   jobQuery.State != JobState.Canceled);
                });
                
            progressPrintTask.Start();

            Task progressJobTask = job.GetExecutionProgressTask(
                                                 CancellationToken.None);
            progressJobTask.Wait();

            // If job state is Error, the event handling
            // method for job progress should log errors.  Here we check
            // for error state and exit if needed.
            if (job.State == JobState.Error)
            {
                ErrorDetail error = job.Tasks.First().ErrorDetails.First();
                Console.WriteLine(string.Format("Error: {0}. {1}",
                                                error.Code,
                                                error.Message));  
                return false;                  
            }

        DownloadAsset(job.OutputMediaAssets.First(), output);
        return true;
    }

    static void DownloadAsset(IAsset asset, string outputDirectory)
    {
        foreach (IAssetFile file in asset.AssetFiles)
        {
            file.Download(Path.Combine(outputDirectory, file.Name));
        }
    }


    static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
    {
        IAsset asset = context.Assets.Create(assetName, options);

        var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
        assetFile.Upload(filePath);

        return asset;
    }

    static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = context.MediaProcessors
        .Where(p => p.Name == mediaProcessorName)
        .ToList()
        .OrderBy(p => new Version(p.Version))
        .LastOrDefault();

        if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor",
                                                       mediaProcessorName));

        return processor;
    }
```

### <a id="file_types"></a>Tipos de archivo admitidos
* MP4
* MOV
* WMV

## <a name="media-services-learning-paths"></a>Rutas de aprendizaje de Media Services
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Envío de comentarios
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>Vínculos relacionados
[Azure Media Services Analytics Overview (Información general sobre Azure Media Services Analytics)](media-services-analytics-overview.md)

[Demostraciones de Azure Media Analytics](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

