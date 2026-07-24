Behzad Ghabaei <br>
CS 85 PHP <br>
Module 10 Assignment 10A User Authentication <br>
Instructor Seno <br>
7/22/2026 <br>

# Authentication Lab 

### 1. What is the difference between authentication and authorization?
* **Authentication** confirms identity (proving *who* you are, usually via login credentials).  Verifying who a user is (login).  Verifies who a user is by checking identity credentials like passwords.
* **Authorization** determines permissions (proving *what* you are allowed to do after logging in).  Deciding what a verified user may do.  Determines what an identified user is allowed to access, such as admin permissions.

### 2. Why are passwords hashed instead of stored as plain text?
* **Security protection** ensures that if a database is leaked or stolen, hackers cannot read user passwords.
* **One-way encryption** means hashes cannot be reversed back into plain text, protecting user credentials across other services.
* **Hashing**  a one-way scramble of a password; can't be reversed back.
* **Example Code** in a Controller folder with a file like UserController.php here is a samole of hashing a password:
   ```
    $incomingFields = $request -> validate([
    'password' => ['required', 'min:8','max:200'] ]);
    $incomingFields['password'] = bcrypt( $incomingFields['password'] );
   ```
* **Data breach protection** ensures that stolen database records only reveal unreadable cryptographic strings.
* **Irreversible verification** lets systems validate user entries securely without storing actual secrets.

### 3. Which file defines the /login and /register routes?
* **`routes/auth.php`** defines these routes (which are automatically imported into `routes/web.php` by the starter kit).
* **`routes/web.php`** maps them via standard Livewire component routing declarations.

When you chose Laravel Volt during the project setup. <br>
In Laravel 12-13 when you combine the new Livewire Starter Kit with Laravel Volt, <br>
traditional route files like routes/auth.php are completely removed. <br>
Instead, Laravel uses single-file Volt components that handle their own routing, actions, and layouts in one place. <br>
Another good answer is: to use the command: 
**`Herd\auth-demo>php artisan route:list`**
looking at the terminal's return output list:
```
GET|HEAD login .... login › Laravel\Fortify › AuthenticatedSessionController@create
POST login .... login.store › Laravel\Fortify › AuthenticatedSessionController@store

GET|HEAD register .... register › Laravel\Fortify › RegisteredUserController@create
POST register .... register.store › Laravel\Fortify › RegisteredUserController@store
```
the file **`vendor/laravel/fortify/routes/routes.php`** is the authentication management.

the **/login** definitions in the file are:
```
// GET /login
Route::get(RoutePath::for('login', '/login'), [AuthenticatedSessionController::class, 'create'])

// POST /login
Route::post(RoutePath::for('login', '/login'), [AuthenticatedSessionController::class, 'store'])
```
and the **/register** definitions in the file are:
```
// GET /register
Route::get(RoutePath::for('register', '/register'), [RegisteredUserController::class, 'create'])

// POST /register
Route::post(RoutePath::for('register', '/register'), [RegisteredUserController::class, 'store'])

```

### 4. What does the auth middleware do, and what happens when a logged-out user hits a page protected by it?
* **Guard duty** checks if an incoming request originates from an active, authenticated user session.
* **Automatic redirection** intercepts unauthenticated guests and bounces them back to the `/login` page.
* **Route protection** intercepts all incoming web traffic to verify active login tokens.
* **Guest rejection** stops unauthenticated visitors instantly and forces a redirect to `/login`.
