# Mybusiness2.0 Backend - Updated for Laravel 11

## Basic installation instructions

**1.** Git clone.

**2.** Move .env-example.env to .env.

**3.** Change the sql server credentials as needed.

**4.** Download the SQL Server ODBC drivers (both normal and PDO) and place the dll files in php/ext.

**5.** In php.ini, enable the sodium and zip extensions.

**6.** In php.ini, add the sqlsrv extensions.

**7.** First time install will require the following commands to be executed. Some may have to be done after every update.

    `composer install`

    `php artisan migrate`
   
    `php artisan passport:keys`

    `php artisan passport:client --personal`
   
    `php artisan:serve`


**8.** Seed the database with values.

    (for some reason, in Unix, I have to run these as SUDO. Also, Step 2 below fails
    and seems to be unnecessary)

    `php artisan db:seed --class=PermissionSeeder`

    `php artisan db:seed --class=RoleSeeder`

    `php artisan db:seed --class=UserSeeder`


**9.** Go to http://127.0.0.1:8000/api/documentation

**10.** Settings for accessing files stored in the storage
     `php artisan storage:link`
    `chmod -R 775 storage bootstrap/cache`

**-------- IMPORTANT --------**
Every time it is necessary you should run the following seeder to assign the new permissions to the administrator user
    `php artisan db:seed --class=AssignNewPermissions`
    `php artisan db:seed --class=SessionsTableSeeder`

Generate the Swagger documentation if it hasn't been generated before.

    `php artisan l5-swagger:generate v1`

1. Start the local server:

    `php artisan:serve`


**10.** Now you can test any of the API endpoints. Using Postman is higly recommended for testing.


## Instructions for Running Unit Tests

**1.** After migrating the database, run the database seeders and verify the email address of the Administrator user.

**2.** If you are using a different database for testing other than localhost, modify the baseUrl found in tests/TestCase.php

**3.** Use  

`php artisan test` 

   command to run all currently available Unit Tests

**4.** Use

  `php artisan test --group="insert group name here"` 

   to run a single feature test or predefined groups

**5.** To run a single feature test using the above command, type the name of the model in camel case.
   For example, if you want to run the feature test for visa type, the command to enter will be:

   `php artisan test --group=visaType`

   The same principle is applied to running all currently defined groups.

Currently Defined Groups

**1.** hr

**2.** common

**3.** Academic

## Tips

**1.** To run migrations or database seeders on a testing environment, create or modify a .env.testing file and add

   `--env=testing` 

   to the end of each `php artisan` command.

   For example:
   `php artisan migrate --env=testing`

**2.** `If you run into unexplained issues with unit tests that should work, please make sure that your migration files
       are up to date. Having out of date database tables will lead to failed unit tests. You can rerun them with:
       
`php artisan migrate:fresh --env=testing` or `php artisan migrate:refresh --env=testing`

    This will drop all tables and rerun the migrations. Also make sure to generate a personal access token and run the seeders on the testing database using:

`php artisan passport:client --personal --env=testing`
`php artisan db:seed --env=testing`

    Also check to verify that project changes don't clash with feature tests. This will help to determine if
    a failed unit test is caused by bugs in the code or simply an out of date unit tests.
=======
    - Generate an auth token /api/login
    - Add the token to authorize lock  Bearer [pasted token from above]

    if Authorization fails, the PAC needs to be re-generated using:

    `php artisan passport:client --personal`
