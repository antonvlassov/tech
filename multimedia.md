# Softwares

## Install
```
sudo apt install ffmpeg

sudo apt install openshot

```


# Studio

## Hydrogen Drum Machine
https://repology.org/project/hydrogen-drum-machine/versions

sudo apt install hydrogen 

whereis pulseaudio
sudo apt install pavucontrol

sudo apt install qjackctl

cat /etc/security/limits.d/audio.conf



# Screencast / Podcast

Kazam - Software Manager => System Package


# FFMPEG - Principais Comandos

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

**Converter Audio**

Realiza coversão do codec de audio de ddp5.1 para ac3, sem alterar o codec de video:
`ffmpeg -i file_ddp51.mkv -c:v copy -c:a ac3 file_ac3.mkv`

**Padronizar o Video Codec**
Em alguns casos, após realizar _concat_, audio e video são dessincronizados no arquivo resultante. Para contornar esse problema, precisa antes realizar uma padronização de codec, e somente depois realizar _concat_:

`ffmpeg -i hlf_1_intro.mp4 -r 25 -af apad -vf scale=1280:720 -crf 15.0 -vcodec libx264 -acodec aac -ar 48000 -b:a 192k -coder 1 -rc_lookahead 60 -threads 0 -shortest -avoid_negative_ts make_zero -fflags +genpts hlf_1_standard.mp4`

## Audio

Converte .wav para .mp3:
`ffmpeg -i input-file.wav -vn -ar 44100 -ac 2 -b:a 320k output-file.mp3`

Altera sampling rate do wav (no exemplo, para 16bits):
`ffmpeg -i input-file.wav -sample_fmt s16 output-file.wav`