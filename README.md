* # 数据结构相关
	* ## [链表](./链表)

	* ## 哈希表





## 栈和队列
### 有效的括号<br>
[leetcode:20---valid parentheses](https://leetcode-cn.com/problems/valid-parentheses/description/)<br>
	`题目：定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。
	有效字符串需满足：
	左括号必须用相同类型的右括号闭合。
	左括号必须以正确的顺序闭合`<br>
	我的解法<br>
	利用栈类去计算，左括号是一类，最先出现的右括号必须要和最后一个左括号配对。当用到最后的元素时一般考虑用栈来实现。<br>
```
public static boolean isValid(String s) {
		Stack<Character> stack = new Stack<Character>();
        for (Character c : s.toCharArray()) {
	        if ("({[".contains(String.valueOf(c))) {
	                stack.push(c);
	        } 
	        else {
	            if (!stack.isEmpty() && is_valid(stack.peek(), c)) {
	                   stack.pop();
	            }
	            else {
	                   return false;
	            }
	       }
       }
       return stack.isEmpty();
    }

    
    
private static boolean is_valid(char c1, char c2) {//找到括号的对应情况
    return (c1 == '(' && c2 == ')') || (c1 == '{' && c2 == '}')
        || (c1 == '[' && c2 == ']');
}
```
### 最小栈<br>
[leetcode:155--min stack](https://leetcode-cn.com/problems/min-stack/description/)<br>
	`题目：设计一个支持 push，pop，top 操作，并能在常数时间内检索到最小元素的栈。
	push(x) -- 将元素 x 推入栈中。
	pop() -- 删除栈顶的元素。
	top() -- 获取栈顶元素。
	getMin() -- 检索栈中的最小元素。`
	
	主要是实现栈的基本功能，这里用数组进行实现，重点是设置一个`栈顶指针`来表示栈顶元素的位置，pop后只需要把指针后移一个位置即可不需要删除元素
```
class MinStack {

    /** initialize your data structure here. */
	int default_capacity=2;
	int top=-1;//定义栈顶指针
	int[] s=new int[default_capacity];
	 public MinStack() {
	    }
       public boolean Empty(){
    	   return top==-1;
       }
	    public void push(int x) {
	        if(top==s.length-1){//当容量满时要进行扩容
	        	int[] s1=new int[s.length*2];
	        	System.arraycopy(s, 0, s1, 0, s.length);//将小的数组中的值放到一个更大的数组中
	        	s=s1;
	        }
	        s[++top]=x;
	    }
	    
	    public void pop() {
	        if(this.Empty())
	        	System.out.println("出现错误");
	        top--;//出栈只是栈顶指针退一格，原来的元素位置没有变
	    }
	    
	    public int top() {//返回栈顶元素
	    	if(this.Empty())
	    		System.out.println("空的栈");
	    	System.out.println(s[top]);
	        return s[top];
	    }
	    
	    public int getMin() {
	    	int min=s[0];
	        for(int i=1;i<=top;i++){
	        	if(s[i]<min)
	        		min=s[i];
	        }
	        System.out.println(min);
	        return min;
	    }
}
```
### 棒球比赛<br>
[leetcode:682---baseballGame](https://leetcode-cn.com/problems/baseball-game/description/)<br>
	`题目：现在是棒球比赛记录员。<br>
	给定一个字符串列表，每个字符串可以是以下四种类型之一：<br>
	1.整数（一轮的得分）：直接表示您在本轮中获得的积分数。<br>
	2. "+"（一轮的得分）：表示本轮获得的得分是前两轮有效 回合得分的总和。<br>
	3. "D"（一轮的得分）：表示本轮获得的得分是前一轮有效 回合得分的两倍。<br>
	4. "C"（一个操作，这不是一个回合的分数）：表示您获得的最后一个有效 回合的分数是无效的，应该被移除<br>
	你需要返回你在所有回合中得分的总和。`
	
	可以看到明显与最后处理的结果是有关的，于是考虑用栈来实现
	一个思路：想要把栈顶的两个元素相加并把结果压栈，但是又不能弹出那两个元素，可以采用pop第一个元素后peek第二个元素，然后再push第一个元素，再把和push
```
public static int calPoints(String[] ops) {
    	int a,b,c,d;
    	int sum=0;
    	Stack<String> stack=new Stack<>();
    	for(int i=0;i<ops.length;i++){
    		switch (ops[i]) {
			case "+":{
				a=Integer.parseInt(stack.pop());//想要把栈顶的两个元素相加并把结果压栈，但是又不能弹出那两个元素，可以采用pop第一个元素后peek第二个元素，然后再push第一个元素，再把和push
				b=Integer.parseInt(stack.peek());
				c=a+b;
				stack.push(String.valueOf(a));
				stack.push(String.valueOf(c));
				sum+=c;
				break;
			}

			case "D":{
				d=Integer.parseInt(stack.peek())*2;
				stack.push(String.valueOf(d));
				sum+=d;
				break;
			}

			case "C":{
				sum-=Integer.parseInt(stack.pop());
				break;
			}

			default:{
				stack.push(ops[i]);
				sum+=Integer.parseInt(ops[i]);
				break;
			}

			}
    	}
       
        return sum;
    }
```
### 用队列实现栈<br>
[leetcode:225. Implement Stack using Queues](https://leetcode-cn.com/problems/implement-stack-using-queues/description/)<br>
	`题目：用队列实现栈的基本操作，如push，pop,top,is_empty等`<br>
	因为在java中Queue是一个接口无法直接实例化，于是采用实现他的类ArrayDeque，这是一个双端队列，实则便可以当作栈来用，所以这里投机取巧了。实际		上，如果要实现push操作，必须要在push方法中再定义一个队列temp来存放加入新元素之前的元素，然后把老的队列所有元素弹出后再添加新的元素进去，最后	    把temp中存放的元素再push进去，这样新加入的元素便放在了队头。
```
class MyStack {

    /** Initialize your data structure here. */
    ArrayDeque<Integer> arrayDeque;
    public MyStack() { 
    	arrayDeque=new ArrayDeque<>();//双端队列,默认大小为16
    }
    
    /** Push element x onto stack. */
    public void push(int x) {
        arrayDeque.addFirst(x);
    }
    
    /** Removes the element on top of the stack and returns that element. */
    public int pop() {
        return arrayDeque.poll();
    }
    
    /** Get the top element. */
    public int top() {
        return arrayDeque.peek();
    }
    
    /** Returns whether the stack is empty. */
    public boolean empty() {
        return arrayDeque.isEmpty();
    }
}
```
### 用栈实现队列<br>
[leetcode:mplement Queue using Stacks](https://leetcode-cn.com/problems/implement-queue-using-stacks/description/)<br>
	和用队列实现栈相同的思路，仍旧是实现基本的功能，push操作需要定义一个中间的栈对象<br>
```
class MyQueue {

    /** Initialize your data structure here. */
	Stack<Integer> stack=new Stack<>();
    public MyQueue() {
        
    }
    
    /** Push element x to the back of queue. */
    public void push(int x) {
    	Stack<Integer> temp=new Stack<>();
    	while(!stack.isEmpty()){
    		temp.push(stack.pop());//讲原栈的元素一个个的放入到中间栈中
    	}
    	stack.push(x);
    	while(!temp.isEmpty()){
    		stack.push(temp.pop());
    	}
    }
    
    /** Removes the element from in front of queue and returns that element. */
    public int pop() {
        return stack.pop();
    }
    
    /** Get the front element. */
    public int peek() {
        return stack.peek();
    }
    
    /** Returns whether the queue is empty. */
    public boolean empty() {
        return stack.isEmpty();
    }
}
```
### 比较含退格的字符串
[leetcode:844. Backspace String Compare](https://leetcode-cn.com/problems/backspace-string-compare/description/)<br>
	`题目：给定 S 和 T 两个字符串，当它们分别被输入到空白的文本编辑器后，判断二者是否相等，并返回结果。 # 代表退格字符。`<br>
	考虑把两个字符串转变为字符数组然后存放在栈中，一个退格键#则将栈顶元素出栈，最后比较两个栈的元素即可<br>
```
    public boolean backspaceCompare(String S, String T) {
        char[] s=S.toCharArray();
    	char[] t=T.toCharArray();
    	Stack<Character> s1=new Stack<>();
    	Stack<Character> s2=new Stack<>();
    	int cout=0;
    	for(char a:s){
    		if(a=='#'){
    			if(!s1.isEmpty())
    				s1.pop();
    		}
    			
    		else s1.push(a);
    	}
    	for(char b:t){
    		if(b=='#'){
    			if(!s2.isEmpty())
    				s2.pop();
    		}
    		else s2.push(b);
    	}
    	int len=s1.size();
    	if(s1.size()!=s2.size())
    		return false;
    	while(!s1.isEmpty()){
    		if(s1.pop()==s2.pop())
    			cout++;
    	}
		return cout==len;
    }
```
## 树
### 相同的树<br>
[leetcode:100. Same Tree](https://leetcode-cn.com/problems/same-tree/description/)<br>
	`题目：给定两个二叉树，编写一个函数来检验它们是否相同。
	如果两个树在结构上相同，并且节点具有相同的值，则认为它们是相同的。`
	采用递归的方法来写，另外用传入根节点来表示一个二叉树<br>
```
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public boolean isSameTree(TreeNode p, TreeNode q) {
        			if(p!=null&&q!=null){
				if(p.val==q.val){
					return isSameTree(p.left, q.left)&&isSameTree(p.right, q.right);
				}
				else return false;
			}
			else if(p==null&&q==null)
				return true;
			else return false;
    }
}
```
### 对称二叉树	(没做出来)
	`题目：给定一个二叉树，检查它是否是镜像对称的。`
	使用BFS对整个二叉树进行层级遍历。在每层中使用Stack判断是否对称。<br>
```
public boolean isSymmetric(TreeNode root) {
        // Write your code here
        Queue<TreeNode> queue = new LinkedList<>();
        Stack<TreeNode> stack = new Stack<>();
        queue.offer(root);
        TreeNode current;
        int size;
        boolean isSymmetric;
        while (!queue.isEmpty()){
            size = queue.size();
            for (int i = 0; i < size; i ++){
                current = queue.poll();
                if (current != null){
                    queue.offer(current.left);
                    queue.offer(current.right);
                }
                if (size == 1){
                    break;
                }
                if (size % 2 != 0){
                    return false;
                }
                if (i < size / 2){
                    stack.push(current);
                } else {
                    isSymmetric = current == null 
                        ? current == stack.pop()
                        : current.val == stack.pop().val;
                    if (!isSymmetric){
                        return false;
                    }
                }
            }
        }
        return true;
    }
```
## 哈希表
### 两数之和<br>
[Leetcode : 1. Two Sum (Easy)](https://leetcode-cn.com/problems/two-sum/description/)  
`题目：给定一个整数数组和一个目标值，找出数组中和为目标值的两个数。你可以假设每个输入只对应一种答案，且同样的元素不能被重复利用
		input: nums = [2, 7, 11, 15], target = 9 
		output: [0, 1]`

我的解法<br>
```java
    public int[] twoSum(int[] nums, int target) {
        int[] index=new int[2];
		for(int i=0;i<nums.length;i++){
			for(int j=i+1;j<nums.length;j++){
				if(nums[i]+nums[j]==target){
	               index[0]=i;
	               index[1]=j;
	               return index;
				}
			}
		}
        return index;
    }
```
	复杂度太高，是O（n^2）应该避免使用嵌套循环，好的的方法可以采用`hashmap(利用两次哈希表)`，宁可使用两次for循环也不要叠加使用！<br>
如：
```java
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        map.put(nums[i], i);
    }
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        if (map.containsKey(complement) && map.get(complement) != i) {
            return new int[] { i, map.get(complement) };
        }
    }
    throw new IllegalArgumentException("No two sum solution");
}
```
