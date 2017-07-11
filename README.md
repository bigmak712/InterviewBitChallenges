# InterviewBitChallenges

<img src='http://i.imgur.com/jnGyf4v.gif' title='Video Walkthrough' width='' alt='Video Walkthrough' />

Checkpoint 1:
    O(n)

Checkpoint 2: 

PrettyPrint

    public class Solution {
        public int[][] prettyPrint(int A) {
            int size = (2 * A) - 1;
            int[][]result = new int[size][size];
            for (int i = 0; i < size; i++) {
                int dec = A - 1 - Math.abs(A - 1 - i);
                int repeat = size - dec;
                int num = A;
                for (int j = 0; j < dec; j++) {
                    result[i][j] = num;
                    num = num - 1;
                }
                for(int k = dec; k < repeat; k++) {
                    result[i][k] = num;
                }
                for(int l = repeat; l < size; l++) {
                    num = num + 1;
                    result[i][l] = num;
                }
            }
            return result;
        }
    }

Checkpoint 3:

Kth Smallest Element in the Array  

    public class Solution {
        // DO NOT MODIFY THE ARGUMENTS WITH "final" PREFIX. IT IS READ ONLY
        public int kthsmallest(final int[] A, int B) {
            PriorityQueue<Integer> pq = new PriorityQueue<Integer>(B, Collections.reverseOrder());
            for(int i = 0; i < B; i++) {
                pq.add(A[i]);
            }
            for(int j = B; j < A.length; j++) {
                if(pq.peek() > A[j]) {
                    pq.poll();
                    pq.add(A[j]);
                }
            }

            return pq.peek();
        }
    }

NUMRANGE

    public class Solution {
        public int numRange(int[] A, int B, int C) {
            int num_sequences = 0;
            int curr_sum = 0;
            for(int i = 0; i < A.length; i++) {
                for(int j = i; j < A.length; j++) {
                    curr_sum = curr_sum + A[j];
                    if(curr_sum > C) {
                        break;
                    }
                    if (curr_sum >= B && curr_sum <= C){
                        num_sequences++;
                    }
                }
                curr_sum = 0;
            }
            return num_sequences;
        }
    }

Checkpoint 4:

SUBTRACT

    public class Solution {
        public ListNode subtract(ListNode A) {
            if(A == null){
                return A;
            }
            int count = 1;
            ArrayList<Integer> values = new ArrayList<Integer>();
            values.add(A.val);
            ListNode next_node = A.next;

            // Get the count and values of the nodes in the list
            while (next_node != null){
                values.add(next_node.val);
                count++;
                next_node = next_node.next;
            }

            // count: number of nodes
            // values: array list containing each value
            ListNode curr_node = A;
            int sub = count - 1;
            for(int i = 0; i < count/2; i++) {
                curr_node.val = values.get(sub) - curr_node.val;
                sub--;
                curr_node = curr_node.next;
            }
            return A;
        }
    }

NEXTGREATER
    
    public class Solution {
        public int[] nextGreater(int[] A) {
            int[]result = new int[A.length];
            Arrays.fill(result, -1);
            for(int i = 0; i < A.length; i++) {
                for(int j = i; j < A.length; j++) {
                    if(A[j] > A[i]){
                        result[i] = A[j];
                        break;
                    }
                }
            }
            return result;
        }
    }

Checkpoint 5:

All Unique Permutations

    public class Solution {
        ArrayList<Integer> map = new ArrayList<Integer>();

        public int[][] permute(int[] A) {
            Arrays.sort(A);
            ArrayList<ArrayList<Integer>> res = new ArrayList<ArrayList<Integer>>();
            permutation(A, 0, res);
        
            ArrayList<int[]> res1 = new ArrayList<int[]>();
            for(ArrayList<Integer> list : res) {
                res1.add(listToArray(list));
            }
        
            int[][] res2 = new int[res1.size()][];
            for(int i = 0; i < res1.size(); i++) {
                res2[i] = res1.get(i);
            }
        
            return res2;
        }
    
        public void permutation(int[] num, int start, ArrayList<ArrayList<Integer>> res) {
            if(start >= num.length) {
                ArrayList<Integer>item = arrayToList(num);
            
                int temp_code = hash(num);
                if(!map.contains(temp_code)){
                    map.add(temp_code);
                    res.add(item);
                }
            }    
        
            for(int j = start; j <= num.length - 1; j++) {
                swap(num, start, j);
                permutation(num, start + 1, res);
                swap(num, start, j);
            }
        }
    
        public int[] listToArray(ArrayList<Integer> list) {
            int[] arr = new int[list.size()];
            for(int i = 0; i < list.size(); i++) {
                arr[i] = list.get(i);
            }
            return arr;
        }
    
        public ArrayList<Integer> arrayToList(int[] arr) {
            ArrayList<Integer> list = new ArrayList<Integer>();
            for(int val: arr) {
                list.add(val);
            }
            return list;
        }
    
        public void swap(int[] arr, int i, int j) {
            int temp = arr[i];
            arr[i] = arr[j];
            arr[j] = temp;
        }
        public int hash(int[] arr) {
            int code = 7;
            for(int i = 0; i < arr.length; i++){
                code += (code*31) + (53*i) + (7*arr[i]) - (code*i*arr[i]*3719);
            }
            return code;
        }
    }

Longest Consecutive Sequence

    public class Solution {
    // DO NOT MODIFY THE ARGUMENTS WITH "final" PREFIX. IT IS READ ONLY
    public int longestConsecutive(final int[] A) {
        
        int curr_key;
        int left_len;
        int right_len;
        int curr_len;
        int max_len = 0;
        HashMap<Integer, Integer> hm = new HashMap<Integer, Integer>();
        
        for(int i = 0; i < A.length; i++) {
            curr_key = A[i];
            
            // Checks for duplicates
            if(!hm.containsKey(curr_key)) {
                
                // Get the length of the sequence on the left if it exists
                if(hm.containsKey(curr_key - 1)){
                    left_len = hm.get(curr_key - 1);
                }
                else {
                    left_len = 0;
                }
                
                // Get the length of the sequence on the right if it exists
                if(hm.containsKey(curr_key + 1)){
                    right_len = hm.get(curr_key + 1);
                }
                else {
                    right_len = 0;
                }
                
                // Store the current key with its length
                curr_len = left_len + right_len + 1;
                hm.put(curr_key, curr_len);
                
                max_len = Math.max(curr_len, max_len);
                
                // Store the current length in the edges of the sequence
                hm.put(curr_key - left_len, curr_len);
                hm.put(curr_key + right_len, curr_len);
            }
        }
        return max_len;
    }
}

