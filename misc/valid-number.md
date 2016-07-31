> [Validate if a given string is numeric.](https://leetcode.com/problems/valid-number/)

```
Some examples:
"0" => true
" 0.1 " => true
"abc" => false
"1 a" => false
"2e10" => true
Note: It is intended for the problem statement to be ambiguous. 
You should gather all requirements up front before implementing one.
```


[思路题解来自](https://discuss.leetcode.com/topic/9490/clear-java-solution-with-ifs)  
We start with trimmed string. 
- If we see [0-9] we reset the number flags.
- We can only see '.' If we didn't see 'e' or '.'  
- We can only see 'e' if we didn't see 'e' but we did see a number. We reset numberAfterE flag.  
- We can only see '+' and '-' in the beginning and after an 'e'
any other character break the validation.  
- At the end it is only valid if there was at least 1 number and if we did see an 'e' then a number after it as well.  

```
So basically the number should match this regular expression:
[-+]?[0-9]*(.[0-9]+)?(e[-+]?[0-9]+)?
``` 

```java
public boolean isNumber(String s) {
    String regex = "[-+]?(\\d+\\.?|\\.\\d+)\\d*(e[-+]?\\d+)?";  
    if(s.trim().matches(regex)){  
        return true;  
    }
    return false;
}
```

分类讨论，常规解法：

```java
public static boolean isNumber(String s) {
    char[] arr = s.trim().toCharArray();
    
    boolean dot = false;
    boolean e = false;
    boolean num = false;
    boolean numAfterE = true;//default true for non 'e' case
    for (int i = 0; i < arr.length; i++) {
    	if (arr[i] >= '0' && arr[i] <= '9') {
    		num = true;
    		numAfterE = true;
    	} else if (arr[i] == '.') {
    		if (dot || e) {
    			return false;
    		}
    		dot = true;
    	} else if (arr[i] == 'e') {
    		if (!num || e) {
    			return false;
    		}
    		e = true;
    		numAfterE = false;
    	} else if (arr[i] == '+' || arr[i] == '-'){
    		if (i != 0 && arr[i - 1] != 'e') {
    			return false;
    		}
    	} else {
    		return false;
    	}
    }
    //At least 1 num, if 'e' exist, must be with at least 1 num after 'e'
    return num && numAfterE;
}
```