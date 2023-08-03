# PHP-7-and-8-difference


**Named arguments:**
```php
// PHP 7
hash("md5", "Hello")

// PHP 8
hash(data: "Hello", algo: "md5");
```
<br>

**Constructor property promotion**
```php
// PHP 7
class Cube{
    public float $x;
    public float $y;
    public float $z;
    
    public function __construct(
    float $x = 0.0,
    float $y = 0.0,
    float $z = 0.0,
    ){
        $this->x = $x;
        $this->y = $x;
        $this->z = $x;
    }
}

// PHP 8
class Cube{
    public function __construct(
    public float $x = 0.0,
    public float $y = 0.0,
    public float $z = 0.0,
    ) {}
}
```
<br>

**Union types:**
```php
// PHP 7
class Calculator{
    /**
    * @param float|int $a
    * @param float|int $b
     */
    public function multiple($a, $b){
        return ($a + $b);
    }
}

echo (new Calculator)->multiple(NULL, 1); // OK: 1

// PHP 8
class Calculator{
    public function multiple(int|float $a, int|float $b){
        return ($a + $b);
    }
}

echo (new Calculator)->multiple(NULL, 1); // Fatal error
```

<br>

**Match expressions:**
```php
// PHP 7 
switch(5){
    case "5":
    $result = "Type problem";
    break;
    case 5:
    $result = "Store in variable";
    break;
}

// PHP 8
 return match(5){
     5  => "No extra variable",
    "5" => "Strict type compare"
 }
```

<br>

**Nullsafe operators:**
```php
// PHP 7 
$categoryId = null;

$post = Post::find(10);
if($post !== null){
    if($post->category != null){
        $categoryId = $post->category->id;
    }  
}

// PHP 8
 $username =  User::find(10)?->category?->id;
```
<br>

**Enumerations:**
```php
// PHP 7 
class PaymentStatus
{
    const CREATED   = 'created';
    const PAYED     = 'paid';
    const CANCELLED = 'cancelled';
    const ERROR     = 'error';
}
function changeStatus(string $status) {}

// PHP 81
enum PaymentStatus: string
{
    CREATED   = 'created';
    PAYED     = 'paid';
    CANCELLED = 'cancelled';
    ERROR     = 'error';
}
function changeStatus(PaymentStatus $status) {}
```
<br>

**Readonly properties:**
```php
// PHP 7 
class Post
{
    private string $title;
    private int $authorId;
   
    public function __construct(string $title, int $authorId) 
    {
        $this->title = $title;
        $this->authorId = $authorId;
    }
    
    public function __set($name, $value) {
        throw new \Exception("Setting property '$name' is not allowed.");
    }
}

// PHP 81
class Post
{
    public function __construct(
        public readonly string $title,
        public readonly int $authorId,
    ) {}
}

$post = new Post(
    title: 'PHP readonly properties', 
    status: 123, 
);

echo $post->title = 'Another title'; // Fatal error
```

<br>

**Final keyword:**
```php
// PHP 7 
class A {}
class B extends A {}  // OK

class A {
    public function write(){}
}

class B extends A {
    public function write(){}
}

// PHP 81
final class A {}
class B extends A {} // Fatal error: Class A may not inherit from final class

class A {
    final public function write(){}
}

class B extends A {
    public function write(){} // Fatal error on overriding
}
```
