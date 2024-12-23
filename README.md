# Minimum-Number-of-Operations-to-Sort-a-Binary-Tree-by-Level

You are given the root of a binary tree with unique values.
In one operation, you can choose any two nodes at the same level and swap their values.
Return the minimum number of operations needed to make the values at each level sorted in a strictly increasing order.
The level of a node is the number of edges along the path between it and the root node.


class Solution:
    def minimumOperations(self, root: Optional[TreeNode]) -> int:
        def min_swaps_to_sort(arr: List[int]) -> int:
            n = len(arr)
            indexed_arr = [(val, i) for i, val in enumerate(arr)]
            indexed_arr.sort()  # Sort by value
            visited = [False] * n
            swaps = 0

            for i in range(n):
                if visited[i] or indexed_arr[i][1] == i:
                    continue

                cycle_size = 0
                x = i

                while not visited[x]:
                    visited[x] = True
                    x = indexed_arr[x][1]
                    cycle_size += 1

                if cycle_size > 1:
                    swaps += cycle_size - 1

            return swaps

        queue = deque([root])
        total_swaps = 0

        while queue:
            level_size = len(queue)
            current_level = []

            for _ in range(level_size):
                node = queue.popleft()
                current_level.append(node.val)

                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)

            total_swaps += min_swaps_to_sort(current_level)

        return total_swaps

