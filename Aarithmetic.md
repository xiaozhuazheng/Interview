## 记录算法！

- [算法复杂度](#算法复杂度)
- [排序](#排序)
  - [冒泡排序](#冒泡排序)
  - [选择排序](#选择排序)
  - [插入排序](#插入排序)
  - [快速排序](#快速排序)
  - [堆排序](#堆排序)
- [查找](#查找)
  - [二分查找](#二分查找)
- [递归](#递归)
- [数组](#数组)
- [链表](#链表)
- [字符串](#字符串)
- [队列](#队列)
- [栈](#栈)
- [二叉树](#二叉树)
- [堆](#堆)
- [图](#图)
- [逻辑思维](#逻辑思维)
  - [鸡兔同笼问题](#鸡兔同笼问题)
  - [洗牌算法](#洗牌算法)
  - [瓶子倒水问题](#瓶子倒水问题)

### 算法复杂度

所有好的算法应该具备时效高和存储低的特点。这里的「时效」是指时间效率，也就是算法的执行时间，对于同一个问题的多种不同解决算法，执行时间越短的算法效率越高，越长的效率越低；「存储」是指算法在执行的时候需要的存储空间，主要是指算法程序运行的时候所占用的内存空间。在如见硬件条件变得越来越好的情况下，更多的考虑的是时间复杂度的分析，下面是主要时间复杂度的比较图：</br>

![avatar](/image/time.jpg)

### 排序
##### 冒泡排序:</br>
冒泡排序是比较简单的排序，通过循环不断的比较相邻位置的大小，将大的数一个一个的浮出，放在最右端。</br>
![conv_ops](/image/maopao.gif)
```java
public int[] bubbleSort(int[] a){
        int len = a.length;
        if (len == 0){
            return a;
        }
        for (int i =0;i<len - 1;i++){
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

##### 选择排序:</br>
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
##### 插入排序：</br>
插入排序的思想为假定前面j项已经排列完成。</br>
![conv_ops](/image/insert.gif)
```java
public int[] insertionSort(int[] nums){
        for (int i = 1;i < nums.length;i++){
            int j;
            int temp = nums[i];
            //j前面的都已经是有序，位置j的值寻找其正确位置的过程中，将大于j的元素不断往右移最后留出来的位置就是j的正确位置；
            for (j = i;j > 0 && nums[j] < nums[j -1];j--){
                nums[j] = nums[j -1];
            }
            nums[j] = temp;
        }
        return nums;
    }
```

##### 快速排序：</br>
思路：首先，取一个值，这个值可以是数组的第一个元素，也可以是数组最中间的元素，第一遍将比该值大的元素移到最右边，比该值小的元素移到左边，后面递归左右的数组！
```java
public int[] quikSort(int[] nums,int start,int end){
        int left = 0;
        int right = end;
        
        int first = nums[0];
        //左右两个指针分别指向开始和结尾
        while (left < right){
            //从最右边开始，指针想左移，知道找到比first小的元素
            while (left < right && nums[right] >= first){
                right --;
            }
            //将找到的元素赋值给left，此时right的位置空出来
            nums[left] = nums[right];
            
            //从最左边开始，指针想右移，知道找到比first大的元素
            while (left < right && nums[left] <= first){
                left ++;
            }
            //将该找到比first大的元素赋值给上面空出来的right的位置
            nums[right] = nums[left];
        }

        nums[left] = first;
        if (start < left)quikSort(nums,start,left);
        if (right < end)quikSort(nums,right+1,end);

        return nums;
    }
```
##### 堆排序：</br>

### 查找
##### 二分查找

```java
  private static int halfFound(int[] a,int target){
       int high = a.length - 1;
       int low = 0;
        while (low <= high) {
            int mid = (low + high) / 2;
            if (a[mid] == target) {
               return mid;
            } else if (a[mid] < target) {
               low = mid + 1;
            } else{
               high = mid - 1;
            }
            
        }
       return -1;
    }
```

### 递归
##### 斐波那契数列，现在要求输入一个整数n，请你输出斐波那契数列的第n项（从0开始，第0项为0）。n<=39
其实，这是数学上的兔子问题：<br/>
在第一个月有一对刚出生的小兔子，在第二个月小兔子变成大兔子并开始怀孕，第三个月大兔子会生下一对小兔子，并且以后每个月都会生下一对小兔子。 如果每对兔子都经历这样的出生、成熟、生育的过程，并且兔子永远不死，那么兔子的总数是如何变化的?
通过规律总结，我们发现：

* 前一个月的大兔子对数就是下一个月的小兔子对数。</br>
* 前一个月的大兔子和小兔子对数的和就是下个月大兔子的对数。
* 兔子总对数的排列规则为1 1 2 3 5 8 13 21...当前n的值，是n-1、n-2两项之和。

通过规律，我们首先想到的是递归方法：</br>
```java
public int Fibonacci(int n) {
    if(n<=1) return n;
    else return Fibonacci(n-1)+Fibonacci(n-2);
}
```
时间复杂度：O(2^n)</br>
空间复杂度：O(1)</br>
采用递归的话，会出现很多重复的运算，造成栈溢出问题，时间复杂度上也不允许,因此换成数组实现：

```java
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
```
时间复杂度：O(n)</br>
空间复杂度：O(n)</br>
这样以来，在时间复杂度上优化了，但是数组比较耗内存呀；但是会发现我们可以通过两个变量就解决问题了：

```java
public int Fibonacci(int n) {
        if(n <2){
            return n;
        } else{
            int sum = 1;
            int one = 0;
            for(int i = 2;i < n+1;i++){
                sum += one;
                one = sum - one;
            }
            return sum;
        }
    }
```

### 数组
##### 1、在一个二维数组中（每个一维数组的长度相同），每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。
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

##### 2、给定一个int数组，里面包含正、负整数，设定一个算法将正整数移动到左边，负整数移动到右边，时间复杂度尽量O(n)
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

##### 3、两个有序整数数组 nums1 和 nums2，请你将 nums2 合并到 nums1 中，使 nums1 成为一个有序数组
思路：主要考虑到的前提是，两个数组都是有序的，所以从两个数组的最右边开始比较，将最大的值放到右边，比较完成后如果nums2还有剩余，则全部放到数组最左边。
```java
public void merge(int[] nums1, int m, int[] nums2, int n) {
        int i = m - 1, j = n - 1, index = m + n - 1;
        while (i >= 0 && j >= 0)
            nums1[index--] = nums1[i] > nums2[j] ? nums1[i--] : nums2[j--];
        while (j >= 0)
            nums1[index--] = nums2[j--];
    }
```

##### 4、给你一个有序数组 nums ，请你 原地 删除重复出现的元素，使每个元素 只出现一次 ，返回删除后数组的新长度。
思路：快慢指针！
```java
public int removeDuplicates(int[] nums) {
        int left = 0;
        int right = left + 1;
        while (right < nums.length){
            if (nums[left] != nums[right]){
                nums[++left] = nums[right];
            }
            right++;
        }
        return ++left;
    }
```

### 链表
##### 1、输入一个链表，按链表值从尾到头的顺序返回一个ArrayList。
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
##### 2、如何知道一个链表里是否有环？
快慢指针：采用两个指针，同时从head触发，p指针每次前进一步，q指针每次前进两步，如果纯在环路，那么两者在某一时刻一定会相遇。
```java
public boolean hasRing(Node head){
        Node p = head;
        Node q = head;
        
        while (p != null && q != null) {            
            p = p.next;
            q = q.next;
            
            if (q.next == null) {
                break;
            }
            
            q = q.next;
            if (p != null && p == q) {
                return true;
            }
            
        }
        return false;
    }
    
    public class Node{
        int value;
        Node next;
    }
    
```

##### 3、对两个有序链表进行合并？
设定一个哨兵节点 prehead ，这可以在最后让我们比较容易地返回合并后的链表。我们维护一个 prev 指针，我们需要做的是调整它的 next 指针。然后，我们重复以下过程，直到 l1 或者 l2 指向了 null ：如果 l1 当前节点的值小于等于 l2 ，我们就把 l1 当前的节点接在 prev 节点的后面同时将 l1 指针往后移一位。否则，我们对 l2 做同样的操作。不管我们将哪一个元素接在了后面，我们都需要把 prev 向后移一位。在循环终止的时候， l1 和 l2 至多有一个是非空的，将非空的添加到链表后面。
```java
 public ListNode Merge(ListNode list1,ListNode list2) {
        ListNode prehead = new ListNode(-1);

        ListNode prev = prehead;
        while (l1 != null && l2 != null) {
            if (l1.val <= l2.val) {
                prev.next = l1;
                l1 = l1.next;
            } else {
                prev.next = l2;
                l2 = l2.next;
            }
            prev = prev.next;
        }

        // 合并后 l1 和 l2 最多只有一个还未被合并完，我们直接将链表末尾指向未合并完的链表即可
        prev.next = l1 == null ? l2 : l1;

        return prehead.next;
    }
```

##### 4、输入一个链表，反转链表后，输出新链表的表头。
采用两个指针，一个指向当前head，一个指向head前一个数据。然后不断的将两个指针同时向后移动，直到最后一位。
```java
public ListNode ReverseList(ListNode head){
        if(head == null){
            return null;
        }
        
        ListNode pre;
        ListNode current = head;
        while(current != null){
            ListNode temp = current.next;
            current.next = pre;
            pre = current;
            current = temp;
        }
        return pre；//这里要返回pre，而不是current
       
    }
```

### 字符串
##### 请实现一个函数用来找出字符流中第一个只出现一次的字符。例如，当从字符流中只读出前两个字符"go"时，第一个只出现一次的字符是"g"。当从该字符流中读出前六个字符“google"时，第一个只出现一次的字符是"l"。如果当前字符流没有存在出现一次的字符，返回#字符。
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

##### 实现一个函数，将一个字符串中的每个空格替换成“%20”。例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。
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

##### 字符串反转（采用双指针）
```java
public void reverseString(char[] s) {
        int len = s.length;
        for (int left = 0,right = len -1 ;left <= right;++left,--right){
            char temp = s[left];
            s[left] = s[right];
            s[right] = temp;
        }
    }
```

##### 实现一个四则运算

### 队列
##### 用两个栈来实现一个队列，完成队列的Push和Pop操作。 队列中的元素为int类型。
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

### 栈

1、给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串 s ，判断字符串是否有效。
思路：通过栈的先进后出的逻辑！
```java
public boolean isValid(String s) {
        Stack<Character> stack = new Stack<>();
        char[] chars = s.toCharArray();
        for (char c : chars){
            if (c == '('){
                stack.push(')');
            } else if (c == '['){
                stack.push(']');
            } else if (c == '{'){
                stack.push('}');
            } else if (stack.isEmpty() || stack.pop() != c){
                return false;
            }
        }
        return stack.isEmpty();
    }
```

### 二叉树
##### 1、二叉树的创建与遍历
```java
     //TreeNode结构
     static class TreeNode{
        private String data;
        private TreeNode left;
        private TreeNode right;

        public TreeNode(String data){
            this.data = data;
        }
     }
              
     String[] arr = {"1","2","3","#","#","4","#","#","5","#","6"};
     LinkedList<String> in = new LinkedList<>();
     for (String item : arr){
        in.add(item);
     }
     createBinaryTree(in)；
              
     /**
     * 递归三要素：
     * 1、特殊情况判断
     * 2、递归循环操作
     * 3、循环退出条件
     */
    private static TreeNode createBinaryTree(LinkedList<String> inputList) {
        //1、特殊情况判断
        if (inputList == null || inputList.isEmpty()) {
            return null;
        }


        String value = inputList.remove(0);

        //2、递归循环操作
        if (value == "#"){
            return null;
        }

        //2、循环
        TreeNode node = new TreeNode(value);
        node.left = createBinaryTree(inputList);
        node.right = createBinaryTree(inputList);

        return node;
    }
              
    //先序
    private static void getBefor(TreeNode head){
        if (head == null) {//最简单问题
            System.out.print("#");
            return;
        }

        System.out.print(head.data + " ");
        getBefor(head.left);
        getBefor(head.right);
    }

    //中序
    private static void getMind(TreeNode head){
        if (head == null) {//最简单问题
            return;
        }

        getMind(head.left);
        System.out.print(head.data + " ");
        getMind(head.right);
    }

    //后序
    private static void getEnd(TreeNode head){
        if (head == null) {//最简单问题
            return;
        }

        getEnd(head.left);
        getEnd(head.right);
        System.out.print(head.data + " ");
    }       
```
##### 2、输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如输入前序遍历序列{1,2,4,7,3,5,6,8}和中序遍历序列{4,7,2,1,5,3,8,6}，则重建二叉树并返回。
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

### 堆

### 图

### 逻辑思维

##### 鸡兔同笼问题
一个笼子里只有鸡和兔子，从上看一共有35个头，从下看一共有94只脚，那么笼子里鸡兔各有多少只？
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

##### 洗牌算法
思路：将54张牌抽象成[0，53]的数字，放在一个数组里，那么问题就变成了如何将数组随机排列了。既然是随机，那当然就要用到随机函数了。将数组的前n个数进行随机，得到一个随机数对应在数组中的某位置的值，然后将这一位置的值与第n位置进行互换，依次这样进行，最后就会得到一个随机数组。
```java
private static int[] poker(int[] a){
        Random random = new Random();
        for (int i = a.length - 1; i > 0; i--) {
            int j = random.nextInt(i);
            int temp = a[j];
            a[j] = a[i];
            a[i] = temp; 
        }
        return a;
    }
```

##### 瓶子倒水问题
5L,4L的瓶子，怎么得到3L的水
1、首先将装满水的5L瓶子向4L的瓶子倒水，直到倒满，那么5L的杯子还剩1L，将4L瓶子里的水倒掉，再将5L瓶子里剩下的1L水倒进4L的瓶子里；<br/>
2、其次将5L的瓶子装满水，并倒进已经有1L水的4L瓶子里，知道4L的瓶子装满，那么5L的瓶子还剩2L水，将4L瓶子里的水倒掉，将剩下的2L水倒进4L的瓶子；<br/>
3、最后，将装满水的5L瓶子向已经有2L水的4L瓶子里倒水知道倒满，那么5L的瓶子里就剩下3L水了。
