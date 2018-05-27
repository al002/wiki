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

input, output pin 的位宽在 chip header 里声明了，internal pin 则是隐式推导出来的

