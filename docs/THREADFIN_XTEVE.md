# Guia rápido: Threadfin e xTeVe

Este guia explica duas opções para **importar, organizar e entregar** a playlist a aplicativos compatíveis. Threadfin e xTeVe são alternativas; normalmente basta escolher um deles.

> **Importante:** esses programas funcionam como gerenciadores/proxies da playlist. Podem filtrar canais, organizar categorias, integrar guia XMLTV e retransmitir um fluxo acessível. **Não consertam um servidor de origem fora do ar, não criam sinal de TV e não transformam um link inválido em transmissão funcional.** A disponibilidade continua dependendo da emissora/servidor que fornece cada URL.

## Playlist deste repositório

Arquivo: [Canais.m3u8](../Canais.m3u8)

URL HTTP atual do arquivo na branch padrão do repositório:

`https://raw.githubusercontent.com/woodyytalo/playlist.canais.turcos/man.github/workflows/atualizar-logos.yml/Canais.m3u8`

Use a URL HTTP acima como fonte M3U. Se a branch ou o caminho do arquivo mudar, atualize a URL no Threadfin/xTeVe.

## Opção 1 — Threadfin

Threadfin é um proxy M3U para clientes como Plex, Jellyfin e Emby, além de aplicativos que aceitam M3U. O método abaixo usa Docker Compose.

### Instalação com Docker Compose

1. Instale Docker e o plugin Docker Compose no computador/NAS que ficará ligado.
2. Crie uma pasta para o Threadfin e, dentro dela, crie um arquivo chamado `compose.yaml`.
3. Cole o exemplo abaixo. Ajuste `PUID` e `PGID` para o usuário do seu sistema, se necessário.

```yaml
services:
  threadfin:
    image: fyb3roptik/threadfin
    container_name: threadfin
    ports:
      - "34400:34400"
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=America/Sao_Paulo
    volumes:
      - ./data/conf:/home/threadfin/conf
      - ./data/temp:/tmp/threadfin
    restart: unless-stopped
```

4. Na pasta do arquivo, execute:

```bash
docker compose up -d
```

5. Abra no navegador `http://IP_DO_SERVIDOR:34400/` (se necessário, tente `http://IP_DO_SERVIDOR:34400/web/`).

### Configuração inicial

1. No assistente, escolha a capacidade de tuners/streams adequada ao seu uso.
2. Escolha **PMS** se Plex/Jellyfin/Emby cuidará do guia; escolha **XEPG** se quiser importar XMLTV e fazer o mapeamento no Threadfin.
3. Em **M3U Playlist**, informe a URL HTTP da seção “Playlist deste repositório”.
4. Conclua o assistente e aguarde a importação.
5. Em **Sources → Playlist**, confira disponibilidade e quantidade de canais.
6. Em **Lineup → Filter**, selecione os grupos/canais desejados. No modo XEPG, configure também XMLTV e **Mapping**, salve o mapeamento e confira os endereços em **Delivery → Connections**.
7. Adicione ao aplicativo de destino o endereço M3U/DVR fornecido pelo Threadfin e teste um canal.

Guia de referência: [Threadfin — repositório oficial](https://github.com/Threadfin/Threadfin) · [Guia de usuário da interface](https://github.com/volschin/Threadfin/blob/main/docs/user-guide.md).

## Opção 2 — xTeVe

xTeVe é outro proxy M3U, originalmente voltado a Plex DVR e Emby Live TV. O procedimento abaixo usa o executável oficial; os passos e nomes das telas podem variar conforme a versão.

### Instalação no Windows

1. Abra o [repositório oficial xTeVe](https://github.com/xteve-project/xTeVe) e siga a seção **Downloads** para obter o executável compatível com seu Windows.
2. Extraia o arquivo ZIP para uma pasta permanente.
3. Execute `xteve.exe`.
4. Confira no console o endereço da interface web. A porta padrão é normalmente `34400`; se já estiver ocupada, escolha outra porta conforme a documentação.

### Instalação no Linux

1. Baixe a versão Linux compatível com a arquitetura do servidor a partir da seção **Downloads** do [repositório oficial](https://github.com/xteve-project/xTeVe).
2. Extraia o arquivo e entre na pasta.
3. Dê permissão de execução e inicie:

```bash
chmod +x xteve
./xteve
```

4. Use no navegador o endereço de interface mostrado no console (normalmente porta `34400` e caminho `/web/`).

### Configuração inicial

1. No assistente, escolha a quantidade de tuners que o servidor e a origem conseguem suportar.
2. Escolha **PMS** para deixar o guia com Plex/Emby ou **XEPG** para usar XMLTV e o mapeamento do xTeVe.
3. Em **M3U Playlist**, informe a URL HTTP da seção “Playlist deste repositório” ou o caminho local do arquivo, se ele estiver acessível ao processo xTeVe.
4. Se escolher XEPG, informe também uma fonte XMLTV.
5. Depois da importação, use **Filter** para selecionar canais e **Mapping** para relacionar canais ao guia.
6. Copie o endereço de saída gerado pelo xTeVe e configure-o no Plex/Emby ou em um aplicativo compatível. Teste a reprodução no cliente.

Documentação de referência: [xTeVe — repositório oficial](https://github.com/xteve-project/xTeVe) · [Configuração e telas do xTeVe](https://github.com/xteve-project/xTeVe-Documentation/blob/master/en/configuration.md).

## Cuidados e solução de problemas

- Não edite a playlist original só para instalar Threadfin ou xTeVe. Importe-a como fonte e mantenha a M3U deste repositório separada.
- Se um canal falhar também na reprodução direta, verifique primeiro a emissora e o servidor de origem. O proxy não corrige a origem.
- Se a fonte não carregar, confira se a URL está acessível **a partir do computador/contêiner** que executa o programa, e não apenas do celular.
- Use uma quantidade de tuners compatível com os limites da origem; muitos acessos simultâneos podem causar bloqueio ou falhas.
- Faça backup das configurações antes de atualizar ou alterar filtros/mapeamentos.
- Não exponha a interface administrativa diretamente à internet. Prefira a rede local e ative autenticação quando disponível.
