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
    
len(M,N) is the longest common subsequence for arrays x\[0...M) and y\[0...N). Note that time complexity is exponential O(2<sup>min(M,N)</sup>). Performance can be improved through memoization, but this adds a space complexity of M\*N.

## Dynamic programming solution

There is however also a dynamic programming solution. The straight-forward dynamic programming solution requires additional space (matrix of MxN) to store the values needed for computation, but this can be improved upon. Thus an additional requirements is to use minimal space.

The idea to compute function l row by row is formalized as follows: we introduce an integer array h\[0...N], integer m and accompanying invariants:<br />
P<sub>0</sub>: h = l&#8729;m (i.e. &forall;i : 0&le;i&le;N: h&#8729;i = l&#8729;m&#8729;i))<br/>
P<sub>1</sub>: 0&le;m&le;M<br/>
then P<sub>0</sub> &and; m = M => h&#8729;N = l&#8729;M&#8729;N<br/>
hence m &ne; M is the guard of the repetition where m represents a row index and n represents a column index.<br/>
Substitution of m=0: <br/>
(&forall;i: 0&le;i&le;N: h&#8729;i = l&#8729;0&#8729;i)<br/>
&equiv; \<by definition of l\><br/>
(&forall;i: 0&le;i&le;N: h&#8729;i = 0)

Thus initialization step is:<br/>
k = 0;<br/>
do k &ne; N+1 -> h&#8729;k = 0; k = k + 1; od<br/>
m  = 0;<br/>
{ P<sub>0</sub> &and; P<sub>1</sub> }<br/>
do m &ne; M -> <b>h = l&#8729;(m+1);</b> m = m + 1; od<br/>
with the bolded <b>h = l&#8729;(m+1);</b> remaining to solve for.

P<sub>0</sub>(m = m + 1)<br/>
&equiv; \< by definition of P<sub>0</sub> \><br/>
(&forall;i: 0&le;i&le;N: h&#8729;i = l&#8729;(m+1)&#8729;i)<br/>

For this we introduce another repetition (using integer n) with invariants<br/>
Q<sub>0</sub>: (&forall;i: 0&le;i&le;n: h&#8729;i = l&#8729;(m+1)&#8729;i)<br/>
Q<sub>1</sub>: (&forall;i: n<i&le;N: h&#8729;i = l&#8729;m&#8729;i)<br/>
Q<sub>2</sub>: o&le;n&le;N<br/>
When n=N, then Q<sub>1</sub> is defined over an empty domain, thus N provides a valid upper limit. For the lower limit of 0 we have the following base case:<br/>
(Q<sub>0</sub> &and; Q<sub>1</sub>)(n :=0)<br/>
&equiv; <br/>
h&#8729;0 = l&#8729;(m+1)&#8729;0 &and; (&forall;i: 0<i&le;N: h&#8729;i = l&#8729;m&#8729;i)<br/>
&equiv;  \{ l&#8729;(m+1)&#8729;0 = l&#8729;m&#8729;0 (=0) \}<br/>
P<sub>0</sub>

For n+1:<br/>
Q<sub>0</sub>(n=n+1)<br/>
&equiv; \{ substitute \}<br/>
(&forall;i: 0&le;i&le;n+1: h&#8729;i = ;&#8729;(m+1)&#8729;i)<br/>
&equiv; \{ split off i=n+1, Q<sub>0</sub>\}<br/>
Q<sub>0</sub>(n) &and; h&#8729;(n+1)=l&#8729;(m+1)&#8729;(n+1)<br/>

Based upon the definition of l&#8729;(m+1)&#8729;(n+1) we know it can be expressed in terms of l&#8729;m&#8729;n, l&#8729;(m+1)&#8729;n and l&#8729;m&#8729;(n+1).<br/>
l&#8729;(m+1)&#8729;n = {Q<sub>0</sub>} h&#8729;n<br/>
l&#8729;m&#8729;(n+1) = {Q<sub>1</sub>} h&#8729;(n+1)<br/>
l&#8729;m&#8729;n = {introduce Q<sub>3</sub>} a<br/>
with new invariant Q<sub>3</sub> a = l&#8729;m&#8729;n<br/>

For the base case Q<sub>3</sub>(n=0)<br/>
&equiv;<br/>
l&#8729;m&#8729;0<br/>
&equiv;<br/>
0</br>
thus a = 0 is the initialization step. 

Invariance for Q3 (n := n+1):<br/>
l&#8729;m&#8729;(n+1)<br/>
&equiv; {Q<sub>1</sub>}<br/>
h&#8729;(n+1)<br/>
hence a := h(n+1)

Time complexity is O(M\*N) and space complexity O(N) (note that O(min(M,N)) is easily achieved by choosing the smallest array for y). An observation is that there is a small space advantage to passing the array of shortest length as y although this does not impact overall complexity. All arrays are traversed from left to right, so they can be for example files. Note that we relied upon N for initialization of h. This dependency was removed by introducing a list for h.


```csharpTime 
public int LongestCommonSubsequence<T>(
      IEnumerable<T> x, 
      IEnumerable<T> y, 
      IEqualityComparer<T>? comparer = default) {
   var m = 0;
   using var xEnum = x.GetEnumerator();
   using var yEnum = y.GetEnumerator();
   var h = new List<int>(new [] {0});
   comparer ??= EqualityComparer<T>.Default;
   while (xEnum.MoveNext()) {
      var a = 0;
      var n = 0;
      yEnum.Reset();
      while (yEnum.MoveNext()) {
         if (h.Count <= n+1) {
            h.Add(0);
         }
         if (comparer.Equals(xEnum.Current, yEnum.Current)) {
            var tmp = a;
            a = h[n+1];
            h[n+1] = ++tmp;
         }
         else {
            a = h[n+1];
            if (h[n] > h[n+1]) {
                h[n+1] = h[n];
            }
         }
         n++;
      }
      m++;
   }
   return h[h.Count-1]; 
}
```

A common subsequence can be represented as a set V of pairs (i,j) such that (i,j) &isin; V => x&#8729;i = y&#8729;j<br/>
V is totally ordered with respect to the product order on \[0...M)x\[0...N): (a,b) < (c,d) &equiv; a<c &and; b<d<br/>
Add to the algorithm invariants by introducing an array v\[0...N] of set \[0...M)x\[0...N) and demonstrate that |v\[n]|=h&#8729;n<br/>


```csharpTime 
public record Sequence {
    public int Count {get;}
    public Sequence? Previous {get;}
    public (int,int) Value {get;}
    
    public Sequence(
            Sequence? previous,
            int m,
            int n) {
        Previous = previous;
        Count = (previous?.Count??0)+1;
        Value = new(m,n);
    }
        
    public IEnumerable<(int,int)> Enumerate() {
         if (Previous != default) {
            foreach(var item in Previous.Enumerate()) {
                yield return item;
            }
        }
        yield return value;
    }
}

public (int,int)[] LongestCommonSubsequence<T>(
      IEnumerable<T> x, 
      IEnumerable<T> y, 
      IEqualityComparer<T>? comparer = default) {
   var m = 0;
   using var xEnum = x.GetEnumerator();
   using var yEnum = y.GetEnumerator();
   var v = new List<Sequence?>(new [] { new default(Sequence?) });
   comparer ??= EqualityComparer<T>.Default;
   while (xEnum.MoveNext()) {
      var a = default(Sequence?);
      var n = 0;
      yEnum.Reset();
      while (yEnum.MoveNext()) {
         if (v.Count <= n+1) {
             v.Add(default);
         }
         if (comparer.Equals(xEnum.Current, yEnum.Current)) {
            var tmp = a;
            a = v[n+1];
            v[n+1] = new Sequence(tmp, m, n);
         }
         else {
            a = v[n+1];
            if ((v[n]?.Count??0) > (v[n+1]?.Count??0)) {
                v[n+1] = v[n];
            }
         }
         n++;
      }
      m++;
   }
   return v[v.Count-1]?.Enumerate().ToArray() ?? Array.Empty<(int,int)>(); 
}
```

Space complexity for v is O(M\*N) worst case (when all elements are equal), but will typically be better since it is in the order of matches between elements from x and y. 

A final note is that solutions with lower complexity do exist, for example <a href="https://en.wikipedia.org/wiki/Hunt%E2%80%93Szymanski_algorithm">the Hunt-Szymanski algorithm</a> or <a href="https://en.wikipedia.org/wiki/Hirschberg%27s_algorithm">Hirschberg's algorithm</a>.
