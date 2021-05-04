# Longest Common Subsequence of two arrays

Given M &ge; 0, N &ge; 0 and arrays x\[0...M) and y\[0...N), derive a program for the computation of the length of a longest common subsequence of x and y. 
Define function l&#8729;m&#8729;n as the length of a LCM of x\[0...m) and y\[0...n) with 0&le;m&lt;M and 0&le;n&lt;N.
The longest common subsequence of x and y is given by l&#8729;M&#8729;N. 

The challenge is to find the longest common subsequence of elements that appear left-to-right (but not necessarily in a contiguous block) in both the arrays. For example \[0,1,2,3,4,5\] and \[8,0,1,3,4,5,6\] has a longest common subsequence of 5 \[0,1,3,4,5\]. 

## Derivation

Trivially, l&#8729;0&#8729;n = 0 and l&#8729;m&#8729;0 = 0 for any 0&le;m&lt;M and 0&le;n&lt;N.

Derive l&#8729;(m+1)&#8729;(n+1): 

if x\[m\] = y\[n\] then every common subsequence of x\[0...m) and y\[0...n) can be extended with x\[m\] so l&#8729;(m+1)&#8729;(n+1) = 1 + l&#8729;m&#8729;n

if x\[m\] &ne; y\[n\] this implies (&forall;z:: z &ne; x\[m\] &or; z &ne; y\[n\]). Intuitively: if there is a z equal to a and b, then a and b are equal. Thus  x\[m\] &ne; y\[n\] implies  l&#8729;(m+1)&#8729;(n+1) =  l&#8729;m&#8729;(n+1) max  l&#8729;(m+1)&#8729;n because either x\[m\] or y\[n\] does not play a role for a LCM.

## Recursive solution

A natural solution is a recursive function len(m,n:int) : int ;<br/>
{ pre: 0&le;m&le;M, 0&le;n&le;N, post: len = l&#8729;m&#8729;n }<br/>
if m = 0 &or; n = 0 -> len := 0<br/>
elif x(m-1) = y(n-1) -> len := 1 + len(m-1,n-1)<br/>
else len := len(m-1,n) max len(m,n-1)<br/>
    
len(M,N) is the longest common subsequence for arrays x\[0...M) and y\[0...N).

## Dynamic programming solution

There is however also a dynamic programming solution. The dynamic programming solution requires additional space (matrix of MxN) to store the values needed for computation. Thus an additional requirement is to minimize the storage requirement.

The idea to compute function l row by row is formalized as follows: we introduce an integer array h\[0...N], integer m and accompanying invariants:<br />
P<sub>0</sub>: h = l&#8729;m (i.e. &forall;i : 0&le;i&le;N: h&#8729;i = l&#8729;m&#8729;i))
P<sub>1</sub>: 0&le;m&le;M
then P<sub>0</sub> &and; m = M => h&#8729;N = l&#8729;M&#8729;N<br/>
hence m &neq; M is the guard of the repetition where m represents a row index and n represents a column index.<br/>
Substitution of m=0: <br/>
(&forall;i: 0 $le;i&le;N: h&#8729;i = l&#8729;0&#8729;i)<br/>
&equiv; \<by definition of l\><br/>
(&forall;i: 0&le;i&le;N: h&equiv;i = 0)
