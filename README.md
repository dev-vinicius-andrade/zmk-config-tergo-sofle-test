# Firmware Tergo Sofle Wireless (ZMK) - Tergo Teclados

Esta é a pasta do código-fonte do _firmware_ do [teclado Tergo Sofle](https://tecladoergonomico.com.br/) na versão sem fio (wireless).

Caso queira baixar a versão mais recente do _firmware_, visite os releases [clicando aqui](https://github.com/TergoTeclados/zmk-config-tergo-sofle/releases).

Caso tenha chegado aqui por acidente, visite a [documentação oficial do Tergo Sofle](https://github.com/TergoTeclados/Tergo-Sofle-Documentation).

## Por onde começar

Configure seu ambiente para customização do código-fonte vendo [essa nossa documentação](https://github.com/TergoTeclados/Tergo-Sofle-Documentation/blob/main/guias/especifico_versao_wireless/COMO_MODIFICAR_CODIGO_FONTE.md#tergo-sofle---manual-de-modifica%C3%A7%C3%A3o-do-firmware).

Então, leia o [tópico abaixo](#visão-geral) para entender um pouco mais sobre o firmware.

Por fim, para gravar o firmware no seu teclado, leia [esse nosso guia](https://github.com/TergoTeclados/Tergo-Sofle-Documentation/blob/main/guias/especifico_versao_wireless/COMO_ATUALIZAR_FIRMWARE.md#manual-de-atualiza%C3%A7%C3%A3o-do-firmware---vers%C3%A3o-wireless).

## Visão Geral

### Firmware ZMK

O firmware ZMK não espera que você modifique o código-fonte dele em si, mas sim crie uma configuração para o seu teclado, que é o caso deste repositório.

A partir dessa configuração você customiza seu teclado.

### Como funciona

#### Configurações gerais

A pasta [config](./config/) possui a configuração e customizações do teclado.

Você irá querer customizá-lo pelo arquivo [config/sofle.keymap](./config/sofle.keymap).

#### Shields

Na pasta [boards/shields](./boards/shields/) você encontra "shields".

"Shields" são customizações adicionais que originam do hardware.

Nela você encontrará as shields:

- `dongle_display`, que configura uma tela para o receptor
- `sofle`, que cria as **metades** do teclado.

### O coração do seu teclado é o receptor (dongle)

Para você entender a essência dos arquivos presentes nesse repositório:

> [!IMPORTANT]
>
> As configurações gerais do teclado são aplicadas no arquivo `config/sofle.keymap`, como falado antes, e não na pasta `boards/shields/sofle`.
>
> Isso pois essa configuração é colocada no dongle em si.
>
> Quem interpreta todos sinais das partes do teclado é o receptor.
>
> As partes do teclado são meramente "shields", que enviam ao receptor os sinais de digitação.
>
> É o receptor quem processa esses sinais.
>
> É por isso que a maioria das customizações que você realizar no `sofle.keymap` que adicionem funcionalidades não necessitam da regravação do firmware nas partes do teclado, e sim apenas no receptor.

### Outros

A pasta `.github` contém configurações que fazem com que uma "action" para compilar o firmware seja executada automaticamente ao dar push no repositório.

O arquivo [build.yaml](./build.yaml) especifica o que deve ser compilado e com quais configurações.

## Suporte a múltiplos dongles

Este repositório habilita suporte experimental para as metades do teclado parearem com **até 3 dongles diferentes**, alternando automaticamente entre eles por disponibilidade.

Ideal para quem usa:
- Um dongle no **trabalho**
- Um dongle em **casa**
- Um dongle para **viagens**

A configuração está em [config/sofle.conf](./config/sofle.conf):

```conf
CONFIG_ZMK_SPLIT_PERIPHERAL_DONGLE_PROFILES=3
```

Aumente esse número (máximo 5) para suportar mais dongles.

---

### Firmwares disponíveis

| Artifact | Descrição |
|---|---|
| `sofle_dongle_display_international` | Dongle **com display**, layout internacional |
| `sofle_dongle_display_abnt2` | Dongle **com display**, layout ABNT2 |
| `sofle_dongle_no_display_international` | Dongle **sem display**, layout internacional |
| `sofle_dongle_no_display_abnt2` | Dongle **sem display**, layout ABNT2 |
| `sofle_left` | Metade esquerda do teclado |
| `sofle_right` | Metade direita do teclado |
| `reset-firmware` | Firmware de reset (apaga bonds e configurações) |

---

### Como parear do zero (todos os dongles)

> [!IMPORTANT]
> Este processo apaga todos os bonds existentes. Realize apenas quando precisar adicionar/refazer dongles.

#### Passo 1 — Zere tudo

1. Grave o `reset-firmware` no **dongle 1**, aguarde ele inicializar e desligue.
2. Grave o `reset-firmware` no **dongle 2**, aguarde ele inicializar e desligue.
3. Grave o `reset-firmware` no **dongle 3**, aguarde ele inicializar e desligue.
4. Grave o `reset-firmware` na **metade esquerda**, aguarde ela inicializar.
5. Grave o `reset-firmware` na **metade direita**, aguarde ela inicializar.

#### Passo 2 — Grave o firmware definitivo

Com **todos os dongles desligados** e **teclado desconectado**:

1. Grave o firmware correto no **dongle 1** e desligue.
2. Grave o firmware correto no **dongle 2** e desligue.
3. Grave o firmware correto no **dongle 3** e desligue.
4. Grave o firmware na **metade esquerda**.
5. Grave o firmware na **metade direita**.
6. Desconecte as metades (desligue ou retire o cabo).

#### Passo 3 — Pareamento com o dongle 1

1. Ligue **apenas o dongle 1**.
2. Conecte as duas metades ao computador (ou ligue-as por bateria).
3. Aguarde a conexão — as metades irão parear automaticamente com o dongle 1.
4. Teste as teclas.
5. Desligue as metades.

#### Passo 4 — Pareamento com o dongle 2

1. Desligue o **dongle 1**, ligue o **dongle 2**.
2. Ligue as metades novamente.
3. Aguarde a conexão — as metades irão parear automaticamente com o dongle 2.
4. Teste as teclas.
5. Desligue as metades.

#### Passo 5 — Pareamento com o dongle 3

1. Desligue o **dongle 2**, ligue o **dongle 3**.
2. Ligue as metades novamente.
3. Aguarde a conexão — as metades irão parear automaticamente com o dongle 3.
4. Teste as teclas.

Pronto. Os três dongles estão pareados.

---

### Uso no dia a dia

- Ligue **apenas um dongle por vez**. As metades detectam qual dongle está disponível e conectam automaticamente.
- Nunca ligue dois dongles simultaneamente próximos às metades — isso causa conflito de bond.
- Não é necessário refazer o pareamento ao trocar de dongle.

> [!WARNING]
> Se as metades pararem de conectar em algum dongle após uma atualização de firmware, repita o processo completo de pareamento a partir do Passo 1.
