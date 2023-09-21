- Strategy is a behavioral design pattern that lets you define a family of algorithms, put each of them into a separate class, and make their objects interchangeable.

- This pattern is particularly useful when you want to dynamically select and switch between different algorithms or strategies at runtime.

- The strategy interface declares operations common to all  supported versions of some algorithm. The context uses this interface to call the algorithm defined by the concrete strategies.

```php
interface Strategy is
    method execute(a, b)
```

- Concrete strategies implement the algorithm while following the base strategy interface. The  interface makes them interchangeable in the context.

```php
class ConcreteStrategyAdd implements Strategy is

    method execute(a, b) is
        return a + b

class ConcreteStrategySubtract implements Strategy is
    method execute(a, b) is
        return a - b

class ConcreteStrategyMultiply implements Strategy is
    method execute(a, b) is
        return a * b
```

- The context defines the interface of interest to clients.

```php
class Context is
    // The context maintains a reference to one of the strategy
    // objects. The context doesn't know the concrete class of a
    // strategy. It should work with all strategies via the
    // strategy interface.

    private strategy: Strategy

    // Usually the context accepts a strategy through the
    // constructor, and also provides a setter so that the
    // strategy can be switched at runtime.

    method setStrategy(Strategy strategy) is
        this.strategy = strategy

    // The context delegates some work to the strategy object
    // instead of implementing multiple versions of the
    // algorithm on its own.

    method executeStrategy(int a, int b) is
        return strategy.execute(a, b)

// The client code picks a concrete strategy and passes it to
// the context. The client should be aware of the differences
// between strategies in order to make the right choice.
```
```php
class ExampleApplication is
    method main() is
        // Create context object.
        // Read first number.
        // Read last number.
        // Read the desired action from user input.

        if (action == addition) then
            context.setStrategy(new ConcreteStrategyAdd())

        if (action == subtraction) then
            context.setStrategy(new ConcreteStrategySubtract())

        if (action == multiplication) then
            context.setStrategy(new ConcreteStrategyMultiply())

        result = context.executeStrategy(First number, Second number)
        Print result.
```

**Example**

```php
// Step 1: Define the PaymentProcessor interface and concrete strategies

interface PaymentProcessor {
    public function processPayment(float $amount);
}

class CreditCardProcessor implements PaymentProcessor {
    public function processPayment(float $amount) {
        echo "Processing payment of $amount via Credit Card.\n";
        // Additional Credit Card specific logic
    }
}

class PayPalProcessor implements PaymentProcessor {
    public function processPayment(float $amount) {
        echo "Processing payment of $amount via PayPal.\n";
        // Additional PayPal specific logic
    }
}

class BitcoinProcessor implements PaymentProcessor {
    public function processPayment(float $amount) {
        echo "Processing payment of $amount via Bitcoin.\n";
        // Additional Bitcoin specific logic
    }
}

// Step 2: Define the Context class

class PaymentContext {
    private $paymentProcessor;

    public function __construct(PaymentProcessor $processor) {
        $this->paymentProcessor = $processor;
    }

    public function processPayment(float $amount) {
        $this->paymentProcessor->processPayment($amount);
    }
}

// config.php

return [
    'payment_method' => 'credit_card'
];

// client.php

$config = include('config.php');

if ($config['payment_method'] === 'credit_card') {
    $paymentProcessor = new CreditCardProcessor();
} elseif ($config['payment_method'] === 'paypal') {
    $paymentProcessor = new PayPalProcessor();
} elseif ($config['payment_method'] === 'bitcoin') {
    $paymentProcessor = new BitcoinProcessor();
} else {
    throw new Exception("Invalid payment method specified in configuration.");
}

$paymentContext = new PaymentContext($paymentProcessor);
$paymentContext->processPayment(100.00);
```