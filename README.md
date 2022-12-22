# Crash Log

## here you can track all errors found in your application, and report it to your taskk manager app.
## the idea depends on collect the error data, fire exception, and send.
## if failier in sending, a json file will be created for later as soon as possible to resend them.
## the updates will be in 


### first add the Crash Folder to 'app\Services' in your laravel app. 

1. Exception Class
``` php 
## LARAVEL > 7 {Throwable Class}
public function register(){
    $this->reportable(function(Throwable $e){

        App\Services\Crash\CrashController::handler($e);

    });
}

## LARAVEL < 8 { Exception Class }
public function render($request, Exception $exception){

    App\Services\Crash\CrashController::handler($exception);

    return parent::render($request, $exception);
}

```
2. Routes
``` php

## track failed errors
Route::get('/resend-error-logs', function(){
    App\Services\Crash\CrashController::resendErrors();
})->name('resend-errors');

```
3. Your master Blade Script
``` javasript

// JS script to interval sending
<script>
    setInterval(resendErrors, 1000*60*10) // every 10 minutes
    function resendErrors(){
        $.get('{{ route('resend-errors') }}', function(){});
    }
</script>

```

4. in Crashcontroller, You will find the url Constant to your api like me.
``` php
const _URL = 'https://your-domain/task-manager/crash-log';
```
5. and if you have a multiple projects, assign it in your env file 
``` readme
PROJECT=x
```
7. Add the crash logs table to your tasks database
``` mysql
    CREATE TABLE `log_crashs` (
    `id` bigint(20) NOT NULL AUTO_INCREMENT,
    `msg` text DEFAULT NULL,
    `happend_at` timestamp NULL DEFAULT NULL,
    `timezone` varchar(255) DEFAULT NULL,
    `request_url` text DEFAULT NULL,
    `project_id` bigint(20) DEFAULT NULL,
    `query` text DEFAULT NULL,
    `headers` text DEFAULT NULL,
    `created_at` timestamp NULL DEFAULT NULL,
    `updated_at` timestamp NULL DEFAULT NULL,
    `company_id` int(11) DEFAULT 5,
    `app_type` varchar(255) DEFAULT NULL,
    `details` text DEFAULT NULL,
    `status` tinyint(1) NOT NULL DEFAULT 0,
    PRIMARY KEY (`id`)
    ) ENGINE=InnoDB AUTO_INCREMENT=6421 DEFAULT CHARSET=utf8
```
6. logs will be created in 'app/Services/Crash/crash-logs' folder.

