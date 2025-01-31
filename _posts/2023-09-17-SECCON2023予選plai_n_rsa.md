---
title: "SECCON2023予選plai_n_rsa"
date: 2023-09-17
---

式展開まではたどり着けた…が、n=…で、だから何❓となり挫折。

## 問題

```python
import os

from Crypto.Util.number import bytes_to_long, getPrime

flag = os.getenvb(b"FLAG", b"SECCON{THIS_IS_FAKE}")
assert flag.startswith(b"SECCON{")
m = bytes_to_long(flag)
e = 0x10001
p = getPrime(1024)
q = getPrime(1024)
n = p * q
e = 65537
phi = (p-1)*(q-1)
d = pow(e, -1, phi)
hint = p+q
c = pow(m,e,n)

print(f"e={e}")
print(f"d={d}")
print(f"hint={hint}")
print(f"c={c}")
```

秘密鍵dはあるがnがない。与えられた情報からnを復元するという問題。

## 既知の数を整理

```python
e = 65537

d = pow(e, -1, phi) = e^-1 mod(p-1)(q-1) = 15353693384417089838724462548624665131984541847837698089157240133474013117762978616666693401860905655963327632448623455383380954863892476195097282728814827543900228088193570410336161860174277615946002137912428944732371746227020712674976297289176836843640091584337495338101474604288961147324379580088173382908779460843227208627086880126290639711592345543346940221730622306467346257744243136122427524303881976859137700891744052274657401050973668524557242083584193692826433940069148960314888969312277717419260452255851900683129483765765679159138030020213831221144899328188412603141096814132194067023700444075607645059793

hint = p + q = 275283221549738046345918168846641811313380618998221352140350570432714307281165805636851656302966169945585002477544100664479545771828799856955454062819317543203364336967894150765237798162853443692451109345096413650403488959887587524671632723079836454946011490118632739774018505384238035279207770245283729785148
```

## d = …に注目する

```python
ed-1
	= 0mod(p-1)(q-1)
	= k(p-1)(q-1)
	= k(pq - (p+q) + 1)

...(ed-1)は(p-1)(q-1)=phiで割り切れる
```

k is 何

```python
# 切り捨て除算
((ed-1)//phi)
	= 0 から eの値を取る
```

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/20629c22-51f5-4379-9752-46ad24eb9c33/ec84a24a-3b46-41a1-b4be-9cf4935a1000/Untitled.png)

この程度ならブルートフォースが可能

## nを取得する

```python
for k in range(1, e):
    # e*d - 1 = kn - k*hint + k
    # kn = e*d - 1 + k*hint - k  <= 式展開を頑張る。n=の形にする
    n = (e*d - 1 + k*hint - k)//k
```

👇if で処理高速にしてるらしい　long_to_bytesが必要

```python
for k in range(1, e):
    if (e*d - 1 + k*hint - k) % k != 0:
        continue
    n = (e*d - 1 + k*hint - k)//k
    print(long_to_bytes(pow(c,d,n)))
```


THIS_IS_FAKEを取れてるのを確認。

output.txtのcを召喚すると🆗

## 参考

https://www.youtube.com/live/jwM5OP81cl0?si=nIiuwnUpl70Ag-aF
