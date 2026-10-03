# 长期记忆

## 毕设工程：OPS9 全局定位模块（STM32F103C8/T8 + HAL + MDK-ARM）

仓库位置：`C:\Users\24475\Desktop\毕设\ops9-main\ops9-main\`
- `code/` 与 `code - 副本/`：同一份基础工程，当前工作区为 `code - 副本\Core\Src`
- `ABC/`：副本的**重构改良版**（功能相同，架构/引脚有差异），对比结论见 2026-10-03.md
- `example/`、`ops9-visualize/`、`pcb/`、`model/`：参考例程、上位机可视化、PCB 与结构件

### 工程定位
纯「里程计 / 位姿板」，**无电机 PWM 输出**：
双路编码器脉冲（TIM1/TIM2|TIM4 外部时钟计数 + 方向 GPIO）+ WIT 陀螺仪 yaw → 航迹推算 →
USART2 以 `0x5C + float x/y/z + CRC8`（14B）上报位姿，接收 `0xC5 + cmd`（0x22 复位，0x30~0x33 方向）。

### 硬件版本差异（改代码前务必确认当前用的是哪版）
| 项 | code / 副本 | ABC |
|---|---|---|
| 编码器脉冲输入 | TIM1/PA8、TIM4/PB6 | TIM1/PA8、TIM2/PA0 |
| 方向脚 | PA9、PB7 | PA10、PA11 |
| IMU 串口 | USART3 PB10/PB11（DMA1_Ch3） | USART1 remap PB6/PB7（DMA1_Ch5） |
| ENCODER_PULSE | 1024.0f | 254.0f |

### 约定 / 偏好
- 用户要求「先解读排查、不动代码」时，只输出分析与方案，不修改文件。
- 注释与说明使用中文；代码风格沿用原工程（4 空格/.c 中文注释）。
