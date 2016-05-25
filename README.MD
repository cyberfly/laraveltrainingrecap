Laravel 5.2 Basic - Create AduanICT system


----------------------------------------------
----------------------------------------------
Day 1
----------------------------------------------
----------------------------------------------

-------
1. Create a master layout
-------

a) Copy your HTML layout to folder views/layouts

b) Choose one layout to edit and make it master layout by naming it into app.blade.php and put in views/layouts folder 

c) Move CSS and JS use by the HTML layouts into Laravel public folder. Put it into seperate folder by types. Example if CSS file, put into public/css folder, and JS file put into public/js folder.

d) Reference the CSS and JS in app.blade.php using Laravel URL helper

{{ URL::asset('css/style.css') }}
{{ URL::asset('js/bootstrap.min.js') }}

---------
2. Build a MVC for Complain object
---------

a) Controller : ComplainController.php
b) Model : Complain.php
c) Views : complains/index.blade.php
		   complains/create.blade.php	
		   complains/edit.blade.php	
		   complains/show.blade.php	

command to generate :

controller : php artisan make:controller ComplainController --resource
model : php artisan make:model Complain
views : manually create folder and file in views, or rename your existing html file into .blade.php file

----
3. Update views to use Laravel Collective form tag
----

a) Install Laravel Collective => https://laravelcollective.com/docs/5.2/html#installation

b) Update <form> and </form> tag to use Laravel Collective form helper syntax

create.blade.php

{!! Form::open(array('route' => 'complain.store','class'=>"form-horizontal")) !!}

edit.blade.php

{!! Form::open(array('route' => ['complain.update',$complain->ADUAN_ID],'method'=>'put','class'=>"form-horizontal")) !!}

----------
----------
Day 2
----------
----------

-------------
1. Create a simple function to Store and Update complains
-------------

Since this is Complain controller, make sure to load Complain model to interact with database

a) function create() return a views create.blade.php
b) function store() will process create form and store into database

$complain = new Complain;
$complain->aduan = $aduan;
$complain->save();

c) function edit() return a views edit.blade.php

d) function update() process edit form and store the change into database

$complain = Complain::find($id);

$complain->aduan = $aduan;

$complain->save();

----------
2. Create a validation for ComplainController (function Store and function Update)
----------

a) Use Laravel request class to manage validation. Terminal command to generate:

php artisan make:request ComplainRequest

b) Use $this->method in the ComplainRequest to determine POST and PUT method

c) Write validation rules for POST and PUT based on this documentation

https://laravel.com/docs/5.2/validation

d) Update views create.blade.php, edit.blade.php to show Validation errror message, Validation error box outline and also hold older form value before submit

----------
3. Create a login, logout, register using Laravel Auth
----------

use this command at terminal:

php artisan make:auth

--------------
4. Update default login route
--------------

a) Change the route where user will redirect after login

protected $redirectTo = 'complain';

--------------
5. Get the current login user info
--------------

Use this code :

Auth::user()->name
Auth::user()->email
Auth::user()->id

If you use the code in Controller, make sure to put this code before opening the class:

use Auth;

--------------
6. Check permission only for login user can access
--------------

Add constructor to your Controller and using middleware auth

class ComplainController extends Controller
{
	public function __construct(Request $request)
	{
	    $this->middleware('auth');
	}
}

---
7. Use dynamic dropdown on create.blade.php and edit.blade.php
---

a) ComplainController.php, function create()

$users = User::where('id','!=',Auth::user()->id)->lists('name','id');

$users = array(''=>'Pilih Pengguna') + $users->all();

return view('complains/create',compact('users'));


b) view create.blade.php

{!! Form::select('login_daftar', $users, '', ['class' => 'form-control']); !!}




