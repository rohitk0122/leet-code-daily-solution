# leet-code-daily-solution
class Solution {
    public int maxNumEdgesToRemove(int n, int[][] edges) {
                int[] rootA = new int[n + 1];
        int[] rootB = new int[n + 1];
        for (int i = 1; i <= n; i++) {
            rootA[i] = i;
            rootB[i] = i;
        }

        int res = 0;
        int aliceEdges = 0;
        int bobEdges = 0;

        // Alice and Bob
        for (int[] edge : edges) {
            if (edge[0] == 3) {
                boolean unionA = uni(edge[1], edge[2], rootA);
                boolean unionB = uni(edge[1], edge[2], rootB);
                if (unionA || unionB) {
                    aliceEdges += unionA ? 1 : 0;
                    bobEdges += unionB ? 1 : 0;
                } else {
                    res++;
                }
            }
        }

        int[] rootA_copy = rootA.clone();

        // only Alice
        for (int[] edge : edges) {
            if (edge[0] == 1) {
                if (uni(edge[1], edge[2], rootA)) {
                    aliceEdges++;
                } else {
                    res++;
                }
            }
        }

        // only Bob
        rootA = rootA_copy;
        for (int[] edge : edges) {
            if (edge[0] == 2) {
                if (uni(edge[1], edge[2], rootB)) {
                    bobEdges++;
                } else {
                    res++;
                }
            }
        }

        return (aliceEdges == n - 1 && bobEdges == n - 1) ? res : -1;
    }

    private boolean uni(int a, int b, int[] root) {
        int rootA = find(a, root);
        int rootB = find(b, root);
        if (rootA == rootB) {
            return false;
        }
        root[rootA] = rootB;
        return true;
    }

    private int find(int a, int[] root) {
        if (root[a] != a) {
            root[a] = find(root[a], root);
        }
        return root[a];

    }
}
