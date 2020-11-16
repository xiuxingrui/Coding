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
- 在数组的指定下标插入元素：
```java
//people为二维数组，p[1]为数组下标
List<int[]> list=new LinkedList<>();
for(int[] p:people){
    list.add(p[1],p);
}
return list.toArray(new int[list.size()][2]);
```