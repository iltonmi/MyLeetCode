#  324. Wiggle Sort II

 https://leetcode.com/problems/wiggle-sort-ii/discuss/77682/Step-by-step-explanation-of-index-mapping-in-Java 



> This is to explain why mapped index formula is (1 + 2*index) % (n | 1)
>
> 
>
> Notice that by placing the median in it's place in the array we divided the array in 3 chunks: all numbers less than median are in one side, all numbers larger than median are on the other side, median is in the dead center of the array.
>
> 
>
> [We want to place any a group of numbers (larger than median) in odd slots, and another group of numbers (smaller than median) in even slots](https://discuss.leetcode.com/topic/32861/3-lines-python-with-explanation-proof). So all numbers on left of the median < n / 2 should be in odd slots, all numbers on right of the median > n / 2 should go into even slots (remember that median is its correct place at n / 2)
>
> 
>
> PS: I'm ignoring the discussion of odd/even array length for simplicity.
>
> 
>
> So let's think about the first group in the odd slots, **all numbers is the left side of the array should go into these odd slots. What's the formula for it? Naturally it would be:**
> **(1 + 2 x index) % n**
>
> 
>
> **All these indexes are less than n / 2 so multiplying by 2 and add 1 (to make them go to odd place) and then mod by n will always guarantee that they are less than n.**
>
> 
>
> Original Index => Mapped Index
> 0 => (1 + 2 x 0) % 6 = 1 % 6 = 1
> 1 => (1 + 2 x 1) % 6 = 3 % 6 = 3
> 2 => (1 + 2 x 2) % 6 = 5 % 6 = 5
>
> 
>
> These are what's less than median, if we continue this with **indexes 3, 4, 5 we will cycle again**:
> 3 => (1 + 2 x 3) % 6 = 7 % 6 = 1
> 4 => (1 + 2 x 4) % 6 = 9 % 6 = 3
> 5 => (1 + 2 x 5) % 6 = 11 % 6 = 5
>
> 
>
> and we don't want that, so **for indexes larger than n/2 we want them to be even**, (n|1) does that exactly. **What n|1 does it that it gets the next odd number to n if it was even**
> if n = 6 for example 110 | 1 = 111 = 7
> if n = 7 for example 111 | 1 = 111 = 7
>
> 
>
> and this is what we want, **instead of cycling the odd numbers again we want them to be even, and odd % odd number is even** so updating the formula to :
> (1 + 2*index) % (n | 1)
>
> 
>
> Then we have:
> 3 => (1 + 2 x 3) % 7 = 7 % 7 = 0
> 4 => (1 + 2 x 4) % 7 = 9 % 7 = 2
> 5 => (1 + 2 x 5) % 7 = 11 % 7 = 4

对上述总结：

1. 将数组的左半部分映射到奇数位置的公式：i -> (1 + 2 * index) % n，n是数组大小
2. 对于数组的右半部分运用此公式：i -> (1 + 2 * index) % n，进入了循环，将数组的右半部分也映射到奇数位置。
3. 若n是偶数，则（n | 1）是n的下一个奇数，所以对于数组的右半部分，应该使用公式：i -> (1 + 2 * index) % （n | 1），而这并不会影响到数组的右半部分，因为它们小于n，取余操作不起实际作用。
4. 所以总的映射公式就是：i -> (1 + 2 * index) % (n | 1)



 https://leetcode.com/problems/wiggle-sort-ii/discuss/77684/Summary-of-the-various-solutions-to-Wiggle-Sort-for-your-reference 



