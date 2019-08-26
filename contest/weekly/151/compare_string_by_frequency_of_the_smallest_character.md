# 比较字符串最小字母出现频次

> 思路：暴力计算，没啥技巧。

```python
class Solution:
    def numSmallerByFrequency(self, queries: List[str], words: List[str]) -> List[int]:
        fws = [self.getf(w) for w in words]
        res = []
        for q in queries:
            res.append(0)
            fq = self.getf(q)
            for fw in fws:
                if fq < fw:
                    res[-1] += 1
        return res
                    
    def getf(self,s):
        charCntDict = collections.defaultdict(int)
        for c in s:
            charCntDict[c] += 1
        minChar = "z"
        minCnt = 100
        for k,v in charCntDict.items():
            if k <= minChar:
                minChar = k
                minCnt = v
        return minCnt
```

> 大神思路：用 collections.Counter 统计字母频率，然后对统计出的 words 频率进行排序，最后利用 bisect.bisect 二分查找到每一个 query 在排序后 words 频率中的位置，进而计算出结果