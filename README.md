# OpenWrt for D-Link DIR-825-B1

Automatic OpenWrt builds for D-Link DIR-825-B1 router with support for modified flash memory.

## Firmware Variants

| Flash | Description |
|-------|-------------|
| 8MB   | Stock flash |
| 16MB  | W25Q128 mod |
| 32MB  | W25Q256 mod |
| 64MB  | W25Q512 mod |

## Optimization Levels

| Level  | Description              | Default |
|--------|--------------------------|---------|
| base   | Standard -Os             | Yes     |
| x      | Experimental flags       | Yes     |
| lto    | Link-Time Optimization   | No      |
| lto-gc | LTO + GC sections        | No      |

Default: 8 builds (4 flash x 2 opt). Enable lto/lto-gc via workflow UI.

## Download

Ready-to-use firmware available in [Releases](../../releases).

## Flash Modification

DIR-825-B1 uses mtd-concat: caldata is located in the middle of flash (0x660000-0x670000).
When replacing flash with a larger one, only the fwconcat1 partition expands.

### Partition Layout

```
+------------------+
| 0x000000 u-boot  |
| 0x040000 config  |
| 0x050000 fwconcat0 (firmware part 1)
| 0x660000 caldata | <- DOES NOT MOVE!
| 0x670000 fwconcat1 (firmware part 2)
| ...              | <- expands with larger flash
+------------------+
```

### fwconcat1 Sizes

| Flash | fwconcat1 size      |
|-------|---------------------|
| 8MB   | 0x190000  ( 1.6MB)  |
| 16MB  | 0x990000  ( 9.6MB)  |
| 32MB  | 0x1990000 (25.6MB)  |
| 64MB  | 0x3990000 (57.6MB)  |

## GitHub Actions

Firmware is built automatically:
- On new OpenWrt release (checked hourly)
- Manually via workflow_dispatch

## License

GPL-2.0 (same as OpenWrt)
