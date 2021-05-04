Given M &ge; 0, N &ge; 0 and arrays x`[0...M)` and y`[0...N)`, derive a program for the computation of the length of a longest common subsequence (LCM) of x and y. 
Define function l&#8729;m&#8729;n as the length of a LCM of x`[0...m)` and y`[0...n)` with 0&le;m&lt;M and 0&le;n&lt;N.
The longest common subsequence of x and y is given by l&#8729;M&#8729;N.
Trivially, l&#8729;0&#8729;n = 0 and l&#8729;m&#8729;0 = 0 for any 0&le;m&lt;M and 0&le;n&lt;N.
Derive l&#8729;(m+1)&#8729;(n+1): 
if x\[m\] = y\[n\]
