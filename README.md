# laravel9_send_mail_using_gmail_smtp_server
## 1. Install Laravel 9
```Dockerfile
composer create-project laravel/laravel laravel9_send_mail_using_gmail_smtp_server
```

```Dockerfile
composer create-project laravel/laravel laravel9_send_mail_using_gmail_smtp_server
```
## 2. Make Configuration
- Vào .env
```Dockerfile
MAIL_MAILER=smtp
MAIL_HOST=smtp.gmail.com
MAIL_PORT=587
MAIL_USERNAME=1812767@dlu.edu.vn
MAIL_PASSWORD=nhập password của email
MAIL_ENCRYPTION=tls
MAIL_FROM_ADDRESS=1812767@dlu.edu.vn
MAIL_FROM_NAME="${APP_NAME}"
```
- Chú ýnhập password của email
- Chú ý bật tính năng sau của email

![Container](a2.png)
![Container](a1.png)

## 3. Create Mail Class
```Dockerfile
php artisan make:mail DemoMail
```
- Vào app/Mail/DemoMail.php
```Dockerfile
<?php
  
namespace App\Mail;
  
use Illuminate\Bus\Queueable;
use Illuminate\Contracts\Queue\ShouldQueue;
use Illuminate\Mail\Mailable;
use Illuminate\Queue\SerializesModels;
  
class DemoMail extends Mailable
{
    use Queueable, SerializesModels;
  
    public $mailData;
  
    /**
     * Create a new message instance.
     *
     * @return void
     */
    public function __construct($mailData)
    {
        $this->mailData = $mailData;
    }
  
    /**
     * Build the message.
     *
     * @return $this
     */
    public function build()
    {
        return $this->subject('Mail from ItSolutionStuff.com')
                    ->view('emails.demoMail');
    }
}
```
## 4. Create Controller
```Dockerfile
php artisan make:controller MailController
```
- Vào app/Http/Controllers/MailController.php
```Dockerfile
<?php
  
namespace App\Http\Controllers;
  
use Illuminate\Http\Request;
use Mail;
use App\Mail\DemoMail;
  
class MailController extends Controller
{
    /**
     * Write code on Method
     *
     * @return response()
     */
    public function index()
    {
        $mailData = [
            'title' => 'Mail from ItSolutionStuff.com',
            'body' => 'This is for testing email using smtp.'
        ];
         
        Mail::to('your_email@gmail.com')->send(new DemoMail($mailData));
           
        dd("Email is sent successfully.");
    }
}
```
- your_email@gmail.com là tài khoản emain để nhận email
## 5. Create Routes
- Vào routes/web.php
```Dockerfile
<?php
  
use Illuminate\Support\Facades\Route;
  
use App\Http\Controllers\MailController;
  
/*
|--------------------------------------------------------------------------
| Web Routes
|--------------------------------------------------------------------------
|
| Here is where you can register web routes for your application. These
| routes are loaded by the RouteServiceProvider within a group which
| contains the "web" middleware group. Now create something great!
|
*/
  
Route::get('send-mail', [MailController::class, 'index']);
```
## 6. Create Blade View
- Vào resources/views/emails/demoMail.blade.php
```Dockerfile
<!DOCTYPE html>
<html>
<head>
    <title>ItsolutionStuff.com</title>
</head>
<body>
    <h1>{{ $mailData['title'] }}</h1>
    <p>{{ $mailData['body'] }}</p>
  
    <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
    tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
    quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
    consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse
    cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non
    proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</p>
     
    <p>Thank you</p>
</body>
</html>
```
## 7: Run Laravel App:
```Dockerfile
php artisan serve
```
- Vào http://localhost:8000/send-mail

![Container](a3.png)
![Container](a.png)


