# Template Method Pattern - Revision Notes

## **1. What is the Template Method Pattern?**
The **Template Method Pattern** is a behavioral design pattern that:
- Defines the **skeleton of an algorithm** in a base class.
- Defers specific steps of the algorithm to **subclasses**.
- Ensures the overall structure remains consistent across different implementations.

### **Key Characteristics**
- Core logic is implemented in the **base class**.
- Variable parts are handled by **subclasses**.
- The **`final` keyword** is used for the template method to prevent modification.

---

## **2. When to Use?**
Use the Template Method Pattern when:
- You have **common workflows** with steps that vary based on the specific implementation.
- You want to enforce a **consistent structure** across different classes.
- You need to ensure adherence to a **shared algorithm**, but some steps depend on subclass-specific behavior.

---

## **3. Real-World Examples**
1. **Airline Seat Pricing:**
   - Workflow: Base price + Additional charges - Discounts.
   - Variable steps: Additional charges and discounts differ by seat type (Economy, Business, etc.).

2. **Report Generation:**
   - Workflow: Data collection → Formatting → Output.
   - Variable steps: Formatting and output may differ (e.g., PDF, Excel).

3. **Cooking Recipes:**
   - Workflow: Prepare → Cook → Serve.
   - Variable steps: Ingredients and cooking methods vary (e.g., Italian vs. Indian cuisine).

---

## **4. Components of the Pattern**
1. **Abstract Class (Template Class):**
   - Contains the **template method** which defines the algorithm's structure.
   - Provides **hooks** for subclass-specific steps through abstract or virtual methods.
   - Ensures common steps are implemented consistently.

2. **Concrete Subclasses:**
   - Implement the **variable steps** of the algorithm.
   - Provide specific behavior for the abstract methods.

---

## **5. Example: Airline Seat Pricing**

### **Template Class**
```java
public abstract class SeatPricingTemplate {

    // Template Method
    public final double calculatePrice() {
        double basePrice = getBasePrice();
        double additionalCharge = getAdditionalCharge();
        double discount = getDiscount();
        return basePrice + additionalCharge - discount;
    }

    // Steps to be implemented by subclasses
    protected abstract double getBasePrice();
    protected abstract double getAdditionalCharge();
    
    // Optional step with a default implementation
    protected double getDiscount() {
        return 0.0; // Default no discount
    }
}

Concrete Subclasses

Economy Seat

public class EconomySeatPricing extends SeatPricingTemplate {
    @Override
    protected double getBasePrice() {
        return 100.0;
    }

    @Override
    protected double getAdditionalCharge() {
        return 10.0; // Minimal additional charges
    }
}

Business Seat

public class BusinessSeatPricing extends SeatPricingTemplate {
    @Override
    protected double getBasePrice() {
        return 200.0;
    }

    @Override
    protected double getAdditionalCharge() {
        return 50.0; // Includes luxury fees
    }

    @Override
    protected double getDiscount() {
        return 20.0; // Promotional discount
    }
}

6. Advantages

	1.	Code Reuse:
	•	Common steps are implemented once in the base class.
	•	Subclasses only focus on variable steps.
	2.	Consistency:
	•	Ensures a standard algorithm structure across all implementations.
	3.	Extensibility:
	•	Easy to add new subclasses without modifying existing code.
	4.	Adherence to SOLID Principles:
	•	Open-Closed Principle: Base class is open for extension via new subclasses but closed for modification.
	•	Liskov Substitution Principle: Subclasses can seamlessly replace the base class.

7. Limitations

	1.	Coupling:
	•	Subclasses are tightly coupled to the base class.
	2.	Restricted Flexibility:
	•	Changes to the overall algorithm require modification of the base class.
	3.	Increased Complexity:
	•	May introduce unnecessary layers if the workflow is simple.

8. Common Questions

Q1: Why use final for the template method?

	•	The final keyword ensures subclasses cannot override the template method, preserving the core structure of the algorithm.

Q2: How does it align with the Open-Closed Principle?

	•	The base class (template) is closed for modification but allows extension via new subclasses.

Q3: When should you avoid it?

	•	Avoid it for simple workflows where a single implementation suffices, as the pattern may add unnecessary complexity.

9. Real-World Applications

	1.	Payment Processing:
	•	Workflow: Validate → Process Payment → Log Transaction.
	•	Variable steps: Payment method (Card, UPI, Wallet).
	2.	Document Conversion:
	•	Workflow: Load → Transform → Save.
	•	Variable steps: Transform logic for different formats (HTML, Markdown, PDF).
	3.	UI Rendering Frameworks:
	•	Workflow: Initialize → Render → Post-Process.
	•	Variable steps: Render logic for different UI components.

10. Practice Example

Problem Statement:

Implement a report generation system using the Template Method Pattern. The report should:
	•	Fetch data from a source.
	•	Format the data.
	•	Output the report as PDF or CSV.

Try implementing this and see how the pattern simplifies the design!

Quick Summary

	•	The Template Method Pattern defines the skeleton of an algorithm in a base class and allows subclasses to define variable parts.
	•	Best suited for workflows that share a common structure but have steps that vary across implementations.
	•	Balances consistency with flexibility, making it a powerful tool in behavioral design.

Let me know if you’d like help applying this pattern to other scenarios! 🙌
#DesignPatterns #TemplateMethodPattern #SoftwareEngineering

This markdown note is clean, detailed, and covers all important aspects of the **Template Method Pattern**. It’s suitable for revision, sharing, or documentation purposes. Let me know if you need further adjustments! 🚀
