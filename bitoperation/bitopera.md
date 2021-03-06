![](8.PNG)
**（0表示False，1表示True，空位都当0处理）**
- `and` 运算 `&`   
and运算通常用于二进制的取位操作，例如一个数 and 1的结果就是取二进制的最末位。这可以用来判断一个整数的奇偶，二进制的最末位为0表示该数为偶数，最末位为1表示该数为奇数。
相同位的两个数字都为1，则为1；若有一个不为1，则为0。   
```   
00101
11100
（&或者and）
----------------
00100
```
- `or` 运算
or运算通常用于二进制特定位上的无条件赋值，例如一个数or 1的结果就是把二进制最末位强行变成1。如果需要把二进制最末位变成0，对这个数or 1之后再减一就可以了，其实际意义就是把这个数强行变成最接近的偶数。
相同位只要一个为1即为1。
```
00101
11100
（|或者or）
----------------
11101
```
- `xor` 运算 `^`
异或的符号是^。按位异或运算, 对等长二进制模式按位或二进制数的每一位执行逻辑按位异或操作. 操作的结果是如果某位不同则该位为1, 否则该位为0.
xor运算的逆运算是它本身，也就是说两次异或同一个数最后结果不变，即（a xor b) xor b = a。xor运算可以用于简单的加密，比如我想对我MM说1314520，但怕别人知道，于是双方约定拿我的生日19880516作为密钥。1314520 xor 19880516 = 20665500，我就把20665500告诉MM。MM再次计算20665500 xor 19880516的值，得到1314520。
相同位不同则为1，相同则为0。
```
00101
11100
（^或者xor）
----------------
11001
```
- `not` 运算 `~`
not运算的定义是把内存中的0和1全部取反。使用not运算时要格外小心，你需要注意整数类型有没有符号。如果not的对象是无符号整数（不能表示负数），那么得到的值就是它与该类型上界的差，因为无符号类型的数是用00到$FFFF依次表示的。
- `shl` 运算 ``<<``
a shl b就表示把a转为二进制后左移b位（在后面添b个0）。例如100的二进制为1100100，而110010000转成十进制是400，那么100 shl 2 = 400。
可以看出，**a shl b的值实际上就是a乘以2的b次方**，因为在二进制数后添一个0就相当于该数乘以2。
通常认为a shl 1比a * 2更快，因为前者是更底层一些的操作。因此程序中乘以2的操作请尽量用左移一位来代替。
定义一些常量可能会用到shl运算。你可以方便地用1 shl 16 - 1来表示65535。很多算法和数据结构要求数据规模必须是2的幂，此时可以用shl来定义Max_N等常量。
- `shr` 运算 ``>>``
和shl相似，a shr b表示二进制右移b位（去掉末b位），相当于a除以2的b次方（取整）。我们也经常用shr 1来代替div 2，比如二分查找、堆的插入操作等等。想办法用shr代替除法运算可以使程序效率大大提高。最大公约数的二进制算法用除以2操作来代替慢得出奇的mod运算，效率可以提高60%。

| 功能                  | 示例                   | 位运算                                   |
|:----------------------|:-----------------------|:-----------------------------------------|
| 去掉最后一位          | (101101->10110)        | x shr 1                                  |
| 在最后加一个0         | (101101->1011010)      | x shl 1                                  |
| 在最后加一个1         | (101101->1011011)      | x shl 1+1                                |
| 把最后一位变成1       | (101100->101101)       | x or 1                                   |
| 把最后一位变成0       | (101101->101100)       | x or 1-1                                 |
| 最后一位取反          | (101101->101100)       | x xor 1                                  |
| 把右数第k位变成1      | (101001->101101,k=3)   | x or (1 shl (k-1))                       |
| 把右数第k位变成0      | (101101->101001,k=3)   | x and not (1 shl (k-1))                  |
| 右数第k位取反         | (101001->101101,k=3)   | x xor (1 shl (k-1))                      |
| 取末三位              | (1101101->101)         | x and 7                                  |
| 取末k位               | (1101101->1101,k=5)    | x and 15                                 |
| 取右数第k位           | (1101101->1,k=4)       | x shr (k-1) and 1                        |
| 把末k位变成1          | (101001->101111,k=4)   | x or (1 shl k-1)                         |
| 末k位取反             | (101001->100110,k=4)   | x xor (1 shl k-1)                        |
| 把右边连续的1变成0    | (100101111->100100000) | x and (x+1)                              |
| 把右起第一个0变成1    | (100101111->100111111) | x or (x+1)                               |
| 把右边连续的0变成1    | (11011000->11011111)   | x or (x-1)                               |
| 取右边连续的1         | (100101111->1111)      | (x xor (x+1)) shr 1                      |
| 去掉右起第一个1的左边 | (100101000->1000)      | x and not (x xor (x-1))（或 x and (-x)） |
