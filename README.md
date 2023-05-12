# PlatformIO M5Stack

[Arduino IDE] Al igual que el entorno (https://www.arduino.cc/en/software) [IDE de Arduino] Al igual que el entorno (https://www.arduino.cc/en/software), el entorno [PlatformIO IDE]  [PlatformIO IDE](https://platformio.org/platformio-ide) se puede compilar y ejecutar inmediatamente escribiendo el contenido de 'setup()' y 'loop()'.

## Dispositivos M5Stack compatibles

| Modelo                    | Entorno                                                         | Notas                                                                                                     |
| :------------------------ | :------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| M5Stack BASIC             | env:m5stack-basic                                              |                                                                                                          |
| M5Stack Fire              | env:m5stack-fire                                               |                                                                                                          |
| M5Stack M5GO              | env:m5stack-m5go                                               |                                                                                                          |
| M5Stack CORE2             | env:m5stack-core2                                              |                                                                                                          |
| M5Stack CORES3             | env:m5stack-cores3-m5unified                                   | [M5Unified](https://github.com/m5stack/M5Unified) を使用                                                                                                         |
| M5StickC                  | env:m5stack-c                                                  |                                                                                                          |
| M5StickC Plus             | env:m5stack-c-plus                                             |                                                                                                          |
| M5ATOM Lite/Matrix/Echo/U | env:m5stack-atom <br> env:m5stack-atom-m5unified               | Utilizar la biblioteca oficial使用<br>[M5Unified](https://github.com/m5stack/M5Unified) を使用                         |
| M5ATOMS3                  | env:m5stack-atoms3 <br> env:m5stack-atoms3-m5unified           | Utilizar la biblioteca oficial<br>[M5Unified](https://github.com/m5stack/M5Unified) を使用。USB CDC On Boot が有効 |
| M5ATOMS3 Lite             | env:m5stack-atoms3-lite <br> env:m5stack-atoms3-lite-m5unified | Utilizar la biblioteca oficial<br>[M5Unified](https://github.com/m5stack/M5Unified) を使用。USB CDC On Boot が有効 |
| M5Stack CoreInk           | env:m5stack-core-ink                                           |                                                                                                          |
| M5Stack Paper             | env:m5stack-paper                                              |                                                                                                          |

## Preaparación

### Configuración del proyecto

El formato se establece en `.vscode/settings.json` `"C_Cpp.clang_format_style": "file"` `.clang-format` puedes cambiarlo a gusto

### Preferencias

#### Configuración del puerto de conexión

`platformio.ini`の`[platformio]`セクションにある`upload_port`と`monitor_port`のコメントを外し，`upload_port`に設定するポートを実機が接続しているポートに変更します。

```platformio.ini
upload_port = COM16
monitor_port = ${env.upload_port}
```

※PlatformIO IDE [v3.0.0](https://github.com/platformio/platformio-vscode-ide/releases/tag/v3.0.0)より，ステータスバーからポートの切り替えができるようになりました。

#### 環境名の設定

「Switch PlatformIO Project Environment」（VSCode のステータスバーにある）で機種に合った環境名を設定します。

`platformio.ini`の`[platformio]`セクションで`default_envs`を明示的に指定することでも環境を設定できます（既に書いてあるので，いずれかのコメントを外す）。以下の例では`m5stack-basic`を指定しています。

```platformio.ini
[platformio]
default_envs = m5stack-basic
; default-envs = m5stack-fire
; default-envs = m5stack-m5go
; default_envs = m5stack-core2
; default_envs = m5stack-cores3-m5unified
; default_envs = m5stick-c
; default_envs = m5stick-c-plus
; default_envs = m5stack-atom
; default_envs = m5stack-atom-m5unified
; default_envs = m5stack-atoms3
; default_envs = m5stack-atoms3-m5unified
; default_envs = m5stack-atoms3-lite
; default_envs = m5stack-atoms3-lite-m5unified
; default_envs = m5stack-coreink
; default_envs = m5stack-paper
```

### 外部ライブラリの追加

外部ライブラリを使用する場合は，`[env]`セクションにある`lib_deps`に追加します。

```
lib_deps =
	fastled/FastLED
```

### コードの記述

`main.cpp`の`setup()`，`loop()`にコードを書きます。必要なヘッダファイルは`main.hpp`で環境名に合わせて実機に合ったヘッダファイルをインクルードするようにしています。

各機種で`M5.begin()`の引数がまちまちでわかりにくいので，`M5_BEGIN`というマクロを定義しています。定義内容に関しては`main.hpp`を参照してください。

注意：M5Unified で`SD.h`や`SPIFFS.h`を使用する場合は，`#include "main.hpp"`より前に入れてください。

```c++
// clang-format off
#include <SPIFFS.h>
#include "main.hpp"
// clang-format on
```

### 実機へのアップロード

PlatformIO: Upload（VSCode のステータスバーにある → ボタン）を実行します。
