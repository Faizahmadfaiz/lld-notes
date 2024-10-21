Decorator Pattern - Detailed Notes with Code Example

Concept:

	•	The Decorator Pattern dynamically adds behavior to objects without altering their structure.
	•	This pattern wraps the object with a series of decorators to extend functionality, allowing a flexible alternative to subclassing and avoiding “class explosion.”

Scenario: Car Assembly with Different Feature Packages

You have a basic car (BasicCar) and want to add additional features like a Sports Package and a Luxury Package. Each feature adds new functionality without modifying the existing car class.

Step-by-Step Example:

1. Interface: Car

	•	Defines the common behavior for all cars.

public interface Car {
    void assemble();
}

2. Concrete Component: BasicCar

	•	Implements the basic car with default functionality.

public class BasicCar implements Car {
    @Override
    public void assemble() {
        System.out.println("Basic car is assembled.");
    }
}

3. Decorator: CarDecorator

	•	Abstract class that holds a reference to a Car object and delegates calls to it.

public abstract class CarDecorator implements Car {
    protected Car car;

    public CarDecorator(Car car) {
        this.car = car;
    }

    @Override
    public void assemble() {
        this.car.assemble();  // Delegating the call to the wrapped car
    }
}

4. Concrete Decorators: SportsCar and LuxuryCar

	•	Each decorator extends CarDecorator and adds new features.

SportsCarDecorator:

public class SportsCarDecorator extends CarDecorator {
    public SportsCarDecorator(Car car) {
        super(car);
    }

    @Override
    public void assemble() {
        super.assemble();  // Calls the base car's assemble method
        System.out.println("Adding sports car features.");
    }
}

LuxuryCarDecorator:

public class LuxuryCarDecorator extends CarDecorator {
    public LuxuryCarDecorator(Car car) {
        super(car);
    }

    @Override
    public void assemble() {
        super.assemble();  // Calls the sports car's assemble method
        System.out.println("Adding luxury car features.");
    }
}

Client Code: Using Decorators

public class Main {
    public static void main(String[] args) {
        Car basicCar = new BasicCar();
        Car sportsCar = new SportsCarDecorator(basicCar);
        Car luxurySportsCar = new LuxuryCarDecorator(sportsCar);

        // Assembling a car with both sports and luxury features
        luxurySportsCar.assemble();
    }
}

Output:

Basic car is assembled.
Adding sports car features.
Adding luxury car features.

Explanation:

	•	BasicCar is wrapped by SportsCarDecorator, which adds sports features.
	•	Then, LuxuryCarDecorator wraps the SportsCarDecorator, adding luxury features on top of the sports features.

Advantages:

	•	Flexibility: New functionality (like sports or luxury features) can be added dynamically at runtime.
	•	Single Responsibility: Each decorator class has one responsibility (e.g., adding sports or luxury features), promoting clean design.
	•	Avoids Class Explosion: Instead of creating multiple subclasses for every combination (like LuxurySportsCar), decorators stack functionalities.

When to Use:

	•	When you want to add behavior to individual objects, not all objects of a class.
	•	When subclassing leads to too many combinations of behavior (class explosion).

This example demonstrates how the Decorator Pattern provides flexibility while keeping the core structure clean and adhering to SOLID principles.
