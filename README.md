# GPIOViewer

Biblioteca Arduino que serve uma aplicação web para visualizar ao vivo os GPIOs (digital, analógico, PWM, ADC), heap e PSRAM de placas ESP32.

## Requisitos

- ESP32 Arduino Core **3.x ou superior**
- [ESPAsyncWebServer](https://github.com/ESP32Async/ESPAsyncWebServer)
- [AsyncTCP](https://github.com/ESP32Async/AsyncTCP)

## Instalação

### Arduino IDE

Instale ESPAsyncWebServer e AsyncTCP pelo Library Manager e adicione esta biblioteca via `Sketch > Include Library > Add .ZIP Library...`.

### PlatformIO

O `espressif32` oficial usa o core 2.x; use o `pioarduino`:

```ini
[env:your_board]
platform = https://github.com/pioarduino/platform-espressif32/releases/download/stable/platform-espressif32.zip
board = <your-esp32-board>
framework = arduino

lib_deps =
    https://github.com/ESP32Async/ESPAsyncWebServer.git
    https://github.com/ESP32Async/AsyncTCP.git
    https://github.com/rafaelcruzmartins/gpio-viewer.git
```

## Uso

```c
#include <gpio_viewer.h> // Deve ser o primeiro include
GPIOViewer gpio_viewer;

void setup()
{
  Serial.begin(115200);

  // Remova se o seu código já conecta ao WiFi em modo WIFI_STA
  gpio_viewer.connectToWifi("SSID", "senha");
  // gpio_viewer.setPort(5555);                 // padrão 8080

  // Seu setup aqui

  // gpio_viewer.setSamplingInterval(25);       // padrão 100 ms
  // gpio_viewer.setSkipPeripheralPins(false);  // padrão true: ignora pinos de I2C/SPI/UART
  gpio_viewer.begin();                          // sempre no fim do setup
}

void loop() {}
```

- Acesse `http://gpioviewer.local:8080` (mDNS) ou o IP impresso no monitor serial.
- WiFi deve estar em `WIFI_STA` (`WIFI_AP` e `WIFI_AP_STA` não são suportados).
- Os assets da interface web são carregados de `baseURL` em `src/gpio_viewer.h`; o navegador precisa de acesso à internet.

## Problemas comuns

- **Detecção de funções de pino (ADC/Touch) travando a placa** (ex.: XIAO ESP32-S3 Sense): defina `NO_PIN_FUNCTIONS` antes do include.
  ```c
  #define NO_PIN_FUNCTIONS
  #include <gpio_viewer.h>
  ```
- **Não compila**: tente colocar `#include <gpio_viewer.h>` depois dos demais includes.
- **`ERROR: Too many messages queued`**: o navegador não está lendo rápido o suficiente; aumente o intervalo de amostragem ou melhore o WiFi.

## Exemplos

- `examples/gpioviewer`: exemplo mínimo para Arduino IDE.
- `examples/platformio`: projeto PlatformIO (renomeie `src/_secrets.h` para `src/secrets.h`).

## Licença

MIT — ver [LICENSE](LICENSE).
