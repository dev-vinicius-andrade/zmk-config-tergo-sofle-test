# Tergo Sofle Wireless Firmware (ZMK) - Tergo Teclados

This is the source code for the _firmware_ of the [Tergo Sofle keyboard](https://tecladoergonomico.com.br/) in its wireless version.

To download the latest _firmware_ release, visit the releases [by clicking here](https://github.com/TergoTeclados/zmk-config-tergo-sofle/releases).

If you arrived here by accident, visit the [official Tergo Sofle documentation](https://github.com/TergoTeclados/Tergo-Sofle-Documentation).

## Getting Started

Set up your environment for source customization by reading [our documentation](https://github.com/TergoTeclados/Tergo-Sofle-Documentation/blob/main/guias/especifico_versao_wireless/COMO_MODIFICAR_CODIGO_FONTE.md#tergo-sofle---manual-de-modifica%C3%A7%C3%A3o-do-firmware).

Then read the [section below](#overview) to learn more about the firmware.

Finally, to flash the firmware onto your keyboard, read [this guide](https://github.com/TergoTeclados/Tergo-Sofle-Documentation/blob/main/guias/especifico_versao_wireless/COMO_ATUALIZAR_FIRMWARE.md#manual-de-atualiza%C3%A7%C3%A3o-do-firmware---vers%C3%A3o-wireless).

## Overview

### ZMK Firmware

ZMK firmware does not expect you to modify its source code directly. Instead, you create a configuration for your keyboard — which is exactly what this repository is.

Your keyboard is customized through that configuration.

### How it works

#### General settings

The [config](./config/) folder contains the keyboard configuration and customizations.

You will primarily want to edit [config/sofle.keymap](./config/sofle.keymap).

#### Shields

Inside [boards/shields](./boards/shields/) you will find "shields".

Shields are additional hardware-level customizations. This repo includes:

- `dongle_display` — configures a screen for the dongle
- `sofle` — defines the two keyboard **halves**

### The dongle is the heart of your keyboard

> [!IMPORTANT]
>
> General keyboard settings are applied in `config/sofle.keymap`, not inside `boards/shields/sofle`.
>
> This is because that configuration is loaded onto the dongle itself.
>
> The dongle is responsible for interpreting all signals from the keyboard halves.
>
> The halves are merely "shields" that send keypress signals to the dongle.
>
> The dongle processes those signals.
>
> This is why most customizations you make in `sofle.keymap` only require reflashing the dongle — not the halves.

### Other

The `.github` folder contains the workflow configuration that automatically compiles the firmware on every push.

The [build.yaml](./build.yaml) file specifies what gets compiled and with which settings.

---

## Multiple Dongle Support

This repository enables experimental support for the keyboard halves to pair with **up to 3 different dongles**, switching between them automatically based on availability.

Ideal for:
- One dongle at **work**
- One dongle at **home**
- One dongle for **travel**

The setting is in [config/sofle.conf](./config/sofle.conf):

```conf
CONFIG_ZMK_SPLIT_PERIPHERAL_DONGLE_PROFILES=3
```

Increase that number (maximum 5) to support more dongles.

---

### Available firmware artifacts

| Artifact | Description |
|---|---|
| `sofle_dongle_display_international` | Dongle **with display**, international layout |
| `sofle_dongle_display_abnt2` | Dongle **with display**, ABNT2 layout |
| `sofle_dongle_no_display_international` | Dongle **without display**, international layout |
| `sofle_dongle_no_display_abnt2` | Dongle **without display**, ABNT2 layout |
| `sofle_left` | Left keyboard half |
| `sofle_right` | Right keyboard half |
| `reset-firmware` | Reset firmware (wipes bonds and settings) |

---

### Full pairing from scratch

> [!IMPORTANT]
> This process erases all existing bonds. Only do this when adding or redoing dongles.

#### Step 1 — Reset everything

1. Flash `reset-firmware` to **dongle 1**, wait for it to boot, then unplug it.
2. Flash `reset-firmware` to **dongle 2**, wait for it to boot, then unplug it.
3. Flash `reset-firmware` to **dongle 3**, wait for it to boot, then unplug it.
4. Flash `reset-firmware` to the **left half**, wait for it to boot.
5. Flash `reset-firmware` to the **right half**, wait for it to boot.

#### Step 2 — Flash the real firmware

With **all dongles unplugged** and **keyboard halves disconnected**:

1. Flash the correct firmware to **dongle 1** and unplug it.
2. Flash the correct firmware to **dongle 2** and unplug it.
3. Flash the correct firmware to **dongle 3** and unplug it.
4. Flash the firmware to the **left half**.
5. Flash the firmware to the **right half**.
6. Disconnect the halves (turn off or unplug).

#### Step 3 — Pair with dongle 1

1. Plug in **only dongle 1**.
2. Connect both halves (via USB or battery).
3. Wait — the halves will automatically pair with dongle 1.
4. Test the keys.
5. Disconnect the halves.

#### Step 4 — Pair with dongle 2

1. Unplug **dongle 1**, plug in **dongle 2**.
2. Connect the halves again.
3. Wait — the halves will automatically pair with dongle 2.
4. Test the keys.
5. Disconnect the halves.

#### Step 5 — Pair with dongle 3

1. Unplug **dongle 2**, plug in **dongle 3**.
2. Connect the halves again.
3. Wait — the halves will automatically pair with dongle 3.
4. Test the keys.

All three dongles are now paired.

---

### Day-to-day usage

- Keep **only one dongle plugged in at a time**. The halves detect which dongle is available and connect automatically.
- Never power on two dongles simultaneously near the halves — this causes bond conflicts.
- No re-pairing is needed when switching dongles.

> [!WARNING]
> If the halves stop connecting to a dongle after a firmware update, repeat the full pairing process from Step 1.
