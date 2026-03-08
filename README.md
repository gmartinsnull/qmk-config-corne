# qmk-config-corne

Corne V3

## 📦 About This Repository
- **Source**: Based on [qmk_firmware](https://github.com/qmk/qmk_firmware)
- **Purpose**: Custom keymap configurations for corne v3 keyboard
- **License**: MIT (same as qmk_firmware)

## 🚀 How to Use
1. **Install qmk_firmware locally**:
   ```bash
   git clone https://github.com/qmk/qmk_firmware
   cd qmk_firmware
   ```
2. **Setup qmk environment**:
   ```bash
   qmk setup
   ```
3. **Create new keymap**:
   ```bash
   qmk new-keymap -kb crkbd -km <my_keymap>
   ```
4. **Copy files (exclude keymap_old.c)**:
   - Copy all files except `keymap_old.c` (backup file)
5. **Compile firmware**:
   ```bash
   qmk compile -kb crkbd -km <my_keymap>
   ```
6. **Flash firmware**:
   - Use [QMK Toolbox](https://github.com/qmk/qmk_toolbox) to flash the compiled firmware to your keyboard

## 📌 Notes
- Always keep the original `keymap_old.c` file as a backup
- Update your keymap configuration regularly to sync with upstream changes
- For advanced customization, refer to the [qmk documentation](https://docs.qmk.fm/)
