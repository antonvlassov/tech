# Principais Comandos

## Editar Video


**Recortar** o trecho do video entre "ss" e "to", e gravar em um arquivo de saída sem coverter o codec: `ffmpeg -i origin.mp4 -ss 00:00:00 -to 00:48:18 -c copy part01.mp4`

**Concatenar** diversos arquivos sem alterar o codec:

* Criar uma lista de arquivos: `vim list.txt`
com formato:
```
file '/full/path/file.mp4'
file '/full/path/file.mp4'

```

`ffmpeg -f concat -safe 0 -i list.txt -c copy output.mp4`

## Converter / Formatar

Realiza coversão do codec de audio de ddp5.1 para ac3, sem alterar o codec de video:
`ffmpeg -i file_ddp51.mkv -c:v copy -c:a ac3 file_ac3.mkv`