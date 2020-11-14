# Tips

- 重写`Comparator`时，`compare`函数中的参数要包装类型，比如是`(Integer a,Integer b)`而非`(int a,int b)`;
- 重写`Comparator`的正则表达式版本：
```java
Collections.sort(list, (x, y) ->x-y);
```
```java
Collections.sort(list, (x, y) -> {
    return x - y;
});
```