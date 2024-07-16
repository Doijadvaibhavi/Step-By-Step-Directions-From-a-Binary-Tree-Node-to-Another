# Step-By-Step-Directions-From-a-Binary-Tree-Node-to-Another

You are given the root of a binary tree with n nodes. Each node is uniquely assigned a value from 1 to n. You are also given an integer startValue representing the value of the start node s, and a different integer destValue representing the value of the destination node t.

Find the shortest path starting from node s and ending at node t. Generate step-by-step directions of such path as a string consisting of only the uppercase letters 'L', 'R', and 'U'. Each letter indicates a specific direction:

'L' means to go from a node to its left child node.
'R' means to go from a node to its right child node.
'U' means to go from a node to its parent node.
Return the step-by-step directions of the shortest path from node s to node t.

Example 1:

Input: root = [5,1,2,3,null,6,4], startValue = 3, destValue = 6
Output: "UURL"
Explanation: The shortest path is: 3 → 1 → 5 → 2 → 6.

Example 2:
Input: root = [2,1], startValue = 2, destValue = 1
Output: "L"
Explanation: The shortest path is: 2 → 1.
 
Constraints:

The number of nodes in the tree is n.
* 2 <= n <= 105
* 1 <= Node.val <= n
* All the values in the tree are unique.
* 1 <= startValue, destValue <= n
* startValue != destValue

 # SOLUTION

# Intuition
First of all, before I explain you this approach I recommend you solving first this problem to understand basic concept of the LCA: problem

In the tree there's exactly one path from node to node and if we look at some examples here's pattern how we're reaching destination node
This is pattern: we go up to LCA of the nodes given to us and then go from it to destination node, so "U" will never be at the end of the path if path contains anything except "U"
But in fact we don't need to explicitly find LCA, all we want to do is to find paths from root to given nodes and then "cut" their common path
When we found the length from the root to LCA or the length of common paths to start node and destination node we want to first go up as disscussed above up to LCA so length_from_root_to_start_node - length_from_root_to_LCA "U" and then go to the right node which is just part of path that is not common between start_node and destination_node

# Coding
Define Helper Function: find_path_from_root is defined to recursively find the path from the root to a specified target node (target_value). It appends "R" or "L" to path_to_append to denote right or left moves, respectively.
Calculate Paths: Initialize path_to_start and path_to_destination as empty lists. Populate them using find_path_from_root with startValue and destValue, respectively.
Find Common Path Length: Initialize common_path_len to determine the length of the shared path between path_to_start and path_to_destination. Iterate until paths differ or reach the end.
Construct Result:
Initialize res with "U" repeated for each step from startValue to the lowest common ancestor (LCA) with destValue.
Append the path from the LCA to destValue excluding the common path.
Return Constructed String: Join res into a single string and return it as the result.

 Complexity Analysis
* Time complexity: O(n), this is still O(n) since we traverse through whole binary tree but this approach is faster in real-world situations due to the smaller coef in front of n
* Space complexity: O(n), since we use two arrays to store the path

# JAVA CODE

class Solution {

    public String getDirections(TreeNode root, int startValue, int destValue) {
    
        List<String> pathToStart = new ArrayList<>();
        
        List<String> pathToDestination = new ArrayList<>();
        
        findPathFromRoot(root, startValue, pathToStart);
        
        findPathFromRoot(root, destValue, pathToDestination);

        int commonPathLen = 0;
        
        while (commonPathLen < pathToStart.size() && commonPathLen 
        
        < pathToDestination.size() &&  pathToStart.get(commonPathLen)
        
        .equals(pathToDestination.get(commonPathLen))) {
        
            commonPathLen++;
        }

        List<String> res = new ArrayList<>();
        
        for (int i = 0; i < pathToStart.size() - commonPathLen; i++) {
        
            res.add("U");
            
        }
        res.addAll(pathToDestination.subList(commonPathLen, pathToDestination.size()));

        return String.join("", res);
    }

    private boolean findPathFromRoot(TreeNode curNode, int targetValue, List<String> pathToAppend) {
    
        if (curNode == null) {
        
            return false;
        }
        if (curNode.val == targetValue) {
        
            return true;
        }

        pathToAppend.add("R");
        
        if (findPathFromRoot(curNode.right, targetValue, pathToAppend)) {
        
            return true;
        }
        pathToAppend.remove(pathToAppend.size() - 1);

        pathToAppend.add("L");
        
        if (findPathFromRoot(curNode.left, targetValue, pathToAppend)) {
        
            return true;
        }
        pathToAppend.remove(pathToAppend.size() - 1);

        return false;
    }
}
