// NMIT-SE-PROJECT-
// Software Engineering Project 
// RAHUL POLICE PATIL (1NT24CB041)
#!/usr/bin/env python3
"""
Simple Hangman game (terminal).
Save as hangman.py and run: python3 hangman.py
"""

import random
import sys

WORD_LIST = [
    "python", "hangman", "github", "program", "developer", "keyboard",
    "internet", "function", "variable", "algorithm", "module", "package"
]

HANGMAN_PICS = [
    """
     +---+
     |   |
         |
         |
         |
         |
    =========""",
    """
     +---+
     |   |
     O   |
         |
         |
         |
    =========""",
    """
     +---+
     |   |
     O   |
     |   |
         |
         |
    =========""",
    """
     +---+
     |   |
     O   |
    /|   |
         |
         |
    =========""",
    """
     +---+
     |   |
     O   |
    /|\\  |
         |
         |
    =========""",
    """
     +---+
     |   |
     O   |
    /|\\  |
    /    |
         |
    =========""",
    """
     +---+
     |   |
     O   |
    /|\\  |
    / \\  |
         |
    =========""",
]


def choose_word():
    return random.choice(WORD_LIST)


def display_state(secret, guessed_letters, wrong_guesses):
    print(HANGMAN_PICS[min(wrong_guesses, len(HANGMAN_PICS) - 1)])
    shown = " ".join([c if c in guessed_letters else "_" for c in secret])
    print("\nWord:  ", shown)
    print("Guesses:", " ".join(sorted(guessed_letters)))
    print(f"Wrong guesses: {wrong_guesses}\n")


def get_guess(already_guessed):
    while True:
        guess = input("Enter a letter (or full word to guess): ").strip().lower()
        if not guess:
            print("Please enter something.")
            continue
        if len(guess) == 1:
            if not guess.isalpha():
                print("Please enter a letter (a-z).")
                continue
            if guess in already_guessed:
                print("You already guessed that letter.")
                continue
            return guess
        else:
            # full-word guess
            if not guess.isalpha():
                print("Words must contain only letters.")
                continue
            return guess


def play_once():
    secret = choose_word()
    guessed_letters = set()
    wrong_guesses = 0
    max_wrong = len(HANGMAN_PICS) - 1

    print("Welcome to Hangman! Try to guess the word.")
    while True:
        display_state(secret, guessed_letters, wrong_guesses)

        guess = get_guess(guessed_letters)
        if len(guess) > 1:  # guessing the whole word
            if guess == secret:
                print("\nCorrect! You guessed the word:", secret)
                return True
            else:
                wrong_guesses += 1
                print(f"'{guess}' is not the word.")
        else:
            # single letter
            if guess in secret:
                guessed_letters.add(guess)
                print(f"Nice! The letter '{guess}' is in the word.")
            else:
                wrong_guesses += 1
                guessed_letters.add(guess)
                print(f"Nope. The letter '{guess}' is not in the word.")

        # win check
        if all(c in guessed_letters for c in secret):
            print("\nCongratulations! You revealed the word:", secret)
            return True

        # lose check
        if wrong_guesses >= max_wrong:
            display_state(secret, guessed_letters, wrong_guesses)
            print("\nYou've been hanged! The word was:", secret)
            return False


def main():
    random.seed()  # system time
    while True:
        result = play_once()
        again = input("\nPlay again? (y/n): ").strip().lower()
        if not again or again[0] != "y":
            print("Thanks for playing! Goodbye.")
            break


if __name__ == "__main__":
    try:
        main()
    except KeyboardInterrupt:
        print("\n\nGoodbye!")
        sys.exit(0)
