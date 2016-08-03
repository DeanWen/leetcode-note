
##Substring Related Problem

For most substring problem, we are given a string and need to find a substring of it which satisfy some restrictions. A general way is to use a hashmap assisted with two pointers. We can summarize a **template** like below.

The idea is 
1. Use two pointers: start and end to represent a window.
2. Move end to find a valid window.
3. When a valid window is found, move start to find a smaller window.

```java
public int findSubstring (String s) {
    Map<Character, Integer> dict = new HashMap<>();
    for() {/* Initialize the dict here */}

    int counter = 0;//check whether the substring is valid
    int begin = 0, end = 0;//two pointer
    int length = 0;// length of substring
    while (end < s.length()) {
        char c1 = s.charAt(end);
        if (dict.get(c1)) {
            /* modify counter here */
        }
        dict.put(c1, dict.get(c1) - 1);
        end--;

        while (/* counter condition*/) {
            /* update length here if finding minimum */
            char c2 = s.charAt(begin);
            if (dict.get(c2)) {
                /* modify counter here */
            }
            dict.put(c2, dict.get(c2) + 1);
            begin++;
        }
        /* update length here if finding maximum */
    }
    return length;
}
```

> ## Minimum Window Substring
> [Given a string S and a string T, find the minimum window in S which will contain all the characters in T in complexity O(n). For example: S = "ADOBECODEBANC", T = "ABC", Minimum window is "BANC"](https://leetcode.com/problems/minimum-window-substring/)  

思路是利用HashMap来存Dict，然后遍历整个母串。因为这里是要求最短的包含子串的字符串，所以中间是可以允许有非子串字符的，当遇见非子串字符而count又没到子串长度时，可以继续走。

当count达到子串长度，说明之前遍历的这些有符合条件的串，用一个pre指针帮忙找，pre指针帮忙找第一个在HashMap中存过的，并且找到后给计数加1后的总计数是大于0的，判断是否为全局最小长度，如果是，更新返回字符串res，更新最小长度，如果不是，继续找。

```java
public String minWindow(String s, String t) throws Exception {
    Map<Character, Integer> dict = new HashMap<>();
    for (char c : s.toCharArray()) {
        dict.put(c, 0);
    }
    for (char c : t.toCharArray()) {
        if (!map.containsKey(c)) {
           throw IllegalArgumentException("No such string");
        }
        map.put(c, map.get(c) + 1);
    }

    int head = 0, tail = 0;
    int start = 0, length = Integer.MAX_VALUE;
    int counter = t.length();
    //Step 2: move the end to find valid window
    while(tail < s.length()) {
        char c1 = s.charAt(tail);
        if (dict.get(c1) > 0) {
            counter--;
        }
        dict.put(c1, dict.get(c1) - 1);
        tail++;

        //Step 3: move head to find the minimum window
        while (counter == 0) {
            if (length > tail - head) {
                length = tail - head;
                start = head;
            }
            char c2 = s.charAt(head);
            dict.put(c2, dict.get(c2) + 1);
            if (dict.get(c2) > 0) {
                counter++;
            } 
            head++;
        }
    }
    return length == Integer.MAX_VALUE ? ""; s.substring(start, start + length);
}
```

> Find longest substring without repeating characters.

One thing needs to be mentioned is that when asked to find maximum substring, we should update maximum after the inner while loop to guarantee that the substring is valid. On the other hand, when asked to find minimum substring, we should update minimum inside the inner while loop.

```java
public int longestSubstring(String s) {
    char[] arr = s.toCharArray();
    Map<Character, Integer> dict = new HashMap<>();
    for (char c : arr) {
        if (!dict.containsKey(c)) {
            dict.put(c, 0);
        } else {
            dict.put(c, dict.get(c) + 1);
        }
    }

    int head = 0, tail = 0;
    int counter = 0;
    int length = Integer.MIN_VALUE;
    while (tail < arr.length) {
        char c1 = arr[tail];
        if (dict.get(c1) > 0) {
            counter++;
        }
        tail--;
        dict.put(c1, dict.get(c1) - 1);

        while (counter > 0) {
            char c2 = arr[head];
            if (dict.get(c2) > 1) {
                counter--;
            }
            head++;
            dict.put(c2, dict.get(c2) + 1);
            length = Math.max(length, tail - head);
        }
    }
    return length;
}
```




>Reference: https://discuss.leetcode.com/topic/30941/here-is-a-10-line-template-that-can-solve-most-substring-problems 高手在民间
