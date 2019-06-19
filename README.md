Modified version of the
[gcc-xtensa-esp32-elf AUR package](https://aur.archlinux.org/packages/gcc-xtensa-esp32-elf-git/)
which fixes a couple of build issues.

So far the only issue I found is that the
[latest stable version of esp-idf](https://github.com/espressif/esp-idf/tree/release/v3.2)
warns about the resulting toolchain not being supported as the version string
differs.

You can either ignore the warning or make it go away by modifying the expected
version string found in the file `tools/toolchain_versions.mk` in your copy of
esp-idf: Replace all occurrences of `1.22.0-80-g6c4433a` with
`1.22.0-81-g2411e6ac`.
