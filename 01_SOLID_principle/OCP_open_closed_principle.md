# Definition 
As per OCP, Software entities (classes, modules, functions, etc.) should be **open for extension, but closed for modification.**

This means that the behavior of a module can be extended without modifying its source code. **The goal is to reduce the risk of breaking existing functionality when requirements change.** 

# Real life Analogy 
Mobile charger (fixed functionality) + travel adapter (extension -> for US , INDIA, Europe)

# Real-World Example

Region-based tax calculation (e.g., India, US, UK) in an Invoicing System to explain the Open/Closed Principle. As an invoicing system grows, it must handle tax rules for different regions :

    India: GST 18%
    US: Sales Tax 8%
    UK: VAT 12%


New regions maybe added over time.
## Bad Design - Violating OCP
```cpp
#include <bits/stdc++.h>
using namespace std;

class InvoiceProcessor {
public:
    double calculateTotal(string region, double amount) {
        if (region == "India") {
            return amount + amount * 0.18;
        } else if (region == "US") {
            return amount + amount * 0.08;
        } else if (region == "UK") {
            return amount + amount * 0.12;
        } else {
            return amount; // No tax for unknown region
        }
    }
};
```

```python
class InvoiceProcessor:
    def calculateTotal(self, region, amount):
        if region.lower() == "india":
            return amount + amount * 0.18
        elif region.lower() == "us":
            return amount + amount * 0.08
        elif region.lower() == "uk":
            return amount + amount * 0.12
        else:
            return amount  # No tax for unknown region

```

The above code is considered a **bad practice** because:

    Adding a new region (e.g., Germany) requires modifying this method.
    You risk breaking existing logic while adding new functionality.
    Hard to test, maintain, or scale.
    Violates the Open/Closed Principle.

## Good Design - Follows OCP
```cpp
#include <bits/stdc++.h>
using namespace std;

// Tax strategy Interface
class TaxCalculator {
public:
    virtual double calculateTax(double amount) = 0;
};

// Implementing Region-Specific Tax Calculators
class IndiaTaxCalculator : public TaxCalculator {
public:
    double calculateTax(double amount) override {
        return amount * 0.18; // GST
    }
};

class USTaxCalculator : public TaxCalculator {
public:
    double calculateTax(double amount) override {
        return amount * 0.08; // Sales Tax
    }
};

class UKTaxCalculator : public TaxCalculator {
public:
    double calculateTax(double amount) override {
        return amount * 0.12; // VAT
    }
};

// Invoice class
class Invoice {
private:
    double amount;
    TaxCalculator* taxCalculator;

public:
    Invoice(double amount, TaxCalculator* taxCalculator) : amount(amount), taxCalculator(taxCalculator) {}

    double getTotalAmount() {
        return amount + taxCalculator->calculateTax(amount);
    }
};

// Main function
int main() {
    double amount = 1000.0;

    Invoice indiaInvoice(amount, new IndiaTaxCalculator());
    cout << "Total (India): ₹" << indiaInvoice.getTotalAmount() << endl;

    Invoice usInvoice(amount, new USTaxCalculator());
    cout << "Total (US): $" << usInvoice.getTotalAmount() << endl;

    Invoice ukInvoice(amount, new UKTaxCalculator());
    cout << "Total (UK): £" << ukInvoice.getTotalAmount() << endl;

    return 0;
}
```

```python
from abc import ABC, abstractmethod

# Tax strategy Interface
class TaxCalculator(ABC):
    @abstractmethod
    def calculateTax(self, amount):
        pass

# Implementing Region-Specific Tax Calculators
class IndiaTaxCalculator(TaxCalculator):
    def calculateTax(self, amount):
        return amount * 0.18  # GST

class USTaxCalculator(TaxCalculator):
    def calculateTax(self, amount):
        return amount * 0.08  # Sales Tax

class UKTaxCalculator(TaxCalculator):
    def calculateTax(self, amount):
        return amount * 0.12  # VAT

# Invoice class
class Invoice:
    def __init__(self, amount, taxCalculator):
        self.amount = amount
        self.taxCalculator = taxCalculator

    def getTotalAmount(self):
        return self.amount + self.taxCalculator.calculateTax(self.amount)

# Example usage
amount = 1000.0

india_invoice = Invoice(amount, IndiaTaxCalculator())
print(f"Total (India): ₹{india_invoice.getTotalAmount()}")

us_invoice = Invoice(amount, USTaxCalculator())
print(f"Total (US): ${us_invoice.getTotalAmount()}")

uk_invoice = Invoice(amount, UKTaxCalculator())
print(f"Total (UK): £{uk_invoice.getTotalAmount()}")

```

Explanation:

    - **Define a Tax Strategy Interface**: The TaxCalculator interface defines a contract for all region-specific tax classes to follow, enabling polymorphism and extension.
    - **Implement Region-Specific Tax Calculators**: The IndiaTaxCalculator, USTaxCalculator, and UKTaxCalculator classes provide concrete implementations of the TaxCalculator interface for each region, encapsulating tax logic.
    - **Using Dependency Injection**: The Invoice class is decoupled from specific tax types by receiving a TaxCalculator from the outside (this is called **Dependency Injection**).
    - **Main Running Code**: In Main class, we create the appropriate tax calculator and inject it into the Invoice class, making the system easily extensible for new regions.

# When to apply OCP

 The Open/Closed Principle is especially useful in the following scenarios: 

    - When a module is expected to change or evolve due to shifting business or technical requirements.
    - When there is a need to extend functionality without modifying existing, tested code.
   - When developing frameworks, plugins, or extensible systems such as billing engines, tax calculators, or UI components.
    - When aiming to safeguard stable, production-ready modules from regression caused by direct changes.
    - When a class is becoming a God Class — handling too many responsibilities or branching logic — which signals a need to extract behaviors into separate, extendable components.

It is generally most effective when applied **in response to observed patterns of change** or **a well-understood need for scalability.**

# Common misconception about OCP

| Misconception  | Reality |
|----------|----------|
| OCP means never touching old code .  |  NO. Code refactoring  makes code compliant. But avoid changing the core logic.  |
| OCP leads to more claasses. So its is an overkill  | extra callses are fine if it improves modulairity , testibility and maintainability  |
| It makes code harder to read  | Adding abstraction to smaller code is unnecessary but for system with complex behaviour or frequent changes requires well-structured extensibility (improves clarity and reduces conditional logic)  |
