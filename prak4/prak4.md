# Basic Routing dan Migration

Anas Al Halimi Arif<br />a
215150700111036<br />
PEMIN - A<br /><br />
 
* ## Percobaan
>- **GET**<br /><br />
>Untuk menambahkan endpoint dengan method GET pada aplikasi kita, kita dapat mengunjungi file web.php pada folder routes. Kemudian tambahkan baris ini pada akhir file<br /><br />
>```javascript
>...
>$router->get('/get', function () {
>    return 'GET';
>});
>```
>![Screenshot](../Screenshoot/prak4/1.png)<br /><br />
>Setelah itu coba jalankan aplikasi dengan command,<br /><br />
>```php -S localhost:8000 -t public```<br /><br />
>![Screenshot](../Screenshoot/prak4/2.png)<br /><br />
>Setelah aplikasi berhasil dijalankan, kita dapat membuka browser dengan url, ```http://localhost:8000/get``` , path yang akan kita akses akan berbentuk demikian, ```http://{BASE_URL}{PATH}``` , jika BASE_URL kita adalah ```localhost:8000``` dan PATH kita adalah ```/get```, maka url akan berbentuk seperti diatas.<br /><br />
>![Screenshot](../Screenshoot/prak4/3.png)<br /><br />
>- **POST, PUT, PATCH, DELETE, dan OPTIONS**<br /><br />
> Sama halnya saat menambahkan method GET, kita dapat menambahkan methode POST, PUT, PATCH, DELETE, dan OPTIONS pada file web php dengan code seperti ini,<br /><br />
>```javascript
>...
>$router->post('/post', function () {
>    return 'POST';
>});
>
>$router->put('/put', function () {
>    return 'PUT';
>});
>    
>$router->patch('/patch', function () {
>    return 'PATCH';
>});
>    
>$router->delete('/delete', function () {
>    return 'DELETE';
>});
>    
>$router->options('/options', function () {
>    return 'OPTIONS';
>});
>```
>![Screenshot](../Screenshoot/prak4/4.png)<br /><br />
>Setelah selesai menambahkan route untuk method POST, PUT, PATCH, DELETE, dan OPTIONS, kita dapat menjalankan server seperti pada saat percobaan GET. Setelah server berhasil menyala, kita dapat membuka aplikasi Postman atau Insomnia atau kita juga dapat menggunakan PowerShell (Windows) / Terminal (Linux atau Mac) untuk melakukan request ke server. Namun, pada percobaan kali ini kita akan menggunakan extensions pada VSCode yaitu Thunder Client.<br /><br />
>1. Kita dapat menginstall ekstensi dengan membuka panel extensions lalu mencari thunder client<br /><br />
>![Screenshot](../Screenshoot/prak4/exm1.png)<br /><br />
>2. Setelah menginstall Thunder Client, kita akan melihat logo seperti petir pada activity bar kita (sebelah kiri).<br /><br />
>3. Kita dapat membuat request dengan menekan "New Request" pada ekstensi<br /><br />
>![Screenshot](../Screenshoot/prak4/exm2.png)<br /><br />
>4. Setelah itu kita dapat memasukkan method dan url yang dituju<br /><br />
>![Screenshot](../Screenshoot/prak4/exm3.png)<br /><br />
>5. Akses url yang baru saja ditambahkan pada aplikasi dengan methodnya. Di sini kita akan mencoba semua method, yaitu POST, PUT, PATCH, DELETE, dan OPTIONS<br /><br />
>![Screenshot](../Screenshoot/prak4/5.png)<br /><br />
>![Screenshot](../Screenshoot/prak4/6.png)<br /><br />
>![Screenshot](../Screenshoot/prak4/7.png)<br /><br />
>![Screenshot](../Screenshoot/prak4/8.png)<br /><br />
>![Screenshot](../Screenshoot/prak4/9.png)<br /><br />
>- **Migrasi Database**<br /><br />
>1. Sebelum melakukan migrasi database pastikan server database aktif kemudian
pastikan sudah membuat database dengan nama ```lumenapi``` <br /><br />
>![Screenshot](../Screenshoot/prak4/10.png)<br /><br />
>2.  Kemudian ubah konfigurasi database pada file ```.env``` menjadi seperti ini<br /><br />
>```javascript
>DB_CONNECTION=mysql
>DB_HOST=127.0.0.1
>DB_PORT=3306
>DB_DATABASE=lumenapi
>DB_USERNAME=root
>DB_PASSWORD=<<password masing-masing>>
>```
>![Screenshot](../Screenshoot/prak4/11.png)<br /><br />
>3. Setelah mengubah konfigurasi pada file .env, kita juga perlu menghidupkan beberapa library bawaan dari lumen dengan membuka file app.php pada folder bootstrap dan mengubah baris ini,<br /><br />
>```javascript
>//$app->withFacades();
>//$app->withEloquent();
>```
>Menjadi.<br /><br />
>```javascript
>$app->withFacades();
>$app->withEloquent();
>```
>![Screenshot](../Screenshoot/prak4/12.png)<br /><br >
>4. Setelah itu jalankan command berikut untuk membuat file migration,<br /><br />
>```javascript
>php artisan make:migration create_users_table #membuat migrasi untuk tabel users
>php artisan make:migration create_products_table #membuat migrasi untuk tabel products
>```
>Setelah menjalankan 2 syntax diatas akan terbuat 2 file pada folder ```database/migrations``` dengan format YYYY_MM_DD_HHmmss_nama_migrasi. Pada file migrasi kita akan menemukan fungsi up() dan fungsi down(), fungsi up() akan digunakan pada saat kita melakukan migrasi, fungsi down() akan digunakan saat kita ingin me-rollback migrasi<br /><br />
>![Screenshot](../Screenshoot/prak4/13.png)<br /><br >
>5. Ubah fungsi up pada file migrasi ```create_users_table```<br /><br />
>```javascript
>...
>    public function up()
>    {
>        Schema::create('users', function (Blueprint $table) {
>            $table->id();
>            $table->timestamps();
>        });
>    }
>...
># diubah menjadi
>...
>    public function up()
>    {
>        Schema::create('users', function (Blueprint $table) {
>            $table->id();
>            $table->timestamps();
>            $table->string('name');
>            $table->string('email');
>            $table->string('password');
>        });
>    }
>...
>```
>![Screenshot](../Screenshoot/prak4/14.png)<br /><br >
>6. Ubah fungsi up pada file migrasi ```create_products_table```<br /><br />
>```javascript
># sebelumnya
>...
>public function up()
>{
>    Schema::create('products', function (Blueprint $table) {
>        $table->id();
>        $table->timestamps();
>    });
>}
>...
># diubah menjadi
>...
>    public function up()
>    {
>        Schema::create('products', function >(Blueprint $table) {
>            $table->id();
>            $table->timestamps();
>            $table->string('name');
>            $table->integer('category_id');
>            $table->string('slug');
>            $table->integer('price');
>            $table->integer('weight');
>            $table->text('description');
>        });
>    }
>...
>```
>![Screenshot](../Screenshoot/prak4/15.png)<br /><br >
>7. Kemudian jalankan command,<br /><br />
>```php artisan migrate```<br /><br >
>![Screenshot](../Screenshoot/prak4/16.png)<br /><br >