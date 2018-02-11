# Routing theo chuẩn RESTful

## 1. RESTful API Là Gì

**REST** (Representational State Transfer) là một trong những kiểu kiến trúc được sử dụng khá phổ biến trong lập trình web để quản lý các tài nguyên của ứng dụng. **REST** đưa ra cách thiết cấu trúc URL và phương thức HTTP tương ứng để thực hiện từng tác vụ quản lý tài nguyên (như tạo mới, hiển thị thông tin, cập nhật hay xoá tài nguyên). **RESTful** được sử dụng trong giao tiếp giữa máy khách (thường thông qua trình duyệt) và máy chủ do đó còn được gọi giao diện lập trình ứng dụng (applicaiton programming interface hay API).

## 2. Tại sao nên dùng REST?

* Thiết kế web trước đây sử dụng **SOAP** (Simple Object Access Protocol) và **WSDL** (Web Service Definition Language), tuy nhiên bây giờ **REST** tối ưu hơn so với 2 phương pháp này.
* Rõ ràng về URL (REST URL đại diện cho resource xác định chứ không phải hành động)
* Trả về nhiều định dạng khác nhau như: html, xml, ...
* Code ngắn gọn dễ hiểu
* Hiệu suất tốt, tin cậy, dễ phát triển.

## 3. Resource Controllers

Laravel resource routing gán kiểu "CRUD" routes cho một controller chỉ với một dòng code. Sử dụng lệnh make:controller Artisan, chúng ta có thể nhanh chóng tạo ra một controller:

`php artisan make:controller PhotoController --resource`

Controller sẽ bao gồm method cho các action của resource có sẵn.

## 4. Resource Routes

Tiếp theo, bạn phải đăng ký một resourceful route cho controller:

```
Route::resource('photos', 'PhotoController');
```

Khai báo route này sẽ tạo ra nhiều route để xử lý đa dạng các actions trong resource. Controller tạo ra sẽ có sẵn vài phương thức gốc dễ cho từng action, gồm những thông báo cho bạn những method HTTP và URIs nó xử lý.

[![KdJCJ.png](https://uphinhnhanh.com/images/2018/02/11/KdJCJ.png)](https://uphinhnhanh.com/image/szAgqL)

HTML forms không hỗ trợ các request `PUT`, `PATCH`, hoặc `DELETE`, bạn sẽ cần thêm một trường hidden  `_method` vào spoof HTTP verbs. Phương thức `method_field` có thể làm điều đó gúp bạn:

```
{{ method_field('PUT') }}
```

### a. Tên tham số Resource Route

Mặc định, Route::resource sẽ sinh ra tham số route cho resource routes dựa trên tên của resource. Bạn có thể dễ dàng ghi đè cho từng phần resource cơ bản bằng cách truyền parameters trong mảng như bên dưới.

``` 
Route::resource('user', 'AdminUserController', ['parameters' => [
    'user' => 'admin_user'
]]);
```
### b. Bổ sung Resource Controllers

Bạn cũng có thể bổ sung thêm Resource Controllers.Bạn nên định nghĩa những routes đó trướckhi gọi `Route::resource`, nếu không thì những route đã được định nghĩa bởi resource method có thể vô tình bị ưu tiên hơn những route bạn bổ sung:
```
Route::get('photos/popular', 'PhotoController@method');
Route::resource('photos', 'PhotoController');
```
### c. Tên Resource Routes

Mặc định, tất cả các action của resource controller đều có tên route. Tuy nhiên, bạn có thể ghi đè tên đó bằng cách truyền thêm mảng chứa names với tùy chọn của bạn:

```
Route::resource('photo', 'PhotoController', ['names' => [
    'create' => 'photo.build'
]]);
```
### d. Từng phần Resource Routes

Khi bạn khai báo một resource route, bạn có thể chỉ định các tập con action của controller cần xử lý thay vì toàn bộ action mặc định ban đầu:

```
Route::resource('photo', 'PhotoController', ['only' => [
    'index', 'show'
]]);
Route::resource('photo', 'PhotoController', ['except' => [
    'create', 'store', 'update', 'destroy'
]]);
```

Xem thêm tại:  [Route group document](https://laravel.com/docs/5.5/controllers#restful-partial-resource-routes)


