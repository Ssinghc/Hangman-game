#include <iostream>
#include <string>
#include <vector>
#include <ctime>
#include <cstdlib>

using namespace std;

// Function to choose a random word from the word list
string chooseRandomWord() {
    vector<string> wordList = {"programming", "hangman", "cplusplus", "developer", "challenge"};
    int randomIndex = rand() % wordList.size();
    return wordList[randomIndex];
}

// Function to display the current state of the word with guessed letters
void displayWord(const string& word, const vector<char>& guessedLetters) {
    for (char letter : word) {
        if (find(guessedLetters.begin(), guessedLetters.end(), letter) != guessedLetters.end()) {
            cout << letter << " ";
        } else {
            cout << "_ ";
        }
    }
    cout << endl;
}

int main() {
    // Seed the random number generator
    srand(static_cast<unsigned int>(time(nullptr)));

    cout << "Welcome to Hangman!\n\n";

    string secretWord = chooseRandomWord();
    vector<char> guessedLetters;
    int maxAttempts = 6; // You can adjust the number of allowed attempts

    while (maxAttempts > 0) {
        cout << "Attempts left: " << maxAttempts << "\n";
        displayWord(secretWord, guessedLetters);

        // Get a letter from the player
        cout << "Enter a letter: ";
        char guess;
        cin >> guess;

        // Check if the letter has already been guessed
        if (find(guessedLetters.begin(), guessedLetters.end(), guess) != guessedLetters.end()) {
            cout << "You've already guessed that letter. Try again.\n";
            continue;
        }

        // Add the guessed letter to the list
        guessedLetters.push_back(guess);

        // Check if the guessed letter is in the secret word
        if (secretWord.find(guess) == string::npos) {
            cout << "Incorrect guess!\n";
            maxAttempts--;
        } else {
            cout << "Correct guess!\n";
        }

        // Check if the player has guessed all the letters in the word
        bool allLettersGuessed = true;
        for (char letter : secretWord) {
            if (find(guessedLetters.begin(), guessedLetters.end(), letter) == guessedLetters.end()) {
                allLettersGuessed = false;
                break;
            }
        }

        if (allLettersGuessed) {
            cout << "Congratulations! You guessed the word: " << secretWord << "\n";
            break;
        }
    }

    if (maxAttempts == 0) {
        cout << "Sorry, you ran out of attempts. The correct word was: " << secretWord << "\n";
    }

    return 0;
}
