# Python in Data Analytics Cheatsheet (Basic)

## Arithmetic operators
1. Modulus Operator %: 3 % 2 = 1
2. Division Operator /: 3 / 2 = 1.5
3. Exponentiation Operator \**: 2 ** 3 = 8
4. Floor division //: 3 // 2 = 1 = math.floor(3/2)

Others:  
1. float printing:     print('%.6f'% ratio) == print('{0:.6f}'.format(ratio)) == print(round(ratio,6))



## String
### string
1. string.ascii_letters: 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ'
2. string.ascii_lowercase
3. string.ascii_uppercase
4. string.digits: '0123456789'
5. string.hexdigits: '0123456789abcdefABCDEF'
6. string.octdigits: '01234567'
7. string.punctuation: !"#$%&'()*+,-./:;<=>?@\[\]^_`{|}~
### other
1. Change a string to a list to edit: list_s = list(s)
2. Merge a string from list: s = ''.join(list_s)



## List
1. Create a list: \[0]\*n, \[i for i in range(n)]
2. Exchange two positions' values: a\[mid], a\[n-1] = a\[n-1], a\[mid]
3. Get index: list.index(), index(element, start, end), start (optional) - start searching from this index, end (optional) - search the element up to this index



### sorted(iterable, key=None, reverse=False)
1. sorted_list = sorted(list), default: ascending = True, list != sorted_list
2. sorted('Python') -> \['P', 'h', 'n', 'o', 't', 'y']
3. "It may seem backwards that lowercase letters are greater than uppercase, but that's due to historical reasons: the earliest encodings only had uppercase letters. Lowercase letters were added decades later, and naturally they were added to the ends of the existing character tables for backwards compatibility." - From online sources
4. dictionary: sorted({'e': 1, 'a': 2, 'u': 3, 'o': 4, 'i': 5}) ->\['a', 'e', 'i', 'o', 'u'] (It sorted the keys.)
5. dictionary: sorted({'e': 1, 'a': 2, 'u': 3, 'o': 4, 'i': 5}.values()) -> \[1, 2, 3, 4, 5]

### sort(reverse=False, key=myFunc)
1. list.sort() return None
2. list == sorted(list)

## Dictionary
1. Assign keys with a list: a_dict = {key: 0 for key in key_list}
2. Assign keys and values with lists simultaneous: my_dict = {key: value for key,value in zip(key_list, value_list)}
3. Obtain keys with restricted values/keys: \[k for k,v in my_dict.items() if v == 1]
4. 
