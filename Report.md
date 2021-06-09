---
toc:
  depth_from: 2
  depth_to: 3
  ordered: false
---

[![hackmd-github-sync-badge](https://hackmd.io/JZUhv-byTRqV46cumxZ9Xw/badge)](https://hackmd.io/JZUhv-byTRqV46cumxZ9Xw)

<center>

# Homework 3: Nonprogramming part

邱紹庭

`r07945001@ntu.edu.tw`

</center>

**Table of contents**

[toc]

---

## Problem 1 - Hash (60pt)


### 1. (10pt) (WIP)


### 2. (10pt) (WIP)


### 3. (20pt) (WIP)


### 4. (20pt) (WIP)

---

## Problem 2 - String matching (60pt)


### 1. (10pt) (WIP)

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


### 2. (10pt) (WIP)


### 3. (20pt) (WIP)


### 4. (20pt) (WIP)

---

## Problem 3 - Having fun with Disjoint Sets (80pt)


### 1. (15pt) (WIP)


### 2. (15pt) (WIP)


### 3. (15pt) (WIP)


### 4. (15pt) (WIP)


### 5. (20pt) (WIP)


---

## References