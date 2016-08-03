
>[Word Ladder](https://leetcode.com/problems/word-ladder/)

```
Given:
beginWord = "hit"
endWord = "cog"
wordList = ["hot","dot","dog","lot","log"]
As one shortest transformation is "hit" -> "hot" -> "dot" -> "dog" -> "cog",
return its length 5.
```

典型的BFS题，这道题只用求距离，所以不用建图，BFS的时候，update dict就好了。如果要指定 点 到 点的 所有路径，那就是典型的先从终点到起点 BFS 建立无向有权图，然后从起点DFS到终点的所有路径即可。


```java
public int ladderLength(String beginWord, String endWord, Set<String> dict) {
    Queue<String> q = new LinkedList<>();
    q.offer(beginWord);
    dict.remove(beginWord);
    int distance = 1;
    while (!q.isEmpty()) {
        int size = q.size();
        for (int i = 0; i < size; i++) {
            String curr = q.poll();
            for (int i = 0; i < curr.length(); i++) {
                char[] sArr = curr.toCharArray();
                for (char c = 'a'; c <= 'z'; c++) {
                    sArr[i] = c;
                    String temp = new String(sArr);
                    if (temp.equals(endWord)) {
                        return distance + 1;
                    }
                    if (dict.contains(temp)) {
                        q.offer(temp);
                        set.remove(temp);
                    }
                }
            }
        }
        dictance++;
    }

    return 0;
}
```


通常做法可以预处理dict，build a graph, 然后可以套用图的BFS去找距离。

```java
private Map<String, Set<String>> buildGraph (Set<String> dict) {
    Map<String, Set<String>> map = new HashMap<>();
    for (String str : dict) {
        Set<String> neighbors = new HashSet<>();
        for (int i = 0; i < str.length(); i++) {
            char[] arr = str.toCharArray();
            for (char c = 'a'; c <= 'z'; c++) {
                arr[i] = c;
                String temp = new String(arr);
                if (temp.equals(str)) continue;
                if (dict.contains(temp)) {
                    neighbors.add(temp);
                }
            }
        }
        map.put(str, neighbors);
    }
    return map;
}
```