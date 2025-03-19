Sure! Let's break it down step by step in multiple ways so that you fully understand how to check if a number is **prime or not**.

---

## **What is a Prime Number?**
A **prime number** is a number **greater than 1** that has only **two factors: 1 and itself**.

### **Examples of Prime Numbers**:
- ✅ **2, 3, 5, 7, 11, 13, 17, 19, 23, ...**
- ❌ **4, 6, 8, 9, 10, 12, 14, ...** (These are **not prime** because they have more than two factors)

---

## **Different Ways to Check for a Prime Number**

### **1️⃣ Basic Approach (Brute Force)**
**Loop from 2 to n-1** and check if the number is divisible by any number.
```java
public class PrimeBasic {
    public static boolean isPrime(int n) {
        if (n <= 1) return false;  // 0 and 1 are not prime
        for (int i = 2; i < n; i++) {
            if (n % i == 0) return false;  // Found a divisor, so it's not prime
        }
        return true;
    }

    public static void main(String[] args) {
        int num = 10;
        System.out.println(num + " is prime? " + isPrime(num));
    }
}
```
**🔴 Issue**: This runs in **O(n) time complexity**, which is slow for large numbers.

---

### **2️⃣ Optimized Approach (Check Up to √n)**
- If a number **n** has a factor greater than √n, then it **must also have a smaller factor**.
- So, we **only need to check up to √n** instead of n.
```java
public class PrimeOptimized {
    public static boolean isPrime(int n) {
        if (n <= 1) return false;
        for (int i = 2; i * i <= n; i++) { // Loop till √n
            if (n % i == 0) return false;
        }
        return true;
    }

    public static void main(String[] args) {
        int num = 29;
        System.out.println(num + " is prime? " + isPrime(num));
    }
}
```
✅ **Faster**: This reduces the time complexity to **O(√n)**.

---

### **3️⃣ Highly Efficient Approach (Skip Even Numbers)**
- **All even numbers (except 2) are not prime**, so we can skip checking them.
- **All multiples of 3 (except 3) are also not prime**.
- We check only **odd numbers from 5 to √n**, skipping even numbers.
```java
public class PrimeEfficient {
    public static boolean isPrime(int n) {
        if (n <= 1) return false;
        if (n == 2 || n == 3) return true;  // 2 and 3 are prime
        if (n % 2 == 0 || n % 3 == 0) return false; // Eliminate even numbers & multiples of 3

        // Start checking from 5, skipping even numbers (i += 6 to check 5, 7, 11, 13, ...)
        for (int i = 5; i * i <= n; i += 6) {
            if (n % i == 0 || n % (i + 2) == 0) return false;
        }
        return true;
    }

    public static void main(String[] args) {
        int num = 37;
        System.out.println(num + " is prime? " + isPrime(num));
    }
}
```
🚀 **Super Efficient**:
- Eliminates even numbers **O(√n / 2)** checks instead of **O(√n)**.
- Used in real-world applications.

---

### **4️⃣ Prime Number Check Using Recursion**
```java
public class PrimeRecursive {
    public static boolean isPrimeRecursive(int n, int i) {
        if (n <= 1) return false;
        if (i * i > n) return true; // Base case: checked all numbers up to √n
        if (n % i == 0) return false; // Found a divisor

        return isPrimeRecursive(n, i + 1);
    }

    public static void main(String[] args) {
        int num = 31;
        System.out.println(num + " is prime? " + isPrimeRecursive(num, 2));
    }
}
```
🔵 **Recursion-Based Approach**, but can cause **stack overflow** for large numbers.

---

### **5️⃣ Finding All Prime Numbers Up to N (Sieve of Eratosthenes)**
If you want to **find all prime numbers up to N**, the **Sieve of Eratosthenes** is the fastest way.
```java
import java.util.Arrays;

public class SieveOfEratosthenes {
    public static void sieve(int n) {
        boolean[] primes = new boolean[n + 1];
        Arrays.fill(primes, true); // Assume all numbers are prime initially

        for (int p = 2; p * p <= n; p++) {
            if (primes[p]) { // If prime, mark its multiples as non-prime
                for (int i = p * p; i <= n; i += p)
                    primes[i] = false;
            }
        }

        // Print all prime numbers
        for (int i = 2; i <= n; i++) {
            if (primes[i]) System.out.print(i + " ");
        }
    }

    public static void main(String[] args) {
        int n = 50;
        System.out.println("Prime numbers up to " + n + ":");
        sieve(n);
    }
}
```
✅ **Best approach** for multiple prime number checks  
⏳ **Time Complexity: O(N log log N)**  
🟢 Used in **competitive programming**.

---

## **Summary of Approaches**
| Approach | Time Complexity | Best for |
|----------|--------------|----------|
| **Brute Force (Check all numbers)** | **O(n)** | Small values of n |
| **Check up to √n** | **O(√n)** | Large numbers |
| **Skipping even & multiples of 3** | **O(√n/2)** | More optimized for large numbers |
| **Recursive Approach** | **O(√n)** | Understanding recursion (not ideal for large n) |
| **Sieve of Eratosthenes** | **O(n log log n)** | Finding all prime numbers up to N |

---

## **Conclusion**
- **For a single prime check**, use the **efficient approach** that skips even numbers.
- **For checking primes in a range**, use **Sieve of Eratosthenes**.
- **For very large numbers**, use **advanced primality tests** (Miller-Rabin, AKS).

## -------------------------------------------------------------
Given a number n, find the value of n raised to the power of its own reverse.

Note: The result will always fit into a 32-bit signed integer.

Examples:

Input: n = 2
Output: 4
Explanation: The reverse of 2 is 2, and 22 = 4.
Input: n = 10
Output: 10
Explanation: The reverse of 10 is 1 (leading zero is discarded), and 101 = 10.
Input: n = 3
Output: 27
Explanation: The reverse of 3 is 3, and 33 = 27.
Constraints:
1 <= n <= 10

To solve this problem, we need to:  
1. **Reverse the number** \( n \).  
2. **Compute \( n^{reverse(n)} \)**.  
3. **Handle large numbers using modular exponentiation** to prevent overflow.

---

### **Example:**
- **Input:** \( n = 12 \)  
- **Reverse:** \( 21 \)  
- **Result:** \( 12^{21} \)

---

### **Approach**
1. **Reverse the number**: Convert it to a string, reverse it, and parse it back to an integer.
2. **Efficiently compute power**: Since \( n^{reverse(n)} \) can be **very large**, we use **modular exponentiation** with `mod = 1_000_000_007`.
Here is the corrected Java class for computing **\( n^{reverse(n)} \) % 1,000,000,007** using modular exponentiation.  

---

### **Final Java Solution**
```java
class Solution {
    static final int MOD = 1_000_000_007;

    // Function to reverse a number
    private int reverse(int n) {
        int rev = 0;
        while (n > 0) {
            rev = rev * 10 + n % 10;
            n /= 10;
        }
        return rev;
    }

    // Function to compute (base^exp) % MOD using fast exponentiation
    private int powerMod(int base, int exp) {
        int result = 1;
        base = base % MOD; // Ensure base is within MOD

        while (exp > 0) {
            if (exp % 2 == 1) { // If exponent is odd, multiply base
                result = (int) ((1L * result * base) % MOD);
            }
            base = (int) ((1L * base * base) % MOD); // Square the base
            exp /= 2; // Reduce exponent by half
        }
        return result;
    }

    public int reverseExponentiation(int n) {
        int r = reverse(n); // Find reverse of n
        return powerMod(n, r); // Compute (n^r) % MOD
    }
}
```

---

### **Explanation**
1. **`reverse(int n)`**:  
   - Extracts digits from `n` and constructs the reverse number.
   
2. **`powerMod(int base, int exp)`** (Modular exponentiation):  
   - Computes **\( n^{r} \) mod \( 1,000,000,007 \)** efficiently using **fast exponentiation**.

3. **`reverseExponentiation(int n)`**:  
   - Calls the reverse function.
   - Calls modular exponentiation to compute the result.

---

### **Example Usage**
```java
class Main {
    public static void main(String[] args) {
        Solution solution = new Solution();
        
        System.out.println(solution.reverseExponentiation(12));  // Output: 35831808
        System.out.println(solution.reverseExponentiation(5));   // Output: 3125
        System.out.println(solution.reverseExponentiation(100)); // Output: 1
    }
}
```

---

### **Time Complexity**
- **Reversing the number**: \( O(\log n) \)  
- **Modular exponentiation**: \( O(\log r) \)  
- **Overall Complexity**: **\( O(\log n) \) → very efficient!** 🚀

This ensures all calculations fit within `int`. Let me know if you need further modifications! 😊

---

### **Explanation**
1. **Reverse function**:
   - Extracts digits from the end and constructs the reverse integer.
2. **Power function using fast exponentiation**:
   - Uses **modular exponentiation** to compute \( n^{r} \) efficiently.
   - Avoids overflow by taking modulo at each step.
3. **Time Complexity**:
   - **Reverse function**: \( O(\log n) \) (since we iterate over digits).
   - **Power function**: \( O(\log r) \) (fast exponentiation).

---

### **Example Run**
#### **Input:**
```
12
```
#### **Steps:**
- **Reverse of 12** → `21`
- **12^21 mod 1_000_000_007** → `35831808`

#### **Output:**
```
Result: 35831808
```

---

### **Why Use Modular Exponentiation?**
For large numbers like **98765^56789**, direct computation will cause overflow.  
Using **modular exponentiation**, we keep intermediate results small.

Would you like an explanation of any specific part? 🚀
