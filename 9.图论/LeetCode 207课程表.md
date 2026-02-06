递归/拓扑排序

---

### 问题描述

你需要完成`numCourses`门课程。某些课程有先修课程要求，用`prerequisites[i] = [a, b]`表示学习课程`a`前必须先学课程`b`。判断是否能完成所有课程。

**本质**：给你一个有向图，判断是否存在环

### 解题思路

#### 拓扑排序

1. 计算每个节点的入度（有多少门课程依赖它）
2. 将入度为0的课程加入队列
3. 逐个处理队列中的课程，减少依赖课程的入度
4. 如果最后处理的课程数等于总数，则无环

时空复杂度
- **时间复杂度**：O(V + E)，其中 `V` 是课程数，`E` 是依赖关系数。
    
- **空间复杂度**：O(V + E)，其中 `V` 是课程数，`E` 是依赖关系数。
### Java代码实现

```java
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        // 1. 构建邻接表和入度数组
        List<List<Integer>> graph = new ArrayList<>();
        int[] indegree = new int[numCourses];
        
        for (int i = 0; i < numCourses; i++) {
            graph.add(new ArrayList<>());
        }
        
        // 2. 填充图和入度
        for (int[] pre : prerequisites) {
            int course = pre[0];      // 要学的课程
            int prerequisite = pre[1]; // 先修课程
            graph.get(prerequisite).add(course);
            indegree[course]++;
        }
        
        // 3. 将所有入度为0的课程加入队列
        Queue<Integer> queue = new LinkedList<>();
        for (int i = 0; i < numCourses; i++) {
            if (indegree[i] == 0) {
                queue.offer(i);
            }
        }
        
        // 4. 拓扑排序
        int count = 0;
        while (!queue.isEmpty()) {
            int course = queue.poll();
            count++;
            
            // 遍历依赖该课程的所有课程
            for (int nextCourse : graph.get(course)) {
                indegree[nextCourse]--;
                if (indegree[nextCourse] == 0) {
                    queue.offer(nextCourse);
                }
            }
        }
        
        // 5. 如果处理的课程数等于总数，则无环
        return count == numCourses;
    }
}
```