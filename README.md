# Create Laravel 9 Vue JS CRUD Single Page Application

Throughout this Laravel Vue js CRUD example tutorial, you will see and understand how to build Create, Read, Update, and Delete API and how to consume APIs to perform CRUD operations in Laravel Vue JS application.

In computer science, create, read, update, and delete are not just mere words. They are the foundational building block of every application for creating, reading, updating, and deleting the data.

The terminology might be different sometimes, for instance, retrieval rather than read, modify somewhat of the update, or destroy alternately of delete.

Nevertheless, in this laravel vue js crud tutorial, we will help you ascertain how to integrate the laravel vue js CRUD operations in a single page application with vue js components and laravel app.

**Laravel 9 Vue JS CRUD Example**
**Create a Laravel Project**
**Enable database connection**
**Create a model and run migration**
**Create and configure the controller**
**Add routes**
**Install Laravel Vue UI**
**Install NPM dependencies**
**Build Vue Js CRUD Components**
**Test Laravel Vue JS CRUD operations**
**Let’s start creating Laravel Vue Js SPA CRUD application gradually.**

# Install Laravel App

Open terminal further run below command to manifest a new laravel project, ignore this step if project is already installed:

```php
composer create-project laravel/laravel laravel-vue-crud --prefer-dist
```

# Database Connection

This step explains how to make database connection by adding database name, username and password in .env config file of your project:

```php
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=db
DB_USERNAME=root
DB_PASSWORD=
```

.properties

# Set Up Model and Run Migration

Run the below process to respectively create the Model (database table) and migration file:
``Bash
php artisan make:model Product -m

````
Add following code in database/migrations/create_products_table.php:
```php
<?php
use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;
class CreateProductsTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('products', function (Blueprint $table) {
            $table->id();
            $table->string('name');
            $table->text('detail');
            $table->timestamps();
        });
    }
    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::dropIfExists('products');
    }
}
````

Define Product table values in app/Models/Product.php:

```
<?php
namespace App\Models;
use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;
class Product extends Model
{
    use HasFactory;
    protected $fillable = [
        'name',
        'detail'
    ];
}
```

Next, you need to evoke migration with below command:

```Bash
php artisan migrate
```

# Create Product Controller

You need to create the product controller and define the CRUD operations methods:

php artisan make:controller ProductController
Bash
Open and update the below code in app\Http\Controllers\ProductController.php:

```php
<?php
namespace App\Http\Controllers;
use Illuminate\Http\Request;
use App\Models\Product;
class ProductController extends Controller
{
    public function index()
    {
        $products = Product::all()->toArray();
        return array_reverse($products);
    }
    public function store(Request $request)
    {
        $product = new Product([
            'name' => $request->input('name'),
            'detail' => $request->input('detail')
        ]);
        $product->save();
        return response()->json('Product created!');
    }
    public function show($id)
    {
        $product = Product::find($id);
        return response()->json($product);
    }
    public function update($id, Request $request)
    {
        $product = Product::find($id);
        $product->update($request->all());
        return response()->json('Product updated!');
    }
    public function destroy($id)
    {
        $product = Product::find($id);
        $product->delete();
        return response()->json('Product deleted!');
    }
}
```

# Create CRUD Routes

Open routes/web.php file, in here; you have to define the following route:

```php
<?php
use Illuminate\Support\Facades\Route;
/*
|--------------------------------------------------------------------------
| Web Routes
|--------------------------------------------------------------------------
|
*/

Route::get('{any}', function () {
    return view('app');
})->where('any', '.*');
```

Next, you need to open the routes/api.php file. First, import the product controller on top, then define the CRUD API routes in the route group method as given below:

```php
<?php
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Route;
use App\Http\Controllers\ProductController;
/*
|--------------------------------------------------------------------------
| API Routes
|--------------------------------------------------------------------------
*/
Route::middleware('api')->group(function () {
    Route::resource('products', ProductController::class);
});
```

# Install Laravel Vue UI

Run composer command to install Vue UI in laravel, it will manifest vue laravel scaffold:

```bash
composer require laravel/ui
```

php artisan ui vue
``
After that, use the command to install the vue router and vue axios packages. These packages are used to consume Laravel CRUD APIs.

```Bash
npm install vue-router vue-axios
```

Subsequently, execute the command to install npm packages:

```Bash
npm install
```

The npm run watch command compiles the assets, not just that with run watch command you don’t fret about re-run the compiler over and over again.

```Bash
npm run watch
```

# Initiate Vue in Laravel

To set up a vue application in Laravel, you have to create a layout folder in the resources/views directory and create an app.blade.php file within the layout folder.

Place below code in resources/views/layout/app.blade.php file:

```php
<!doctype html>
<html lang="{{ str_replace('_', '-', app()->getLocale()) }}">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <meta name="csrf-token" value="{{ csrf_token() }}" />
    <title>Vue JS CRUD Operations in Laravel</title>
    <link href="{{ mix('css/app.css') }}" type="text/css" rel="stylesheet" />
</head>
<body>
    <div id="app"></div>
    <script src="{{ mix('js/app.js') }}" type="text/javascript"></script>
</body>
</html>
```

# Crete Vue CRUD Components

Next, you have to add the router-view directive in App.vue file; this template will help invoke all the vue routes associated with the components.

So, create App.js file in resources/js folder, put the below code in resources/js/App.js file:

```JavaScript
<template>
    <div class="container">
        <nav class="navbar navbar-expand-lg navbar-light bg-light">
            <div class="collapse navbar-collapse">
                <div class="navbar-nav">
                    <router-link to="/" class="nav-item nav-link">Products List</router-link>
                    <router-link to="/create" class="nav-item nav-link">Create Product</router-link>
                </div>
            </div>
        </nav>
        <router-view> </router-view>
    </div>
</template>

<script>
    export default {}
</script>
```

Now, you need to create the following vue js component files inside the resources/js/components folder:

**_AllProduct.vue_**
**_CreateProduct.vue_**
**_EditProduct.vue_**
Add code in resources/js/components/AllProduct.vue file; in here we are getting all data from database and deleting single product object from database through vue component.

```JavaScript
<template>
    <div>
        <h2 class="text-center">Products List</h2>

        <table class="table">
            <thead>
            <tr>
                <th>ID</th>
                <th>Name</th>
                <th>Detail</th>
                <!-- <th>Actions</th> -->
            </tr>
            </thead>
            <tbody>
            <tr v-for="product in products" :key="product.id">
                <td>{{ product.id }}</td>
                <td>{{ product.name }}</td>
                <td>{{ product.detail }}</td>
                <td>
                    <div class="btn-group" role="group">
                        <router-link :to="{name: 'edit', params: { id: product.id }}" class="btn btn-success">Edit</router-link>
                        <button class="btn btn-danger" @click="deleteProduct(product.id)">Delete</button>
                    </div>
                </td>
            </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                products: []
            }
        },
        created() {
            this.axios
                .get('http://localhost:8000/api/products/')
                .then(response => {
                    this.products = response.data;
                });
        },
        methods: {
            deleteProduct(id) {
                this.axios
                    .delete(`http://localhost:8000/api/products/${id}`)
                    .then(response => {
                        let i = this.products.map(data => data.id).indexOf(id);
                        this.products.splice(i, 1)
                    });
            }
        }
    }
</script>
```

Place code in resources/js/components/CreateProduct.vue file:

```JavaScript
<template>
    <div>
        <h3 class="text-center">Create Product</h3>
        <div class="row">
            <div class="col-md-6">
                <form @submit.prevent="addProduct">
                    <div class="form-group">
                        <label>Name</label>
                        <input type="text" class="form-control" v-model="product.name">
                    </div>
                    <div class="form-group">
                        <label>Detail</label>
                        <input type="text" class="form-control" v-model="product.detail">
                    </div>
                    <button type="submit" class="btn btn-primary">Create</button>
                </form>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                product: {}
            }
        },
        methods: {
            addProduct() {
                this.axios
                    .post('http://localhost:8000/api/products', this.product)
                    .then(response => (
                        this.$router.push({ name: 'home' })
                    ))
                    .catch(err => console.log(err))
                    .finally(() => this.loading = false)
            }
        }
    }
</script>
```

Head over to resources/js/components/EditProduct.vue template and add the below code:

```JavaScript
<template>
    <div>
        <h3 class="text-center">Edit Product</h3>
        <div class="row">
            <div class="col-md-6">
                <form @submit.prevent="updateProduct">
                    <div class="form-group">
                        <label>Name</label>
                        <input type="text" class="form-control" v-model="product.name">
                    </div>
                    <div class="form-group">
                        <label>Detail</label>
                        <input type="text" class="form-control" v-model="product.detail">
                    </div>
                    <button type="submit" class="btn btn-primary">Update</button>
                </form>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                product: {}
            }
        },
        created() {
            this.axios
                .get(`http://localhost:8000/api/products/${this.$route.params.id}`)
                .then((res) => {
                    this.product = res.data;
                });
        },
        methods: {
            updateProduct() {
                this.axios
                    .patch(`http://localhost:8000/api/products/${this.$route.params.id}`, this.product)
                    .then((res) => {
                        this.$router.push({ name: 'home' });
                    });
            }
        }
    }
</script>
```

# Create Vue CRUD Routes

In this step, you have to create vue routes, create routes.js inside resources/js directory, add the code in the resources/js/routes.js file:

```JavaScript
import AllProduct from './components/AllProduct.vue';
import CreateProduct from './components/CreateProduct.vue';
import EditProduct from './components/EditProduct.vue';

export const routes = [
    {
        name: 'home',
        path: '/',
        component: AllProduct
    },
    {
        name: 'create',
        path: '/create',
        component: CreateProduct
    },
    {
        name: 'edit',
        path: '/edit/:id',
        component: EditProduct
    }
];
```

# Define Vue App.js

This final step brings you to the point where you must include the required packages in the app.js file. Without further ado, please add the following code in the resources/js/app.js file:

```JavaScript
/**
 * First we will load all of this project's JavaScript dependencies which
 * includes Vue and other libraries. It is a great starting point when
 * building robust, powerful web applications using Vue and Laravel.
 */
require('./bootstrap');
window.Vue = require('vue');
import App from './App.vue';
import VueAxios from 'vue-axios';
import VueRouter from 'vue-router';
import axios from 'axios';
import { routes } from './routes';
/**
 * Next, we will create a fresh Vue application instance and attach it to
 * the page. Then, you may begin adding components to this application
 * or customize the JavaScript scaffolding to fit your unique needs.
 */
Vue.use(VueRouter);
Vue.use(VueAxios, axios);

const router = new VueRouter({
    mode: 'history',
    routes: routes
});

const app = new Vue({
    el: '#app',
    router: router,
    render: h => h(App),
});
```

# Start Laravel Vue CRUD App

To start the CRUD app, you need to run the two following commands respectively in two different terminals simultaneously:

```Bash
npm run watch
```

```Bash
php artisan serve
```

Open the URL in the browser:

```php
http://127.0.0.1:8000
```

<img src="https://www.positronx.io/wp-content/uploads/2020/12/12514-01.jpg" alt="">
More about This article at
<a href="https://www.positronx.io/create-laravel-vue-js-crud-single-page-application/">
Digamber
</a>
