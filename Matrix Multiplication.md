# Matrix multiplication

Given are an integer L (L &ge; 1), an array A\[0...L) of matrices and an array r\[0...L] of integers. Matrix A&sdot;i has r&sdot;i rows and r&sdot;(i+1) columns (matrix A&sdot;i is an r&sdot;i x r&sdot;(i+1) matrix). Problem: (&prod;p: 0&le;p<L: A&sdot;p) has to be computed as efficient as possible.

For example given Matrix A 5x2, matrix B 2x4 and matrix C 4x2 compute A&sdot;B&sdot;C which can either be computed as (A&sdot;B)&sdot;C or as A&sdot;(B&sdot;C). Multiplication of an axb matrix and a bxc matrix costs a&sdot;b&sdot;c. (A&sdot;B)&sdot;C costs 80 steps (5x2x4 = 40, 40x2=80). However, A&sdot;(B&sdot;C) costs 36 steps (2x4x2=16, 5x2x2=20). Conclusion is that the order of multiplication impacts the number of steps needed.

Let C&sdot;i&sdot;j = minimal cost for computing (&prod;p: i&le;p<j: A&sdot;p) for 0&le;i<j&le;L , then C&sdot;0&sdot;L is the minimal cost for computing (&prod;p: 0&le;p<L: A&sdot;p). 

C&sdot;i&sdot;(i+1) = 0 (0&le;i<L)

C&sdot;i&sdot;j is computed as follows: C&sdot;i&sdot;j is split at some k for the last matrix multiplication (A&sdot;i ... A&sdot;(k-1)) * (A&sdot;k ... A&sdot;(j-1)). This multiplies a r&sdot;i x r&sdot;k matrix with a r&sdot;k * r&sdot;j matrix. The cost of splitting at k is C&sdot;i&sdot;k + C&sdot;k&sdot;j + r&sdot;i * r&sdot;k * r&sdot;j. We want the minimum across all k's that we can choose in this expression. Thus: C&sdot;i&sdot;j = (min k: i<k<j: C&sdot;i&sdot;k + C&sdot;k&sdot;j + r&sdot;i * r&sdot;k * r&sdot;j). At this point we note that the cost function C is 0 on the diagonal of LxL square and we are calculating the other values in the square. We introduce an array X\[0...L)x\[1...L] of integer to represent this square.

