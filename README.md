#include <iostream>
#include <string>
#include <vector>
#include <ctime>
#include <cstdlib>
#include <chrono>
#include <thread>

using namespace std;

// Function to generate a random word from a list
string getRandomWord(const vector<string>& wordList) {
    random_device rd;
    mt19937 gen(rd());
    uniform_int_distribution<> distrib(0, wordList.size() - 1);
    return wordList[distrib(gen)];
}

// Function to check if a character is a valid keystroke
bool isValidKeystroke(char ch) {
    return (ch >= 'a' && ch <= 'z') || (ch >= 'A' && ch <= 'Z') || ch == ' ';
}

// Function to display the typing tutor interface
void displayInterface(const string& targetWord, const string& typedWord) {
    cout << "\nType this word: " << targetWord << endl;
    cout << "Your input: " << typedWord << endl;
    cout << endl;
}

int main() {
    // List of words for the typing tutor
    vector<string> wordList = {
        "hello", "world", "quick", "brown", "fox", "jumps", 
        "over", "lazy", "dog", "the", "cat", "sat", "on", 
        "mat", "run", "jump", "walk", "fly", "swim", "read", 
        "write", "code", "learn", "grow", "love", "life", 
        "happy", "smile", "laugh", "dream", "hope"
    };

    // Initialize variables
    string targetWord = getRandomWord(wordList);
    string typedWord = "";
    int correctChars = 0;
    int totalChars = 0;
    int mistakes = 0;
    clock_t startTime, endTime;
    double timeTaken;

    cout << "Welcome to the Typing Tutor!" << endl;
    cout << "Press 'Enter' to start." << endl;
    cin.get(); // Wait for enter key press

    startTime = clock();

    // Main typing loop
    while (true) {
        displayInterface(targetWord, typedWord);

        char ch;
        cin.get(ch);

        if (ch == '\n') {
            // User pressed enter
            if (typedWord == targetWord) {
                cout << "Correct! You typed the word correctly." << endl;
                targetWord = getRandomWord(wordList);
                typedWord = "";
                correctChars += targetWord.length();
                totalChars += targetWord.length();
            } else {
                cout << "Incorrect. Try again!" << endl;
                mistakes++;
                totalChars += typedWord.length();
            }
        } else if (isValidKeystroke(ch)) {
            // User typed a valid character
            typedWord += ch;
        } else {
            // Invalid character, ignore
        }

        if (totalChars > 0) {
            endTime = clock();
            timeTaken = double(endTime - startTime) / CLOCKS_PER_SEC;
            cout << "\nTime taken: " << timeTaken << " seconds" << endl;
            cout << "Words per minute (WPM): " << (int)(correctChars / timeTaken * 60) << endl;
            cout << "Accuracy: " << (int)(correctChars * 100.0 / totalChars) << "%" << endl;
        }

        // Exit if user types 'q'
        if (ch == 'q') {
            cout << "Exiting Typing Tutor..." << endl;
            break;
        }
    }

    return 0;
}
