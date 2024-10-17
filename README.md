#include <iostream>
#include <cstdlib>
#include <cstring>
#include <string>

using namespace std;

class Vigenere{
public:
    Vigenere(string keyword);
    string encrypt(string plaintext);
    string decrypt(string encrypted_text);
    string simplify(string text);
    
private:
    char shift(char, char, bool);
    string keyword;
};

Vigenere::Vigenere(string k) {
    keyword = simplify(k);
}

char Vigenere::shift(char in, char shft, bool encoding) {
    int mult = 1;
    if (!encoding) {
        mult = -1;
    }
    int shift_amount = shft - 'A';
    int char_26 = in - 'A';
    char_26 += 26;
    // cout << "Add 26: " << (int) char_26 << endl;
    char_26 = char_26 + (mult * shift_amount);
    // cout << "shift 26: " << (int) char_26 << endl;
    char_26 %= 26;
    // cout << "mod 26: " << (int) char_26 << endl;
    return char_26 + 'A';
}
    
string Vigenere::encrypt(string plaintext) {
    string ciphertext = "";
    int size = plaintext.size();
    for (int i = 0; i < size; i++) {
        ciphertext += shift(plaintext[i], keyword[i % keyword.size()], true);
    }
    return ciphertext;
}

string Vigenere::decrypt(string plaintext) {
    string ciphertext = "";
    int size = plaintext.size();
    for (int i = 0; i < size; i++) {
        ciphertext += shift(plaintext[i], keyword[i % keyword.size()], false);
    }
    return ciphertext;
}
    
    
string Vigenere::simplify(string text) {
    string simplified = "";
    int size = text.size();
    for (int i = 0; i < size; i++) {
        if (isalpha(text[i])) {
            simplified = simplified + (char)toupper(text[i]);
        }
    }
    return simplified;
}

int main(int argc, char *argv[]) {
    if (argc != 3) {
        cerr << "USAGE: " << argv[0] << " -d|e keyword" << endl;
        exit(1);
    }
    string keyword = argv[2];
    bool encrypt;
    string option = argv[1];
    string line;
    string encodedLine;
    
    if (option == "-e") {
        encrypt = true;
    } else if (option == "-d") {
        encrypt = false;
    }
    Vigenere cipher(keyword);
    // string decodedLine;
    while (getline(cin, line)) {
        if (encrypt) {
            encodedLine = cipher.encrypt(cipher.simplify(line));
            // decodedLine = cipher.decrypt(encodedLine);
        } else {
            encodedLine = cipher.decrypt(cipher.simplify(line));
            // decodedLine = cipher.encrypt(encodedLine);
        }
        cout << encodedLine << endl;
        // cout << decodedLine << endl;
    }
    return 0;
}
