
---

title: java经验

urlname: ec4gpt

date: 2020-02-11 11:29:58 +0800

tags: []

---
<a name="7Ght7"></a>
# java 经验
<a name="88wtx"></a>
## what different between Boolean.TRUE and true
<a name="Cu6Cf"></a>
### 参考链接
[What the difference between Boolean.TRUE and true](https://stackoverflow.com/questions/31775618/what-the-difference-between-boolean-true-and-true)
<a name="RDmHr"></a>
### 说明
> 
> Firstly, I have

> ```
private Map<String, Boolean> mem = new HashMap<String, Boolean>();
```

> And then:

> ```
if (wordDict.contains(s) ||  Boolean.TRUE==mem.get(s)) {
        return true;
    }
```

> why can't I use "mem.get(s)==true" in the if statement. There will be a error "Line 6: java.lang.NullPointerException"

> I think I still cannot understant wrapper class well. Please give me some guidance. Thank you!



