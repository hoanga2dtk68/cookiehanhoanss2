`Decription`

![image](https://github.com/hoanga2dtk68/cookiehanhoanss2/assets/110059218/0f1c7448-6a5a-4e00-9f22-1bed2bf5dd7f)

[Download challange](https://drive.google.com/file/d/1V81b8M61loIfYuyQePewjaFBX26ku-r6/view?usp=sharing)

Mở bằng volatility check bằng pstree để xem cây quy trình thì có thấy winword.exe được sinh ra từ explorer.exe

![image](https://github.com/hoanga2dtk68/cookiehanhoanss2/assets/110059218/353c0df5-c3d0-49b7-b9c9-bff33acc3d4c)

Thử dùng filescan rồi grep 'asd' hay 'doc' để tìm file được autorecovery

![image](https://github.com/hoanga2dtk68/cookiehanhoanss2/assets/110059218/0b612e85-c1f0-4f7d-9d76-5e3504eb16a8)

Tiến hành dumpfile để tìm kiếm flag thôi

![image](https://github.com/hoanga2dtk68/cookiehanhoanss2/assets/110059218/43ac60b4-666c-43eb-be7a-2d8d831864fe)

Sau khi mở file mình đã nghĩ tác giả có lẽ giấu flag ở đâu đó và có thử dùng tool binwalk để extract các file con nằm trong file word và may mắn đã tìm được flag

![image](https://github.com/hoanga2dtk68/cookiehanhoanss2/assets/110059218/720153df-e204-4338-8143-197fc05f917c)

`flag: CHH{4ut0R3c0v3r_s4v3_my_l1f3}`
