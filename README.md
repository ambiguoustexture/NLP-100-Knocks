# 100 Language Processing Knocks
Amateur practices and handmade codes in separate chapter folder.

## 第1章: 準備運動

### 00. 文字列の逆順
文字列"stressed"の文字を逆に（末尾から先頭に向かって）並べた文字列を得よ．
```Python
# In Python Interective Shell
>>> str = "stressed"
>>> str = str[::-1]
>>> str
'desserts'
```

### 01. 「パタトクカシーー」
「パタトクカシーー」という文字列の1,3,5,7文字目を取り出して連結した文字列を得よ．
```Python
# In Python Interective Shell
>>> str = "パタトクカシーー"
>>> str[::2]
'タクシー'
```

### 02. 「パトカー」＋「タクシー」＝「パタトクカシーー」
「パトカー」＋「タクシー」の文字を先頭から交互に連結して文字列「パタトクカシーー」を得よ．
```Python
# In Python Interective Shell
>>> str0 = "パトカー"
>>> str1 = "タクシー"
>>> res  = ""
>>> str(res)
>>> for i in range(len(str0)):
...     for j in range(len(str1)):
...             res += str0[i]
...             res += str1[i]
...             break
...
>>> res
'パタトクカシーー'
```

### 03. 円周率
"Now I need a drink, alcoholic of course, after the heavy lectures involving quantum mechanics."という文を単語に分解し，各単語の（アルファベットの）文字数を先頭から出現順に並べたリストを作成せよ．
```Python
# In Python Interective Shell
>>> str = "Now I need a drink, alcoholic of course, after the heavy lectures involving quantum mechanics."
>>> str = str.split(' ')
>>> str_len = [len(word.rstrip('.,')) for word in str]
>>> print(str_len)
[3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5, 8, 9, 7, 9]
```

### 04. 元素記号
"Hi He Lied Because Boron Could Not Oxidize Fluorine. New Nations Might Also Sign Peace Security Clause. Arthur King Can."という文を単語に分解し，1, 5, 6, 7, 8, 9, 15, 16, 19番目の単語は先頭の1文字，それ以外の単語は先頭に2文字を取り出し，取り出した文字列から単語の位置（先頭から何番目の単語か）への連想配列（辞書型もしくはマップ型）を作成せよ．
```Python
# Author：ambiguoustexture
# Date:   2020-02-02

str = "Hi He Lied Because Boron Could Not Oxidize Fluorine. New Nations Might Also Sign Peace Security Clause. Arthur King Can."

str = str.split(" ")

res = []

for word, index in zip(str, range(len(str))):
    if index+1 in (1, 5, 6, 7, 8, 9, 15, 16, 19):
       res.append(word[0])
    else:
       res.append(word[0:2])

print(res)
```
```Shell
ch01 準備運動 git:(master) ✗ python 04.py
['H', 'He', 'Li', 'Be', 'B', 'C', 'N', 'O', 'F', 'Ne', 'Na', 'Mi', 'Al', 'Si', 'P', 'S', 'Cl', 'Ar', 'K', 'Ca']
```

### 05. n-gram
与えられたシーケンス（文字列やリストなど）からn-gramを作る関数を作成せよ．この関数を用い，"I am an NLPer"という文から単語bi-gram，文字bi-gramを得よ．
```Python
# Author：ambiguoustexture
# Date:   2020-02-02

import re

def sequence_to_words(sequence):
    """clean the given sequence
    :param sequence:    given sequence (string, list, etc.)
    :return:            words in the sequence
    """
    sequence = re.sub('\n+', ' ', sequence).lower()
    words = re.split('\W+', sequence)

    return words

def get_n_gram(sequence, n, key):
    """naive implement of n-gram based on word or letter
    :param sequence:    given sequence (string, list, etc.)
    :param n:           n-gram for n=2
    :param key:         switch word based or letter based
                        "key = word" for word based
                        "key = letter" for letter based
    :return:            n-gram dictionary
    """
    words = sequence_to_words(sequence)
    if key == "word":
        n_gram_dic = {}
        for i in range(len(words) - n + 1):
            n_gram_current = " ".join(words[i:i + n])
            if n_gram_current not in n_gram_dic:
                n_gram_dic[n_gram_current] = 1
            else:
                n_gram_dic[n_gram_current] += 1

    elif key == "letter":
        str = ''.join(words)
        n_gram_dic = {}
        for i in range(len(str) - n + 1):
            n_gram_current = str[i:i + n]
            if n_gram_current not in n_gram_dic:
                n_gram_dic[n_gram_current] = 1
            else:
                n_gram_dic[n_gram_current] += 1

    return n_gram_dic

if __name__ == "__main__":
    # sequence = open("poem.txt").read()
    sequence = "I am an NLPer"
    bi_gram_word_based = get_n_gram(sequence, 2, key = "word")
    bi_gram_letter_based = get_n_gram(sequence, 2, key = "letter")
    print("Sentence: I am an NLPer")
    print("Word based bi-gram of the sentence is:\n" + str(bi_gram_word_based))
    print("letter based bi-gram of the sentence is\n:" + str(bi_gram_letter_based))
```
```Shell
ch01 準備運動 git:(master) ✗ python n_gram.py
Sentence: I am an NLPer
Word based bi-gram of the sentence is:
{'i am': 1, 'am an': 1, 'an nlper': 1}
letter based bi-gram of the sentence is
:{'ia': 1, 'am': 1, 'ma': 1, 'an': 1, 'nn': 1, 'nl': 1, 'lp': 1, 'pe': 1, 'er': 1}
```

### 06. 集合
"paraparaparadise"と"paragraph"に含まれる文字bi-gramの集合を，それぞれ, XとYとして求め，XとYの和集合，積集合，差集合を求めよ．さらに，'se'というbi-gramがXおよびYに含まれるかどうかを調べよ．
```Python
# In Python Interective Shell
>>> from n_gram import *
>>> str0 = "paraparaparadise"
>>> str1 = "paragraph"
>>> X = set(get_n_gram(str0, 2, key = "letter"))
>>> Y = set(get_n_gram(str1, 2, key = "letter"))
>>> X.union(Y)
{'ph', 'gr', 'pa', 'is', 'ar', 'ra', 'di', 'ap', 'se', 'ad', 'ag'}
>>> X.intersection(Y)
{'pa', 'ar', 'ap', 'ra'}
>>> X.difference(Y)
{'di', 'is', 'se', 'ad'}
>>> 'se' in X.union(Y)
True
```

### 07. テンプレートによる文生成
引数x, y, zを受け取り「x時のyはz」という文字列を返す関数を実装せよ．さらに，x=12, y="気温", z=22.4として，実行結果を確認せよ．
```Python
# In Python Interective Shell
>>> def func(x, y, z):
...     return str(x) + "時の" + str(y) + "は" + str(z)
...
>>> x, y, z = 12, '気温', 22.4
>>> print(func(x, y ,z))
12時の気温は22.4
```

### 08. 暗号文
与えられた文字列の各文字を，以下の仕様で変換する関数cipherを実装せよ．

英小文字ならば(219 - 文字コード)の文字に置換
- その他の文字はそのまま出力
- この関数を用い，英語のメッセージを暗号化・復号化せよ．
```Python
# Author：ambiguoustexture
# Date:   2020-02-02

def cipher (sequence):
    """replace with lowercase (219 - ascii of character)
    :param sequence:    given text
    :return:            encrypted text
    """
    sequence_list = []
    res = ''

    for i in range(len(sequence)):
        if sequence[i].islower():
            sequence_list.insert(i, chr(219 - ord(sequence[i])))
        else:
            sequence_list.insert(i, sequence[i])

    for i in range (len(sequence)):
        res += sequence_list[i]

    return res

if __name__ == "__main__":
    sequence = "I am not an NLPer."
    print(cipher(sequence))
```
```Python
ch01 準備運動 git:(master) ✗ python cipher.py
I zn mlg zm NLPvi.
```
### 09. Typoglycemia
スペースで区切られた単語列に対して，各単語の先頭と末尾の文字は残し，それ以外の文字の順序をランダムに並び替えるプログラムを作成せよ．ただし，長さが４以下の単語は並び替えないこととする．適当な英語の文（例えば"I couldn't believe that I could actually understand what I was reading : the phenomenal power of the human mind ."）を与え，その実行結果を確認せよ．
```Python
# Author：ambiguoustexture
# Date:   2020-02-02

import random

def typoglycemia(sequence):
    """
    Function that leaves the characters at the beginning and end of each word,
    and rearranges the order of the other characters randomly.
    However, words with a length of 4 or less are not rearranged.
    :param sequence:    given text
    :return:            rearranged text
    """
    words = sequence.split()

    for i in range(len(words)):
        if len(words[i]) > 4:
            word_partial = list(words[i][1:-1])
            random.shuffle(word_partial)
            word_shuffle = "".join(word_partial)
            words[i] = words[i][0] + word_shuffle + words[i][-1]

    return ' '.join(words)

if __name__ == "__main__":
    sequence = "I couldn't believe that I could actually understand what I was reading : the phenomenal power of the human mind ."
    print(typoglycemia(sequence))
```
```Shell
ch01 準備運動 git:(master) ✗ python 09.py
I clu'nodt belviee that I cluod alatcluy ursdtnnead what I was rednaig : the pemnnheoal peowr of the human mind .
```
