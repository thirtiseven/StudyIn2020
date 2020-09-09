原文：http://learnyouahaskell.com/chapters

翻译：http://fleurer.github.io/lyah/

[TOC]

# 常用

## 输入输出

```haskell
readInt :: IO Int
readInt = readLn
 
readTuple :: IO (Int, Int)
readTuple = readLn

line <- getLine
let test_case = (map castToInt . take 2 . words) line
```



# 奇怪的地方

- 每个if必须写else
- /= 不等于

# List

- 使用let定义常量
- ++可以合并两个list
- 字符串实际上就是一组字符的List
- !!可以索引取得list中的元素
- \>和\>=比较列表大小：字典序
- list方法：head tail last init length null reverse
-  (take 数字) (drop 数字) minimum minimum sum product (数字 elem)
- [1..20] ['a'..'z'] \[2,4..20] （避免在range中使用浮点数）
- cycle 循环一个列表 repeat 循环一个数字 replicate 长度 数字 返回一个列表
- 通过反单引号，我们不仅可以以中缀形式调用函数，也可以在定义函数的时候使用它。有时这样会更易读

## List Comprehension

基本形式：

```haskell
[x2 | x <- [1..10], x2 >= 12]  
[ x | x <- [50..100], x `mod` 7 == 3]  
boomBangs xs = [ if x < 10 then "BOOM!" else "BANG!" | x <- xs, odd x] 
```



## Tuple

fst 序对首项 snd 尾项 （pair only

zip 用两个序列生成一个pair序列



# 类型

t 表达式 返回表达式的类型

函数名 :: 类型 

```haskell
addThree :: Int -> Int -> Int -> Int   
addThree x y z = x + y + z
```

类型 Int Integer Float Double Bool Char

类型类：Eq 判断相等 Ord 可比较大小：GT,LT,EQ

```haskell
ghci> "Abrakadabra" < "Zebra"   
True   
ghci> "Abrakadabra" `compare` "Zebra"   
LT   
ghci> 5 >= 2   
True   
ghci> 5 `compare` 3   
GT
```

show：把元素转成字符串

read：把字符串转成推导的类型 或指定类型

# 模式匹配

```haskell
sayMe :: (Integral a) => a -> String   
sayMe 1 = "One!"   
sayMe 2 = "Two!"   
sayMe 3 = "Three!"   
sayMe 4 = "Four!"   
sayMe 5 = "Five!"   
sayMe x = "Not between 1 and 5"  
```

# 门卫和where

```haskell
bmiTell :: (RealFloat a) => a -> a -> String   
bmiTell weight height   
    | bmi <= skinny = "You're underweight, you emo, you!"   
    | bmi <= normal = "You're supposedly normal. Pffft, I bet you're ugly!"   
    | bmi <= fat    = "You're fat! Lose some weight, fatty!"   
    | otherwise     = "You're a whale, congratulations!"   
    where bmi = weight / height ^ 2   
          skinny = 18.5   
          normal = 25.0   
          fat = 30.0
```

# Case

```haskell
case expression of pattern -> result   
                   pattern -> result   
                   pattern -> result   
```

# Map

map取一个函数和List做参数，遍历该List的每个元素来调用该函数产生一个新的List

```haskell
ghci> map (+3) [1,5,3,1,6]   
[4,8,6,4,9]   
ghci> map (++ "!") ["BIFF"，"BANG"，"POW"]   
["BIFF!","BANG!","POW!"]   
ghci> map (replicate 3) [3..6]   
[[3,3,3],[4,4,4],[5,5,5],[6,6,6]] 
```

# filter

filter函数取一个限制条件和一个List，返回该List中所有符合该条件的元素

```haskell
ghci> filter (>3) [1,5,3,2,1,6,4,3,2,1]   
[5,6,4]   
ghci> filter (==3) [1,2,3,4,5]   
[3]   
ghci> filter even [1..10]   
[2,4,6,8,10]  
```

# 模块

```haskell
import Data.List   
import Data.List (nub，sort)
import Data.List hiding (nub)
import qualified Data.Map
import qualified Data.Map as M
```

# Data.List

takeWhile从一个List中取元素，一旦遇到不符合条件的某元素就停止

```haskell
ghci> takeWhile (>3) [6,5,4,3,2,1,2,3,4,5,4,3,2,1]   
[6,5,4]   
ghci> takeWhile (/=' ') "This is a sentence"   
"This"
```

dropWhile 一旦限制条件返回False，它就返回List的余下部分

```haskell
ghci> dropWhile (/=' ') "This is a sentence"   
" is a sentence"   
ghci> dropWhile (<3) [1,2,2,2,3,4,5,4,3,2,1]   
[3,4,5,4,3,2,1]
```

sort可以排序一个List

```haskell
ghci> sort [8,5,3,2,1,6,4,2]   
[1,2,2,3,4,5,6,8]   
ghci> sort "This will be sorted soon"   
" Tbdeehiillnooorssstw"
```

partition取一个限制条件和List作参数，返回两个List，第一个List中包含所有符合条件的元素，而第二个List中包含余下的.

```haskell
ghci> partition (`elem` ['A'..'Z']) "BOBsidneyMORGANeddy"   
("BOBMORGAN","sidneyeddy")   
ghci> partition (>3) [1,3,5,6,3,2,1,0,3,7]   
([5,6,7],[1,3,3,2,1,0,3])
```

# Data.Char

有很多判定函数 和转换hanshu

# Data.Map

fromList取一个关联列表，返回一个与之等价的map。

```
ghci> Map.fromList [("betty","555-2938"),("bonnie","452-2928"),("lucille","205-2928")]  
fromList [("betty","555-2938"),("bonnie","452-2928"),("lucille","205-2928")]  
ghci> Map.fromList [(1,2),(3,4),(3,2),(5,5)]  
fromList [(1,2),(3,2),(5,5)]
```

insert取一个键，一个值和一个map做参数，给这个map插入新的键值对，并返回一个新的map。

```haskell
ghci> Map.empty  
fromList []  
ghci> Map.insert 3 100  
Map.empty fromList [(3,100)]  
ghci> Map.insert 5 600 (Map.insert 4 200 ( Map.insert 3 100  Map.empty))  
fromList [(3,100),(4,200),(5,600)] 
ghci> Map.insert 5 600 。Map.insert 4 200 . Map.insert 3 100 $ Map.empty  
fromList [(3,100),(4,200),(5,600)]
```

# Data.set

```haskell
ghci> let set1 = Set.fromList text1   
ghci> let set2 = Set.fromList text2   
ghci> set1   
fromList " .?AIRadefhijlmnorstuy"   
ghci> set2   
fromList " !Tabcdefghilmnorstuvwy"
```

# Record Syntax

```haskell
data Person = Person { firstName :: String
											, lastName :: String
											, age :: Int
											, height :: Float
											, phoneNumber :: String 
											, flavor :: String
											} deriving (Show)
```

