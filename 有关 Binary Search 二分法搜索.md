### 有关 Binary Search 二分法搜索

这学期遇到了这样一种问题，题目的名称叫 Equal Circle Segments，输入一个列表，列表中的数字是每个圆的半径，同时指定需要从这些圆中切割出Segment的数量，同时需要保证每一个Segment面积都相同（面积相同即可，形状不一定需要相同），需要我们返回最大可能的Segment的面积（小数精度要求四位小数）。

不管给的圆再多，Segment的上限面积，就是最大的大圆的面积，因此可以考虑用二分法搜索，找出符合条件的最大的Segment的面积。

```python
import math

def largestSegemnt(radius, segement):

    areas = [math.pow(r,2)*math.pi for r in radius]
    # calculate the areas of each circle

    def test_segment_area(x): # x is the assumed segment area

        k = 0
        for area in areas:
            k += area // x
            # for any given area x, we try it with every circle
            # to see how many segments we could get in total
            if k >= segement:
                return True
                # the number of segments is more than the required
                # which means the given segment area, x, is too small
        return False
        # after iterating through the area list
        # the total number of segments is less than the required
        # which means the given segment area, x, is too large


    lower_bound = 0
    upper_bound = max(areas)

    while lower_bound + 1e-5 <= upper_bound: # 1e-5 = 1×10^(-5)

        x = (lower_bound+upper_bound)/2
        if test_segment_area(x): 
            # if true, the given area,x, is too small
				# we need to level up the lower bound
            lower_bound = x
        else:
            # if false, the given area,x, is too big
				# we need to shrink the upper bound
            upper_bound = x
    # until the difference between two bounds is less than 1×10^(-5)
    return str(round(x,4))

```

