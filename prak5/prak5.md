# Dynamic Route & Middleware

Anas Al Halimi Arif<br />
215150700111036<br />
PEMIN - A<br /><br />
 
* ## Dynamic Route
> Dynamic route adalah route yang dapat berubah-ubah, contohnya pada saat kita membuka suatu halaman web, kadang kita melihat ```/users/1``` atau ```/users/2```, hal ini yang dinamakan dynamic routes.<br />
>Untuk menambahkan dynamic routes pada aplikasi lumen kita, kita dapat menggunakan syntax berikut,<br /><br />
>```javascript
>$router->get('/user/{id}', function ($id) {
>    return 'User Id = ' . $id;
>});
>```
>![Screenshot](../Screenshoot/prak5/1.png)<br /><br />
>![Screenshot](../Screenshoot/prak5/1.1.png)<br /><br />
>Saat menambahkan parameter pada routes, kita tidak terbatas pada 1 variable saja, namun kita dapat menambahkan sebanyak yang diperlukan seperti kode berikut,<br /><br />
>```javascript
>$router->get('/post/{postId}/comments/{commentId}', function ($postId, $commentId) {
>    return 'Post ID = ' . $postId . ' Comments ID = ' . $commentId;
>});
>```
>![Screenshot](../Screenshoot/prak5/2.png)<br /><br />
>![Screenshot](../Screenshoot/prak5/2.1.png)<br /><br />
> Pada dynamic routes kita juga bisa menambahkan optional routes, yang mana optional routes tidak mengharuskan kita untuk memberi variable pada endpoint kita, namun saat kita memanggil endpoint, dapat menggunakan parameter variable ataupun tidak, seperti pada kode dibawah ini,<br /><br />
>```javascript
>$router->get('/users[/{userId}]', function ($userId = null) {
>    return $userId === null ? 'Data semua users' : 'Data user dengan id ' . $userId;
>});
>```
>![Screenshot](../Screenshoot/prak5/3.png)<br /><br />
>![Screenshot](../Screenshoot/prak5/3.1.png)<br /><br />
>![Screenshot](../Screenshoot/prak5/3.2.png)<br /><br />
* ## Aliases Route
> Aliases Route digunakan untuk memberi nama pada route yang telah kita buat, hal ini dapat membantu kita, saat kita ingin memanggil route tersebut pada aplikasi kita. Berikut syntax untuk menambahkan aliases route<br /><br />
>```javascript
>$router->get('/auth/login', ['as' => 'route.auth.login', function (...) {...}])
>...
>$router->get('/profile', function (Request $request) {
>    if ($request->isLoggedIn) {
>       return redirect()->route('route.auth.login');
>    }
>});
>```
>![Screenshot](../Screenshoot/prak5/4.png)<br /><br />
>![Screenshot](../Screenshoot/prak5/4.1.png)<br /><br />
* ## Group Route
> Pada lumen, kita juga dapat memberikan grouping pada routes kita agar lebih mudah pada saat penulisan route pada web.php kita. Kita dapat melakukan grouping dengan menggunakan syntax berikut,<br /><br />
>```javascript
>$router->group(['prefix' => 'users'], function () use ($router) {
>    $router->get('/', function () { // menjadi /users/, /users => prefix, / => path
>        return "GET /users";
>    });
>});
>```
>![Screenshot](../Screenshoot/prak5/5.png)<br /><br />
>![Screenshot](../Screenshoot/prak5/5.1.png)<br /><br />
>Selain dapat mengelompokkan prefix, kita juga dapat mengelompokkan middleware dan namespace pada kelompok routes kita.<br /><br />
* ## Middleware
> Middleware adalah penengah antara komunikasi aplikasi dan client. Middleware biasanya digunakan untuk membatasi siapa yang dapat berinteraksi dengan aplikasi kita dan semacamnya, kita dapat menambahkan middleware dengan menambahkan file pada folder ```app/Http/Middleware``` . Pada folder tersebut terdapat file ```ExampleMiddleware``` , kita dapat men-copy file tersebut untuk membuat middleware baru.<br />
>Pada praktikum kali ini akan dibuat middleware Age dengan isi,<br /><br />
>```javascript
><?php
>
>namespace App\Http\Middleware;
>
>use Closure;
>
>class AgeMiddleware
>{
>    /**
>     * Handle an incoming request.
>     *
>     * @param  \Illuminate\Http\Request  $request
>     * @param  \Closure  $next
>     * @return mixed
>     */
>    public function handle($request, Closure $next)
>    {
>        return $next($request);
>    }
>}
>```
>![Screenshot](../Screenshoot/prak5/6.png)<br /><br />
> Kemudian, setelah menambahkan filter pada ```AgeMiddleware``` , kita harus mendaftarkan ```AgeMiddleware``` pada aplikasi kita, pada file ```bootstrap/app.php``` seperti berikut ini,<br /><br />
>```javascript
>...
>// $app->middleware([
>//     App\Http\Middleware\ExampleMiddleware::class
>// ]);
>
>$app->routeMiddleware([
>    //       'auth' => App\Http\Middleware\Authenticate::class,
>        'age' => App\Http\Middleware\AgeMiddleware::class
>    ]);
>...
>```
>![Screenshot](../Screenshoot/prak5/7.png)<br /><br />
> Pada baris 65 terdapat comment mengenai proses mendaftarkan suatu middleware dalam aplikasi kita. Untuk menambahkan middleware pada aplikasi kita, kita dapat men- uncomment baris 75 hingga 77, kemudian menambahkan age middleware ke dalamnya. Namun, karena kita hanya ingin menambahkan middleware pada route tertentu, kita akan menghapus comment pada baris 79 hingga 81, kemudian menambahkan middleware age di dalamnya.<br />
>Lalu, kita dapat menambahkan middleware pada routes kita dengan menambahkan opsi middleware pada salah satu route, contohnya,<br /><br />
>```javascript
>$router->get('/admin/home/', ['middleware' => 'age', function () {
>    return 'Dewasa';
>}]);
>
>$router->get('/fail', function () {
>    return 'Dibawah umur';
>});
>```
>![Screenshot](../Screenshoot/prak5/8.png)<br /><br />
>![Screenshot](../Screenshoot/prak5/8.1.png)<br /><br />
