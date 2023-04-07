##Abstract Factory Pattern

- The abstract factory pattern is a creational design pattern that allows you to create families of related objects without specifying their concrete classes.

- Let's take the example of a furniture store that produces chairs, sofas, and coffee tables. 
- The store can use an abstract factory pattern to create furniture that matches a particular style, such as modern, contemporary, or traditional.
- Here's how the abstract factory pattern would work in PHP for this example:

#####Define an abstract factory interface that declares the methods for creating the different types of furniture.

```php
interface FurnitureFactory {
    public function createChair(): Chair;
    public function createSofa(): Sofa;
    public function createCoffeeTable(): CoffeeTable;
}
```
#####Create concrete factory classes that implement the abstract factory interface and create furniture objects that belong to a specific style.

```php
class ModernFurnitureFactory implements FurnitureFactory {
    public function createChair(): Chair {
        return new ModernChair();
    }

    public function createSofa(): Sofa {
        return new ModernSofa();
    }

    public function createCoffeeTable(): CoffeeTable {
        return new ModernCoffeeTable();
    }
}

class ContemporaryFurnitureFactory implements FurnitureFactory {
    public function createChair(): Chair {
        return new ContemporaryChair();
    }

    public function createSofa(): Sofa {
        return new ContemporarySofa();
    }

    public function createCoffeeTable(): CoffeeTable {
        return new ContemporaryCoffeeTable();
    }
}
```
#####Define the abstract classes for each type of furniture that declare the methods that must be implemented by their concrete implementations.

```php
abstract class Chair {
    abstract public function sit(): void;
}

abstract class Sofa {
    abstract public function lie(): void;
}

abstract class CoffeeTable {
    abstract public function putOn(): void;
}
```

#####Create concrete classes that implement the abstract classes for each type of furniture.

```php
class ModernChair extends Chair {
    public function sit(): void {
        echo "Sitting on a modern chair\n";
    }
}

class ContemporaryChair extends Chair {
    public function sit(): void {
        echo "Sitting on a contemporary chair\n";
    }
}

class ModernSofa extends Sofa {
    public function lie(): void {
        echo "Lying on a modern sofa\n";
    }
}

class ContemporarySofa extends Sofa {
    public function lie(): void {
        echo "Lying on a contemporary sofa\n";
    }
}

class ModernCoffeeTable extends CoffeeTable {
    public function putOn(): void {
        echo "Putting something on a modern coffee table\n";
    }
}

class ContemporaryCoffeeTable extends CoffeeTable {
    public function putOn(): void {
        echo "Putting something on a contemporary coffee table\n";
    }
}
```

#####Finally, the client code can use the abstract factory interface to create furniture objects that match a particular style.

```php
function buildFurniture(FurnitureFactory $factory): void {
    $chair = $factory->createChair();
    $sofa = $factory->createSofa();
    $coffeeTable = $factory->createCoffeeTable();

    $chair->sit();
    $sofa->lie();
    $coffeeTable->putOn();
}

$modernFurniture = new ModernFurnitureFactory();
buildFurniture($modernFurniture);

$contemporaryFurniture = new ContemporaryFurnitureFactory();
buildFurniture($contemporaryFurniture);
```
