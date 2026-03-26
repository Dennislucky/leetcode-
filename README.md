# LeetCode Hot 100 题解教程

> 精选 LeetCode 最热门的 100 道算法题，提供详细的思路分析、图解说明和 Python 实现。

## 📚 项目结构

按照算法类型分类，由浅入深：

| 分类 | 题数 | 目录 |
|------|------|------|
| 哈希表 | 2 | [01_hash](./01_hash/) |
| 双指针 | 4 | [02_two_pointers](./02_two_pointers/) |
| 滑动窗口 | 2 | [03_sliding_window](./03_sliding_window/) |
| 子串/子数组 | 3 | [04_substring](./04_substring/) |
| 普通数组 | 5 | [05_array](./05_array/) |
| 矩阵 | 4 | [06_matrix](./06_matrix/) |
| 链表 | 7 | [07_linked_list](./07_linked_list/) |
| 二叉树 | 11 | [08_binary_tree](./08_binary_tree/) |
| 图论 | 4 | [09_graph](./09_graph/) |
| BFS | 3 | [10_bfs](./10_bfs/) |
| 回溯 | 7 | [11_backtracking](./11_backtracking/) |
| 二分查找 | 4 | [12_binary_search](./12_binary_search/) |
| 栈 | 6 | [13_stack](./13_stack/) |
| 堆/优先队列 | 3 | [14_heap](./14_heap/) |
| 贪心 | 4 | [15_greedy](./15_greedy/) |
| 动态规划 | 15 | [16_dp](./16_dp/) |
| 多维动态规划 | 6 | [17_dp_multi](./17_dp_multi/) |
| 技巧题 | 6 | [18_techniques](./18_techniques/) |

## 🚀 如何使用

1. 按分类阅读，每个文件夹下有独立的题解 Markdown 文件
2. 每道题包含：题目描述、思路分析、代码实现、复杂度分析
3. 所有代码使用 **Python** 编写，部分提供多语言实现
4. 建议按照分类顺序学习，由浅入深

## 📖 题解格式

每道题的题解包含以下部分：

- **题目描述**：简洁的题目说明和示例
- **思路分析**：详细解题思路，为什么选择该方法
- **代码实现**：带详细注释的 Python 代码
- **复杂度分析**：时间和空间复杂度
- **关键要点**：总结核心知识点

## 📋 完整题目列表

### 哈希表
- [1. 两数之和 (Two Sum)](./01_hash/0001_two_sum.md)
- [49. 字母异位词分组 (Group Anagrams)](./01_hash/0049_group_anagrams.md)
- [128. 最长连续序列 (Longest Consecutive Sequence)](./01_hash/0128_longest_consecutive_sequence.md)

### 双指针
- [283. 移动零 (Move Zeroes)](./02_two_pointers/0283_move_zeroes.md)
- [11. 盛最多水的容器 (Container With Most Water)](./02_two_pointers/0011_container_with_most_water.md)
- [15. 三数之和 (3Sum)](./02_two_pointers/0015_three_sum.md)
- [42. 接雨水 (Trapping Rain Water)](./02_two_pointers/0042_trapping_rain_water.md)

### 滑动窗口
- [3. 无重复字符的最长子串 (Longest Substring Without Repeating Characters)](./03_sliding_window/0003_longest_substring.md)
- [438. 找到字符串中所有字母异位词 (Find All Anagrams in a String)](./03_sliding_window/0438_find_all_anagrams.md)

### 子串/子数组
- [560. 和为 K 的子数组 (Subarray Sum Equals K)](./04_substring/0560_subarray_sum_equals_k.md)
- [239. 滑动窗口最大值 (Sliding Window Maximum)](./04_substring/0239_sliding_window_maximum.md)
- [76. 最小覆盖子串 (Minimum Window Substring)](./04_substring/0076_minimum_window_substring.md)

### 普通数组
- [53. 最大子数组和 (Maximum Subarray)](./05_array/0053_maximum_subarray.md)
- [56. 合并区间 (Merge Intervals)](./05_array/0056_merge_intervals.md)
- [189. 轮转数组 (Rotate Array)](./05_array/0189_rotate_array.md)
- [238. 除自身以外数组的乘积 (Product of Array Except Self)](./05_array/0238_product_except_self.md)
- [41. 缺失的第一个正数 (First Missing Positive)](./05_array/0041_first_missing_positive.md)

### 矩阵
- [73. 矩阵置零 (Set Matrix Zeroes)](./06_matrix/0073_set_matrix_zeroes.md)
- [54. 螺旋矩阵 (Spiral Matrix)](./06_matrix/0054_spiral_matrix.md)
- [48. 旋转图像 (Rotate Image)](./06_matrix/0048_rotate_image.md)
- [240. 搜索二维矩阵 II (Search a 2D Matrix II)](./06_matrix/0240_search_2d_matrix.md)

### 链表
- [160. 相交链表 (Intersection of Two Linked Lists)](./07_linked_list/0160_intersection.md)
- [206. 反转链表 (Reverse Linked List)](./07_linked_list/0206_reverse_linked_list.md)
- [234. 回文链表 (Palindrome Linked List)](./07_linked_list/0234_palindrome_linked_list.md)
- [141. 环形链表 (Linked List Cycle)](./07_linked_list/0141_linked_list_cycle.md)
- [142. 环形链表 II (Linked List Cycle II)](./07_linked_list/0142_linked_list_cycle_ii.md)
- [21. 合并两个有序链表 (Merge Two Sorted Lists)](./07_linked_list/0021_merge_two_sorted_lists.md)
- [2. 两数相加 (Add Two Numbers)](./07_linked_list/0002_add_two_numbers.md)
- [19. 删除链表的倒数第 N 个结点 (Remove Nth Node From End)](./07_linked_list/0019_remove_nth_from_end.md)
- [24. 两两交换链表中的节点 (Swap Nodes in Pairs)](./07_linked_list/0024_swap_nodes_in_pairs.md)
- [25. K 个一组翻转链表 (Reverse Nodes in k-Group)](./07_linked_list/0025_reverse_nodes_in_k_group.md)
- [138. 随机链表的复制 (Copy List with Random Pointer)](./07_linked_list/0138_copy_list_with_random_pointer.md)
- [148. 排序链表 (Sort List)](./07_linked_list/0148_sort_list.md)
- [23. 合并 K 个升序链表 (Merge k Sorted Lists)](./07_linked_list/0023_merge_k_sorted_lists.md)
- [146. LRU 缓存 (LRU Cache)](./07_linked_list/0146_lru_cache.md)

### 二叉树
- [94. 二叉树的中序遍历 (Binary Tree Inorder Traversal)](./08_binary_tree/0094_inorder_traversal.md)
- [104. 二叉树的最大深度 (Maximum Depth of Binary Tree)](./08_binary_tree/0104_maximum_depth.md)
- [226. 翻转二叉树 (Invert Binary Tree)](./08_binary_tree/0226_invert_binary_tree.md)
- [101. 对称二叉树 (Symmetric Tree)](./08_binary_tree/0101_symmetric_tree.md)
- [543. 二叉树的直径 (Diameter of Binary Tree)](./08_binary_tree/0543_diameter.md)
- [102. 二叉树的层序遍历 (Binary Tree Level Order Traversal)](./08_binary_tree/0102_level_order.md)
- [108. 将有序数组转换为二叉搜索树 (Convert Sorted Array to BST)](./08_binary_tree/0108_sorted_array_to_bst.md)
- [98. 验证二叉搜索树 (Validate BST)](./08_binary_tree/0098_validate_bst.md)
- [230. 二叉搜索树中第 K 小的元素 (Kth Smallest Element in BST)](./08_binary_tree/0230_kth_smallest.md)
- [199. 二叉树的右视图 (Binary Tree Right Side View)](./08_binary_tree/0199_right_side_view.md)
- [114. 二叉树展开为链表 (Flatten Binary Tree to Linked List)](./08_binary_tree/0114_flatten.md)
- [105. 从前序与中序遍历序列构造二叉树 (Construct Binary Tree)](./08_binary_tree/0105_construct_binary_tree.md)
- [437. 路径总和 III (Path Sum III)](./08_binary_tree/0437_path_sum_iii.md)
- [236. 二叉树的最近公共祖先 (Lowest Common Ancestor)](./08_binary_tree/0236_lca.md)
- [124. 二叉树中的最大路径和 (Binary Tree Maximum Path Sum)](./08_binary_tree/0124_max_path_sum.md)

### 图论
- [200. 岛屿数量 (Number of Islands)](./09_graph/0200_number_of_islands.md)
- [994. 腐烂的橘子 (Rotting Oranges)](./09_graph/0994_rotting_oranges.md)
- [207. 课程表 (Course Schedule)](./09_graph/0207_course_schedule.md)
- [208. 实现 Trie (前缀树) (Implement Trie)](./09_graph/0208_implement_trie.md)

### 回溯
- [46. 全排列 (Permutations)](./11_backtracking/0046_permutations.md)
- [78. 子集 (Subsets)](./11_backtracking/0078_subsets.md)
- [17. 电话号码的字母组合 (Letter Combinations)](./11_backtracking/0017_letter_combinations.md)
- [39. 组合总和 (Combination Sum)](./11_backtracking/0039_combination_sum.md)
- [22. 括号生成 (Generate Parentheses)](./11_backtracking/0022_generate_parentheses.md)
- [79. 单词搜索 (Word Search)](./11_backtracking/0079_word_search.md)
- [131. 分割回文串 (Palindrome Partitioning)](./11_backtracking/0131_palindrome_partitioning.md)
- [51. N 皇后 (N-Queens)](./11_backtracking/0051_n_queens.md)

### 二分查找
- [35. 搜索插入位置 (Search Insert Position)](./12_binary_search/0035_search_insert.md)
- [74. 搜索二维矩阵 (Search a 2D Matrix)](./12_binary_search/0074_search_2d_matrix.md)
- [34. 在排序数组中查找元素的第一个和最后一个位置 (Find First and Last Position)](./12_binary_search/0034_find_first_last.md)
- [33. 搜索旋转排序数组 (Search in Rotated Sorted Array)](./12_binary_search/0033_search_rotated.md)
- [153. 寻找旋转排序数组中的最小值 (Find Minimum in Rotated Sorted Array)](./12_binary_search/0153_find_minimum.md)
- [4. 寻找两个正序数组的中位数 (Median of Two Sorted Arrays)](./12_binary_search/0004_median_two_arrays.md)

### 栈
- [20. 有效的括号 (Valid Parentheses)](./13_stack/0020_valid_parentheses.md)
- [155. 最小栈 (Min Stack)](./13_stack/0155_min_stack.md)
- [394. 字符串解码 (Decode String)](./13_stack/0394_decode_string.md)
- [739. 每日温度 (Daily Temperatures)](./13_stack/0739_daily_temperatures.md)
- [84. 柱状图中最大的矩形 (Largest Rectangle in Histogram)](./13_stack/0084_largest_rectangle.md)

### 堆/优先队列
- [215. 数组中的第 K 个最大元素 (Kth Largest Element)](./14_heap/0215_kth_largest.md)
- [347. 前 K 个高频元素 (Top K Frequent Elements)](./14_heap/0347_top_k_frequent.md)
- [295. 数据流的中位数 (Find Median from Data Stream)](./14_heap/0295_find_median.md)

### 贪心
- [121. 买卖股票的最佳时机 (Best Time to Buy and Sell Stock)](./15_greedy/0121_best_time_stock.md)
- [55. 跳跃游戏 (Jump Game)](./15_greedy/0055_jump_game.md)
- [45. 跳跃游戏 II (Jump Game II)](./15_greedy/0045_jump_game_ii.md)
- [763. 划分字母区间 (Partition Labels)](./15_greedy/0763_partition_labels.md)

### 动态规划
- [70. 爬楼梯 (Climbing Stairs)](./16_dp/0070_climbing_stairs.md)
- [118. 杨辉三角 (Pascal's Triangle)](./16_dp/0118_pascals_triangle.md)
- [198. 打家劫舍 (House Robber)](./16_dp/0198_house_robber.md)
- [279. 完全平方数 (Perfect Squares)](./16_dp/0279_perfect_squares.md)
- [322. 零钱兑换 (Coin Change)](./16_dp/0322_coin_change.md)
- [139. 单词拆分 (Word Break)](./16_dp/0139_word_break.md)
- [300. 最长递增子序列 (Longest Increasing Subsequence)](./16_dp/0300_lis.md)
- [152. 乘积最大子数组 (Maximum Product Subarray)](./16_dp/0152_max_product_subarray.md)
- [416. 分割等和子集 (Partition Equal Subset Sum)](./16_dp/0416_partition_equal_subset.md)
- [32. 最长有效括号 (Longest Valid Parentheses)](./16_dp/0032_longest_valid_parentheses.md)

### 多维动态规划
- [62. 不同路径 (Unique Paths)](./17_dp_multi/0062_unique_paths.md)
- [64. 最小路径和 (Minimum Path Sum)](./17_dp_multi/0064_minimum_path_sum.md)
- [5. 最长回文子串 (Longest Palindromic Substring)](./17_dp_multi/0005_longest_palindrome.md)
- [1143. 最长公共子序列 (Longest Common Subsequence)](./17_dp_multi/1143_lcs.md)
- [72. 编辑距离 (Edit Distance)](./17_dp_multi/0072_edit_distance.md)

### 技巧题
- [136. 只出现一次的数字 (Single Number)](./18_techniques/0136_single_number.md)
- [169. 多数元素 (Majority Element)](./18_techniques/0169_majority_element.md)
- [75. 颜色分类 (Sort Colors)](./18_techniques/0075_sort_colors.md)
- [31. 下一个排列 (Next Permutation)](./18_techniques/0031_next_permutation.md)
- [287. 寻找重复数 (Find the Duplicate Number)](./18_techniques/0287_find_duplicate.md)

---

**持续更新中...**
