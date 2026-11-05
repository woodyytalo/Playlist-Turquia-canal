# Threadfin — integração com a playlist

Este guia configura o Threadfin para **consumir** a playlist publicada neste repositório. O Threadfin é um serviço externo: o GitHub armazena a lista e os arquivos de configuração, mas não mantém o Threadfin executando 24 horas por dia.

## Playlist de entrada

Use esta URL no campo de playlist M3U do Threadfin:

```
https://raw.githubusercontent.com/woodyytalo/playlist.canais.turcos/github/workflows/atualizar-logos.yml/Canais.m3u8
```

> A URL acima corresponde ao caminho e à branch atualmente retornados pelo GitHub para este repositório. Se o arquivo for movido para outra branch ou caminho, atualize a URL no Threadfin.

## Configuração

1. Instale e inicie o Threadfin em um computador ou servidor compatível, seguindo as instruções oficiais: https://github.com/Threadfin/Threadfin
2. Abra a interface web do Threadfin no endereço e na porta informados pela instalação.
3. Na configuração de playlist, escolha M3U e informe a URL de entrada acima.
4. Salve e atualize/import a playlist.
5. Confira nomes, categorias, logos e canais antes de publicar ou usar a saída do Threadfin.
6. Configure o XMLTV/EPG separadamente, se desejar guia de programação.

## Cuidados com a playlist original

- O Threadfin deve ler a URL publicada; ele não deve sobrescrever `Canais.m3u8`.
- Não habilite sincronização de volta nem automações que alterem o arquivo original.
- Faça ajustes de mapeamento, filtros e ordem dentro do Threadfin, mantendo a fonte original intacta.
- Se um canal falhar, isso não significa que o Threadfin consiga reparar a transmissão de origem.

## Limites do GitHub Actions

Um workflow do GitHub Actions pode validar arquivos ou testar a URL da playlist, mas não é um servidor permanente para hospedar o Threadfin. Para manter o Threadfin disponível continuamente, use um host compatível que permaneça ligado.

## Projeto oficial

https://github.com/Threadfin/Threadfin
