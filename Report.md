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

The probability of no collision ($X'$)[^AC-P1-1]:


$$\begin{align}
Pr(X') &= \frac{\text{Combination of non-overlapping hashes}}{\text{All combination of hashes}} \\
       &= \frac{C^{n^2}_{n}}{H^{n^2}_{n}} \\
       &= \frac{C^{n^2}_{n}}{C^{n^2 + n - 1}_{n^2 - 1}}\\
       &= \frac{ \frac{(n^2)!}{(n^2-n)!n!} }{ \frac{(n^2+n-1)!}{(n^2-1)!n!} }\\
       &= \frac{ \frac{(n^2)!}{(n^2-n)!} }{ \frac{(n^2+n-1)!}{(n^2-1)!} }\\
       &= \frac{(n^2)!(n^2-1)!}{(n^2-n)!(n^2+n-1)!}_{\#}
\end{align}$$


**Definition**

- $C^{n}_{k} = \frac{n!}{k!(n-k)!}$
- $H^{n}_{k} = C^{n+k-1}_{n-1} = \frac{(n+k-1)!}{k!(n-1)!}$


[^AC-P1-1]: Thanks to 張杰輝's inspiration.

### 2. (10pt) 

**💡 Idea**

Suppose we add unique keys one by one. Inserting first unique key at start require extected $1$ time of insertion. For second unique key, since there is already one position occupied, the acquisition of second key requires $\frac{|P|}{|P|-1}$ expected times of insertion[^AC-P1-1].


**🔧 Formulation**

For inserting $a+1^{th}$ unique keys when there is already $a$ slots occupied. Let $p$ be the probability of success insertion of unique key with single trial. The probability of success insertion after $n$ trials is

$$Pr_{a+1}(p_{a+1},n) = (1-p_{a+1})^{n-1}p_{a+1}$$

where $p_{a+1} = \frac{|P|-a}{|P|}$ and $n\in \{1,...,\infty\}$. The probability  $Pr_{a+1}(p_{a+1},n) \in$ **Geometric distribution**.

Since $Pr_a$ belongs to geometric distribution, the expected value is[^geom]

$$E[\text{Times to inserting $a+1^{th}$ key} | a \text{ slots occupied in } P] = \frac{1}{Pr_{a+1}} =\frac{|P|}{|P|-a}$$

For inserting $k$ keys,

$$E[\text{# of inserting $k$ keys}] = \sum_{i=0}^{\frac{\lfloor P \rfloor}{4} -1}\frac{|P|}{|P|-i}_{\#}$$


[^geom]: https://en.wikipedia.org/wiki/Geometric_distribution

### 3. (20pt) 
- $h(k,i) = (h_1(k)+i)~mod~m$
- $h_{1}(k) = k~mod~m$
- $m=11$

**Open addressing with linear probing**

|k|h1(k)|0|1|2|3|4|5|6|7|8|9|10|
|---|---|---|---|---|---|---|---|---|---|---|---|---|
|18|7||||||||18|||
|34|1||34||||||18|||
|9|9||34||||||18||9|
|37|4||34|||37|||18||9|
|40|7||34|||37|||18|40|9|
|32|9||34|||37|||18|40|9|32|
|89|1||34|89||37|||18|40|9|32|


**Open addressing with double hashing**

- $h(k,i) = (h_1(k) + ih_2(k))~mod~m$
- Primary Hash function: $h_{1}(k) = k~mod~m$
- Secondary hash function: $h_{2}(k) = 1+ (k~mod~(m-1))$
- $m=11$

|k|h1(k)|h2(k)|0|1|2|3|4|5|6|7|8|9|10|
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|18|7|9||||||||18|||
|34|1|5||34||||||18|||
|9|9|10||34||||||18||9|
|37|4|8||34|||37|||18||9|
|40|7|1||34|||37|||18|40|9|
|32|9|3||34|||37|||18|40|9|32|
|89|1|10|89|34|||37|||18|40|9|32|


- 40
    - H1:40%11 = 7 
    - H2: 1+(40%10) = 1 
- 32
    - H1: 32%11 = 10
- 89
    - H1: 89%11=1
    - H2: 1+ 89%10 = 10


### 4. (20pt) 


**Table T1**[^AC-P1-1]

- $h_1(k)=k~mod~7$

|k|h1(k)|0|1|2|3|4|5|6|
|---|---|---|---|---|---|---|---|---|
|6|6|||||||6|
|31|3||||31|||6|
|2|2|||2|31|||6|
|41|6|||2|31|||41|
|30|2|||30|31|||6|
|45|3|||30|45|||6|
|44|2|||44|31|||6|

**Table T2**

- $h_2(k) = \lfloor \frac{k}{7} \rfloor~mod~7$

|k|h2(k)|0|1|2|3|4|5|6|
|---|---|---|---|---|---|---|---|---|
|6|0|||||||||
|31|4||||||||
|2|0|||||||||
|41|5|6|||||||
|30|4|2|||||41||
|45|6|2||||31|41||
|44|6|2||||30|41|45|

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


### 1. (15pt) 

**💡 Idea**
- Initiation [^findbipart][^stackbipart]
    - Set singular sets for all verteces
- Add
    - If same set, return `false`
    - If different set, `union`
  


**Initiation**

```cpp=
INIT(N)
    Create MAP[0,..,N-1] with NIL-filled
    for i = 0 to N-1
        MAKE-SET(i)
    end
    
    isBiparatite = True
end
```
- `isBiparatite` can be seen and modified globally.
- Vector `MAP`: contains a nighbor of each verteces.
    - If `MAP[1] == 2`, it means vertex `1` is connected to vertex `2`

**Adding edge**

```cpp=
ADD-EDGE(x,y)
    // Check bipartite
    if FIND-SET(x) == FIND-SET(y)
        isBiparatite = False
    end
    
    if MAP[x] == NIL
        MAP[x] = y
    else
        UNION(MAP[x], y)
    end
    
    if MAP[y] == NIL
        MAP[y] = x
    else
        UNION(MAP[y], x)
    end
end
```

- `isBiparatite` is generated by `INIT`.


**Check bipartite**

```cpp=
IS-BIPARTITE()
    return isBiparatite
end
```

**🔢 Analysis**

- **Initiation**: Operates in linear time complexity for `Make-Set` is $O(1)$ and iterates $N$ times. Therefore, the total time complexity is $O(N)$.
- **Add Edge**: the union by size technique can confirm the smaller set enlarges at least twice of its size, which yields $O(\log N)$ time complexity for `Union` operation. Since the maximum calling of `ADD-EDGE` is $M$, the worst case of time complexity is: $O(M \log N)$ .
- **Check bipartite**: The time complexity of `IS-BIPARTITE` is $O(1)$.
- **Summary**: The total operation requires $O(N) + O(M\log N)$ in time complexity.


[^findbipart]: [How to Find If a Graph Is Bipartite?](https://www.baeldung.com/cs/graphs-bipartite)
[^stackbipart]: https://stackoverflow.com/questions/53246453/detect-if-a-graph-is-bipartite-using-union-find-aka-disjoint-sets


### 2. (15pt) 

**💡 Idea**
1. Create $3N$ nodes. $N$ nodes labelled with `scissor`, $N$ for `stone` and $N$ for `paper`
2. For `Win` operation, links all of 3 victory conditions. That is `scissor -> paper` / `paper->stone` / `stone->scissor`, and link these conditions into three disjoint sets.
3. For `tie` operation. For example: `Tie(i,j)`: Link $Node_{i}^{stone}$ to  $Node_{j}^{stone}$ , and the other two pairs (**Fig 3-1.**).
4. Contradiction: if more than 1 $Node_{i}^{\#}$ belongs to the same set, it means there is one person revealing two results at the same time which is the contradiction.

<center>

<img height=100 src="https://i.imgur.com/WQe2ymI.png">

</center>

**Fig 3-1. Sets of all possibility.**

**🔧Implementation**

**Initiation**

```cpp=
function INIT(N)
    Create list of vertexes: 
        Scissor[1..N]
        Stone[1..N]
        Paper[1..N]
        
    for i = 1 to N
        MAKE-SET(Scissor[i])
        MAKE-SET(Stone[i])
        MAKE-SET(Paper[i])
    end
end
```

- The list contains **Vertex** object.
- These function creates $3N$ verteces stored in three lists.
- Noted that `Scissor[1]` means `person 1` with `scissor` output, and `Scissor[1]` and `Paper[1]` are two independent objects.

**Win**

```cpp=
function WIN(a,b)
    UNION(Scissor[a], Paper[b])
    UNION(Stone[a], Scissor[b])
    UNION(Paper[a], Stone[b])
end
```

**Tie**

```cpp=
function TIE(a,b)
    UNION(Scissor[a], Scissor[b])
    UNION(Stone[a], Stone[b])
    UNION(Paper[a], Paper[b])
end
```

**Contradiction**

```cpp=
function IS-CONTRADICT()
    for i = 1 to N
        if(FIND-SET(Scissor[i]) == FIND-SET(Stone[i]))
            return true
        end
        
        if(FIND-SET(Scissor[i]) == FIND-SET(Paper[i]))
            return true
        end
        
        if(FIND-SET(Paper[i]) == FIND-SET(Stone[i]))
            return true
        end
    end
    
    return false
end
```


**🔢 Analysis**
- Time:
    - `Init`: $O(N)$
    - `WIN`: $O(\log N)$
    - `TIE`: $O(\log N)$
    - `IS-CONTRADICT`: $O(\log N)$
    - Total: $O(N + M \log N)$. Time complexity results from union by size, that is used for `WIN`/`TIE`/`IS-CONTRADICT` function. Noted that these three function are called $M$ times.
- Space: 
    - Individual outputs are set as nodes, that triple the size of $N$ verteces. The total space complexity is $O(N)$.


### 3. (15pt) (WIP)


### 4. (15pt) (WIP)


### 5. (20pt) (WIP)




## References