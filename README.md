# Exp-11-Implement-Ellip-c-Curve-Cryptography-ECC-


**AIM:**

To implement the EllipƟc Curve Cryptography (ECC) key exchange algorithm and securely derive a
shared secret key.

**DESIGN STEPS:**

Step 1: Choose a large prime number p and an ellipƟc curve defined by the equaƟon y^2 = x^3 + ax
+b mod p along with a base point G on the curve.


Step 2: Alice and Bob choose private keys.


Step 3: Compute public keys
public_key = private_key * G (point mulƟplicaƟon).


Step 4: Exchange public keys.


Step 5: Compute the shared secret
shared_secret = private_key * public_key_received.

**PROGRAM:**
```
#include <stdio.h>

// A simple structure to represent points on the ellipƟc curve
typedef struct {
long long int x, y;
} Point;

// FuncƟon to compute modular inverse (using Extended Euclidean Algorithm)
long long int modInverse(long long int a, long long int m) {
long long int m0 = m, t, q;
long long int x0 = 0, x1 = 1;
if (m == 1) return 0;
while (a > 1) {

q = a / m;
t = m;
m = a % m, a = t;
t = x0;
x0 = x1 - q * x0;
x1 = t;
}
if (x1 < 0) x1 += m0;
return x1;
}

// FuncƟon to perform point addiƟon on ellipƟc curves
Point pointAddiƟon(Point P, Point Q, long long int a, long long int p) {
Point R;
long long int lambda;

// Check if P == Q (Point doubling)
if (P.x == Q.x && P.y == Q.y) {
lambda = (3 * P.x * P.x + a) * modInverse(2 * P.y, p) % p;
} else { // Point addiƟon
lambda = (Q.y - P.y) * modInverse(Q.x - P.x, p) % p;
}

// Calculate resulƟng point
R.x = (lambda * lambda - P.x - Q.x) % p;
R.y = (lambda * (P.x - R.x) - P.y) % p;

// Ensure values are posiƟve
R.x = (R.x + p) % p;
R.y = (R.y + p) % p;

return R;
}

// FuncƟon to perform scalar mulƟplicaƟon (EllipƟc Curve Point MulƟplicaƟon)
Point scalarMulƟplicaƟon(Point P, long long int k, long long int a, long long int p) {
Point result = P;
k--; // Subtract 1 because we start with the base point

while (k > 0) {
result = pointAddiƟon(result, P, a, p); // Add the point to itself (k Ɵmes)
k--;
}

return result;
}

int main() {
long long int p, a, b, privateA, privateB;
Point G, publicA, publicB, sharedSecretA, sharedSecretB;

// Step 1: Input parameters of the ellipƟc curve
prinƞ("Enter the prime number (p): ");
scanf("%lld", &p);
prinƞ("Enter the curve parameters (a and b) for equaƟon y^2 = x^3 + ax + b: ");
scanf("%lld %lld", &a, &b);
prinƞ("Enter the base point G (x and y): ");
scanf("%lld %lld", &G.x, &G.y);

// Step 2: Alice and Bob input private keys
prinƞ("Enter Alice's private key: ");
scanf("%lld", &privateA);

prinƞ("Enter Bob's private key: ");
scanf("%lld", &privateB);

// Step 3: Compute public keys (EllipƟc Curve Point MulƟplicaƟon)
publicA = scalarMulƟplicaƟon(G, privateA, a, p); // Alice's public key
publicB = scalarMulƟplicaƟon(G, privateB, a, p); // Bob's public key

prinƞ("Alice's public key: (%lld, %lld)\n", publicA.x, publicA.y);
prinƞ("Bob's public key: (%lld, %lld)\n", publicB.x, publicB.y);

// Step 4: Compute shared secrets (EllipƟc Curve Point MulƟplicaƟon)
sharedSecretA = scalarMulƟplicaƟon(publicB, privateA, a, p); // Alice's shared secret
sharedSecretB = scalarMulƟplicaƟon(publicA, privateB, a, p); // Bob's shared secret

// Step 5: Display shared secret
prinƞ("Shared secret computed by Alice: (%lld, %lld)\n", sharedSecretA.x, sharedSecretA.y);
prinƞ("Shared secret computed by Bob: (%lld, %lld)\n", sharedSecretB.x, sharedSecretB.y);

if (sharedSecretA.x == sharedSecretB.x && sharedSecretA.y == sharedSecretB.y) {
prinƞ("Key exchange successful. Both shared secrets match!\n");
} else {
prinƞ("Key exchange failed. Shared secrets do not match.\n");
}

return 0;
}
```
**OUTPUT:**

![image](https://github.com/user-attachments/assets/27f41e0d-ccb7-4c37-a0c4-d05d7a15a16c)


**RESULT:**


The program for EllipƟc Curve Cryptography (ECC) was executed successfully, and both Alice and Bob
computed the same shared secret.
