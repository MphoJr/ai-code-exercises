Step 1: Analyze the Code
The original calculateShippingCost function has:
- Nested conditionals for shipping method (standard, express, overnight).
- Further nested conditionals for destination country.
- Special rules for dimensional weight and surcharges.
This makes the function long, hard to read, and difficult to extend (e.g., adding new shipping methods or countries).

📝 Step 2: Strategy Pattern Opportunity
The Strategy pattern lets us encapsulate each shipping method’s pricing logic in its own class (or function object). The main calculator then delegates to the chosen strategy.

🛠 Step 3: Refactor with Strategy Pattern
Define Strategies
class StandardShipping {
  calculate(packageDetails, destinationCountry) {
    const { weight, length, width, height } = packageDetails;
    let cost;

    switch (destinationCountry) {
      case 'USA': cost = weight * 2.5; break;
      case 'Canada': cost = weight * 3.5; break;
      case 'Mexico': cost = weight * 4.0; break;
      default: cost = weight * 4.5;
    }

    if (weight < 2 && (length * width * height) > 1000) {
      cost += 5.0;
    }

    return cost;
  }
}

class ExpressShipping {
  calculate(packageDetails, destinationCountry) {
    const { weight, length, width, height } = packageDetails;
    let cost;

    switch (destinationCountry) {
      case 'USA': cost = weight * 4.5; break;
      case 'Canada': cost = weight * 5.5; break;
      case 'Mexico': cost = weight * 6.0; break;
      default: cost = weight * 7.5;
    }

    if ((length * width * height) > 5000) {
      cost += 15.0;
    }

    return cost;
  }
}

class OvernightShipping {
  calculate(packageDetails, destinationCountry) {
    const { weight } = packageDetails;
    if (destinationCountry === 'USA') return weight * 9.5;
    if (destinationCountry === 'Canada') return weight * 12.5;
    return "Overnight shipping not available for this destination";
  }
}


Strategy Context
class ShippingCalculator {
  constructor(strategy) {
    this.strategy = strategy;
  }

  calculate(packageDetails, destinationCountry) {
    const cost = this.strategy.calculate(packageDetails, destinationCountry);
    return typeof cost === 'number' ? cost.toFixed(2) : cost;
  }
}


Usage
const packageDetails = { weight: 5, length: 10, width: 10, height: 10 };

const standardCalculator = new ShippingCalculator(new StandardShipping());
console.log(standardCalculator.calculate(packageDetails, 'USA')); // "12.50"

const expressCalculator = new ShippingCalculator(new ExpressShipping());
console.log(expressCalculator.calculate(packageDetails, 'Canada')); // "27.50"

const overnightCalculator = new ShippingCalculator(new OvernightShipping());
console.log(overnightCalculator.calculate(packageDetails, 'Mexico')); // "Overnight shipping not available for this destination"



✅ Step 4: Verify Behavior
Run the same unit tests or usage examples as before. The outputs should match the original function’s results.

📈 Benefits Gained
- Maintainability: Each shipping method’s logic is isolated. Adding a new method (e.g., “eco”) means creating a new strategy class without touching existing ones.
- Extensibility: Easy to add new countries or rules within a strategy.
- Readability: No more deeply nested conditionals; logic is organized by responsibility.
- Testability: Each strategy can be unit tested independently.

✨ Reflection
- Improved maintainability: The Strategy pattern makes future changes (like new shipping methods) straightforward.
- Future changes easier: Adding surcharges or new rules doesn’t require editing a giant conditional block.
- Unexpected challenge: Ensuring consistent return types (numbers vs. error strings) across strategies required a small adjustment in the context class.
