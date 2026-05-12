// EX 1A - Caesar Cipher


import java.util.Scanner;

public class Ceaser {
    public static void main(String[] args) {

        Scanner s = new Scanner(System.in);

        System.out.print("Enter plain text for encryption: ");
        String plain = s.nextLine().toUpperCase();

        System.out.print("Enter key (shift value): ");
        int k = s.nextInt();
        s.nextLine();

        StringBuilder cipher = new StringBuilder();

        for (int i = 0; i < plain.length(); i++) {
            char p = plain.charAt(i);

            if (Character.isLetter(p)) {
                int pindex = p - 'A';
                int cindex = (pindex + k) % 26;
                cipher.append((char) (cindex + 'A'));
            } else {
                cipher.append(p);
            }
        }

        System.out.println("Resulting Cipher text: " + cipher.toString());

        System.out.print("\nEnter cipher text to decrypt: ");
        String cipherInput = s.nextLine().toUpperCase();

        StringBuilder dec = new StringBuilder();

        for (int i = 0; i < cipherInput.length(); i++) {
            char c = cipherInput.charAt(i);

            if (Character.isLetter(c)) {
                int cindex = c - 'A';
                int pindex = (cindex - k + 26) % 26;
                dec.append((char) (pindex + 'A'));
            } else {
                dec.append(c);
            }
        }

        System.out.println("Decrypted Result: " + dec.toString());

        s.close();
    }
}


// EX 1B - Playfair Cipher


import java.util.*;

public class PlayfairCipher {

    char[][] m = new char[5][5];

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        PlayfairCipher p = new PlayfairCipher();

        System.out.print("Enter Key: ");
        p.gen(sc.nextLine());

        System.out.print("Enter Plaintext: ");
        String enc = p.enc(sc.nextLine());
        System.out.println("Encrypted: " + enc);

        System.out.print("Enter Ciphertext: ");
        System.out.println("Decrypted: " + p.dec(sc.nextLine()));

        sc.close();
    }

    void gen(String k) {
        String s = (k + "ABCDEFGHIKLMNOPQRSTUVWXYZ").toUpperCase()
                .replaceAll("[^A-Z]", "").replace("J", "I");

        StringBuilder u = new StringBuilder();
        for (char c : s.toCharArray())
            if (u.indexOf("" + c) == -1) u.append(c);

        for (int i = 0; i < 25; i++)
            m[i / 5][i % 5] = u.charAt(i);
    }

    String fmt(String t) {
        t = t.toUpperCase().replaceAll("[^A-Z]", "").replace("J", "I");
        StringBuilder s = new StringBuilder(t);

        for (int i = 0; i < s.length() - 1; i += 2)
            if (s.charAt(i) == s.charAt(i + 1)) s.insert(i + 1, 'X');

        if (s.length() % 2 != 0) s.append('X');
        return s.toString();
    }

    int[] pos(char c) {
        for (int i = 0; i < 5; i++)
            for (int j = 0; j < 5; j++)
                if (m[i][j] == c) return new int[]{i, j};
        return null;
    }

    String proc(String t, int d) {
        StringBuilder r = new StringBuilder();

        for (int i = 0; i < t.length(); i += 2) {
            int[] a = pos(t.charAt(i)), b = pos(t.charAt(i + 1));

            if (a[0] == b[0]) {
                r.append(m[a[0]][(a[1] + d + 5) % 5]);
                r.append(m[b[0]][(b[1] + d + 5) % 5]);
            } else if (a[1] == b[1]) {
                r.append(m[(a[0] + d + 5) % 5][a[1]]);
                r.append(m[(b[0] + d + 5) % 5][b[1]]);
            } else {
                r.append(m[a[0]][b[1]]);
                r.append(m[b[0]][a[1]]);
            }
        }
        return r.toString();
    }

    String enc(String t) { return proc(fmt(t), 1); }
    String dec(String t) { return proc(t.toUpperCase(), -1); }
}


// EX 2A - Hill Cipher

import java.util.*;

public class HillCipher {

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.print("Enter matrix size: ");
        int n = sc.nextInt();

        int[][] key = new int[n][n];
        System.out.println("Enter key matrix:");
        for (int i = 0; i < n; i++)
            for (int j = 0; j < n; j++)
                key[i][j] = sc.nextInt();

        sc.nextLine();

        System.out.print("Enter plaintext: ");
        String text = sc.nextLine().toUpperCase().replaceAll("[^A-Z]", "");

        while (text.length() % n != 0) text += "X";

        StringBuilder enc = new StringBuilder();

        for (int i = 0; i < text.length(); i += n) {
            for (int r = 0; r < n; r++) {
                int sum = 0;
                for (int c = 0; c < n; c++)
                    sum += key[r][c] * (text.charAt(i + c) - 'A');
                enc.append((char)((sum % 26) + 'A'));
            }
        }

        System.out.println("Encrypted: " + enc);

        int[][] inv = inverse(key, n);
        if (inv == null) {
            System.out.println("Matrix not invertible");
            return;
        }

        StringBuilder dec = new StringBuilder();

        for (int i = 0; i < enc.length(); i += n) {
            for (int r = 0; r < n; r++) {
                int sum = 0;
                for (int c = 0; c < n; c++)
                    sum += inv[r][c] * (enc.charAt(i + c) - 'A');
                dec.append((char)((sum % 26 + 26) % 26 + 'A'));
            }
        }

        System.out.println("Decrypted: " + dec);
        sc.close();
    }

    static int[][] inverse(int[][] m, int n) {
        int det = det(m, n) % 26;
        if (det < 0) det += 26;

        int invDet = modInv(det, 26);
        if (invDet == -1) return null;

        int[][] adj = adj(m, n);
        int[][] inv = new int[n][n];

        for (int i = 0; i < n; i++)
            for (int j = 0; j < n; j++)
                inv[i][j] = (adj[i][j] * invDet % 26 + 26) % 26;

        return inv;
    }

    static int modInv(int a, int m) {
        for (int i = 1; i < m; i++)
            if ((a * i) % m == 1) return i;
        return -1;
    }

    static int det(int[][] a, int n) {
        if (n == 1) return a[0][0];
        if (n == 2) return a[0][0]*a[1][1] - a[0][1]*a[1][0];

        int d = 0;
        for (int j = 0; j < n; j++)
            d += (j % 2 == 0 ? 1 : -1) * a[0][j] * det(sub(a,0,j,n), n-1);
        return d;
    }

    static int[][] adj(int[][] a, int n) {
        int[][] adj = new int[n][n];

        for (int i = 0; i < n; i++)
            for (int j = 0; j < n; j++) {
                int sign = ((i + j) % 2 == 0) ? 1 : -1;
                adj[j][i] = sign * det(sub(a,i,j,n), n-1);
            }
        return adj;
    }

    static int[][] sub(int[][] a, int r, int c, int n) {
        int[][] s = new int[n-1][n-1];
        int rr = 0;

        for (int i = 0; i < n; i++) {
            if (i == r) continue;
            int cc = 0;
            for (int j = 0; j < n; j++) {
                if (j == c) continue;
                s[rr][cc++] = a[i][j];
            }
            rr++;
        }
        return s;
    }
}


// EX 2B - Vigenere Cipher


import java.util.*;

public class Vigenere {

    static String genKey(String t, String k) {
        StringBuilder r = new StringBuilder(k.toLowerCase());
        for (int i = 0; r.length() < t.length(); i++)
            r.append(k.charAt(i % k.length()));
        return r.toString();
    }

    static String enc(String t, String k) {
        t = t.toLowerCase();
        k = genKey(t, k);
        StringBuilder c = new StringBuilder();

        for (int i = 0; i < t.length(); i++) {
            char ch = t.charAt(i);
            if (ch >= 'a' && ch <= 'z')
                c.append((char)((ch - 'a' + k.charAt(i) - 'a') % 26 + 'a'));
            else
                c.append(ch);
        }
        return c.toString();
    }

    static String dec(String t, String k) {
        t = t.toLowerCase();
        k = genKey(t, k);
        StringBuilder p = new StringBuilder();

        for (int i = 0; i < t.length(); i++) {
            char ch = t.charAt(i);
            if (ch >= 'a' && ch <= 'z')
                p.append((char)((ch - 'a' - (k.charAt(i) - 'a') + 26) % 26 + 'a'));
            else
                p.append(ch);
        }
        return p.toString();
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.print("Enter Plaintext: ");
        String pt = sc.nextLine();

        System.out.print("Enter Key: ");
        String key = sc.nextLine();

        String ct = enc(pt, key);
        System.out.println("Encrypted: " + ct);

        System.out.print("Enter Ciphertext: ");
        String cip = sc.nextLine();

        System.out.println("Decrypted: " + dec(cip, key));

        sc.close();
    }
}

// EX 3A - Rail Fence Cipher

import java.util.*;

public class RailFence {

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.print("Enter message: ");
        String text = sc.nextLine().toUpperCase().replace(" ", "");

        System.out.print("Enter number of rails: ");
        int r = sc.nextInt();

        // ENCRYPTION
        char[][] f = new char[r][text.length()];
        for (char[] row : f) Arrays.fill(row, '\n');

        int row = 0; boolean down = false;

        for (int i = 0; i < text.length(); i++) {
            f[row][i] = text.charAt(i);
            if (row == 0 || row == r - 1) down = !down;
            row += down ? 1 : -1;
        }

        StringBuilder enc = new StringBuilder();
        for (int i = 0; i < r; i++)
            for (int j = 0; j < text.length(); j++)
                if (f[i][j] != '\n') enc.append(f[i][j]);

        System.out.println("Encrypted: " + enc);

        // DECRYPTION
        char[][] d = new char[r][enc.length()];
        row = 0; down = false;

        for (int i = 0; i < enc.length(); i++) {
            d[row][i] = '*';
            if (row == 0 || row == r - 1) down = !down;
            row += down ? 1 : -1;
        }

        int k = 0;
        for (int i = 0; i < r; i++)
            for (int j = 0; j < enc.length(); j++)
                if (d[i][j] == '*' && k < enc.length())
                    d[i][j] = enc.charAt(k++);

        StringBuilder dec = new StringBuilder();
        row = 0; down = false;

        for (int i = 0; i < enc.length(); i++) {
            dec.append(d[row][i]);
            if (row == 0 || row == r - 1) down = !down;
            row += down ? 1 : -1;
        }

        System.out.println("Decrypted: " + dec);
        sc.close();
    }
}


// EX 3B - Row Column Encryption


import java.util.*;

public class RowColumnEncryption {
    public static void main(String[] args) {

        String text = "HELLOWORLD";
        String key = "KEY";

        int col = key.length();
        int row = (text.length() + col - 1) / col;

        char[][] g = new char[row][col];
        int k = 0;

        for (int i = 0; i < row; i++)
            for (int j = 0; j < col; j++)
                g[i][j] = (k < text.length()) ? text.charAt(k++) : 'X';

        int[] order = new int[col];
        for (int i = 0; i < col; i++) order[i] = i;

        for (int i = 0; i < col - 1; i++)
            for (int j = i + 1; j < col; j++)
                if (key.charAt(order[i]) > key.charAt(order[j])) {
                    int t = order[i];
                    order[i] = order[j];
                    order[j] = t;
                }

        StringBuilder enc = new StringBuilder();
        for (int c : order)
            for (int r = 0; r < row; r++)
                enc.append(g[r][c]);

        System.out.println("Encrypted: " + enc);
    }
}


// EX 4 - DES Algorithm

import java.util.Scanner;

public class DES {

    static int[] IP = {
        58,50,42,34,26,18,10,2,
        60,52,44,36,28,20,12,4,
        62,54,46,38,30,22,14,6,
        64,56,48,40,32,24,16,8,
        57,49,41,33,25,17,9,1,
        59,51,43,35,27,19,11,3,
        61,53,45,37,29,21,13,5,
        63,55,47,39,31,23,15,7
    };

    static int[] FP = {
        40,8,48,16,56,24,64,32,
        39,7,47,15,55,23,63,31,
        38,6,46,14,54,22,62,30,
        37,5,45,13,53,21,61,29,
        36,4,44,12,52,20,60,28,
        35,3,43,11,51,19,59,27,
        34,2,42,10,50,18,58,26,
        33,1,41,9,49,17,57,25
    };

    static int[] E = {
        32,1,2,3,4,5,
        4,5,6,7,8,9,
        8,9,10,11,12,13,
        12,13,14,15,16,17,
        16,17,18,19,20,21,
        20,21,22,23,24,25,
        24,25,26,27,28,29,
        28,29,30,31,32,1
    };

    static int[] P = {
        16,7,20,21,
        29,12,28,17,
        1,15,23,26,
        5,18,31,10,
        2,8,24,14,
        32,27,3,9,
        19,13,30,6,
        22,11,4,25
    };

    static int[][] S = {
        {14,4,13,1,2,15,11,8,3,10,6,12,5,9,0,7},
        {0,15,7,4,14,2,13,1,10,6,12,11,9,5,3,8},
        {4,1,14,8,13,6,2,11,15,12,9,7,3,10,5,0},
        {15,12,8,2,4,9,1,7,5,11,3,14,10,0,6,13}
    };

    static String toBinary(String input) {

        String binary = "";

        for (int i = 0; i < input.length(); i++) {

            int val = input.charAt(i);

            for (int j = 7; j >= 0; j--) {

                binary += ((val >> j) & 1);
            }
        }

        return binary;
    }

    static String permute(String input, int[] table) {

        String output = "";

        for (int i = 0; i < table.length; i++) {

            output += input.charAt(table[i] - 1);
        }

        return output;
    }

    static String xor(String a, String b) {

        String result = "";

        for (int i = 0; i < a.length(); i++) {

            if (a.charAt(i) == b.charAt(i))
                result += "0";
            else
                result += "1";
        }

        return result;
    }

    static String sBox(String input) {

        String output = "";

        for (int i = 0; i < 8; i++) {

            String block = input.substring(i * 6, i * 6 + 6);

            int row = Integer.parseInt("" + block.charAt(0) + block.charAt(5), 2);

            int col = Integer.parseInt(block.substring(1, 5), 2);

            int val = S[row][col];

            String bin = Integer.toBinaryString(val);

            while (bin.length() < 4) {

                bin = "0" + bin;
            }

            output += bin;
        }

        return output;
    }

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        System.out.print("Enter plaintext: ");
        String pt = sc.nextLine();

        System.out.print("Enter 8-character key: ");
        String key = sc.nextLine();

        String binaryPT = toBinary(pt);

        String binaryKey = toBinary(key);

        String ip = permute(binaryPT, IP);

        String L = ip.substring(0, 32);

        String R = ip.substring(32, 64);

        for (int i = 0; i < 16; i++) {

            String expandedR = permute(R, E);

            String roundKey = binaryKey.substring(0, 48);

            String xored = xor(expandedR, roundKey);

            String sboxOut = sBox(xored);

            String pOut = permute(sboxOut, P);

            String newR = xor(L, pOut);

            L = R;

            R = newR;

            System.out.println("Round " + (i + 1));

            System.out.println("L" + (i + 1) + " : " + L);

            System.out.println("R" + (i + 1) + " : " + R);

            System.out.println();
        }

        String combined = R + L;

        String cipher = permute(combined, FP);

        System.out.println("Final Encrypted DES : " + cipher);

        sc.close();
    }
}


// EX 5 - RSA Algorithm

import java.util.Scanner;

public class RSA {

    public static int gcd(int a, int b) {
        while (b != 0) {
            int temp = b;
            b = a % b;
            a = temp;
        }
        return a;
    }

    public static int modInverse(int e, int phi) {
        for (int d = 1; d < phi; d++) {
            if ((e * d) % phi == 1) {
                return d;
            }
        }
        return -1;
    }

    public static long power(long base, long exp, long mod) {
        long result = 1;
        base = base % mod;

        while (exp > 0) {
            if (exp % 2 == 1) {
                result = (result * base) % mod;
            }
            base = (base * base) % mod;
            exp = exp / 2;
        }
        return result;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.print("Enter prime number p: ");
        int p = sc.nextInt();

        System.out.print("Enter prime number q: ");
        int q = sc.nextInt();

        System.out.print("Enter public key exponent e: ");
        int e = sc.nextInt();

        System.out.print("Enter message (integer): ");
        int m = sc.nextInt();

        int n = p * q;
        int phi = (p - 1) * (q - 1);

        if (gcd(e, phi) != 1) {
            System.out.println("e must be coprime with phi(n).");
            return;
        }

        int d = modInverse(e, phi);

        System.out.println("\nPublic Key (e, n): (" + e + ", " + n + ")");
        System.out.println("Private Key (d, n): (" + d + ", " + n + ")");

        long cipher = power(m, e, n);
        System.out.println("Encrypted Message: " + cipher);

        long decrypted = power(cipher, d, n);
        System.out.println("Decrypted Message: " + decrypted);

        sc.close();
    }
}


// EX 6 - Diffie Hellman Algorithm

import java.util.Scanner;

class DiffieHellman {

    private static long power(long a, long b, long p) {
        long result = 1;
        a = a % p;

        while (b > 0) {
            if (b % 2 == 1)
                result = (result * a) % p;

            a = (a * a) % p;
            b = b / 2;
        }
        return result;
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.print("Enter a prime number P: ");
        long P = scanner.nextLong();

        System.out.print("Enter a primitive root G: ");
        long G = scanner.nextLong();

        System.out.print("Enter the private key a for Elliot: ");
        long a = scanner.nextLong();

        long x = power(G, a, P);

        System.out.print("Enter the private key b for Wayne: ");
        long b = scanner.nextLong();

        long y = power(G, b, P);

        long ka = power(y, a, P);
        long kb = power(x, b, P);

        System.out.println("The value of P: " + P);
        System.out.println("The value of G: " + G);
        System.out.println("The private key a for Elliot: " + a);
        System.out.println("The private key b for Wayne: " + b);
        System.out.println("Secret key for Elliot is: " + ka);
        System.out.println("Secret key for Wayne is: " + kb);

        scanner.close();
    }
}


// EX 9 - ElGamal Algorithm

import java.util.*;

public class elgamal {

    public static int powerMod(int base, int exp, int mod) {
        int result = 1;
        base = base % mod;

        for (int i = 0; i < exp; i++) {
            result = (result * base) % mod;
        }
        return result;
    }

    public static int modInverse(int a, int mod) {
        for (int i = 1; i < mod; i++) {
            if ((a * i) % mod == 1) {
                return i;
            }
        }
        return -1;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.print("Enter q: ");
        int q = sc.nextInt();

        System.out.print("Enter alpha: ");
        int alpha = sc.nextInt();

        System.out.print("Enter Xb (private key): ");
        int xb = sc.nextInt();

        System.out.print("Enter message M: ");
        int M = sc.nextInt();

        System.out.print("Enter n (random number): ");
        int n = sc.nextInt();

        int Yb = powerMod(alpha, xb, q);
        int K = powerMod(Yb, n, q);
        int K_inv = modInverse(K, q);

        int C1 = powerMod(alpha, n, q);
        int C2 = (K * M) % q;

        System.out.println("\nResults");
        System.out.println("Yb = " + Yb);
        System.out.println("K = " + K);
        System.out.println("K inverse = " + K_inv);
        System.out.println("C1 = " + C1);
        System.out.println("C2 = " + C2);

        int decrypted = (C2 * K_inv) % q;
        System.out.println("Decrypted Message = " + decrypted);

        sc.close();
    }
}


// EX 10A - SHA Hashing

import java.nio.charset.StandardCharsets;
import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;
import java.util.Scanner;

public class SHAHashing {

    public static String hashString(String input, String algorithm) {
        try {
            MessageDigest digest = MessageDigest.getInstance(algorithm);
            byte[] hashBytes = digest.digest(input.getBytes(StandardCharsets.UTF_8));

            StringBuilder hex = new StringBuilder();
            for (byte b : hashBytes) {
                String s = Integer.toHexString(0xff & b);
                if (s.length() == 1) hex.append('0');
                hex.append(s);
            }
            return hex.toString();

        } catch (NoSuchAlgorithmException e) {
            throw new RuntimeException(e);
        }
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        String input = scanner.nextLine();

        System.out.println(hashString(input, "SHA-256"));
        System.out.println(hashString(input, "SHA-512"));

        scanner.close();
    }
}

// EX 10B - MD5 Hashing


import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;
import java.util.Scanner;

public class MD5 {

    public static String hash(String input) {
        try {
            MessageDigest md = MessageDigest.getInstance("MD5");
            byte[] digest = md.digest(input.getBytes());

            StringBuilder hex = new StringBuilder();
            for (byte b : digest) {
                String s = Integer.toHexString(0xff & b);
                if (s.length() == 1) hex.append('0');
                hex.append(s);
            }
            return hex.toString();

        } catch (NoSuchAlgorithmException e) {
            throw new RuntimeException(e);
        }
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        String input = scanner.nextLine();
        System.out.println(hash(input));

        scanner.close();
    }
}
