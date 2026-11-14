# cw32f030-pac

武汉芯源 **CW32F030**（Cortex-M0+）的 Rust PAC（寄存器访问层），由 [cw32-pac-gen](https://github.com/SunWithCloude/cw32-pac-gen) 从原厂 SDK 头文件生成：`no_std`、零依赖、`MIT OR Apache-2.0`，家族最大配置 Flash 64K / SRAM 8K（`memory.x`）。

属于 [cw32-pacs](https://github.com/SunWithCloude/cw32-pacs) 统一仓（10 家族矩阵、发布状态见顶层 README）。

## 用法

```toml
[dependencies]
cw32f030-pac = { git = "https://github.com/SunWithCloude/cw32-pacs" }
```

```rust
use cw32f030_pac::Peripherals;
let p = unsafe { Peripherals::steal() };
```

## 构建

```
cargo test                       # host:布局回归测试
cargo build --target thumbv6m-none-eabi --release --features rt --example blink
```

`rt` feature 拉入 `cortex-m-rt 0.7`（device 中断表 + 链接脚本）；blink 示例默认 PC13 点灯，按板改 `LED_PIN`。
