---
title:  "Dynamic Huffman Coding"
layout: single
date:   2022-01-16 19:10:54
author_profile: true
read_time: true
show_date: true
related: true
header:
  teaser: "https://github.com/DorAzaria/dorazaria.github.io/blob/main/assets/images/dynamic_huffman/image49.png?raw=true"
  image: "https://github.com/DorAzaria/dorazaria.github.io/blob/main/assets/images/dynamic_huffman/image49.png?raw=true"
  og_image: "https://github.com/DorAzaria/dorazaria.github.io/blob/main/assets/images/dynamic_huffman/image49.png?raw=true"
  categories:
  - Data Compression
tags:
  - Compression
  - Science
  - Data
  - Algorithms
comments: true
share: true
related: true
toc: true
toc_sticky: true
usemathjax: true
---
Adaptive Huffman coding (also called Dynamic Huffman coding) is an adaptive coding technique based on Huffman coding. It permits building the code as the symbols are being transmitted, having no initial knowledge of source distribution, that allows one-pass encoding and adaptation to changing conditions in data.

## Algorithm - Pseudo Code
```java
Update()
    q ⇐ leaf(xt)
    if (q is the 0-node)
        replace q by a parent 0-node with two 0-node children;
        q ⇐ left child;
    if (q is a sibling of a 0-node)
        interchange q with the highest numbered leaf of the same weight.
        increment q’s weight by 1;
        q ⇐ parent of q
    while (q is not root)
        interchange q with the highest numbered node of the same weight.
        increment q’s weight by 1;
        q ⇐ parent of q
    increment q’s weight by 1;
```

Given the following message:
```
abracadabra
```

## Compression Example
You must compress this message using dynamic huffman coding.
You must show the huffman tree at each step, and provide the compressed message.

First we'll present the details with the probabilities:

| Symbol | Probability | Duplicates | ASCII code in binary |
| -- | -- | -- | -- |
| a |  0.454 | 5  |   01100001 |  
| b |  0.181 |  2 |  01100010 |  
| c |  0.090 | 1  |  01100011 |  
| d |  0.090 | 1  |  01100100 |  
| r |  0.181 | 2  |  01110010 |  

##### Step 1
Insert 'a' and pass 01100001 in ASCII
![png](https://github.com/DorAzaria/dorazaria.github.io/blob/main/assets/images/dynamic_huffman/image49.png?raw=true)

##### Step 2
Insert 'b' and pass 01100010 in ASCII with its path "0".
So it's **0**01100010
![png](https://github.com/DorAzaria/dorazaria.github.io/blob/main/assets/images/dynamic_huffman/image59.png?raw=true)

##### Step 3
Insert 'r' and pass 01110010 in ASCII with its path "00".
So it's **00**01110010
![png](https://github.com/DorAzaria/dorazaria.github.io/blob/main/assets/images/dynamic_huffman/image51.png?raw=true)

##### Step 4
Fixing level 1 (left child was bigger than the right so we swapped).
![png](https://github.com/DorAzaria/dorazaria.github.io/blob/main/assets/images/dynamic_huffman/image53.png?raw=true)

##### Step 5
Insert 'a' with its path "0", 'a' passed before, therefore we don't need to pass its ASCII code again, we'll pass only its route to the node which represnt this symbol
![png](https://github.com/DorAzaria/dorazaria.github.io/blob/main/assets/images/dynamic_huffman/image43.png?raw=true)


##### Step 6
Insert 'c' and pass 01100011 in ASCII with its path "100".
So it's **100**01100011
![png](https://github.com/DorAzaria/dorazaria.github.io/blob/main/assets/images/dynamic_huffman/image58.png?raw=true)

##### Step 7
Fixing level 2 (left child was bigger than the right so we swapped).
![png](https://github.com/DorAzaria/dorazaria.github.io/blob/main/assets/images/dynamic_huffman/image56.png?raw=true)

##### Step 8
Insert 'a' with its path "0", 'a' passed before, therefore we don't need to pass its ASCII code again, we'll pass only its route to the node which represnt this symbol.
![png](https://github.com/DorAzaria/dorazaria.github.io/blob/main/assets/images/dynamic_huffman/image48.png?raw=true)

##### Step 9
Insert 'd' and pass 01100100 in ASCII with its path "1100".
So it's **1100**01100100
![png](https://github.com/DorAzaria/dorazaria.github.io/blob/main/assets/images/dynamic_huffman/image44.png?raw=true)

##### Step 10
Fixing level 3 (left child was bigger than the right so we swapped).
![png](https://github.com/DorAzaria/dorazaria.github.io/blob/main/assets/images/dynamic_huffman/image57.png?raw=true)

##### Step 11
Insert 'a' with its path "0", 'a' passed before, therefore we don't need to pass its ASCII code again, we'll pass only its route to the node which represnt this symbol.
![png](https://github.com/DorAzaria/dorazaria.github.io/blob/main/assets/images/dynamic_huffman/image50.png?raw=true)

##### Step 12
Insert 'b' with its path "10", 'b' passed before, therefore we don't need to pass its ASCII code again, we'll pass only its route to the node which represnt this symbol.
![png](https://github.com/DorAzaria/dorazaria.github.io/blob/main/assets/images/dynamic_huffman/image55.png?raw=true)

##### Step 13
Insert 'r' with its path "110", 'r' passed before, therefore we don't need to pass its ASCII code again, we'll pass only its route to the node which represnt this symbol.
![png](https://github.com/DorAzaria/dorazaria.github.io/blob/main/assets/images/dynamic_huffman/image52.png?raw=true)

##### Step 14
Insert 'a' with its path "0", 'a' passed before, therefore we don't need to pass its ASCII code again, we'll pass only its route to the node which represnt this symbol.
![png](https://github.com/DorAzaria/dorazaria.github.io/blob/main/assets/images/dynamic_huffman/image54.png?raw=true)

### The final message
```
01100001 001100010 0001110010 0 10001100011 0 110001100100 0 10 110 0
```
