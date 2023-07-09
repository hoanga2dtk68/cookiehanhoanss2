`Decription`

![image](https://github.com/hoanga2dtk68/cookiehanhoanss2/assets/110059218/be283388-ef78-431b-97bf-ec9525308d78)

[Download challange](https://drive.google.com/file/d/1LgVgckxvHWmEhkvEPLNCjOA5yQQ-OqoA/view?usp=sharing)

Sau khi mở bằng wireshark thì có thể thấy đây là data được truyền theo tftp và ở stream udp 25 có thể là ascii

[`Link tham khảo về tftp`](https://en.wikipedia.org/wiki/Trivial_File_Transfer_Protocol)

![image](https://github.com/hoanga2dtk68/cookiehanhoanss2/assets/110059218/5cb51385-10d6-4172-8a11-95728a34ab88)

Thực hiện xóa và thay thế những phần mà cách truyền netascii thêm vào dữ liệu

![image](https://github.com/hoanga2dtk68/cookiehanhoanss2/assets/110059218/43bea909-acef-46b5-9d55-41916f80bdbf)

Xóa 8 kí tự đầu mỗi đoạn với replace
0D 0A -> 0A
0D 00 -> 0D

https://github.com/shylent/python-tx-tftp/blob/master/tftp/netascii.py

![image](https://github.com/hoanga2dtk68/cookiehanhoanss2/assets/110059218/fc671813-e2b2-4112-8963-e247fc633e07)

Download file pdf về và mở file để lấy flag

![image](https://github.com/hoanga2dtk68/cookiehanhoanss2/assets/110059218/3e81fad3-df95-4ace-b8c1-3df115f8fcad)
`flag: CHH{FTP_4nd_TFTP_4r3_b0th_un$af3}`
CHH{FTP_4nd_TFTP_4r3_b0th_un$af3}
