```java
import java.util.Arrays;
import java.io.*;

public class Solution {

    // Recursive implementation of the Extended Euclidean Algorithm.
    // It calculates GCD(a, b) and finds coefficients x and y such that ax + by = gcd(a, b).
    // Returns a long array: {gcd_value, x_coefficient, y_coefficient}.
    public static long[] extended_gcd_recursive(long a, long b) {
        if (b == 0) {
            // Base case: gcd(a, 0) = a.
            // Equation is a*1 + 0*0 = a. So x=1, y=0.
            return new long[]{a, 1, 0};
        }

        // Recursive call for (b, a % b)
        long[] result = extended_gcd_recursive(b, a % b);
        
        long d = result[0];  // GCD of b and (a % b) is also GCD of a and b
        long x1 = result[1]; // x' coefficient for b in b*x' + (a%b)*y' = d
        long y1 = result[2]; // y' coefficient for (a%b) in b*x' + (a%b)*y' = d

        // Calculate x and y for the current (a, b) using x' and y'
        // From: ax + by = d
        // We know: b*x' + (a % b)*y' = d
        // Substitute a % b = a - floor(a/b)*b:
        // b*x' + (a - floor(a/b)*b)*y' = d
        // b*x' + a*y' - floor(a/b)*b*y' = d
        // a*y' + b*(x' - floor(a/b)*y') = d
        // Comparing with ax + by = d, we get:
        // x = y'
        // y = x' - (a/b)*y'
        long x = y1;
        long y = x1 - (a / b) * y1;

        return new long[]{d, x, y};
    }

    // Main function to solve the problem requirements.
    public static int[] extended_gcd(int a, int b) {
        // Call the recursive extended Euclidean algorithm to get GCD and an initial (x0, y0)
        long[] result = extended_gcd_recursive(a, b);
        long D = result[0];  // GCD(A, B)
        long x0 = result[1]; // Initial x coefficient
        long y0 = result[2]; // Initial y coefficient

        // Calculate a_prime = A/D and b_prime = B/D
        long gA = a / D;
        long gB = b / D;

        // Initialize bestX, bestY with the initial solution from extended_gcd_recursive
        long bestX = x0;
        long bestY = y0;
        long minAbsSum = Math.abs(x0 + y0);

        // Sum of initial coefficients for the general solution formula: X+Y = (x0+y0) + k * (gB - gA)
        long sumInitial = x0 + y0;
        long kCoefficient = gB - gA;

        // Case 1: kCoefficient is 0 (i.e., A/D == B/D, which implies A=B=D because GCD(A/D, B/D) = 1)
        if (kCoefficient == 0) {
            // For A=B=D, the extended_gcd_recursive(A,A) typically returns (A, 0, 1) if B is passed as the second argument.
            // This gives X=0, Y=1. However, the problem states that if |X+Y| is tied, X should be maximized.
            // For X+Y=1, both (0,1) and (1,0) are valid. (1,0) has a larger X.
            // So, for this specific case (A=B), we override the initial solution to (1,0).
            bestX = 1;
            bestY = 0;
            // minAbsSum will be Math.abs(1+0) = 1, which is correct for this case.
        } 
        // Case 2: kCoefficient is not 0
        else {
            // We need to find integer k values that minimize |sumInitial + k * kCoefficient|.
            // These are typically the floor and ceiling of -sumInitial / kCoefficient.
            
            // Calculate k1 = floor(-sumInitial / kCoefficient)
            long k1 = (long) Math.floor((double) -sumInitial / kCoefficient);
            // Calculate k2 = ceil(-sumInitial / kCoefficient)
            long k2 = (long) Math.ceil((double) -sumInitial / kCoefficient);

            // Re-initialize current best candidate using the solution from extended_gcd_recursive
            // This ensures we always have a baseline to compare against.
            // (Used as a temporary holder during comparison)
            long[] currentBestCandidate = {bestX, bestY, minAbsSum}; 

            // Evaluate k1
            long x_k1 = x0 + k1 * gB;
            long y_k1 = y0 - k1 * gA;
            long absSum_k1 = Math.abs(x_k1 + y_k1);

            // Compare k1's result with the current best candidate
            if (absSum_k1 < currentBestCandidate[2]) {
                currentBestCandidate[0] = x_k1;
                currentBestCandidate[1] = y_k1;
                currentBestCandidate[2] = absSum_k1;
            } else if (absSum_k1 == currentBestCandidate[2]) {
                // If absolute sums are equal, choose the one with larger X
                if (x_k1 > currentBestCandidate[0]) {
                    currentBestCandidate[0] = x_k1;
                    currentBestCandidate[1] = y_k1;
                }
            }

            // Evaluate k2, but only if k1 and k2 are different.
            // If they are the same (e.g., -sumInitial/kCoefficient is an exact integer),
            // k1's evaluation already covers it.
            if (k1 != k2) {
                long x_k2 = x0 + k2 * gB;
                long y_k2 = y0 - k2 * gA;
                long absSum_k2 = Math.abs(x_k2 + y_k2);

                // Compare k2's result with the current best candidate
                if (absSum_k2 < currentBestCandidate[2]) {
                    currentBestCandidate[0] = x_k2;
                    currentBestCandidate[1] = y_k2;
                    currentBestCandidate[2] = absSum_k2;
                } else if (absSum_k2 == currentBestCandidate[2]) {
                    // If absolute sums are equal, choose the one with larger X
                    if (x_k2 > currentBestCandidate[0]) {
                        currentBestCandidate[0] = x_k2;
                        currentBestCandidate[1] = y_k2;
                    }
                }
            }
            
            // Update bestX and bestY based on the comparisons
            bestX = currentBestCandidate[0];
            bestY = currentBestCandidate[1];
        }

        // Return the GCD and the optimal X, Y coefficients (cast to int as per problem requirement)
        return new int[]{(int)D, (int)bestX, (int)bestY};
    }
}
```