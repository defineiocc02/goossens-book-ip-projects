# 计算机处理器架构指南 - FPGA 实验项目

> 基于 Bernard Goossens 的《Guide to Computer Processor Architecture》(Springer 2023) 一书

## 📖 项目简介

本项目是《计算机处理器架构指南》书籍的配套 FPGA 实验代码，使用 Xilinx Vitis 和 Vivado 工具链进行 RISC-V 处理器的设计与实现。

## 🗂️ 项目结构

```
goossens-book-ip-projects/
├── README.md              # 本文件（中文说明）
├── projects/              # Vitis 2025.1 项目文件夹
│   ├── chapter_2/         # 第2章：基础 IP 核开发
│   ├── chapter_3/         # 第3章：RISC-V 指令集模拟
│   ├── chapter_4/         # 第4章：编译优化技术
│   ├── fde_ip/            # 取指-译码-执行 IP 核
│   ├── fetching_ip/       # 取指单元 IP 核
│   ├── simple_pipeline_ip/# 简单流水线处理器
│   ├── rv32i_pp_ip/       # RV32I 流水线处理器
│   ├── rv32i_npp_ip/      # RV32I 非流水线处理器
│   ├── multihart_ip/      # 多 Hart 处理器
│   ├── multicore_multihart_ip/   # 多核多 Hart 处理器
│   ├── multicore_multicycle_ip/  # 多核多周期处理器
│   ├── multicycle_pipeline_ip/   # 多周期流水线处理器
│   ├── multi_core_multi_ram_ip/  # 多核多 RAM 处理器
│   ├── mibench/           # MiBench 测试套件
│   ├── riscv-tests/       # RISC-V 官方测试
│   └── pynq_io/           # PYNQ 板 IO 实验
├── fir/                   # FIR 滤波器 HLS 实现
└── .git/                  # Git 版本控制
```

## 🚀 快速开始

### 环境要求

- **Vitis 2025.1** - Xilinx 统一开发平台
- **Vivado 2025.1** - FPGA 综合实现工具
- **PYNQ-Z1/Z2 开发板**（可选，用于硬件验证）

### 安装步骤

1. **安装 Vitis 2025.1**
   - 从 [Xilinx 官网](https://www.xilinx.com/support/download.html) 下载
   - 安装时选择 Vitis 和 Vivado

2. **克隆仓库**
   ```bash
   git clone https://github.com/goossens-springer/goossens-book-ip-projects.git
   cd goossens-book-ip-projects
   ```

3. **打开项目**
   - 启动 Vitis 2025.1
   - 选择工作空间
   - 导入项目文件夹中的工程

## 📚 项目详解

### 基础项目 (Chapter 2-4)

| 项目 | 说明 | 对应章节 |
|------|------|----------|
| `chapter_2/my_adder_ip` | 简单加法器 IP 核 | 第2章 |
| `chapter_3/spike` | RISC-V 模拟器配置 | 第3章 |
| `chapter_4/compile` | 编译优化示例 | 第4章 |

### 处理器 IP 核

#### 1. 取指-译码-执行 IP (`fde_ip/`)
基础的三级流水线处理器，包含：
- **取指 (Fetch)** - 从指令存储器读取指令
- **译码 (Decode)** - 解析指令类型和操作数
- **执行 (Execute)** - 执行 ALU 操作

#### 2. 简单流水线 IP (`simple_pipeline_ip/`)
经典的五级流水线处理器：
- IF (Instruction Fetch) - 取指
- ID (Instruction Decode) - 译码
- EX (Execute) - 执行
- MEM (Memory Access) - 访存
- WB (Write Back) - 写回

#### 3. RV32I 处理器 IP (`rv32i_pp_ip/` 和 `rv32i_npp_ip/`)
完整的 RV32I 指令集实现：
- **rv32i_pp_ip** - 流水线版本 (Pipelined)
- **rv32i_npp_ip** - 非流水线版本 (Non-Pipelined)

支持指令：
- 算术运算：ADD, SUB, ADDI
- 逻辑运算：AND, OR, XOR, ANDI, ORI, XORI
- 移位运算：SLL, SRL, SRA, SLLI, SRLI, SRAI
- 比较运算：SLT, SLTU, SLTI, SLTIU
- 分支指令：BEQ, BNE, BLT, BGE, BLTU, BGEU
- 跳转指令：JAL, JALR
- 访存指令：LB, LH, LW, LBU, LHU, SB, SH, SW
- 立即数：LUI, AUIPC

#### 4. 多 Hart 处理器 (`multihart_ip/`)
支持多线程的处理器，可同时执行多个程序。

#### 5. 多核处理器 (`multicore_*_ip/`)
- **multicore_multihart_ip** - 多核多 Hart
- **multicore_multicycle_ip** - 多核多周期
- **multi_core_multi_ram_ip** - 多核多 RAM 架构

### 测试与验证

#### RISC-V 测试套件 (`riscv-tests/`)
官方 RISC-V 指令测试：
- `isa/` - 指令集架构测试
- `benchmarks/` - 性能基准测试（median, mm, multiply, qsort, spmv, towers, vvadd）

#### MiBench 测试 (`mibench/`)
嵌入式系统基准测试套件。

### FIR 滤波器 (`fir/`)
使用 HLS（高层次综合）实现的 FIR 滤波器：
- `fir_seq/` - 顺序实现
- `fir_pipeline/` - 流水线实现
- `fir_reduction_fast/` - 优化版本

## 🛠️ 开发流程

### 1. HLS 设计
```bash
# 进入项目目录
cd projects/simple_pipeline_ip

# 编译 HLS 代码
vitis_hls -f script.tcl
```

### 2. Vivado 集成
```bash
# 打开 Vivado 项目
vivado z1_simple_pipeline_ip.xpr

# 生成比特流
launch_runs impl_1 -to_step write_bitstream
```

### 3. Vitis 软件开发
```bash
# 创建平台项目
# 创建应用项目
# 编译并运行
```

## 📝 关键代码说明

### type.h - 基础类型定义
```cpp
// 处理器中常用的数据类型定义
typedef unsigned int uint32_t;  // 32位无符号整数
typedef int int32_t;            // 32位有符号整数
typedef unsigned char uint8_t;  // 8位无符号整数（字节）
```

### fetch.cpp - 取指单元
```cpp
// 从指令存储器读取指令
// PC (程序计数器) 指向下一条指令地址
// 支持分支和跳转的 PC 更新
```

### decode.cpp - 译码单元
```cpp
// 解析 RISC-V 指令格式
// R-type: 寄存器-寄存器操作
// I-type: 立即数操作
// S-type: 存储操作
// B-type: 分支操作
// U-type: 长立即数
// J-type: 跳转操作
```

### execute.cpp - 执行单元
```cpp
// ALU 运算实现
// 算术运算：加、减、比较
// 逻辑运算：与、或、异或
// 移位运算：左移、右移
```

## 🔧 工具链配置

### RISC-V GCC 工具链
```bash
# 配置工具链（使用 2.2 ISA 版本）
./configure --prefix=/opt/riscv-book \
    --with-multilib-generator="rv32i-ilp32--" \
    --with-isa-spec=2.2

make
sudo make install
```

### Spike 模拟器
```bash
# 运行 RISC-V 程序
spike -d --isa=RV32I test_program

# 调试命令
# r - 运行
# s - 单步执行
# reg - 查看寄存器
# mem - 查看内存
```

## 📖 学习路径建议

1. **入门阶段**
   - 阅读第2章，完成 my_adder_ip 实验
   - 学习 Vitis HLS 基本使用

2. **基础阶段**
   - 阅读第3章，配置 Spike 模拟器
   - 理解 RISC-V 指令集

3. **进阶阶段**
   - 阅读第4章，学习编译优化
   - 实现 fde_ip 项目

4. **高级阶段**
   - 实现完整 RV32I 处理器
   - 添加流水线、多 Hart、多核功能

## 📚 参考资源

- **书籍**: 《Guide to Computer Processor Architecture》by Bernard Goossens
- **RISC-V 规范**: https://riscv.org/technical/specifications/
- **Vitis 文档**: https://docs.xilinx.com/v/u/en-US/ug1393-vitis-application-acceleration
- **PYNQ 项目**: https://www.pynq.io/

## 🤝 贡献指南

欢迎提交 Issue 和 Pull Request！

## 📄 许可证

本项目遵循原仓库的许可证条款。

---

**作者**: Bernard Goossens  
**整理**: FPGA Developer  
**日期**: 2025年
