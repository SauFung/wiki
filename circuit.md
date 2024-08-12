# 电路记录

## ICs

### 74HC165
- 移位寄存器：并行输入转为串行数据，用于拓展 IO

#### 引脚定义
| 引脚号 | 引脚名 | 引脚别名        | 功能描述           |
| ------ | ------ | --------------- | ------------------ |
| 1      | PL     | SH/LD           | 低电平读取数据     |
| 2      | CP     | CLK             | 时钟输入           |
| 3~6    | D4~D7  | -               | 并行数据输入       |
| 11~14  | D0~D3  | -               | 并行数据输入       |
| 7      | Q7'    | $\overline{QH}$ | 串行互补输出       |
| 9      | Q7     | SO              | 串行数据输出       |
| 10     | DS     | SI              | 串行数据输入       |
| 15     | CE     | INH_            | 低电平使能时钟引脚 |

- 1 号引脚为低电平时，8 个数据输入引脚的数据输入到内部寄存器
- 15 号引脚为低电平时，2 号引脚才有效
- 7 号引脚与 9 号引脚输出电平相反，即：
  - 一个高，另一个为低
  - 一个低，另一个为高
- 输出的数据从的顺序为 D7 ~ D0，9 号引脚输出的电平与输入引脚一致


