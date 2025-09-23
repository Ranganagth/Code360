# Intuition

We need to compare the **total 6-month cost** of owning and running a Petrol car vs a Diesel car.
The formula is:

```
totalCost = priceOfCar 
          + maintenanceCostPerMonth * 6 
          + (pricePerLiter * (numberOfKilometerCarRunInaMonth / numberOfkilometerCarRunInOneLiter)) * 6
```

* The last term is fuel cost:
  `(km per month ÷ km per liter) = liters per month`
  `liters per month × price per liter × 6 = fuel cost for 6 months`

After computing both costs:

* If Petrol < Diesel → return `0`
* If Diesel < Petrol → return `1`
* If equal → return `-1`

---

# Approach

1. Write a helper function to compute total cost for a given car.
2. Apply the above formula for both petrol and diesel.
3. Compare and return the result.

---

# Complexity

* **Time:** `O(1)` per test case (just arithmetic).
* **Space:** `O(1)`.

---

# Code

```java
import java.util.* ;
import java.io.*; 

class Car {
    public int priceOfCar;
    public int maintenanceCostPerMonth;
    public int numberOfkilometerCarRunInOneLiter;
    public int pricePerLiter;
    public int numberOfKilometerCarRunInaMonth;

    Car(int priceOfCar, int maintenanceCostPerMonth, int numberOfkilometerCarRunInOneLiter,
        int pricePerLiter, int numberOfKilometerCarRunInaMonth) {

        this.priceOfCar = priceOfCar;
        this.maintenanceCostPerMonth = maintenanceCostPerMonth;
        this.numberOfkilometerCarRunInOneLiter = numberOfkilometerCarRunInOneLiter;
        this.numberOfKilometerCarRunInaMonth = numberOfKilometerCarRunInaMonth;
        this.pricePerLiter = pricePerLiter;
    }
};

public class Solution {
    
    private static long getTotalCost(Car car) {
        // Car price
        long total = car.priceOfCar;

        // Maintenance cost for 6 months
        total += (long) car.maintenanceCostPerMonth * 6;

        // Fuel cost for 6 months
        double litersPerMonth = (double) car.numberOfKilometerCarRunInaMonth / car.numberOfkilometerCarRunInOneLiter;
total += Math.round(litersPerMonth * car.pricePerLiter * 6);

        return total;
    }

	public static int mostCostEfficientCar(Car petrolCar, Car dieselCar) {
  		long petrolCost = getTotalCost(petrolCar);
        long dieselCost = getTotalCost(dieselCar);

        if (petrolCost < dieselCost) return 0;
        else if (dieselCost < petrolCost) return 1;
        else return -1;
	}
}
```

---

## Walkthrough Example

Input:

```
Petrol: 100000 1200 30 60 150
Diesel: 200000 1300 10 20 120
```

* Petrol:
  `price = 100000`
  `maint = 1200 * 6 = 7200`
  `fuel = (150/30=5 liters/month) * 60 * 6 = 1800`
  → Total = `109000`

* Diesel:
  `price = 200000`
  `maint = 1300 * 6 = 7800`
  `fuel = (120/10=12 liters/month) * 20 * 6 = 1440`
  → Total = `209240`

Petrol < Diesel → output `0`. 

---
























