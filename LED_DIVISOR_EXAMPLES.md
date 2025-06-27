# LED_DIVISOR Examples

This file contains examples of how to configure the LED_DIVISOR parameter for different scenarios.

## Basic Configuration

### Example 1: LED_DIVISOR=1 (Default - No Duplication)
```ini
[env:WS281x_RGB]
build_flags = -DNEOPIXEL_RGB -DDATA_PIN=2 -DLED_DIVISOR=1 ${env.build_flags}
custom_prog_version = esp32_WS281x_RGB
board = esp32dev
platform = ${esp32.platform}
lib_deps = ${esp32.lib_deps}
test_ignore = ${esp32.test_ignore}
```
**Result**: Each logical LED controls 1 physical LED (normal operation)

### Example 2: LED_DIVISOR=2 (Duplicate to 2 LEDs)
```ini
[env:WS281x_RGB_DUPLICATE_2]
build_flags = -DNEOPIXEL_RGB -DDATA_PIN=2 -DLED_DIVISOR=2 ${env.build_flags}
custom_prog_version = esp32_WS281x_RGB_DUPLICATE_2
board = esp32dev
platform = ${esp32.platform}
lib_deps = ${esp32.lib_deps}
test_ignore = ${esp32.test_ignore}
```
**Result**: Each logical LED controls 2 physical LEDs (50% bandwidth reduction)

### Example 3: LED_DIVISOR=3 (Duplicate to 3 LEDs)
```ini
[env:WS281x_RGB_DUPLICATE_3]
build_flags = -DNEOPIXEL_RGB -DDATA_PIN=2 -DLED_DIVISOR=3 ${env.build_flags}
custom_prog_version = esp32_WS281x_RGB_DUPLICATE_3
board = esp32dev
platform = ${esp32.platform}
lib_deps = ${esp32.lib_deps}
test_ignore = ${esp32.test_ignore}
```
**Result**: Each logical LED controls 3 physical LEDs (66% bandwidth reduction)

## RGBW Examples

### Example 4: RGBW with LED_DIVISOR=2
```ini
[env:SK6812_RGBW_DUPLICATE_2]
build_flags = -DNEOPIXEL_RGBW -DDATA_PIN=2 -DLED_DIVISOR=2 ${env.build_flags}
custom_prog_version = esp32_SK6812_RGBW_DUPLICATE_2
board = esp32dev
platform = ${esp32.platform}
lib_deps = ${esp32.lib_deps}
test_ignore = ${esp32.test_ignore}
```

### Example 5: RGBW Cold White with LED_DIVISOR=2
```ini
[env:SK6812_RGBW_COLD_DUPLICATE_2]
build_flags = -DNEOPIXEL_RGBW -DCOLD_WHITE -DDATA_PIN=2 -DLED_DIVISOR=2 ${env.build_flags}
custom_prog_version = esp32_SK6812_RGBW_COLD_DUPLICATE_2
board = esp32dev
platform = ${esp32.platform}
lib_deps = ${esp32.lib_deps}
test_ignore = ${esp32.test_ignore}
```

## SPI LED Examples

### Example 6: APA102 with LED_DIVISOR=2
```ini
[env:SPI_APA102_DUPLICATE_2]
build_flags = -DSPILED_APA102 -DDATA_PIN=2 -DCLOCK_PIN=4 -DLED_DIVISOR=2 ${env.build_flags}
custom_prog_version = esp32_SPI_APA102_DUPLICATE_2
board = esp32dev
platform = ${esp32.platform}
lib_deps = ${esp32.lib_deps}
test_ignore = ${esp32.test_ignore}
```

## Usage Scenarios

### Scenario 1: High LED Count Setup
- **Physical LEDs**: 600
- **LED_DIVISOR**: 2
- **HyperHDR Configuration**: 300 LEDs
- **Bandwidth Reduction**: 50%

### Scenario 2: Very High LED Count Setup
- **Physical LEDs**: 900
- **LED_DIVISOR**: 3
- **HyperHDR Configuration**: 300 LEDs
- **Bandwidth Reduction**: 66%

### Scenario 3: Multi-Segment Setup
- **Physical LEDs**: 512 + 400 = 912 total
- **LED_DIVISOR**: 2
- **HyperHDR Configuration**: 456 LEDs
- **Bandwidth Reduction**: 50%

## How to Use

1. **Choose your LED_DIVISOR value** based on your needs
2. **Add the environment** to your `platformio.ini` file
3. **Compile and upload** using the new environment
4. **Configure HyperHDR** with the logical LED count (physical count ÷ LED_DIVISOR)

## Compilation Commands

```bash
# Compile with LED_DIVISOR=2
platformio run -e WS281x_RGB_DUPLICATE_2

# Compile with LED_DIVISOR=3
platformio run -e WS281x_RGB_DUPLICATE_3

# Upload with LED_DIVISOR=2
platformio run -e WS281x_RGB_DUPLICATE_2 --target upload
```

## Verification

After uploading, check the serial output. You should see:
```
LED_DIVISOR = 2
LED duplication enabled: each logical LED will control 2 physical LEDs
```

## Notes

- The LED_DIVISOR must be a positive integer
- Higher values reduce bandwidth but also reduce individual LED control
- Physical LED count should be divisible by LED_DIVISOR for optimal results
- Works with all LED types (RGB, RGBW, SPI)
- Compatible with multi-segment setups 