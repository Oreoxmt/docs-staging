---
title: Numeric Functions and Operators
summary: Learn about the numeric functions and operators.
---

# 数値関数と演算子 {#numeric-functions-and-operators}

TiDB は、 MySQL 5.7で利用可能な[数値関数と演算子](https://dev.mysql.com/doc/refman/5.7/en/numeric-functions.html)をすべてサポートします。

## 算術演算子 {#arithmetic-operators}

| 名前                                                                                                       | 説明         |
| :------------------------------------------------------------------------------------------------------- | :--------- |
| [`+`](https://dev.mysql.com/doc/refman/5.7/en/arithmetic-functions.html#operator_plus)                   | 加算演算子      |
| [`-`](https://dev.mysql.com/doc/refman/5.7/en/arithmetic-functions.html#operator_minus)                  | マイナス演算子    |
| [`*`](https://dev.mysql.com/doc/refman/5.7/en/arithmetic-functions.html#operator_times)                  | 乗算演算子      |
| [`/`](https://dev.mysql.com/doc/refman/5.7/en/arithmetic-functions.html#operator_divide)                 | 部門演算子      |
| [`DIV`](https://dev.mysql.com/doc/refman/5.7/en/arithmetic-functions.html#operator_div)                  | 整数除算       |
| [`%` 、 <code>MOD</code>](https://dev.mysql.com/doc/refman/5.7/en/arithmetic-functions.html#operator_mod) | モジュロ演算子    |
| [`-`](https://dev.mysql.com/doc/refman/5.7/en/arithmetic-functions.html#operator_unary-minus)            | 引数の符号を変更する |

## 数学関数 {#mathematical-functions}

| 名前                                                                                                      | 説明                  |
| :------------------------------------------------------------------------------------------------------ | :------------------ |
| [`POW()`](https://dev.mysql.com/doc/refman/5.7/en/mathematical-functions.html#function_pow)             | 指定された累乗で引数を返します     |
| [`POWER()`](https://dev.mysql.com/doc/refman/5.7/en/mathematical-functions.html#function_power)         | 指定された累乗で引数を返します     |
| [`EXP()`](https://dev.mysql.com/doc/refman/5.7/en/mathematical-functions.html#function_exp)             | のべき乗に上げる            |
| [`SQRT()`](https://dev.mysql.com/doc/refman/5.7/en/mathematical-functions.html#function_sqrt)           | 引数の平方根を返します         |
| [`LN()`](https://dev.mysql.com/doc/refman/5.7/en/mathematical-functions.html#function_ln)               | 引数の自然対数を返す          |
| [`LOG()`](https://dev.mysql.com/doc/refman/5.7/en/mathematical-functions.html#function_log)             | 最初の引数の自然対数を返します     |
| [`LOG2()`](https://dev.mysql.com/doc/refman/5.7/en/mathematical-functions.html#function_log2)           | 引数の 2 を底とする対数を返します  |
| [`LOG10()`](https://dev.mysql.com/doc/refman/5.7/en/mathematical-functions.html#function_log10)         | 引数の 10 を底とする対数を返します |
| [`PI()`](https://dev.mysql.com/doc/refman/5.7/en/mathematical-functions.html#function_pi)               | pi の値を返す            |
| [`TAN()`](https://dev.mysql.com/doc/refman/5.7/en/mathematical-functions.html#function_tan)             | 引数のタンジェントを返します      |
| [`COT()`](https://dev.mysql.com/doc/refman/5.7/en/mathematical-functions.html#function_cot)             | コタンジェントを返す          |
| [`SIN()`](https://dev.mysql.com/doc/refman/5.7/en/mathematical-functions.html#function_sin)             | 引数のサインを返します         |
| [`COS()`](https://dev.mysql.com/doc/refman/5.7/en/mathematical-functions.html#function_cos)             | コサインを返す             |
| [`ATAN()`](https://dev.mysql.com/doc/refman/5.7/en/mathematical-functions.html#function_atan)           | 逆正接を返します            |
| [`ATAN2(), ATAN()`](https://dev.mysql.com/doc/refman/5.7/en/mathematical-functions.html#function_atan2) | 2 つの引数の逆正接を返します     |
| [`ASIN()`](https://dev.mysql.com/doc/refman/5.7/en/mathematical-functions.html#function_asin)           | 逆正弦を返す              |
| [`ACOS()`](https://dev.mysql.com/doc/refman/5.7/en/mathematical-functions.html#function_acos)           | 逆余弦を返す              |
| [`RADIANS()`](https://dev.mysql.com/doc/refman/5.7/en/mathematical-functions.html#function_radians)     | ラジアンに変換された引数を返します   |
| [`DEGREES()`](https://dev.mysql.com/doc/refman/5.7/en/mathematical-functions.html#function_degrees)     | ラジアンを度に変換する         |
| [`MOD()`](https://dev.mysql.com/doc/refman/5.7/en/mathematical-functions.html#function_mod)             | 余りを返す               |
| [`ABS()`](https://dev.mysql.com/doc/refman/5.7/en/mathematical-functions.html#function_abs)             | 絶対値を返す              |
| [`CEIL()`](https://dev.mysql.com/doc/refman/5.7/en/mathematical-functions.html#function_ceil)           | 引数以上の最小の整数値を返します    |
| [`CEILING()`](https://dev.mysql.com/doc/refman/5.7/en/mathematical-functions.html#function_ceiling)     | 引数以上の最小の整数値を返します    |
| [`FLOOR()`](https://dev.mysql.com/doc/refman/5.7/en/mathematical-functions.html#function_floor)         | 引数を超えない最大の整数値を返します  |
| [`ROUND()`](https://dev.mysql.com/doc/refman/5.7/en/mathematical-functions.html#function_round)         | 引数を丸める              |
| [`RAND()`](https://dev.mysql.com/doc/refman/5.7/en/mathematical-functions.html#function_rand)           | ランダムな浮動小数点値を返す      |
| [`SIGN()`](https://dev.mysql.com/doc/refman/5.7/en/mathematical-functions.html#function_sign)           | 引数の符号を返します          |
| [`CONV()`](https://dev.mysql.com/doc/refman/5.7/en/mathematical-functions.html#function_conv)           | 異なる基数間で数値を変換する      |
| [`TRUNCATE()`](https://dev.mysql.com/doc/refman/5.7/en/mathematical-functions.html#function_truncate)   | 指定された小数点以下の桁数に切り捨てる |
| [`CRC32()`](https://dev.mysql.com/doc/refman/5.7/en/mathematical-functions.html#function_crc32)         | 巡回冗長検査値を計算する        |