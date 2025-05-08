# codes
network security
```
////// exp1 
import java.util.Scanner;
public class PasswordManager {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String password = "Admin";
        int choice;
        while (true) {
            System.out.println("Press \n 1: Default password\n 2: Change password\n 3: Print masked password\n 4: Exit");
            System.out.print("Enter your choice: ");
            if (scanner.hasNextInt()) {
                choice = scanner.nextInt();
                scanner.nextLine(); // Consume newline
                switch (choice) {
                    case 1:
                        System.out.print("Enter the Password: ");
                        String inputPass = scanner.nextLine();
                        if (inputPass.equals(password)) {
                            System.out.println("It is the Default Password.");
                        } else {
                            System.out.println("You have entered the Wrong Password.");
                        }
                        break;
                    case 2:
                        System.out.print("Enter the New Password: ");
                        password = scanner.nextLine();
                        System.out.println("Your Password has been Reset.");
                        break;
                    case 3:
                        System.out.println("*".repeat(password.length()));
                        break;
                    case 4:
                        System.out.println("Exiting program. Goodbye!");
                        scanner.close();
                        return;
                    default:
                        System.out.println("You have chosen an invalid choice.");
                }
            } else {
                System.out.println("Invalid input. Please enter a number.");
                scanner.next(); // Clear the invalid input
            }
        }
    }
}


///// exp2 transposition

import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collections;
import java.util.List;
import java.util.Scanner;

public class Main {
    // Function to perform Single Columnar Transposition Cipher Encryption
    public static String encrypt(String plainText, String keyword) {
        int keyLength = keyword.length();
        int numRows = (int) Math.ceil((double) plainText.length() / keyLength); // Calculate number of rows
        char[][] grid = new char[numRows][keyLength]; // Create a grid with numRows and keyLength
        // Fill the grid row by row with the plain text
        int index = 0;
        for (int i = 0; i < numRows; i++) {
            for (int j = 0; j < keyLength; j++) {
                if (index < plainText.length()) {
                    grid[i][j] = plainText.charAt(index++);
                } else {
                    grid[i][j] = '_'; // Fill with underscore if out of bounds
                }
            }
        }
        // Sort the keyword alphabetically and create a map of original indexes
        List<Integer> keywordOrder = new ArrayList<>();
        for (int i = 0; i < keyLength; i++) {
            keywordOrder.add(i);
        }
        // Sort the keyword and keep track of the original indices
        Collections.sort(keywordOrder, (a, b) -> Character.compare(keyword.charAt(a), keyword.charAt(b)));
        // Create the ciphertext by reading columns in the order of the sorted keyword
        StringBuilder cipherText = new StringBuilder();
        for (int i : keywordOrder) {
            for (int r = 0; r < numRows; r++) {
                cipherText.append(grid[r][i]);
            }
        }
        return cipherText.toString();
    }
    // Function to perform Single Columnar Transposition Cipher Decryption
    public static String decrypt(String cipherText, String keyword) {
        int keyLength = keyword.length();
        int numRows = (int) Math.ceil((double) cipherText.length() / keyLength);
        char[][] grid = new char[numRows][keyLength]; // Create a grid with numRows and keyLength
        // Sort the keyword alphabetically and create a map of original indexes
        List<Integer> keywordOrder = new ArrayList<>();
        for (int i = 0; i < keyLength; i++) {
            keywordOrder.add(i);
        }
        // Sort the keyword and keep track of the original indices
        Collections.sort(keywordOrder, (a, b) -> Character.compare(keyword.charAt(a), keyword.charAt(b)));
        // Fill the grid column by column using the ciphertext
        int index = 0;
        for (int i : keywordOrder) {
            for (int r = 0; r < numRows; r++) {
                if (index < cipherText.length()) {
                    grid[r][i] = cipherText.charAt(index++);
                } else {
                    grid[r][i] = '_'; // Fill with underscore if out of bounds
                }
            }
        }
        // Read the grid row by row to reconstruct the plaintext
        StringBuilder plainText = new StringBuilder();
        for (int r = 0; r < numRows; r++) {
            for (int c = 0; c < keyLength; c++) {
                plainText.append(grid[r][c]);
            }
        }
        // Remove trailing filler characters (underscore '_')
        return plainText.toString().replace("_", "");
    }
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter the plain text: ");
        String plainText = scanner.nextLine().toUpperCase(); // Convert to uppercase
        System.out.print("Enter the keyword: ");
        String keyword = scanner.nextLine().toUpperCase(); // Convert to uppercase
        // Encrypting the plain text
        String cipherText = encrypt(plainText, keyword);
        System.out.println("Cipher Text: " + cipherText);
        // Decrypting the cipher text
        String decryptedText = decrypt(cipherText, keyword);
        System.out.println("Decrypted Text: " + decryptedText);
    }
}


//// exp3 substitution
import java.util.Scanner;
public class Main {
    // Normalize the key to be within the range [0, 25]
    private static int normalizeKey(int key) {
        return key % 26; // Ensure the key is between 0 and 25
    }
    // Function for encryption
    public static String encrypt(String plainText, int key) {
        key = normalizeKey(key); // Normalize the key
        StringBuilder cipherText = new StringBuilder();
        // Traverse through each character in the plain text
        for (char ch : plainText.toCharArray()) {
            if (ch >= 'a' && ch <= 'z') {
                // Encrypt lowercase letters using the formula: (key + character index) % 26
                int shiftedChar = (ch - 'a' + key) % 26 + 'a';
                cipherText.append((char) shiftedChar);
            } else if (ch >= 'A' && ch <= 'Z') {
                // Encrypt uppercase letters using the formula: (key + character index) % 26
                int shiftedChar = (ch - 'A' + key) % 26 + 'A';
                cipherText.append((char) shiftedChar);
            } else {
                // Non-alphabetic characters are added as-is
                cipherText.append(ch);
            }
        }
        return cipherText.toString();
    }
    // Function for decryption
    public static String decrypt(String cipherText, int key) {
        key = normalizeKey(key); // Normalize the key
        StringBuilder plainText = new StringBuilder();
        // Traverse through each character in the cipher text
        for (char ch : cipherText.toCharArray()) {
            if (ch >= 'a' && ch <= 'z') {
                // Decrypt lowercase letters using the formula: (p - k) % 26
                int shiftedChar = (ch - 'a' - key + 26) % 26 + 'a';
                plainText.append((char) shiftedChar);
            } else if (ch >= 'A' && ch <= 'Z') {
                // Decrypt uppercase letters using the formula: (p - k) % 26
                int shiftedChar = (ch - 'A' - key + 26) % 26 + 'A';
                plainText.append((char) shiftedChar);
            } else {
                // Non-alphabetic characters are added as-is
                plainText.append(ch);
            }
        }
        return plainText.toString();
    }
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String plainText;
        int key;
        // Taking input from the user
        System.out.print("Enter the plain text: ");
        plainText = scanner.nextLine();
        System.out.print("Enter the key (integer): ");
        key = scanner.nextInt();
        // Encrypting the plain text
        String cipherText = encrypt(plainText, key);
        System.out.println("Cipher Text: " + cipherText);
        // Decrypting the cipher text
        String decryptedText = decrypt(cipherText, key);
        System.out.println("Decrypted Text: " + decryptedText);
        scanner.close();
    }
}
```
