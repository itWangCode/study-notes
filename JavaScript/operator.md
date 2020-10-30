# 运算符

参考：

<https://www.cnblogs.com/seeks/p/7710977.html>

## 左移 << 右移 >>

<https://juejin.im/post/5dc36f39e51d4529ed292910>
***

`>>`

a >> b

有符号右移:将 a 的二进制表示向右移 b (< 32) 位，丢弃被移出的位。

即整体向右移b位，例如 1010, 右移一位 变为 101,0 并丢弃被移除的 0 。变为 101

## 按位非 ~

按位非运算符(~): 反转操作数的位。包括最高位（符号位）

```js
const a = 5;     // 0000000000000101
const b = -3;    // 1111111111111101

console.log(~a); // 1111111111111010
// expected output: -6

console.log(~b); // 0000000000000010
// expected output: 2


~(6.5) // -7
~~(6.5) // 6
```

~ 返回 2 的补码，

并且 ~ 会将数字转换为 32 位整数，因此我们可以使用 ~ 来进行取整操作。

~x 大致等同于 -(x+1)。

## 按位与&

对补码按位进行与操作

## 按位或 |

## 计算机有符号数：原码、反码和补码

计算机有符号数有3种表示法：原码、反码和补码

### 原码(true form)

是一种计算机中对数字的二进制定点表示方法。原码表示法在数值前面增加了一位符号位（即最高位为符号位）：正数该位为0，负数该位为1（0有两种表示：+0和-0），其余位表示数值的大小。

如：

127的原码为0111 1111

-127的原码为1111 1111

### 反码

正数的反码与原码一致；

负数的反码是对原码按位取反，只是最高位(符号位)不变。

127的反码为0111 1111

-127的反码为1000 0000

### 补码

正数的补码与原码一致；

负数的补码是该数的反码加1。

127的补码为0111 1111

-127的补码为1000 0001