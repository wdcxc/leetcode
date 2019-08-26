# 查询无效交易 

> 思路：开多重嵌套字典存储信息，dict[name][city][time][amount]，最里层 amount 存储的是该交易是否已经被放到结果集里面，防止重复存放。当然代码还可以优化，比如把结果集开成 set 类型，就可以减少嵌套字典层数。

```python
class Solution:
    def invalidTransactions(self, transactions: List[str]) -> List[str]:
        res = []
        trans = {}
        
        for tran in transactions:
            name, time, amount, city = tran.split(",")
            if not name in trans:
                trans[name] = {}
            if not city in trans[name]:
                trans[name][city] = {}
            if not int(time) in trans[name][city]:
                trans[name][city][int(time)] = {}
                
            illegal = False
            
            # 判断是否违反第一个条件，交易金额超过1000
            if int(amount) > 1000:
                illegal = True
                
            # 判断是否违反第二个条件，当前交易是否和已有交易在限期内同名不同城
            otherCitys = [c for c in trans[name] if c != city]
            for c in otherCitys:
                for t in trans[name][c]:
                    if abs(t-int(time)) <= 60:
                        for a,visited in trans[name][c][t].items():
                            illegal = True
                            if not visited:
                                trans[name][c][t][a] = True
                                res.append(name+","+str(t)+","+a+","+c)
            if illegal:
                res.append(tran)
                trans[name][city][int(time)][amount] = True
            else:
                trans[name][city][int(time)][amount] = False
                
        return res
```

> 大神思路：后面参考了其他大神的做法，其实只要存个嵌套 list 就可以了，外层存每个交易，内层存每个交易被分割后的字段，然后双重循环判断是否违反规则。