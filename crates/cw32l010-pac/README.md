# cw32l010-pac

武汉芯源 **CW32L010**（Cortex-M0+）的 Rust PAC（寄存器访问层），由 [cw32-pac-gen](https://github.com/SunWithCloude/cw32-pac-gen) 从原厂 SDK 头文件生成：`no_std`、零依赖、`MIT OR Apache-2.0`，家族最大配置 Flash 64K / SRAM 4K（`memory.x`）。

属于 [cw32-pacs](https://github.com/SunWithCloude/cw32-pacs) 统一仓（10 家族矩阵、发布状态见顶层 README）。

## 用法

```toml
[dependencies]
cw32l010-pac = { git = "https://github.com/SunWithCloude/cw32-pacs" }
```

```rust
use cw32l010_pac::Peripherals;
let p = unsafe { Peripherals::steal() };
```

## 构建

```
cargo test                       # host:布局回归测试
```

`rt` feature 拉入 `cortex-m-rt 0.7`（device 中断表 + 链接脚本）；点灯示例参考 [cw32f030-pac/examples/blink.rs](../cw32f030-pac/examples/blink.rs)，移植时按家族改端口/引脚与时钟位。
