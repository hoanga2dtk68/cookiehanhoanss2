`Decription`

![image](https://github.com/hoanga2dtk68/cookiehanhoanss2/assets/110059218/c7d16f18-0e73-4af8-bd67-72b804cfcc4f)

[Download challange](https://drive.google.com/file/d/1oE6lMi0PGyTzveqWBKK6BzpPeQE9Bj72/view?usp=sharing)

Mở file bằng wireshark và export object http thì thấy 1 số file trong đó có file excel có thể đây chính là file mã độc

Tiến hành check bằng oletools và chạy trên win 10 ảo

Sau khi xem bằng olevba thì có thể thấy đây là một đoạn code đã bị obfuscate và mở bằng win ảo có thể nhận được 2 thông điệp đó là

![image](https://github.com/hoanga2dtk68/cookiehanhoanss2/assets/110059218/5846556a-37bd-4e8f-aa2e-72d85798b4b2)

![image](https://github.com/hoanga2dtk68/cookiehanhoanss2/assets/110059218/d9a29cd3-2082-4970-9b8e-83d262e9477c)

Code VBA sau khi deobf qua 

![image](https://github.com/hoanga2dtk68/cookiehanhoanss2/assets/110059218/08adf7a5-8426-4e60-ab07-110a16aecca5)

Có 1 hàm có vẻ là convert 1 chuỗi thành 1 chuỗi khác

![image](https://github.com/hoanga2dtk68/cookiehanhoanss2/assets/110059218/66be9c73-056b-40e7-86a8-3a7f1d359e7a)

Có 2 lần dùng msgbox để hiện lên các thông báo khi mở excel bên trên nên mình sẽ bỏ qua ở đây có thể thấy một đoạn rất giống 1 url

![image](https://github.com/hoanga2dtk68/cookiehanhoanss2/assets/110059218/25f1a248-2635-4912-b8ac-ab1eabeef60e)

Thực hiện gọi hàm convert và get từ url đó và lưu vào biến và mình dùng code python để convert về để lấy url

```
def decode_string(input_string):
    charset = " ?!@#$%^&*()_+|0123456789abcdefghijklmnopqrstuvwxyz.,-~ABCDEFGHIJKLMNOPQRSTUVWXYZ¿¡²³ÀÁÂÃÄÅÒÓÔÕÖÙÛÜàáâãäåØ¶§Ú¥"
    mapping = "ãXL1lYU~Ùä,Ca²ZfÃ@dO-cq³áÕsÄJV9AQnvbj0Å7WI!RBg§Ho?K_F3.Óp¥ÖePâzk¶ÛNØ%G mÜ^M&+¡#4)uÀrt8(ÒSw|T*Â$EåyhiÚx65Dà¿2ÁÔ"
    
    result = ""
    
    for char in input_string:
        if char in charset:
            char_index = charset.index(char)
            result += mapping[char_index]
        else:
            result += char
    
    print(result)
decode_string("Ü³³Bb://uàb³~uà³Ü¿k¿bE²6xi³Ei³~6xQ/k7¿_iQ_i/fÀ3_o-3Yf0_E6m6kk3_km§3Y03ÀY_3__/²_Ä/À3EÀkfmfÀ@Eããoãä§k@_@ã0ä6_E3-ãY036-@@koo/_Àmb6m@§~Bb@")
```
Sau khi chạy thì mình lấy được url là `https://gist.githubusercontent.com/bquanman/98da73d49faec0cbbdab02d4fd84adaa/raw/8de8b90981e667652b1a16f5caed364fdc311b77/a80sc012.ps1`

Lấy được nội dung thì cho vào powerdecode để lấy code đẹp hơn 

[Link code](https://drive.google.com/file/d/1ca505s2_PS8GazVNIPLBuQmPkDowvKav/view?usp=sharing)

Sau khi xem lại wireshark thì có thể thấy những lần get và post với những data bị encrypt thực hiện decrypt các lần để lấy nội dung của chúng với key là biến VxzTmff
iv là 16 bytes đầu của data bị encrypt và post lên

* với result='..' ta cần decode url vì những kí tự url không hiểu thì sẽ bị encode khi post lên
![image](https://github.com/hoanga2dtk68/cookiehanhoanss2/assets/110059218/8be1d2fd-ca24-47b6-be0a-5c9202929f76)

```import base64
from Crypto.Cipher import AES
from Crypto.Util.Padding import pad
import sys

AES_MODE = 'CBC'
AES_PADDING = 'zeros'
AES_BLOCK_SIZE = 128
AES_KEY_SIZE = 256

def create_aes_managed_object(key, iv):
    aes = AES.new(key, AES.MODE_CBC, iv)
    return aes

def decrypt(key, ciphertext):
    key_bytes = base64.b64decode(key)
    ciphertext_bytes = base64.b64decode(ciphertext)
    iv = ciphertext_bytes[:16]
    ciphertext_data = ciphertext_bytes[16:]
    aes = create_aes_managed_object(key_bytes, iv)
    plaintext = aes.decrypt(ciphertext_data)
    plaintext = plaintext.rstrip(b'\x00')
    return plaintext.decode('utf-8')

key = 'd/3KwjM7m2cGAtLI67KlhDuXI/XRKSTkOlmJXE42R+M='
ciphertext = ["RrzBf9o5vTBf+vInYW3OTzBvvNIWSyyKsx6v25jOD9roPGP4gOhaHPc/u7l804cs","tp4pZ9OgpI9uxr4sNupHQJE5hBlTVd7NbIK21rjApBf15tj9AuNo6OU/zJ/K3REi","bK2FX23ydWGbJNdJliRrDqjOE17p1YakRt2cjgaRJJv0zAVVro+Gq1waD0ui+lCe","uGjyY6GcnabYem8450v+e256asufK4JUhfW5/KQfyPeAIkmBiQcwBoQbI8z7v9NLyH9Gwi4k6ViFL0nMTCGGWS0TSS6vqWRHa4ADkfcaVFhcjLmBV23dnOfSoCGUWzCg4TBcpDtc+C4QOc/v+dZSL2ytww2c8+pY1dGwth89dVWej8qifotdP0I9p3f/WNCf","rQp9Lo1Cuh3mvS45DF0m5ZG1lhW4/Jb7T608IpAS5GwBurTGeJ5oomIhg1jHo1x4dLqcRnsNYqKU7cj8gGXkQA==","eq/gZMmuX1cPWZYJ9iQcSi3P1KYsOFCwdkD/L8i6HSGn7yWmXk8YxGN96VndokXT"]
plaintext = ["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",""]
for i in range(0,5):
	decrypted_text = decrypt(key, ciphertext[i])
	print(decrypted_text)
	decrypted_text = decrypt(key, plaintext[i])
	print(decrypted_text)
decrypted_text = decrypt(key, ciphertext[5])
print(decrypted_text)
```
![image](https://github.com/hoanga2dtk68/cookiehanhoanss2/assets/110059218/62e60f34-4ced-435b-8597-48c96fabedad)

Cho thấy kết quả có rất nhiều lần gọi để check log và clear, pwd xem path đang đứng và dir trong đó có 1 lệnh chuyển data của 1 ảnh thành hex lưu hex lại xem ảnh cho chúng ta gì

![image](https://github.com/hoanga2dtk68/cookiehanhoanss2/assets/110059218/a9a2bbdb-ab3b-4407-92c1-cae80ef59875)

Quét qrcode để lấy flag

`flag: CHH{D0n't_w0rRy_n0_st@r_wh3rE}`
