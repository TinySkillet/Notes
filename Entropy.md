
###### Entropy is the measure of unpredictability. In passwords, higher entropy means:

- More possible combinations
- Harder to guess or brute-force


**Say**
- Password: "A8ZK"
- Character set used: uppercase(26) + lowercase(26) + digits(10) = 62
  
  **This means, base = 62**
  
- Length = 4

**Then entropy is:**
``
	Entropy = 4 * log<sub>2</sub>(62) = 23.82 bits


This means a brute-force attacker would have to try about 2^23.82 = 14 millions combinations

**Entropy = length * log<sub>2</sub>(base)


### Change of base formula

$$
\log_b(a) = \frac{\log_k(a)}{\log_k(b)}
$$

Example:
$$
\log_8(24) = \frac{\log_2(24)}{\log_2(8)}
$$

