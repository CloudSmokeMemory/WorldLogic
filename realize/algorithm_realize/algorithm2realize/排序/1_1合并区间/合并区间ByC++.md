```
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        if (intervals.size() == 0) {
            return {};
        }
        sort(intervals.begin(), intervals.end());
        vector<vector<int>> merged;
        for (int i = 0; i < intervals.size(); ++i) {
            int L = intervals[i][0], R = intervals[i][1];
            if (!merged.size() || merged.back()[1] < L) {
                merged.push_back({L, R});
            }
            else {
                merged.back()[1] = max(merged.back()[1], R);
            }
        }
        return merged;
    }
};
```
[返回题目](https://github.com/CloudSmokeMemory/WorldLogic/blob/main/realize/algorithm_realize/algorithm2realize/%E6%8E%92%E5%BA%8F/1_1%E5%90%88%E5%B9%B6%E5%8C%BA%E9%97%B4/%E5%90%88%E5%B9%B6%E5%8C%BA%E9%97%B4.md#%E9%A2%98%E7%9B%AE)

```
作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/merge-intervals/solution/he-bing-qu-jian-by-leetcode-solution/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```
