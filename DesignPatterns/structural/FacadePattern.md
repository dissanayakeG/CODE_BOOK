- Facade is a structural design pattern that provides a simplified interface to a library, a framework, or any other complex set of
classes.

- These are some of the classes of a complex 3rd-party video conversion framework. We don't control that code, therefore can't simplify it.

```php
class VideoFile {...}
class OggCompressionCodec {...}
class MPEG4CompressionCodec {...}
class CodecFactory {...}
class BitrateReader {...}
class AudioMixer {...}
```

- We create a facade class to hide the framework's complexity behind a simple interface. It's a trade-off between functionality and simplicity.
```php
class VideoConverter is

method convert(filename, format):File is
  file = new VideoFile(filename)
  sourceCodec = new CodecFactory.extract(file)
  if (format == "mp4")
    destinationCodec = new MPEG4CompressionCodec()
  else
    destinationCodec = new OggCompressionCodec()
  buffer = BitrateReader.read(filename, sourceCodec)
  result = BitrateReader.convert(buffer, destinationCodec)
  result = (new AudioMixer()).fix(result)
  return new File(result)
```

- Application classes don't depend on a billion classes provided by the complex framework. Also, if you decide to switch frameworks, you only need to rewrite the facade class.
```php
class Application is
  method main() is
    convertor = new VideoConverter()
    mp4 = convertor.convert("youtubevideo.ogg", "mp4")
    mp4.save()
```

**Example**

```php
<?php

// Subsystem 1: Product Management
class Product {
    public function createProduct($name, $price) {
        // Create a new product
        return "Product created: $name, $price";
    }
}

// Subsystem 2: Order Management
class Order {
    public function createOrder($productId, $quantity) {
        // Create a new order
        return "Order created for product $productId with quantity $quantity";
    }
}

// Subsystem 3: User Management
class User {
    public function createUser($name, $email) {
        // Create a new user
        return "User created: $name, $email";
    }
}

// Facade class
class ShopFacade {
    private $product;
    private $order;
    private $user;

    public function __construct() {
        $this->product = new Product();
        $this->order = new Order();
        $this->user = new User();
    }

    public function buyProduct($productName, $price, $quantity, $userName, $email) {
        $productResult = $this->product->createProduct($productName, $price);
        $userResult = $this->user->createUser($userName, $email);
        $orderResult = $this->order->createOrder(1, $quantity); // Assuming product ID is 1

        return "Summary:\n$productResult\n$userResult\n$orderResult";
    }
}

// Usage
$shop = new ShopFacade();
$result = $shop->buyProduct("Product A", 100, 2, "John Doe", "john@example.com");

echo $result;
```
