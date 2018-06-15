# 目录
* 数据结构相关
	*  [栈和队列](#栈和队列)
	*  [树](#树)
	*  [哈希表](#哈希表)





## 栈和队列
### 有效的括号<br>
[leetcode:20---valid parentheses](https://leetcode-cn.com/problems/valid-parentheses/description/)<br>
	`题目：定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。
	有效字符串需满足：
	左括号必须用相同类型的右括号闭合。
	左括号必须以正确的顺序闭合`<br>
	我的解法<br>
		利用栈类去计算，左括号是一类，最先出现的右括号必须要和最后一个左括号配对。当用到最后的元素时一般考虑用栈来实现。<br>
```java
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
```java
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
```java
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
```java
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
```java
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
```java
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
树
===========
### 相同的树<br>
[leetcode:100. Same Tree](https://leetcode-cn.com/problems/same-tree/description/)<br>
	`题目：给定两个二叉树，编写一个函数来检验它们是否相同。
	如果两个树在结构上相同，并且节点具有相同的值，则认为它们是相同的。`
	采用递归的方法来写，另外用传入根节点来表示一个二叉树<br>
```java
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
```java
### 对称二叉树	
	`题目：给定一个二叉树，检查它是否是镜像对称的。`
*解法一：采用迭代的思想。对整个二叉树进行层级遍历,将每一层的元素放到队列中，并用栈保存左子树的中的节点，每次出栈和右子树进行比较。<br>
```
public boolean isSymmetric(TreeNode root) {
		Queue<TreeNode> queue=new LinkedList<>();
		Stack<TreeNode> stack=new Stack<>();//用栈来存放坐边的元素
		TreeNode current;
		int size;
		Boolean isSymmrtric = false;;
		queue.offer(root);//当使用有容量限制的队列时，此方法通常要优于 add(E)，后者可能无法插入元素，而只是抛出一个异常。
		while(!queue.isEmpty()){
			size=queue.size();
			for(int i=0;i<size;i++){//引入此for循环为了使每次在一层中进行比较，一次队列放一行元素，在for循环运行完一次后便可以将上一行元素全部退去加入下一行元素
				current=queue.poll();
				if(current!=null){
					queue.offer(current.left);
					queue.offer(current.right);
				}
				if(size==1)
					break;//将根节点导入的情况除去
				if(size%2!=0)
					return false;
				else if(i<size/2)
					stack.push(current);
				else{
					if(current!=null&&stack.peek()!=null)
						isSymmrtric=stack.pop().val==current.val;
					else if(current==null&&stack.peek()==null)
						isSymmrtric=true;
					else if(current==null&&stack.peek()!=null||current!=null&&stack.peek()==null)
						isSymmrtric=false;
					
					if(!isSymmrtric)
						return false;//有任何一行的一个不相等则为false
				}
					
				
				
			}
			
		}
		
		return true; 
    }
```java
*解法二：采用递归的思想，较为简单。通过比较原节点是否相等以及递归表示左边节点的左子树和右边节点的右子树是否相等以及左边节点的右子树和右边节点的左子树是否相等<br>
```
public boolean isSymmetric(TreeNode root) {
		return check(root, root);
		
	}
	public boolean check(TreeNode x,TreeNode y){
		if(x==null&&y==null){
			return true;
		}
		if(x!=null&&y==null||x==null&y!=null)
			return false;
		return x.val==y.val&&check(x.left, y.right)&&check(x.right, y.left);
		
	}
```
### 二叉树的最大深度
	`题目：给定一个二叉树，找出其最大深度。
	二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。`
	因为每一个树节点的高度是它的左节点和右节点最大值+1，于是可由此进行递归。<br>
```java
public int maxDepth(TreeNode root) {
		
		if(root==null)
			return 0;
		int left=maxDepth(root.left);
		int right=maxDepth(root.right);
		return Math.max(left, right)+1;
		
    }
```
###  将有序数组转换为二叉搜索树
	`题目：将一个按照升序排列的有序数组，转换为一棵高度平衡二叉搜索树。
	本题中，一个高度平衡二叉树是指一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过 1`<br>
	
	注意一个很重要的点，数组是按升序进行排列的，而要求树的平衡的，于是根节点一定是数组中间的节点，这样大于和小于它的数正好都是一半，然后采用递归	      的方式去不断去对每一个节点采用对数组区间分段的方式进行处理，最后的树一定是左右差不多高的。偶数长时高度会差1<br>
	不需要像AVL树一样在没插入一个节点便进行平衡处理，那样显得多余。leetcode中只需要按顺序返回树的各个节点就可以（上到下左到右），不需要指明子树关系<br>
```
private TreeNode buildTree(int[] num, int start, int end) {
        if (start > end) {
            return null;
        }

        TreeNode node = new TreeNode(num[(start + end) / 2]);//因为只需要按顺序返回树的各节点就行，不需要标明左右节点或者子树的关系，于是直接不断new新的节点就可以了
        node.left = buildTree(num, start, (start + end) / 2 - 1);
        node.right = buildTree(num, (start + end) / 2 + 1, end);
        return node;
    }

    public TreeNode sortedArrayToBST(int[] num) {
        if (num == null) {
            return null;
        }
        return buildTree(num, 0, num.length - 1);
    }
```
### 平衡二叉树
	`题目:给定一个二叉树，判断它是否是高度平衡的二叉树。
	本题中，一棵高度平衡二叉树定义为：
	一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过1。`<br>
	*我的解法（采用递归去逐渐判断每一个节点的左右节点是不是高度差1即可，引入了之前写的判断每个节点高度的函数height）*
```
public boolean isBalanced(TreeNode root) {
		if(root==null)
			return true;
        if(Math.abs(height(root.left)-height(root.right))>1)
        	return false;
        if(isBalanced(root.left)&&isBalanced(root.right))
        	return true;
        else return false;
        
    }
	public static int height(TreeNode root) {//得到每一个节点的高度
		int x1;
		int x2;
		if(root==null)
			return 0;
		x1=height(root.left);
		x2=height(root.right);
		return Math.max(x1,x2)+1;
		
    }
```
	*标准答案的解法(更麻烦，专门定义了一个类来存放一个节点的属性，是不是平衡的以及深度)*
```
class ResultType {
    public boolean isBalanced;
    public int maxDepth;
    public ResultType(boolean isBalanced, int maxDepth) {
        this.isBalanced = isBalanced;
        this.maxDepth = maxDepth;
    }
}

public class Solution {
    /**
     * @param root: The root of binary tree.
     * @return: True if this Binary tree is Balanced, or false.
     */
    public boolean isBalanced(TreeNode root) {
        return helper(root).isBalanced;
    }
    
    private ResultType helper(TreeNode root) {
        if (root == null) {
            return new ResultType(true, 0);
        }
        
        ResultType left = helper(root.left);
        ResultType right = helper(root.right);
        
        // subtree not balance
        if (!left.isBalanced || !right.isBalanced) {
            return new ResultType(false, -1);
        }
        
        // root not balance
        if (Math.abs(left.maxDepth - right.maxDepth) > 1) {
            return new ResultType(false, -1);
        }
        
        return new ResultType(true, Math.max(left.maxDepth, right.maxDepth) + 1);
    }
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



[回到目录](#目录)
