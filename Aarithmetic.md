## 记录剑指offer中的算法！

### 1、算法复杂度分析

所有好的算法应该具备时效高和存储低的特点。这里的「时效」是指时间效率，也就是算法的执行时间，对于同一个问题的多种不同解决算法，执行时间越短的算法效率越高，越长的效率越低；「存储」是指算法在执行的时候需要的存储空间，主要是指算法程序运行的时候所占用的内存空间。在如见硬件条件变得越来越好的情况下，更多的考虑的是时间复杂度的分析，下面是主要时间复杂度的比较图：</br>

![avatar](/image/time.jpg)

### 2、常见排序、查找算法
冒泡排序:</br>
冒泡排序是比较简单的排序，通过循环不断的比较相邻位置的大小，将大的数一个一个的浮出，放在最右端。</br>
![conv_ops](/image/maopao.gif)
```java
public int[] bubbleSort(int[] a){
        int len = a.length;
        if (len == 0){
            return a;
        }
        for (int i =0;i<len;i++){
            for (int j = 0;j<len -i-1;j++){
                if (a[j+1] < a[j]){
                    int temp = a[j+1];
                    a[j+1] = a[j];
                    a[j] = temp;
                }
            }
        }
        return a;
    }
```

选择排序:</br>
选择排序的思路是在数组里找到最小的数放第一个位置，然后再去查找第二小的数放在第二位置，依次这样，最后确定所有位置的数值。</br>
![conv_ops](/image/select.gif)
```java
public int[] selectSort(int[] a){
        int len = a.length;
        if (len == 0){
            return a;
        }
        for (int i =0;i<len - 1;i++){
            int index = i;
            for (int j = i;j < len;j++){
                if (a[j] < a[index]){
                    index = j;
                }
            }
            int temp = a[index];
            a[index] = a[i];
            a[i] = temp;
        }
        return a;
    }
```
插入排序：</br>
插入排序的思想为假定前面j项已经排列完成，然后将j+1项插入到钱j项中，最终达到排序的效果。</br>
![conv_ops](/image/insert.gif)
```java
public int[] insertSort(int[] a){
        int len = a.length;
        if (len == 0){
            return a;
        }
        for (int i = 1;i<len - 1;i++){
            for (int j = 0;j < i;j++){
                if (a[j] < a[j+1]){
                    int temp = a[j+1];
                    a[j+1] = a[i];
                    a[i] = temp;
                }
            }
        }
        return a;
    }
```

### 3、在一个二维数组中（每个一维数组的长度相同），每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。
<pre class="prettyprit lang-java">
      public boolean Find(int target, int [][] array) {
            int raw = 0;
            int con = array[raw].length - 1;
            while (raw < array.length && con > 0){
                if (array[raw][con] == target){
                    return true;
                } else if (array[raw][con] > target){
                    con --;
                } else {
                    raw ++;
                }
            }
            return false;
        }
</pre>
可以看到，上面的算法思路主要考虑的是每一行、每列都是有序的前提下，采用类似折半查找的思路进行算法设计。在最糟糕的情况下，时间复杂度为i+j，而如果采用最基本的遍历的话，时间复杂度是i*j；差距还是挺明显的。

### 4、大家都知道斐波那契数列，现在要求输入一个整数n，请你输出斐波那契数列的第n项（从0开始，第0项为0）。n<=39
其实，这是数学上的兔子问题：<br/>
在第一个月有一对刚出生的小兔子，在第二个月小兔子变成大兔子并开始怀孕，第三个月大兔子会生下一对小兔子，并且以后每个月都会生下一对小兔子。 如果每对兔子都经历这样的出生、成熟、生育的过程，并且兔子永远不死，那么兔子的总数是如何变化的?
通过规律总结，我们发现：

* 前一个月的大兔子对数就是下一个月的小兔子对数。</br>
* 前一个月的大兔子和小兔子对数的和就是下个月大兔子的对数。
* 兔子总对数的排列规则为1 1 2 3 5 8 13 21...当前n的值，是n-1、n-2两项之和。
<pre class="prettyprit lang-java">
public int Fibonacci(int n) {
        if(n==0||n==1){
            return  n;
        } else {
            int[] a = new int[n + 1];
            a[0] = 0;
            a[1] = 1;
            for(int i = 2;i < n + 1;i++){
                a[i] = a[i-1] + a[i-2];
            }
            return a[n];
        }
    }
</pre>

### 5、鸡兔同笼问题(一个笼子里只有鸡和兔子，从上看一共有35个头，从下看一共有94只脚，那么笼子里鸡兔各有多少只？)。
思路：假如我们让兔子和鸡同时抬起两只脚，那么此时地上的脚是不是就只可能是兔子的，并且此时地上的脚除以2，就是兔子的个数，从而鸡的数量也迎刃而解了！
<pre class="prettyprit lang-java">
public void checkenRabbit(int head,int foot){
    int chicken;
    int rabbit;
    rabbit = (foot - head * 2) / 2;
    chicken = (foot - rabbit * 4) / 2;
    System.out.println("chicken:" + chicken + " rabbit:" + rabbit);
}
</pre>

### 6、给定一个int数组，里面包含正、负整数，设定一个算法将正整数移动到左边，负整数移动到右边，时间复杂度尽量O(n)
```java
public int[] sort(int[] array){
       //如果数组的长度小于1，直接返回
       if (array.length <=1) {
           return array;
       }
       //折半查找对比
       int length = array.length;
       int end = length - 1;
       for (int i = 0; i < length / 2 + 1; i++) {
           if (array [i] < 0) {
               while (array [end] < 0 && i<end) {
                   end--;
               }
               //左边的负数与右边的正整数交换位置
               int temp = array [i];
               array [i] = array [end];
               array [end] = temp;
           }
       }
     return array; 
}
```

### 7、给定一个“flatten”字典对象，其键是以点分隔的，实现一个函数，将其转换为“嵌套”字典对象。
例如，{'A'：1，'B.A'：2，'B.B'：3，'CC.D.E'：4，'CC.D.F'：5}
转换为：
```java
{
  'A'：1，
  'B'：{
    'A2，
    'B'：3，
 }，
  'CC'：{
    'D'：{
      'E'：4，
      'F'：5，
   }
 }
}
```
要求保证字典中没有键是其他键的前缀。
### 8、请实现一个函数用来找出字符流中第一个只出现一次的字符。例如，当从字符流中只读出前两个字符"go"时，第一个只出现一次的字符是"g"。当从该字符流中读出前六个字符“google"时，第一个只出现一次的字符是"l"。如果当前字符流没有存在出现一次的字符，返回#字符。
```java
使用一个HashMap来统计字符出现的次数，同时用一个ArrayList来记录输入流，
每次返回第一个出现一次的字符都是在这个ArrayList（输入流）中的字符作为key去map中查找。
HashMap<Character, Integer> map=new HashMap();
    ArrayList<Character> list=new ArrayList<Character>();
    //Insert one char from stringstream
    public void Insert(char ch)
    {
        if(map.containsKey(ch)){
            map.put(ch,map.get(ch)+1);
        }else{
            map.put(ch,1);
        }
         
        list.add(ch);
    }
     
    //return the first appearence once char in current stringstream
    public char FirstAppearingOnce()
    {   char c='#';
        for(char key : list){
            if(map.get(key)==1){
                c=key;
                break;
            }
        }
         
        return c;
    }
```
### 9、请实现一个函数，将一个字符串中的每个空格替换成“%20”。例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。
```java
    public String replaceSpace(StringBuffer str) {
        String sti = str.toString();
        char[] strChar = sti.toCharArray();
        StringBuffer stb = new StringBuffer();
        for(int i=0;i<strChar.length;i++){
            if(strChar[i]==' '){
                stb.append("%20");
            }else{
                stb.append(strChar[i]);
            }
        }
        return stb.toString();
    }
```
### 10、输入一个链表，按链表值从尾到头的顺序返回一个ArrayList。
一种比较基础的办法，利用栈‘先进后出’的特性，当然也可以先按顺序存储在list里，最后反向输出。
```java
public ArrayList<Integer> printListFromTailToHead(ListNode listNode) {
        ArrayList<Integer> newList = new ArrayList<Integer>();
        Stack<Integer> stack = new Stack<Integer>();
        while(listNode != null){
            stack.push(listNode.val);
            listNode = listNode.next;
        }
        
        while(!stack.empty()){
            newList.add(stack.peek());
            stack.pop();
        }
        return newList;
    }
```
同时，采用递归，让代码更简洁：
```java
public class Solution {
    ArrayList<Integer> newList = new ArrayList<Integer>();
    public ArrayList<Integer> printListFromTailToHead(ListNode listNode) {
        if(listNode != null){
            printListFromTailToHead(listNode.next);
            newList.add(listNode.val);
        }
        return newList;
    }
}
```
### 11、用两个栈来实现一个队列，完成队列的Push和Pop操作。 队列中的元素为int类型。
```java
    Stack<Integer> stack1 = new Stack<Integer>();
    Stack<Integer> stack2 = new Stack<Integer>();
    
    public void push(int node) {
        stack1.push(new Integer(node));
    }
    
    public int pop() {
        if(stack2.empty()){
        while(!stack1.empty()){
            stack2.push(stack1.pop());
        }
        }
        return stack2.pop().intValue();
    }
```
### 12、输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如输入前序遍历序列{1,2,4,7,3,5,6,8}和中序遍历序列{4,7,2,1,5,3,8,6}，则重建二叉树并返回。
由前序可以知道该树的根节点（前序第一个节点），通过根节点在中序的位置，划分出根节点的左子树与右子树；然后采用递归，分别得到左右树。
```java
    public TreeNode reConstructBinaryTree(int [] pre,int [] in) {
        if(pre.length == 0||in.length == 0){
            return null;
        }
        TreeNode node = new TreeNode(pre[0]);
        for(int i = 0; i < in.length; i++){
            if(pre[0] == in[i]){
                node.left = reConstructBinaryTree(Arrays.copyOfRange(pre, 1, i+1), Arrays.copyOfRange(in, 0, i));
                node.right = reConstructBinaryTree(Arrays.copyOfRange(pre, i+1, pre.length), Arrays.copyOfRange(in, i+1,in.length));
            }
        }
        return node;
    }
    
    //Arrays.copyOfRange(array, i, j)
    //截取array的第1位到j-1位并返回
```
