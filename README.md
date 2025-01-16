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
> `php artisan make:resource ProductResource`
---
### Api/ProductController.php
```
public function index()
{
    $products = Product::get();
    if ($products->count() > 0) {
        return ProductResource::collection($products);
    } else {
        return response()->json(['message' => 'No records available!'], 200);
    }
}
```
---
### Resource/ProductResource.php
```
// if you want to show all the detail then leave this as it is
// return parent::toArray($request);

// if you want to show only specific details then,
return [
    'id' => $this->id,
    'name' => $this->name,
    'description' => $this->description,
    'price' => $this->price,
    'created_at' => $this->created_at,
];
```
---
### Api/ProductController.php
```
public function store(Request $request)
    {
        $validator = Validator::make($request->all(), [
            'name' => 'required|string|max:255',
            'description' => 'required',
            'price' => 'required|numeric|min:1'
        ]);

        if ($validator->fails()) {
            return response()->json([
                'message' => 'All fields are mandatory!',
                'error' => $validator->messages(),
            ], 422);
        }

        $product = Product::create([
            'name' => $request->name,
            'description' => $request->description,
            'price' => $request->price
        ]);

        return response()->json([
            'message' => 'Product Created Successfully !',
            'data' => new ProductResource($product),
        ], 200);
    }
```
---
### Api/ProductController.php
```
public function update(Request $request, Product $product) {
    $validator = Validator::make($request->all(), [
        'name' => 'required|string|max:255',
        'description' => 'required',
        'price' => 'required|numeric|min:1'
    ]);

    if ($validator->fails()) {
        return response()->json([
            'message' => 'All fields are mandatory!',
            'error' => $validator->messages(),
        ], 422);
    }

    $product->update([
        'name' => $request->name,
        'description' => $request->description,
        'price' => $request->price
    ]);

    return response()->json([
        'message' => 'Product Updated Successfully !',
        'data' => new ProductResource($product),
    ], 200);
}
```
---
### Api/ProductController.php
```
public function destroy(Product $product)
{
    $product->delete();
    return response()->json([
        'message' => 'Product Deleted Successfully !',
    ], 200);
}
```

### bootstrap/app.php
```
->withExceptions(function (Exceptions $exceptions) {
    $exceptions->render(function (NotFoundHttpException $e, Request $request){
        if($request->is('api/*')){
            return response()->json([
                'message'=>'Record not found!'
            ], 404);
        }
    });
})->create();
```
---