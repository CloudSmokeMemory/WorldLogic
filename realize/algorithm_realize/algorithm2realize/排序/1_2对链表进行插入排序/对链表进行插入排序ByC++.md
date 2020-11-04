```
class Solution {
public:
ListNode* insertionSortList(ListNode* head) {
	if (!head || !head->next)
		return head;
	ListNode *dummyhead = new ListNode(-1);//伪头指针
	dummyhead->next = head;
	ListNode *prev = head;
	ListNode *node = head->next;
	while (node)
	{
		if (node->val < prev->val)
		{
			ListNode* temp = dummyhead;//！！temp要等于dummyhead，这样才可以比较第一个元素
			while (temp->next->val < node->val)//！！！这里是temp->next：因为要修改前面的temp的指向
			{
				temp = temp->next;//指针后移
			}
			prev->next = node->next;
			node->next = temp->next;
			temp->next = node;
			node = prev->next;//此时不用改变prev指向！因为prev没有变，只是待排序元素变了位置。
		}
		else
		{
			prev = prev->next;
			node = node->next;
		}
	}
	return dummyhead->next;//!!!不能返回head！！因为后面会改变head所指向内存的位置！
}
};


```
[返回题目][返回题目]

```
作者：Smart_Shelly
链接：https://leetcode-cn.com/problems/insertion-sort-list/solution/ccha-ru-pai-xu-xiao-bai-ban-by-tryangel/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

[返回题目]:https://github.com/CloudSmokeMemory/WorldLogic/blob/main/realize/algorithm_realize/algorithm2realize/%E6%8E%92%E5%BA%8F/1_2%E5%AF%B9%E9%93%BE%E8%A1%A8%E8%BF%9B%E8%A1%8C%E6%8F%92%E5%85%A5%E6%8E%92%E5%BA%8F/%E5%AF%B9%E9%93%BE%E8%A1%A8%E8%BF%9B%E8%A1%8C%E6%8F%92%E5%85%A5%E6%8E%92%E5%BA%8F.md

