# Matrix multiplication

Given are an integer L (L &ge; 1), an array A\[0...L) of matrices and an array r\[0...L] of integers. Matrix A&sdot;i has r&sdot;i rows and r&sdot;(i+1) columns (matrix A&sdot;i is an r&sdot;i x r&sdot;(i+1) matrix). Problem: (&prod;p: 0&le;p<L: A&sdot;p) has to be computed as efficient as possible.

For example given Matrix A 5x2, matrix B 2x4 and matrix C 4x2 compute A&sdot;B&sdot;C which can either be computed as (A&sdot;B)&sdot;C or as A&sdot;(B&sdot;C). Multiplication of an axb matrix and a bxc matrix costs a&sdot;b&sdot;c. (A&sdot;B)&sdot;C costs 80 steps (5x2x4 = 40, 40x2=80). However, A&sdot;(B&sdot;C) costs 36 steps (2x4x2=16, 5x2x2=20). Conclusion is that the order of multiplication impacts the number of steps needed.
