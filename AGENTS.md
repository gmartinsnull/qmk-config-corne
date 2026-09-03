# AGENTS.md - QMK Corne Keyboard Configuration

This is a QMK (Quantum Mechanical Keyboard) firmware keymap for the Corne V3 split keyboard.

## Project Overview

- **Keyboard**: Corne V3 (crkbd)
- **Language**: C (QMK firmware)
- **License**: GPL-2.0 (QMK standard)

## File Structure

```
qmk-config-corne/
├── AGENTS.md          # This file
├── config.h           # C preprocessor configuration
├── rules.mk           # Build rules and feature flags
├── keymap.c           # Main keymap definition
├── keymap_old.c       # Backup of previous keymap
└── README.md          # User documentation
```

## Build Commands

### Building the Firmware

This keymap is designed to be used within the QMK firmware tree. The workflow is:

```bash
# 1. Setup QMK environment (one-time)
qmk setup

# 2. Create a new keymap from this configuration
qmk new-keymap -kb crkbd -km <keymap_name>

# 3. Copy files (excluding keymap_old.c) to the new keymap folder
cp config.h rules.mk keymap.c <qmk_firmware>/keyboards/crkbd/keymaps/<keymap_name>/

# 4. Compile the firmware
qmk compile -kb crkbd -km <keymap_name>

# 5. Flash to keyboard (requires bootloader mode)
qmk flash -kb crkbd -km <keymap_name>
```

### Single File Compilation

To compile just the keymap without building the entire keyboard:

```bash
qmk compile -kb crkbd -km <keymap_name>
```

### Flashing

```bash
# Master side (left half)
qmk flash -kb crkbd -km <keymap_name> -bl avrdude-split-left

# Slave side (right half) 
qmk flash -kb crkbd -km <keymap_name> -bl avrdude-split-right
```

### Testing

QMK does not have traditional unit tests for keymaps. Testing is done by:
1. Compiling the firmware
2. Flashing to the keyboard
3. Physical testing on the keyboard

## Code Style Guidelines

### General Rules

- Follow QMK coding conventions as documented in the [QMK docs](https://docs.qmk.fm/)
- This is embedded C code - keep it simple and readable
- Avoid dynamic memory allocation
- Use QMK provided macros and functions

### Formatting

- Use 4-space indentation (no tabs)
- Opening braces on same line as control statements
- Maximum line length: 100 characters (soft limit)
- Use consistent spacing around operators

### Keymap Structure

```c
// Keymap array format: LAYOUT_split_3x6_3
// Layers defined as [LAYER_NAME] = LAYOUT_*(...)
const uint16_t PROGMEM keymaps[][MATRIX_ROWS][MATRIX_COLS] = {
    [0] = LAYOUT_split_3x6_3(
        // Row 1
        KC_ESC,  KC_Q,    KC_W,    KC_E,    KC_R,    KC_T,
        // ...
    ),
    [1] = LAYOUT_split_3x6_3(...),
    // ...
};
```

### Naming Conventions

- **Layers**: `_LAYER_NAME` (e.g., `_DEFAULT`, `_QWERTY`, `_LOWER`, `_RAISE`, `_ADJUST`)
- **Tap Dance**: `TD_*` prefix (e.g., `TD_ESC_CAPS`)
- **Macros**: Uppercase with underscores (e.g., `TG(layer)`)
- **Keycodes**: Use QMK defined keycodes (e.g., `KC_ESC`, `KC_LSFT`)
- **Custom functions**: `function_name` (snake_case)

### Imports

```c
#include <QMK_KEYBOARD_H>  // Required - QMK core
// Add feature-specific includes as needed via config.h
```

### Feature Flags (rules.mk)

Enable features in `rules.mk`:
```makefile
RGBLIGHT_ENABLE = yes/no
RGB_MATRIX_ENABLE = yes/no
TAP_DANCE_ENABLE = yes/no
OLED_ENABLE = yes/no
WPM_ENABLE = yes/no
ENCODER_ENABLE = yes/no
```

### Configuration (config.h)

Define configuration macros:
```c
#define TAPPING_TERM 175
#define TAPPING_TERM_PER_KEY
#define RGB_MATRIX_MAXIMUM_BRIGHTNESS 120
```

### Layer Management

- Use `MO(layer)` for momentary layer switch
- Use `TG(layer)` for toggle
- Use `TT(layer)` for tap-toggle
- Use `DF(layer)` for default layer
- Use `MO(layer)` + `______` for layer buttons

### Mod-Tap and Layer-Tap

- `CTL_T(key)` - Hold for Ctrl, tap for key
- `LT(layer, key)` - Hold for layer, tap for key
- `MT(mod, key)` - Hold for modifier, tap for key

### Tap Dance

Define in `enum`:
```c
enum {
    TD_ESC_CAPS,
    TD_AT_CIRC
};
```

Implement:
```c
tap_dance_action_t tap_dance_actions[] = {
    [TD_ESC_CAPS] = ACTION_TAP_DANCE_DOUBLE(KC_ESC, KC_CAPS),
};
```

### OLED Display

- Use `oled_init_user()` for initialization
- Use `oled_task_user()` for render loop
- Use `oled_write()` / `oled_write_P()` for text
- Use `oled_write_raw_P()` for bitmap images

### RGB Matrix

- Use `RM_*` keycodes for runtime adjustment (QMK 0.24+)
- Use `RGB_MATRIX_*` effects defined in config.h

### Error Handling

- No exceptions in embedded C
- Use `XXXXXXX` (6 X's) for disabled keys
- Use `______` (6 underscores) for transparent/empty keys
- Check `host_keyboard_led_state()` for LED status

### Performance Considerations

- Keep OLED rendering minimal in `oled_task_user()`
- Use `PGM_P` / `PROGMEM` for data in flash
- Avoid complex calculations in hot paths
- Use `is_keyboard_master()` for split keyboard detection

## Testing Changes

Since there are no automated tests:

1. Edit keymap.c
2. Copy to QMK keymaps folder
3. Compile: `qmk compile -kb crkbd -km <keymap>`
4. Flash to device
5. Test physically

## Documentation

- Update README.md when adding new features
- Comment complex key arrangements
- Keep keymap_old.c as backup before major changes

## References

- [QMK Docs](https://docs.qmk.fm/)
- [QMK Keycodes](https://docs.qmk.fm/#/keycodes)
- [Corne Keyboard](https://github.com/foostan/crkbd)
