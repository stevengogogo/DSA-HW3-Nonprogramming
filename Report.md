---
toc:
  depth_from: 2
  depth_to: 3
  ordered: false
---

[![hackmd-github-sync-badge](https://hackmd.io/JZUhv-byTRqV46cumxZ9Xw/badge)](https://hackmd.io/JZUhv-byTRqV46cumxZ9Xw)

<center>

# Homework 3: Nonprogramming part

**邱紹庭**

`r07945001@ntu.edu.tw`

</center>

**Table of contents**

[toc]

---

## Problem 1 - Hash (60pt)


### 1. (10pt) 

The probability of no collision ($X'$)[^UniHash]:

$$\begin{align}
P(X') &= \frac{n^2}{n^2} \frac{n^2-1}{n^2} \cdots \frac{n^2-n+1}{n^2} \\
      &= \prod_{i=1}^{n} \frac{n^2 - i + 1}{n^2}
\end{align}$$

On the other hand, the probability of any collision ($X$):

$$\begin{align}
P(X) &= 1 - P(X')\\
     &= 1 - \prod_{i=1}^{n} \frac{n^2 - i + 1}{n^2}
\end{align}$$


[^UniHash]: [If we store n keys in a hash table of size m=n^2 , then what is the probability of any collision ?](https://gateoverflow.in/41109/store-keys-hash-table-size-then-what-probability-collision)

### 2. (10pt) (WIP)

**💡 Idea**
1. Get expected number of collision
2. Get number of unique slots 
3. Solve equation with the objective unique number.

**Expected number of collision**

Suppose $n$ keys are inserted into the space $P$. The expected number of collision is

$$E[\text{# of collision}] = \sum_{i=1}^{n} \frac{n-i}{|P|} = \frac{n^2 - n}{2|P|}$$

**Expected number of unique hashes**

Since there are $n$ function calls, the total hashes is the summation of unique hashes and number of collisions which are duplicated.

Since we know the expected number of collion, the exptected amount of unique hashes is derived from the substraction:

$$\begin{align}
E[\text{# of unique hashes}] &= n  - E[\text{# of collision}] \\
& = n - \frac{n^2 - n}{2|P|}\\
\end{align}$$

**How much calls we need?**




### 3. (20pt) (WIP)


### 4. (20pt) (WIP)

---

## Problem 2 - String matching (60pt)


### 1. (10pt)

**💡 Idea**
1. Use **Robin-Karp algorithm** to hash strings.
2. **Preprocessing**: Create an array of hashes. Let the `array[i]` stores the hash of `S[i..N]`. This requires 

|Index|1|2|3|4|
|---|---|---|---|---|
|Sting|S1|S2|S3|S4|
|Hash|H1-1|H1-2|H1-3|H1-4|

4. **Query**: Use **Substraction** technique to get the hash value in the region of `S[i..j]` with $O(1)$ time complexity (**Fig. 2-1**).

<center>

<img height=200 src="https://i.imgur.com/DXyMche.jpg">

**Fig 2-1.** Generate `hash(S[2..4])` from `HL` and `HR`.

</center>

**🔧 Implementation**

**Preprocessing**

```cpp=
function Preprocessing(Str, d, q)
    Initialize hash_list[1,...,length(Str)]
    
    p = 0
    for i = 1 to length(Str)
        p = (dp + Str[i]) mod q
        hash_list[i] = p
    end
    
    return hash_list
end
```
- `Str`: Text
- `d`: The radix 
- `q`: modulo

**Hash Generation**

```cpp=
function get_hash(hash_list, d, q, l, r)
    l_hash = hash_list[l]
    r_hash = hash_list[r]
    
    i_hash = (l_hash - r_hash) / d^{r-l}
    i_hash = i_hash mod q
    return i_hash
end
```

**Matching**

```cpp=
function matching(Str, hash_list, l1, l2, n)
    h1 = get_hash(hash_list, l1, l1+n-1)
    h2 = get_hash(hash_list, l2, l2+n-1)
    
   if h1==h2
       if(Str[l1...l1+n-1] == Str[l2...l2+n-1])
           return true
   return false
end
```


**🔢 Analysis**

- Preprocessing:
    - Time: $O(n)$
    - Space: $O(n)$ 
    - where $n$ is the length of the text
- Hash generation
    - Time: $O(1)$
    - Space: $O(1)$
- Matching
    - Similar to Rabin-Karp Matching
    - Time: $O(n)$. 
        - In this case, two string poccess equal lengths. The original time complexity $O((m-n+1)n)$ decays to $O(n)$ where $n$ is the length of the sub-string.
    - Space: $O(1)$.


### 2. (10pt)

- `S[1..N]` = `bcdabcde`

|$i$|$x(i) = \max(p)$|Domain|
|---|---|---|
|1|3|`|bcd|abcde`|
|2|3|`cda|bcd|e`|
|3|3|`da|bcd|e`|
|4|3|`a|bcd|e`|
|5|3|`|bcd|e`|
|6|0|`cde`|
|7|0|`de`|
|8|0|`e`|


>Noted that the region `S[1..p] == S[i..i+p-1]` is marked with `| |`.

### 3. (20pt) 

**💡 Idea**
- From [P2-2](#2-10pt), we can observe that $\max(P)$ is trivally **monotonous increasing** when $i$ is in decreasing order.
    - For $i = N$, we can get $\max(P) = \{p|0,1\}$ with constant time complexity.
    - For $i\in [1,\cdots, n]$ where $n<N$, we can find potential new maximum $p$ in the region `S[i..i+p'-1]` where $p' \in [p, N-i+1]$. 




|<img height=200 src="https://i.imgur.com/8jnGMeG.jpg">|<img height=200 src="https://i.imgur.com/BwhGNmO.jpg">|
|---|---|

**Fig 2-1.** Example of calling hash function. The arguments `l` and `r`  of `get_hash` function proposed in [P2-1](#1-10pt). The symbol $\vdash$ represents a function call.


**🔧 Implementation**

```cpp=
function Max_Duplicate_Length(S)
    //Preprocessing
    hash_list = Preprocessing(S, d, q)
    
    //Iteration
    p = 0
    
    for i = length(S) to 1
        same = matching(S, hash_list, 1, i, p)
        while(same)
            p+=1
            if (p+i > length(S)) break end
            same = matching(S, hash_list, 1, i, p)
        end
    end
    
    return S[1..p]
end
```

- `Preprocessing(Str, d, q)` function is implemeneted in [P2-1](#1-10pt) with $O(n)$ time complexity where $d$ and $q$ can be any positive numbers. This function returns a list of hashes based on Robin-Karp Algorithm.
- `matching(Str, hash_list, l1, l2, n)` function is implemented in [P2-1](#1-10pt) with $O(1)$ time complexity.

**🔢 Analysis**

- Prove `p` is the maximum length
    - When `i=length`, `p` is the maximum value.
    - Suppose `i=k` has the maximum value $p_k$. The while loop keeps $p_{k+1} \geq p_k$ and iterates until $p_{K+1} +1$ which is not valid. Thus `i=k+1` has the maximum value of $p_{k+1}$. 
- Time Complexity
    - Preprocessing: $O(n)$ (Derived in [P2-1](#1-10pt))
    - Iteration: $O(n)$
        - The worst case of single iteration is calling the `matching` function with smaller or equal to maximum p in total. The total function calls is equal to  the maximum of $P$. As described in **Fig 2-1**, when $x(i)$ is no more than $max_{j\in[i,N]} x(j)$. It only requires one function call. On the other hand, if $x(i) > max_{j\in[i,N]} x(j)$, it will continue the recorded longest duplicated pattern and incrementally adds `p_current`. Thus, we can show that the time complexity:



$$
\begin{align}
\text{Time Complexity} &= \text{Preprocessing} + \text{length of P} + \text{Screening over i}\\
                       &= \Theta(n) + \Theta(n_{p}) + \Theta(n)  \\
        &= \Theta(n)
\end{align}
$$

- Space Complexity
    - Preprocessing: $\Theta(n)$
    - Iteration: $\Theta(1)$



### 4. (20pt) 

**💡 Idea**
- Modified `Rabin-Karp Matcher` [^RKMatch]
    - Count how many shifts `s` are valid.


**🔧 Implementation**

```cpp=
RABIN-KARP-MATCHER(str, X, d, q)
    n = Str.length
    m = X.length
    h = d^(m-1) mod q
    p = 0
    t0 = 0
    count = 0
    for i = 1 to m
     p = (d*p+ X[i]) mod q
     t0 = (d*t0 + + X[i]) mode q
    
    ts = t0
    for s = 0 to n-m
        if p==ts
            if X[1..m] == Str[s+1..s+m]
                count+=1
            end
        end
        
        if(s<n-m)
            ts = (d(ts-X[s+1]h) + T[s+m+1]) mod q
        end
    end
    
    return count
end
```


**🔢 Analysis**

- Time: 
    - Preprocessing: $\Theta(m)$
    - Matching time: $\Theta(n)$ 
- Space: $O(n)$


[^RKMatch]: CLRS. PP.993

---

## Problem 3 - Having fun with Disjoint Sets (80pt)


### 1. (15pt) (WIP)

**💡 Idea**
- 剛開始所有 vertex 都是黑色
- 每次加 edge . 上紅黑兩色, 黑-紅. 如果不行, 就 false



**📖 Ref**: [^findbipart]
[^findbipart]: [How to Find If a Graph Is Bipartite?](https://www.baeldung.com/cs/graphs-bipartite)


### 2. (15pt) (WIP)


### 3. (15pt) (WIP)


### 4. (15pt) (WIP)


### 5. (20pt) (WIP)


---

## References