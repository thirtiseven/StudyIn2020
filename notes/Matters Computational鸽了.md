[TOC]

https://www.jjj.de

# 位运算巫术

## 冷知识

- 尽可能使用无符号整数来加速运算（注意是否恒为非负整数

### 平均值

```c++
static inline unsigned long floor_average(unsigned long x, unsigned long y) {
	return  (x & y) + ((x ^ y) >> 1);
	// return  y + ((x-y)>>1);  // works if x>=y
}

static inline unsigned long ceil_average(unsigned long x, unsigned long y) {
	return  (x | y) - ((x ^ y) >> 1);
}
```

### 使x在a和b之间切换

```c++
//整数
t = a ^ b;
x ^= t;
//小数
t = a + b;
x = t - x;
```



### 下一个奇数/偶数

```c++
using ulong = unsigned long;

static inline bool is_even(ulong x)  { return (0==(x&1UL)); }
static inline bool is_odd(ulong x)  { return (0!=(x&1UL)); }

// next or previous even or odd value:
static inline ulong next_even(ulong x)  { return (x|1UL)+1UL; }
static inline ulong prev_even(ulong x)  { return (x-1UL)&~1UL; }
static inline ulong next_odd(ulong x)  { return (x+1UL)|1UL; }
static inline ulong prev_odd(ulong x)  { return (x&~1UL)-1UL; }
//static inline ulong next_even(ulong x)  { return x+2-(x&1UL); }
//static inline ulong prev_even(ulong x)  { return x-2+(x&1UL); }
//static inline ulong next_odd(ulong x)  { return x+1UL+(x&1UL); }
//static inline ulong prev_odd(ulong x)  { return x-1UL-(x&1UL); }


// same or next, and same or previous even or odd value:
static inline ulong next0_even(ulong x)  { return (x+1UL)&~1UL; }
static inline ulong prev0_even(ulong x)  { return x&~1UL; }
static inline ulong next0_odd(ulong x)  { return x|1UL; }
static inline ulong prev0_odd(ulong x)  { return (x-1UL)|1UL; }
//static inline ulong next0_even(ulong x)  { return x+(x&1UL); }
//static inline ulong prev0_even(ulong x)  { return x-(x&1UL); }
//static inline ulong next0_odd(ulong x)  { return x+1UL-(x&1UL); }
//static inline ulong prev0_odd(ulong x)  { return x-1UL+(x&1UL); }
```



### double 转 int

```c++
#define DOUBLE2INT(i, d) { double t = ((d) + 6755399441055744.0); i = *((int *)(&t)); }
double x = 123.0;
int i;
DOUBLE2INT(i, x);
```



## 对特定位的操作

### 测试、设置和删除某一位

```c++
using ulong = unsigned long;

static inline ulong test_bit(ulong a, ulong i)
// Return zero if bit[i] is zero,
//  else return one-bit word with bit[i] set.
{
	return  (a & (1UL << i));
}

static inline bool test_bit01(ulong a, ulong i)
// Return whether bit[i] is set.
{
	return ( 0 != test_bit(a, i) );
}

static inline ulong set_bit(ulong a, ulong i)
// Return a with bit[i] set.
{
	return  (a | (1UL << i));
}

static inline ulong clear_bit(ulong a, ulong i)
// Return a with bit[i] cleared.
{
	return  (a & ~(1UL << i));
}

static inline ulong change_bit(ulong a, ulong i)
// Return a with bit[i] changed.
{
	return  (a ^ (1UL << i));
}
```



### 复制和交换

```c++
using ulong = unsigned long;

static inline ulong copy_bit(ulong a, ulong isrc, ulong idst)
// Copy bit at [isrc] to position [idst].
// Return the modified word.
{
	ulong x = ((a>>isrc) ^ (a>>idst)) & 1;  // one if bits differ
	a ^= (x<<idst);  // change if bits differ
	return  a;
}
// -------------------------


static inline ulong mask_copy_bit(ulong a, ulong msrc, ulong mdst)
// Copy bit according at src-mask (msrc)
//  to the bit according to the dest-mask (mdst).
// Both msrc and mdst must have exactly one bit set.
// Return the modified word.
{
	ulong x = mdst;
	if ( msrc & a )  x = 0;  // zero if source bit set (compiled to cmov)
	x ^= mdst;  // ==mdst if source bit set, else zero
	a &= ~mdst;  // clear dest bit
	a |= x;
	return  a;
}

static inline ulong mask_or_bit(ulong a, ulong msrc, ulong mdst)
// Or bit according to src-mask (msrc)
//  to the bit according to the dest-mask (mdst).
// Both msrc and mdst must have exactly one bit set.
// If the mdst-position is known to be zero this function
//  is equivalent to mask_copy_bit().
// Return the modified word.
{
	if ( msrc & a )  mdst = 0;  // zero if source bit set (compiled to cmov)
	a |= mdst;
	return  a;
}

static inline ulong bit_swap_01(ulong a, ulong k1, ulong k2)
// Return a with bits at positions [k1] and [k2] swapped.
// Bits must have different values (!)
// (i.e. one is zero, the other one)
// k1==k2 is allowed (a is unchanged then)
{
	return  a ^ ( (1UL<<k1) ^ (1UL<<k2) );
}


static inline ulong bit_swap(ulong a, ulong k1, ulong k2)
// Return a with bits at positions [k1] and [k2] swapped.
// k1==k2 is allowed (a is unchanged then)
{
	ulong x = ((a>>k1) ^ (a>>k2)) & 1;  // one if bits differ
	a ^= (x<<k2);  // change if bits differ
	a ^= (x<<k1);  // change if bits differ
	return  a;
}
```



## lowbit 操作

### 最低的1 最低的0 清除最低的1 设置最低的0

```c++
using ulong = unsigned long;

static inline ulong lowest_one(ulong x)
// Return word where only the lowest set bit in x is set.
// Return 0 if no bit is set.
{
	return  x & -x;  // use: -x == ~x + 1
}

static inline ulong lowest_zero(ulong x)
// Return word where only the lowest unset bit in x is set.
// Return 0 if all bits are set.
{
	x = ~x;
	return  x & -x;
}

static inline ulong clear_lowest_one(ulong x)
// Return word where the lowest bit set in x is cleared.
// Return 0 for input == 0.
{
	return  x & (x-1);
}

static inline ulong set_lowest_zero(ulong x)
// Return word where the lowest unset bit in x is set.
// Return ~0 for input == ~0.
{
	return  x | (x+1);
}
```

### lowbit在第几位

```c++
static inline ulong lowest_one_idx(ulong x)
// Return index of lowest bit set.
// Bit index ranges from zero to BITS_PER_LONG-1.
// Examples:
//    ***1 --> 0
//    **10 --> 1
//    *100 --> 2
// Return 0 (also) if no bit is set.
{
	ulong r = 0;
	x &= -x;
#if  BITS_PER_LONG >= 64
	r |= ( (x & 0xffffffff00000000UL) != 0 );  r <<= 1;
	r |= ( (x & 0xffff0000ffff0000UL) != 0 );  r <<= 1;
	r |= ( (x & 0xff00ff00ff00ff00UL) != 0 );  r <<= 1;
	r |= ( (x & 0xf0f0f0f0f0f0f0f0UL) != 0 );  r <<= 1;
	r |= ( (x & 0xccccccccccccccccUL) != 0 );  r <<= 1;
	r |= ( (x & 0xaaaaaaaaaaaaaaaaUL) != 0 );
#else  // BITS_PER_LONG >= 64
	r |= ( (x & 0xffff0000UL) != 0 );  r <<= 1;
	r |= ( (x & 0xff00ff00UL) != 0 );  r <<= 1;
	r |= ( (x & 0xf0f0f0f0UL) != 0 );  r <<= 1;
	r |= ( (x & 0xccccccccUL) != 0 );  r <<= 1;
	r |= ( (x & 0xaaaaaaaaUL) != 0 );
#endif  // BITS_PER_LONG >= 64
	return r;
}
```

### 在低位创造过渡/隔离地位的匹配位/最低的连续1

```c++
static inline ulong low_ones(ulong x)
// Return word where all the (low end) ones are set.
// Example:  01011011 --> 00000011
// Return 0 if lowest bit is zero:
//       10110110 --> 0
{
	x = ~x;
	x &= -x;
	--x;
	return x;
}

static inline ulong low_zeros(ulong x)
// Return word where all the (low end) zeros are set.
// Example:  01011000 --> 00000111
// Return 0 if all bits are set.
{
	x &= -x;
	--x;
	return x;
}


static inline ulong low_match(ulong x, ulong y)
// Return word that contains all bits at the low end where x and y match.
// If x = *0W and y = *1W, then 00W is returned.
{
	x ^= y;   // bit-wise difference
	x &= -x;  // lowest bit that differs in both words
	x -= 1;   // mask that covers equal bits at low end
	x &= y;   // isolate matching bits
	return x;
}

static inline ulong lowest_block(ulong x)
// Isolate lowest block of ones.
{
	// Example:
	// x   = *****011100
	// l   = 00000000100
	// y   = *****100000
	// x^y = 00000111100
	// ret = 00000011100

	ulong l = x & -x;  // lowest bit
	ulong y = x + l;
	y ^= x;
	return  y & x;
}

```

## 过渡位置的0/1/block处理

- 过渡：相邻两个位不等的位置
- 块（block）:一组连续且值相同的位

### 生成第p位开始有n个1的数

```c++
using ulong = unsigned long;

static inline ulong bit_block(ulong p, ulong n)
// Return word with length-n bit block starting at bit p set.
// Both p and n are effectively taken modulo BITS_PER_LONG.
{
	ulong x = (1UL<<n) - 1;
	return  x << p;
}

static inline ulong cyclic_bit_block(ulong p, ulong n)
// Return word with length-n bit block starting at bit p set.
// The result is possibly wrapped around the word boundary.
// Both p and n are effectively taken modulo BITS_PER_LONG.
{
	ulong x = (1UL<<n) - 1;
	return  (x<<p) | (x>>(BITS_PER_LONG-p));
}
```

### 单独的1

```c++
static inline ulong single_ones(ulong x)
// Return word with only the isolated ones of x set.
// Assume outside values are 0. 把1的block变成0
{
	return  x & ~( (x<<1) | (x>>1) );
}

static inline ulong single_ones_xi(ulong x)
// Return word with only the isolated ones of x set.
// Ignore outside values.
{
    return  single_ones(x);  // (identical function)
}

static inline ulong single_zeros(ulong x)
// Return word with only the isolated zeros of x set.
// Assume outside values are 0.
{
	return  ~x & ( (x<<1) & (x>>1) );
}

static inline ulong single_zeros_xi(ulong x)
// Return word with only the isolated zeros of x set.
// Ignore outside values.
{
	return  single_ones( ~x );
}

static inline ulong single_values(ulong x)
// Return word where only the isolated ones and zeros of x are set.
// Assume outside values are 0.
{
    return  (x ^ (x<<1)) & (x ^ (x>>1));
}

static inline ulong single_values_xi(ulong x)
// Return word where only the isolated ones and zeros of x are set.
// Ignore outside values.
{
    return  single_ones_xi(x) | single_zeros_xi(x);
}
```



### 边界和转换上的01

```c++
static inline ulong border_ones(ulong x)
// Return word where only those ones of x are set that lie next to a zero.
{
//    ulong t = x & (x>>1) & (x<<1);  // interior_ones(x)
//    return  x ^ t;
	return  x & ~( (x<<1) & (x>>1) );
}


static inline ulong border_values(ulong x)
// Return word where those bits of x are set that lie on a transition.
// Assume outside values are 0.
{
	return  (x ^ (x<<1)) | (x ^ (x>>1));
}


static inline ulong high_border_ones(ulong x)
// Return word where only those ones of x are set
//   that lie right to (i.e. in the next lower bin of) a zero.
{
	return  x & ( x ^ (x>>1) );
}


static inline ulong low_border_ones(ulong x)
// Return word where only those ones of x are set
//   that lie left to (i.e. in the next higher bin of) a zero.
{
	return  x & ( x ^ (x<<1) );
}



static inline ulong block_border_ones(ulong x)
// Return word where only those ones of x are set
//   that are at the border of a block of at least 2 ones.
{
	return  x & ( (x<<1) ^ (x>>1) );
}


static inline ulong low_block_border_ones(ulong x)
// Return word where only those ones of x are set
//   that are at left of a border of a block of at least 2 ones.
{
	ulong t = x & ( (x<<1) ^ (x>>1) );  // block_border_ones()
	return  t & (x>>1);
}


static inline ulong high_block_border_ones(ulong x)
// Return word where only those ones of x are set
//   that are at right of a border of a block of at least 2 ones.
{
	ulong t = x & ( (x<<1) ^ (x>>1) );  // block_border_ones()
	return  t & (x<<1);
}



static inline ulong block_ones(ulong x)
// Return word where only those ones of x are set
//   that are part of a block of at least 2 ones.
{
	return  x & ( (x<<1) | (x>>1) );
}


static inline ulong block_values(ulong x)
// Return word where only those bits of x are set
// that do not lie next to an opposite value.
{
	return  ~single_values(x);
}



static inline ulong interior_ones(ulong x)
// Return word where only those ones of x are set
//   that do not have a zero to their left or right.
{
	return  x & ( (x<<1) & (x>>1) );
}


static inline ulong interior_values(ulong x)
// Return word where only those values of x are set
//   that do have a transitions on both sides.
{
	return  ~border_values(x);
}
```



