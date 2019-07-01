Pattern của input

{tên site + pattern đường dẫn}

Pattern đường dẫn có thể rỗng, khi đó hiểu là toàn bộ site.

Lưu ý:

 - Có phân biệt subdomain
 - Không cần support regex, chỉ cần check site và xem url có chứa pattern không
 - Các pattern có thể chồng chéo nhau. Ví dụ: abc.com/category, abc.com/category/xyz.html

Ví dụ
```
vatgia.com/572/do-choi-o-to.html
otofun.net/forums/cho-gioi.89
aloola.vn
vi.wikipedia.org
...
```