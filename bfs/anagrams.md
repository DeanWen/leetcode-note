> [Given an array of strings, return all groups of strings that are anagrams](https://oj.leetcode.com/problems/anagrams/)  

思路：  
Anagrams就是两个单词所包含的字符和数量都是一样的，只是顺序不同。比如["tea","ate","aet"], 由于anagrams字符串的所有字符都是一样的，如果可以对每个字符排序，则结果一样，就可以当作key用hashmap去存，然后value就是不同的字符串没排序前的形态。最后遍历一下hashmap，value > 1 就说明有anagram，返回所有的结果。还有一个关键点是如何去遍历hashmap，重写一下iterator就好了
复杂度分析：对每个长度为k的字符串排序，时间复杂度为(O(klogk)),n个字符就是(O(nklogk)),n为数组大小，空间复杂度为O(nk)就是hashmap的大小。

```java
public ArrayList<String> anagrams(String[] strs) {
    ArrayList<String> result = new ArrayList<String>();
    if(strs == null || strs.length ==0) {
        return result;
    }

    // process the data
    HashMap<String, ArrayList<String>> map = new HashMap<String, ArrayList<String>>();
    for (int i = 0; i < strs.length; i++) {
        char[] charArray = strs[i].toCharArray();
        Arrays.sort(charArray);
        String strKey = new String(charArray);
        // if not such key, create value list with current str 
        if (!map.containsKey(strKey)) {
            ArrayList<String> temp = new ArrayList<String>();
            temp.add(strs[i]);
            map.put(strKey, temp);
        }else { // if contains such key, add as a new item in value list
            ArrayList<String> temp = map.get(strKey);
            temp.add(strs[i]);
            map.put(strKey,temp);
        }
    }

    // traversal hashmap re-write the iterator
    Iterator iter = map.values().iterator();
    while(iter.hasNext()) {
        ArrayList<String> item = (ArrayList<String>)iter.next();
        if (item.size() > 1) {
            result.addAll(item);
        }
    }
    
    return result;
}
```