# HDL Specification
文件名以 .hdl 结尾，开头需大写字母。

**Chip Header (Interface)**


    CHIP chip name {
      IN input pin name, input pin name,... ;
      OUT output pin name, output pin name,... ;
      // Here comes the body.
    }

输入和输出默认为一位宽（single-bit wide），如果是多位的，pin 的名字需要是 name[w]，w 为数字，0 是最低位（least significant bit）

**Chip Body (Implementation)**


    PARTS:
    internal chip part;
    internal chip part;
    ...
    internal chip part;

**chip part:**

    chip name (connection,..., connection);

**connection:**

    part’s pin names = chip’s pin name

**internal pins:**

    Part1 (..., out=v); // out of Part1 is piped into v
    Part2 (in=v, ...); // v is piped into in of Part2
    Part3 (a=v, b=v, ...);

v 就是 internal pin ，internal pin 可以有一个输入和无数个输出，比如给 Part2 里的 in 和 Part3 里的 a, b。internal pin 只能有一个输入来源。

**input pins:**
输入来源:

1. chip 的 input pin
2. internal pin
3. 1 或者 0

也只能有一个来源

**output pins:**
输出目的地：

1. chip 的 output pin
2. internal pin

**buses:**
input, output, internal pin 都可能是 multi-bit bus

input, output pin 的位宽在 chip header 里声明了，internal pin 的位宽则是隐式推导出来的


**Built-In Chips**
three special services:

1. Foundation
2. Certification and efficiency
3. Visualization


**Sequential Chips**
电脑芯片要么是组合逻辑（combinational），要么是时序逻辑（sequential, clocked）。

combinational 芯片的操作是瞬时的，sequential 的是由时钟控制的，需要等到下一个时间单位开始。

sequential 芯片也可能在时间改变但 input 没有任何改变的情况下修改 output 值，而 combinational 的不会因为时间的改变而改变值。

**时钟（The Clock）**
时间进展由 tick 和 tock 两个操作来支持，它们用来模拟一系列的 *time unit （时间单位）*。time unit 有两个阶段：tick 结束一个时间单位第一个阶段的结束，并开始第二个阶段；tock 表明下一个时间单位第一个阶段的开始。

*真实时间的流逝与模拟目的无关*。即可以完成用 tick tock 来控制生成一系列的模拟 time unit。

tick 阶段读取值，影响 chip 的内部状态；
tock 阶段才会把新的值写入到 output 内。

**clocked property of chips**
chip 是否有 clocked 属性是递归检查的，如果有依赖的 chip 是 clocked，那么这个 chip 也是 clocked 的

