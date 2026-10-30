# cw32-pacs

武汉芯源（CWsemi）CW32 系列 **Cortex-M0+** MCU 的 Rust PAC（Peripheral Access Crate）合集 —— 由 [cw32-pac-gen](https://github.com/SunWithCloude/cw32-pac-gen) 从原厂 SDK 头文件生成的**零依赖寄存器访问层**，一个家族一个 crate，Cargo workspace 统一管理与验证。

全部 crate：`no_std`、零运行时依赖（`rt` feature 按需拉入 `cortex-m-rt 0.7`）、`MIT OR Apache-2.0` 双许可、命名 `svd2rust` 风格（`Peripherals::steal()` / `take()`、外设单例、寄存器 `read()`/`write()`/`modify()`）。

## 家族矩阵

| 家族 | crate | Flash | SRAM | 实机验证 |
|---|---|---|---|---|
| CW32F003 | [`cw32f003-pac`](crates/cw32f003-pac) | 20K | 3K | 寄存器层待上板 |
| CW32F020 | [`cw32f020-pac`](crates/cw32f020-pac) | 32K | 8K | 寄存器层待上板 |
| CW32F030 | [`cw32f030-pac`](crates/cw32f030-pac) | 64K | 8K | 寄存器层待上板 |
| CW32L010 | [`cw32l010-pac`](crates/cw32l010-pac) | 64K | 4K | 寄存器层待上板 |
| CW32L011 | [`cw32l011-pac`](crates/cw32l011-pac) | 64K | 6K | 寄存器层待上板 |
| CW32L031 | [`cw32l031-pac`](crates/cw32l031-pac) | 64K | 8K | 寄存器层待上板 |
| CW32L052 | [`cw32l052-pac`](crates/cw32l052-pac) | 64K | 8K | 寄存器层待上板 |
| CW32L083 | [`cw32l083-pac`](crates/cw32l083-pac) | 256K | 24K | 寄存器层待上板 |
| CW32R031 | [`cw32r031-pac`](crates/cw32r031-pac) | 64K | 8K | 寄存器层待上板 |
| CW32W031 | [`cw32w031-pac`](crates/cw32w031-pac) | 64K | 8K | 寄存器层待上板 |
| CW32L012 | *（不在本仓）* | 64K | 8K | — |

> **CW32L012 独立维护**于 [`Cw32l012_pac`](https://github.com/SunWithCloude/Cw32l012_pac)（先于本仓发布，已支撑 [`cw32l012-hal`](https://github.com/SunWithCloude/cw32l012-hal) 与实机调试，发布节奏与寄存器补丁独立），故不收入本仓；后续如迁移会保留转发。

"实机验证"列指该家族 PAC 编译出的 blink 示例在真实板上点灯：生成时已逐寄存器核对厂商头文件布局（`tests/layout.rs` 固化），但 10 个家族中目前只有手头有板的家族能点亮。有板子欢迎 PR 更新矩阵。

## 用法

```toml
[dependencies]
cw32f030-pac = "0.1"
```

```rust
#![no_std]
use cw32f030_pac::Peripherals;

let p = unsafe { Peripherals::steal() };
p.SYSCTRL.ahben().modify(|_, w| w.gpio_cen().set_bit());
```

点灯示例（`cw32f030-pac` 提供，其他家族照此模式移植、欢迎 PR）：

```
cargo build --target thumbv6m-none-eabi --release --features rt -p cw32f030-pac --example blink
```

链接脚本由 `rt` feature + 各 crate 的 `memory.x`（家族最大 Flash/SRAM 配置）提供，`.cargo/config.toml` 对整个 workspace 生效。

## 在 CW32 Rust 生态中的位置

```
cw32-pac-gen（生成器，输入厂商 SDK 头文件）
   └─> cw32-pacs（本仓：10 家族 PAC）+ Cw32l012_pac（L012 PAC）
          └─> cw32l012-hal（HAL，目前覆盖 L012）
                 └─> 你的固件

probe-rs（cw32 targets YAML + SunWithCloude/cw32-probe-rs-compat）  ← 烧录/调试
cw32-tools（VS Code 插件：构建/烧录/可视化调试/Agent API）          ← 工具链
```

## 发布状态

crates.io 发布**待定**（待与原厂确认商标/命名合作事宜后定档），当前以 GitHub 依赖为主：

```toml
[dependencies]
cw32f030-pac = { git = "https://github.com/SunWithCloude/cw32-pacs" }
```

## 许可

MIT OR Apache-2.0（见 [LICENSE-MIT](LICENSE-MIT) / [LICENSE-APACHE](LICENSE-APACHE)）。PAC 由生成器从原厂 SDK 头文件（Apache-2.0）转换而来。
