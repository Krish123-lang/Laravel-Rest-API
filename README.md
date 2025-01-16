# REST API 
> `php artisan install:api`
---
# CRUD API
> `php artisan make:migration create_products_table`
---
### `create_products_table.php`
```
Schema::create('products', function (Blueprint $table) {
    $table->id();
    $table->string('name');
    $table->mediumText('description');
    $table->string('price');
    $table->timestamps();
});
```
---
> `php artisan make:model Product`
---
### `models/Product.php`
```
protected $table = 'products';
protected $fillable = [
    "name",
    "description",
    "price"
];
```
---
> `php artisan make:controller Api/ProductController`
---
### routes/api.php
> `Route::apiResource('products', ProductController::class);`
---
### Api/ProductController.php
```
public function index() {}
public function store() {}
public function show() {}
public function update() {}
public function destroy() {}
```
---
