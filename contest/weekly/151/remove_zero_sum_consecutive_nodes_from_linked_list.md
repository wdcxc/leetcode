# 从链表中删去总和值为零的连续节点

> 思路：
> 1. 计算出当前链表的累加和数组 preSum = [0, a[0], a[0]+a[1], a[0]+a[1]+a[2], ...]，preSum 数组长度比链表长度多1，preSum[0] = 0，这是为了方便处理 a[0]。
> 2. 双重循环，判断是否存在 preSum[i] - preSum[j] == 0 ( j < i )，如果存在，则说明 a[j+1] + a[j+2] + ... + a[i] == 0，删之，然后重回第1步

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def removeZeroSumSublists(self, head: ListNode) -> ListNode:
        
        dummyNode = ListNode(0)
        dummyNode.next = head
        
        isDel = True
        while isDel:
            isDel = False
            # preSum 节点第一个值存累加和，第二个值存节点
            preSum = [(0,dummyNode)] 
            curNode = dummyNode
            # 第1步，计算累加和数组
            while curNode.next:
                curNode = curNode.next
                preSum.append((curNode.val+preSum[-1][0],curNode))
            # 第2步，双重循环判断是否存在和为零的子段
            for i in range(1, len(preSum)):
                # 上个i删了一段，需要重新计算累加和数组
                if isDel:
                    break
                for j in range(i):
                    if preSum[i][0] - preSum[j][0] == 0:
                        isDel = True
                        preNode = preSum[j][1]
                        curNode = preNode.next
                        endNode = preSum[i][1].next
                        while not curNode is endNode:
                            preNode.next = curNode.next
                            del curNode
                            curNode = preNode.next
                        break
        
        return dummyNode.next
```

> 大神思路：核心是维持一个累加和不为零的数组。使用双重循环，外层每次添加一个新值到累加和数组中，然后内层循环从后向前判断加入新值后累加和数组中是否有子段为零，如果有，把子段包含的值全部从累加和数组弹出，最后使用累加和数组构建结果链表。