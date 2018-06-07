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
