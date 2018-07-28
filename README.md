# 目录
* 数据结构相关
	*  [栈和队列](#栈和队列)
	*  [树](#树)
	*  [哈希表](#哈希表)
	* [数组](#数组)




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
```
### 对称二叉树	
`题目：给定一个二叉树，检查它是否是镜像对称的。`<br>
*解法一：采用迭代的思想。对整个二叉树进行层级遍历,将每一层的元素放到队列中，并用栈保存左子树的中的节点，每次出栈和右子树进行比较*
```java
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
```
*解法二：采用递归的思想，较为简单。通过比较原节点是否相等以及递归表示左边节点的左子树和右边节点的右子树是否相等以及左边节点的右子树和右边节点的左子树是否相等*<br>
```java
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
[leetcode:104. Maximum Depth of Binary Tree](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)<br>
	`题目：给定一个二叉树，找出其最大深度。
	二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。`<br>
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
[leetcode:108. Convert Sorted Array to Binary Search Tree](https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree/)<br>
`题目：将一个按照升序排列的有序数组，转换为一棵高度平衡二叉搜索树。
本题中，一个高度平衡二叉树是指一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过 1`<br>
	
	注意一个很重要的点，数组是按升序进行排列的，而要求树的平衡的，于是根节点一定是数组中间的节点，这样大于和小于它的数正好都是一半，然后采用递归	      的方式去不断去对每一个节点采用对数组区间分段的方式进行处理，最后的树一定是左右差不多高的。偶数长时高度会差1<br>
	不需要像AVL树一样在没插入一个节点便进行平衡处理，那样显得多余。leetcode中只需要按顺序返回树的各个节点就可以（上到下左到右），不需要指明子树关系<br>
```java
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
[leetcode:110. Balanced Binary Tree](https://leetcode-cn.com/problems/balanced-binary-tree/)<br>
`题目:给定一个二叉树，判断它是否是高度平衡的二叉树。
本题中，一棵高度平衡二叉树定义为：
一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过1。`<br>

*我的解法（采用递归去逐渐判断每一个节点的左右节点是不是高度差1即可，引入了之前写的判断每个节点高度的函数height）*
```java
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
```java
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
### 二叉树的最小深度
`题目：给定一个二叉树，找出其最小深度。
最小深度是从根节点到最近叶子节点的最短路径上的节点数量。
说明: 叶子节点是指没有子节点的节点。`<br>
	说明 ：和计算最大深度类似，只不过取左右子树高度的最小值，但关键是对于有单子树的节点计算时要把单子树的高度加1，于是要分类讨论。<br>
```java
 public int minDepth(TreeNode root) {
	      if(root==null)
	    	  return 0;
	      int left=minDepth(root.left);
	      int right=minDepth(root.right);
	      if(root.left==null)
	    	  return right+1;
	      if(root.right==null)
	    	  return left+1;
	     return Math.min(left, right)+1;
	 }
```
### 路径和
[leetcode:112. Path Sum](https://leetcode-cn.com/problems/path-sum/description/)<br>
`题目：给定一个二叉树和一个目标和，判断该树中是否存在根节点到叶子节点的路径，这条路径上所有节点值相加等于目标和。
说明: 叶子节点是指没有子节点的节点。`
 
	说明 ：之前自己考虑时总是想得出所有路劲的和再比较，实际上采用从上往下逐次迭代，并每次用sum减去路过的节点值作为新的总和是一种更好的方式。
```java
public boolean hasPathSum(TreeNode root, int sum) {
		if(root==null)
			return false;
		if(root.left==null&&root.right==null)
			return root.val==sum;//对于树叶节点，前面路劲经过的和值都是相等的，于是可以采用相减的方式去递归，每经过一个根节点就减去它的值
		return hasPathSum(root.left, sum-root.val)||hasPathSum(root.right, sum-root.val);
        //只要有一个树叶节点满足即可，于是采用或逻辑
    }
```
### 左叶子之和
[leetcode:404. Sum of Left Leaves](https://leetcode-cn.com/problems/sum-of-left-leaves/description/)<br>
`计算给定二叉树的所有左叶子之和。`<br>
	由于只计算左叶子，于是对于右子树应该区别对待，对于每一个右子树节点当做新的根节点去考虑其左子树
```java
public int sumOfLeftLeaves(TreeNode root) {
        if(root == null) {
            return 0;
        }
        int sum = 0;
        if(root.left != null)
        {
            TreeNode left = root.left;
            if(left.left == null && left.right == null) {
                sum += left.val;
            }
            else {
                sum += sumOfLeftLeaves(left);
            }
        }
        if(root.right != null)
        {
            TreeNode right = root.right;
            sum += sumOfLeftLeaves(right);
        }
        return sum;
    }
```
### 另一个树的子树
`题目：给定两个非空二叉树 s 和 t，检验 s 中是否包含和 t 具有相同结构和节点值的子树。s 的一个子树包括 s 的一个节点和这个节点的所有子孙。s 也可以看做它自身的一棵子树。`
	说明 ：先定义一个方法来判定两个树是不是相等，然后再在主要的方法中不断的向下迭代。专门来判断两个树是不是相等是因为如果在isSubtree方法中直接判 	    断时会导致[3,4,5,1,null,null,2]将[3,1,2]判定为子树<br>
```java
    public boolean isSubtree(TreeNode s, TreeNode t) {
    	if (s == null) {
            return t == null;
        } 	
    	if(s.val==t.val&&isSame(s, t))
    		return true;
    	else  
    		return isSubtree(s.left, t)||isSubtree(s.right, t);   
    }
    public boolean isSame(TreeNode s, TreeNode t){//单纯判断连两个树是不是相等
    	if (s == null) {
            return t == null;
        }
    	if(t==null)
    		return false;
    	if(s.val!=t.val)
    		return false;
    	return isSame(s.left, t.left)&&isSame(s.right, t.right);
		
    	
    }
```
### 二叉树的直径
`题目：给定一棵二叉树，你需要计算它的直径长度。一棵二叉树的直径长度是任意两个结点路径长度中的最大值。这条路径可能穿过根结点。`<br>
	我的思路：由于最大的路径可能是经过根节点也可能不经过，但是一定和左右两个子节点的最大高度是有关系的。这里把每个节点的左右子节点的
	高度得出来并相加得到每个节点对应的最大路径（因为路径是算按边数算，于是高度会多1，所以两个子树的高度直接相加而不是再加2）。然后定义
	一个链表用来存放每个节点的最大路径，然后在链表中取最大值就是整个数的直径（最大路径）<br>
*我的解法*
```java
class Solution {
	ArrayList<Integer> list=new ArrayList<>();
    public int diameterOfBinaryTree(TreeNode root) {
    	maxP(root);
    	//Integer[] arr=(Integer[]) list.toArray();
    	if(list.size()==0)
    		return 0;
    	else{
    		Integer temp=list.get(0);
        	for(Integer t:list){
        		if(t.compareTo(temp)>0)
        			temp=t;
        	}
    		return temp.intValue();
            
    	}
    	
    }  
    public void maxP(TreeNode t){
    	 if(t==null)
    	        return;
    	 list.add(maxPath(t));
    	 maxP(t.left);
		 maxP(t.right);	
    }
    public int maxPath(TreeNode root){
    	if(root==null)
    		return 0;
    	return maxDepth(root.left)+maxDepth(root.right); 	
    }
	public int maxDepth(TreeNode root) {//返回每一个节点的最大深度
		if(root==null)
			return 0;
		int left=maxDepth(root.left);
		int right=maxDepth(root.right);
		return Math.max(left, right)+1;
		
    }


}
```
###  把二叉搜索树转换为累加树
`题目：给定一个二叉搜索树（Binary Search Tree），把它转换成为累加树（Greater Tree)，使得每个节点的值是原来的节点值加上所有大于它的节点值之和。`<br>
	此题就是需要把一颗二叉查找树按照元素从大到小的顺序进行遍历，然后把经过的元素累加赋值给下一个要遍历的元素。
	因为二叉树的中序遍历是采用左--中---右的顺序遍历，而这道题则需要反过来采用右--中--左的顺序遍历。
* 一：自己的解法：重新写一个返回为int的函数来实现反向的中序遍历，主函数中调用它*
```java
public class ConvertBSTtoGreaterTree {
    int sum = 0;//因为在每次递归过程中求得的和要保留，所以sum写成类的成员变量。
    public TreeNode convertBST(TreeNode root) {
    	add(root);
		return root;
        
    }
    public int add(TreeNode t){
    	
    	if(t==null)
    		return 0;
    	add(t.right);
    	sum+=t.val;
    	t.val=sum;
    	add(t.left);
    	return sum;
    }
```
* 二：一种复杂度更低，运行时间更短的代码，巧妙的利用sum求和的顺序来完成反向的中序遍历*
```java
class Solution {

	int sum = 0;
    public TreeNode convertBST(TreeNode root) {
        if(root==null)
            return null;
    	convertBST(root.right);
        root.val+=sum;//巧妙的将中序遍历左根右的顺序逆过来，变成右根左的顺序，这样就可以反向计算累加和sum，同时更新结点值.和上一个的区别在于先更新节点值再求和
        sum=root.val;
        convertBST(root.left);
		return root;
        
    }

}
```
### 根据二叉树创建字符串
`题目：你需要采用前序遍历的方式，将一个二叉树转换成一个由括号和整数组成的字符串。
空节点则用一对空括号 "()" 表示。而且你需要省略所有不影响字符串与原始二叉树之间的一对一映射关系的空括号对`<br>
	分类讨论节点是否是叶节点，是否是左或者右节点。
```java
    public String tree2str(TreeNode t) {
        if(t==null)
    		return "";
        if(t.left!=null&&t.right!=null)
            return String.valueOf(t.val)+"("+tree2str(t.left)+")"+"("+tree2str(t.right)+")";
       	else if(t.left==null&&t.right!=null)
    		return String.valueOf(t.val)+"()"+"("+tree2str(t.right)+")";
       	else if(t.right==null&&t.left!=null)
       		return String.valueOf(t.val)+"("+tree2str(t.left)+")";
        else
            return String.valueOf(t.val);
    } 
```
### 二叉树的坡度
`题目：给定一个二叉树，计算整个树的坡度。
一个树的节点的坡度定义即为，该节点左子树的结点之和和右子树结点之和的差的绝对值。空结点的的坡度是0。
整个树的坡度就是其所有节点的坡度之和。`<br>
	我的想法:分别写两个函数，一个用来计算每个节点下面的节点之和，一个用户计算坡度值，最后在主函数中遍历所有节点计算每个节点的坡度值相加  
```java
class Solution {
	int sum;
	 public int findTilt(TreeNode root) {
		 if(root==null)
			 return 0;
	       sum+= grade_count(root);
	       findTilt(root.left);
	       findTilt(root.right);
	       return sum;
	 }
	 public int grade_count(TreeNode t){//计算每个节点的坡度
		 
		return Math.abs(sum(t.left)-sum(t.right)); 
	 }
	 public int sum(TreeNode root){//计算一个树的和
		 if(root==null)
			 return 0;
		 return root.val+sum(root.left)+sum(root.right);
		 
	 }
}
```
	实际我多遍历了一次，在计算每个节点的下面和时就已经遍历一次了，这样直接可以得出每个节点的坡度值那么直接相加就可以了！
```java
class Solution {
	int sum;
	public int findTilt(TreeNode root) {
	       grade(root);
	       return sum;
	 }
	public int grade(TreeNode root){
		if(root==null)
			return 0;
		int left=grade(root.left);
		int right=grade(root.right);
		sum+=Math.abs(left-right);//计算出坡度的过程中直接把它加给最终的sum就可以了，不过再在主函数中遍历一次
		return root.val+left+right;//迭代去计算每个节点下面的值，实际上这个过程就遍历了所有节点
		
	}
}
```
### 合并二叉树
[leetcode:617. Merge Two Binary Trees](https://leetcode-cn.com/problems/merge-two-binary-trees/description/)<br>
`题目：给定两个二叉树，想象当你将它们中的一个覆盖到另一个上时，两个二叉树的一些节点便会重叠。
你需要将他们合并为一个新的二叉树。合并的规则是如果两个节点重叠，那么将他们的值相加作为节点合并后的新值，否则不为 NULL 的节点将直接作为新二叉树的节点。`
```java
    public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
    	if(t1==null&&t2==null)
    		return null;
    	if(t2==null&&t1!=null)
    		return t1;
    	if(t1==null&&t2!=null)
    		return t2;//如果t1这个节点没有值的话就给他重新赋一个节点
    	TreeNode root=new TreeNode(t1.val+t2.val);//新定义一个树节点来存放二者的和值
    	
       root.left= mergeTrees(t1.left, t2.left);//不断的去更新新创建的树的值
       root.right=mergeTrees(t1.right, t2.right);
    	
		return root;
       
    }
```
### 二叉树的层平均值
[leetcode:637. Average of Levels in Binary Tree](https://leetcode-cn.com/problems/average-of-levels-in-binary-tree/description/)<br>
`题目：给定一个非空二叉树, 返回一个由每层节点平均值组成的数组.`
	理解:按照层序遍历的方式去遍历整个树，要用到队列。但是因为队列中并不会每次存放的都是一行元素，于是可以考虑引入null来划分不同层。一层结束则压入null
```java
public List<Double> averageOfLevels(TreeNode root) {
    	List<Double> list=new LinkedList<>();
    	List<Integer> temp=new LinkedList<>();//用一个队列来存放每一层的元素，后面用来求平均值
        LinkedList<TreeNode> queue=new LinkedList<>();//用链表实现一个队列，存放每一层的元素
        queue.offer(root);
        queue.offer(null);//用空节点来表示一层结束
        while(!queue.isEmpty()){
        	TreeNode current=queue.poll();
        	if(current!=null){
        		temp.add(current.val);//将每一层元素放到数组中
        		if(current.left!=null)
        			queue.offer(current.left);
        		if(current.right!=null)
        			queue.offer(current.right);
        	}
        	else{
        		if(!temp.isEmpty()){
        			queue.offer(null);
        			double sum = 0;
        			for(int i=0;i<temp.size();i++)
        				sum+=(double)temp.get(i);
        			list.add(sum/temp.size());
        			temp.clear();
        		}
        	}
        }
        return list;
    }
```
	另一种解法：不需要引入null，而是在每次刚好一层元素的时候记录队列的长度，按照长度弹出队列元素并相加。上一层有几个元素便在for循环中弹出几次，这样下次循环是队列中放的正好就是下一层的元素。这样便不管队列实际是不是一层元素了  
```java
	public List<Double> averageOfLevels(TreeNode root) {
		List<Double> list=new LinkedList<>();
		LinkedList<TreeNode> queue=new LinkedList<>();
		queue.offer(root);
		while(!queue.isEmpty()){
			double sum=0;
			int n=queue.size();//记录每一层元素的数目
			for(int i=0;i<n;i++){
				TreeNode current=queue.poll();
				sum+=current.val;
				if(current.left!=null)
        			queue.offer(current.left);
        		if(current.right!=null)
        			queue.offer(current.right);	
			}
			list.add(sum/n);
		}
		return list;
	}
```
### 修建二叉树
[leetcode:669. Trim a Binary Search Tree](https://leetcode-cn.com/problems/trim-a-binary-search-tree/description/)<br>
`题目：给定一个二叉搜索树，同时给定最小边界L 和最大边界 R。通过修剪二叉搜索树，使得所有节点的值在[L, R]中 (R>=L) 。你可能需要改变树的根节点，所以结果应当返回修剪好的二叉搜索树的新的根节点。`<br>
	分类讨论，如果节点的值小于区间的话那么其左节点一定小于区间的值，那么直接用右节点代替他就可以了  
	若果节点值大于区间值的话，就用左节点代替它！如果在区间中的话那么就向下递归遍历！
```java
	 public TreeNode trimBST(TreeNode root, int L, int R) {
        if(root==null)
	    		return null;
	    	if(root.val<L){
	            root=root.right;
	             root=trimBST(root,L,R);//注意在二叉树中递归必须让他赋值给root来更新节点的值
	         }//该节点以及左子树都删去
	 
	    	else if(root.val>R){
	              root=root.left;
	               root=trimBST(root,L,R);
	        }
	    		//该节点以及右子树的值都删去
	        else{
	              root.left=trimBST(root.left,L,R);//注意如果对二叉树中的节点进行改变时，必须赋值，调用什么实参就赋值给什么，这样才能更新节点的值！
	    	      root.right=trimBST(root.right,L,R);
	            }
			return root;
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
		相关的题目：
[两数之和2——针对排序数组](#两数之和2——针对排序数组)
### 同构字符串
`题目：给定两个字符串 s 和 t，判断它们是否是同构的。
如果 s 中的字符可以被替换得到 t ，那么这两个字符串是同构的。
所有出现的字符都必须用另一个字符替换，同时保留字符的顺序。两个字符不能映射到同一个字符上，但字符可以映射自己本身.`
	** 我的解法
将两个字符串按照字符分别放在map中，因为map中键不能重复，重复得话值会覆盖，所以遍历字符串然后和map中的值进行比较就可以判断对应位置是不是对应了，但是要注意的是不能有两个字符对应同一个相同的字符，于是考虑把值都放在set（元素不重复）中，看map和set长度是不是相等便可判断value有没有重复的<br>
```java
    public static boolean isIsomorphic(String s, String t) {
    	HashMap<Character, Character> map=new HashMap<>();//set中元素不允许重复，可用于判断是不是有两个重复的值
    	Set<Character> set=new HashSet<>();
    	char[] a=s.toCharArray();
    	char[] b=t.toCharArray();
    	for(int i=0;i<a.length;i++){
    		map.put(a[i], b[i]);
    	}
    	for(int i=0;i<a.length;i++){
    		if(map.get(a[i])!=b[i])
    			return false;
    	}//还要考虑如果有多对一的情况。即两个不同的字符映射相同的字符
    	for(java.util.Map.Entry<Character, Character> a1:map.entrySet()){
    		set.add(a1.getValue());
    	}
    	if(map.size()!=set.size())
    		return false;
		return true;
        
    }
```
	** 更简单的解法
不用hashmap，而是根据字符ASCⅡ码一定在256之类来构造两个256长的数组，两字符串相互映射的字符对应数组中的位置放相同的值。遍历数组时只要对应位置的值不同那就返回false<br>
```java
    public boolean isIsomorphic(String s, String t) {
        int[] a = new int[256];
        int[] b = new int[256];
        for (int i = 0; i < 256; i++) {
            a[i] = b[i] = -1;
        }
        int len = s.length();
        for (int i = 0; i < len; i++) {
            if (a[s.charAt(i)] != b[t.charAt(i)]) {
                return false;
            }
            a[s.charAt(i)] = b[t.charAt(i)] = i;
        }
        return true;
    }
```
### 单词匹配
[leetcode:290. Word Pattern](https://leetcode.com/problems/word-pattern/description/)<br>
`给一个字符串pattern和一个字符串str（其中各个子串用空格隔开），使他们完全配对
Input: pattern = "abba", str = "dog cat cat dog"
Output: true`
想法：分别把pattern字符串按字符放到map中，对应的值取他们最后出现的位置。然后把str中对应的各个字符串放在另一个map中，同样的各个字符串的值取他们最后出现的位置（其实就是给各个子串一个对应的和数字），然后遍历整个字符串，只有对应位置出现的值都是一样的那才是匹配的！<br>
	注意点：integer对象必须用equals进行比较，而不能用==！
```java
    public  boolean wordPattern(String pattern, String str) {
        HashMap<Character, Integer> map1=new HashMap<>();
        HashMap<String, Integer> map2=new HashMap<>();
        String[] strings=str.split(" ");
        if(pattern.length()!=strings.length)
        	return false;
        for(int i=0;i<pattern.length();i++){
        	map1.put(pattern.charAt(i), i);
        	map2.put(strings[i], i);
        }
        for(int i=0;i<pattern.length();i++){
        if(!map1.get(pattern.charAt(i)).equals(map2.get(strings[i])))//注意：integer对象必须用equals进行比较，而不能用==！
        		return false;
        }
        return true;
    }
```

###快乐数
`题目：编写一个算法来判断一个数是不是“快乐数”。
一个“快乐数”定义为：对于一个正整数，每一次将该数替换为它每个位置上的数字的平方和，然后重复这个过程直到这个数变为 1，也可能是无限循环但始终变不到 1。如果可以变为 1，那么这个数就是快乐数`<br>
	首先按照题目必须把一个数字的各位取出来，然后再平方和。常规的方法是取模，如：
```java 
    while (n!=0) {
        tmp = n % 10;
        sum += tmp * tmp;
        n /= 10;//除以10取整
    }
```
这里我也可以采用字符串的方法，把数字转换为字符串然后charAt取出每一位数字！这里重点是循环到多久时退出，按道理最多810次没到1那就肯定不能到1了（以为10\*9^2=810，也就是10个9这么大的数字各位平方和也就是810，所以最多810下，如果出现重复数字就不可能到1了！）
```java
    public static boolean isHappy(int n) {
    	int k=0;
    	while(n!=1&&k<6){//这是K=6只是因为这个过程一定可以构成一个环，很多数字各位平方和后都会回到原处，而这个环的大小经过测试应该是6（猜的。。不会数学推倒）
    		String string=String.valueOf(n);
    		int sum=0;
        	for(int i=0;i<string.length();i++){
        		sum+=(string.charAt(i)-48)*(string.charAt(i)-48);
        	}
        	n=sum;
        	k++;
    	}
    	return n==1;
    }
```

## 数组
### 两数之和2——针对排序数组
[leetcode:167. Two Sum II - Input array is sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/description/)<br>
`题目：Given an array of integers that is already sorted in ascending order, find two numbers such that they add up to a specific target number.
The function twoSum should return indices of the two numbers such that they add up to the target, where index1 must be less than index2.`<br>
	这里因为数组已经是排好序的了，所以用不能像之前的TwoSum一样使用HashMap，反而会变得麻烦。应该和二叉查找树中的方法一样（按照中序遍历二叉查找树	    便是从小到大排序），从数组两端往中间进行查找,大了的话左边就右移，小了的话右边就左移。<br>
```java
    private static int[] twoSum(int[] numbers, int target) {
    	int start=0;
    	int end=numbers.length-1;
    	while(start<end){
        	if(numbers[start]+numbers[end]<target)
        		start+=1;
        	else if(numbers[start]+numbers[end]>target)
        		end-=1;
        	else break;
    	}

		return new int[]{start+1,end+1};
        
    }
```
[回到目录](#目录)
