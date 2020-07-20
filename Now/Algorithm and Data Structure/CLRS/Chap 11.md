# Hash Tables

A hash table generalizes the simpler notion of an ordinary array.

- $Hash(key) = value$
- Instead of using key directly, array index is computed from the key.

## Direct-address tables

Worked well when the universe $U$ of keys is reasonably small.

- For key drawn from $U = \{0, 1,\dots, m-1\}$, $T[0\dots m-1]$ as a direct-address table. 
  Each slot corresponds to a key in the universe $U$.

## Hash tables

The downside of direct-address table is size of $U$. If $U$ is large, storing a large table is impractical.

When the set $K$ of keys stored in a dictionary is much smaller than $U$, a hash table requires much less storage.

We use hash function $h$ to compute the slot from key $k$. $h$ maps $U$ to hash table $T[0,\dots, m-1]$. 

**Collision:** Two keys may hash to the same slot,

- Completely avoiding collision is impossible since $|U| > m$, but we can make it more "random" by choosing better hash functions.

**Collision Resolution:**

- **Chaining**, place all elements hash to the same slot into the same linked list.
  **Load factor** $\alpha$ for $T$ is $n/m$ which is the average number of elements  stored in a chain.
- Assuming **simple uniform hashing**, in a hash table in which collisions are resolved by chaining, a search takes average-case time $\Theta(1+\alpha)$.

## Hash functions

Criteria: A good hash function satisfies the assumption of simple uniform hashing.

- Hard to fulfill since we don't know the probability distribution of keys, moreover these keys may not drawn independently.
- In practice, heuristic techniques to create a hash function.

Most hash functions assume that the universe of keys is the set $\mathbb{N} = \{0, 1, 2, \dots\}$. If keys are not natural numbers, we may find a way to interpret them in natural numbers.

### Division method

$h(k) = k \mod m$ where $m$ is selected

- $m = 2 ^p$ is not desirable because it virtually hashes only a fraction of bits of $k$.
- A prime number not too close to an exact power of 2 is often a good choice of m.

### Multiplication method

$h(k) = [m (kA - [kA])]$

- First multiplies k by a constant $A$ in the range $0 < A < 1$ and extract the fractional part of $kA$.
- Multiplies $m$ to decide which slot it should locate.

Advantage:

- $m$ is not critical.
- Choice of $A$ depends on the characteristics of the data being hashed. Knuth suggests that $A$ close to $\frac{\sqrt{5} - 1}{2}$ is a good choice.

### Universal hashing

Any fixed hash table is vulnerable to a worst-case behavior such that all keys are projected to the same slot.

- Choose the hash function *randomly* in a way that is *independent* of the keys that are actually going to be stored.

In universal hashing at the beginning of execution we select the hash function at random from a carefully designed class of functions.

$\mathcal{H}$ is a finite collection of hash function s that map a given universe $U$ of keys into the range $\{0, 1, \dots, m-1\}$. $\mathcal{H}$ is **universal** if for each pair of distinct keys $k_1, k_2 \in U$. the number of hash functions $h \in \mathcal{H}$ for which $h(k_1) = h(k_2)$ is at most $|\mathcal{H}| / m$.

- For a randomly selected hash function $h\in \mathcal{H}$, the collision chance $h(k_1) =  h(k_2) $ would at most be $1/m$.

**Theorem:** Suppose a hash function $h$ chosen randomly from a universal collection of hash functions. Assuming $n$ keys has been hashed in the table $T$ of size $m$. 

- If key $k$ is not in the table, then the expected length $E[n_{h(k)}]$ of the list that key $k$ hashes to is at most the load factor $\alpha = n / m.$
- If key $k$ is in the table, then the expected length $E[n_{h(k)}]$ of the list that key $k$ hashes to is at most $1 + \alpha$.

How to design a universal class of hash functions?

- Choose a prime number $p$ large enough such that every possible key $k$ is in the range $[0, p)$ a collection of functions can be constructed like 

$$
\begin{aligned}
h_{a,b}(k) &= ((a * k + b) \mod p)\mod m \\
&\text{ for all }0\leq a, b \leq p-1\text{ and }a\neq 0.
\end{aligned}
$$

**Theorem:**  ${h_{a,b}}$ is universal.

## Open addressing

In **open addressing**, all elements occupy the hash table itself. each table entry contains either an element of the dynamic set or NIL. Collisions are solved by repeated probing.

The hash table can fill up so that no further insertions can be made, thus $\alpha \leq 1$.

The advantage of open addressing is avoids pointer altogether, thus more memory can be used for storage, faster retrieval and fewer collisions.



To insert into a hash table with open addressing. We need to probe the hash table until find an empty slot.

- The hash function becomes 
  $ h : U \times \{0, 1, \dots, m-1\} \to \{0, 1, \dots, m-1\}$
  It maps the key from U and the probe number to the corresponding slot.
- The **probe sequence** $<h(k, 0), h(k, 1), \dots, h(k, m-1)>$ should be a permutation of $<0, 1, \dots, m-1>$ so that every hash-table position is eventually considered as a slot for a new key as the table fills up.
- Deletion from an open-address hash table is difficult. Chaining is more commonly selected as a collision resolution technique when keys must be deleted.



Assuming **uniform hashing**, the probe sequence of each key is equally likely among permutations of $<0, 1, \dots, m-1>$. Uniform hashing is difficult to implement, suitable approximations in practice are instead used.

- **Linear probing**
  Given an ordinary hash function $h': U \to \{0, 1, \dots, m-1\}$, which we refer to as an **auxiliary hash function**, the method of linear probing uses the hash function $h(k, i) = (k'(k) + i) \mod m$.
  - Only $m$ distinct probe sequences.
  - Easy to implement
  - Suffer from **primary clustering**: long runs slows the process since occupied slots tends to get longer.
- **Quadratic probing**
  $h(k, i) = (h'(k) + c_1 i + c_2 i^2) \mod m$, 
  where $h'$ is an auxiliary hash function, $c_1$ and $c_2$ are positive auxiliary constants.
  - Only $m$ distinct probe sequences.
  - Works better than linear probing, but need to select $c_1, c_2$ carefully.
  - Suffer from **secondary clustering**.
- **Double hashing**
  $h(k,i) = (h_1(k) + i\cdot h_2(k)) \mod m$ 
  where $h_1, h_2$ are auxiliary hash functions.
  - $h_2$ must be relative prime to $m$ for the entire hash table to be searched. 
  - More probe sequences are used.



**Theorem:** Given an open-address hash table with load factor $\alpha = n/m < 1$, the expected number of probes in an unsuccessful search is at most $1/(1-\alpha)$, assuming uniform hashing.

**Corollary:** Inserting an element into an open-address hash table with load factor $\alpha$ requires at most $1/(1-\alpha)$ probes on average, assuming uniform hashing.

**Theorem:** Given an open-address hash table with load factor $\alpha < 1$, the expected number of probes in a successful search is at most $\frac{1}{\alpha} \ln \frac{1}{1-\alpha}$, assuming uniform hashing, and assuming that each key in the table is equally likely to be searched for.

## Perfect hashing

Hashing is a good choice for average-case performance. But when keys are static, the worst case scenario can be improved. A hashing technique is **perfect hashing** if $O(1)$ memory accesses are required to perform a search in the worst case.

- First level, hash $n$ keys to $m$ slots using hashing function $h$ carefully chosen from a family of universal hash functions.
- Use a small **secondary hash table** $S_j$ with an associated hash function $h_j$, guarantee that no collision at the secondary level.
  - $|S_j|$ should be square of the number $n_j$ of keys hashing to slot $j$.
  - The total space used should be $O(n)$.

Use a class of hash function $\mathcal{H}_{p, m}$ where $p$ is a prime number greater than any key value. Those keys hash to slot $j$ are rehashed into a secondary hash table $S_j$ of size $m_j$ using a hash function $h_j$ chosen from $\mathcal{H}_{p,m_j}$.



**Theorem:** Suppose that we store $n$ keys in a hash table of size $m = n^2$ using a hash function $h$ randomly chosen from a universal class of hash functions. Then, the probability is less than $1/2$ that there are any collisions.

The expected collide chance for a randomized chosen hash function is $1/m$, and there are $n(n-1)/2$ pairs of possible collisions. Thus finishes the proof.



Since a hash table directly map from $n$ to $n^2$ is very large, first we map from $n$ to $n$ slots, Then we map $n$ to $n^2$ in a second hashing process using hashing table $S_j$ for each slot $j$.

**Theorem:** Suppose that we store $n$ keys in a hash table of size $m = n$ using a hash function $h$ randomly chosen from a universal class of hash functions. Then we have $E[\sum n_j^2 ] < 2n$

$E[\sum n_j^2] = E[n] + 2\cdot E[collisions] \leq E[n] + 2\cdot n(n-1) / 2 * 1/m \text{(universal hashing)}= 2n - 1 < 2n$ 

















