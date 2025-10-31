import random
import sys
import time

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

# --- AUTO-GUESS LIST ---
# The game will automatically use these guesses.
AUTO_GUESSES = ["a", "e", "i", "o", "u", "s", "t", "r", "n", "l", "python", "hangman"]


def choose_word():
    return random.choice(WORD_LIST)


def display_state(secret, guessed_letters, wrong_guesses):
    print(HANGMAN_PICS[min(wrong_guesses, len(HANGMAN_PICS) - 1)])
    shown = " ".join([c if c in guessed_letters else "_" for c in secret])
    print("\nWord:  ", shown)
    print("Guesses:", " ".join(sorted(guessed_letters)))
    print(f"Wrong guesses: {wrong_guesses}\n")


def get_guess(already_guessed):
    """Simulate a guess automatically instead of waiting for user input."""
    time.sleep(0.5)  # short delay for realism
    if not AUTO_GUESSES:
        print("No more auto guesses left. Ending game.")
        sys.exit(0)
    guess = AUTO_GUESSES.pop(0)
    print(f"(Auto guess: {guess})")
    return guess


def play_once():
    secret = choose_word()
    guessed_letters = set()
    wrong_guesses = 0
    max_wrong = len(HANGMAN_PICS) - 1

    print("Welcome to Hangman! (Auto-play demo mode)\n")
    print(f"The secret word has {len(secret)} letters.\n")

    while True:
        display_state(secret, guessed_letters, wrong_guesses)

        guess = get_guess(guessed_letters)

        if len(guess) > 1:  # guessing the full word
            if guess == secret:
                print("\nCorrect! The word was:", secret)
                return True
            else:
                wrong_guesses += 1
                print(f"'{guess}' is not the word.\n")
        else:
            if guess in secret:
                guessed_letters.add(guess)
                print(f"Good guess! '{guess}' is in the word.\n")
            else:
                wrong_guesses += 1
                guessed_letters.add(guess)
                print(f"'{guess}' is not in the word.\n")

        # win check
        if all(c in guessed_letters for c in secret):
            print("\n🎉 Congratulations! You revealed the word:", secret)
            return True

        # lose check
        if wrong_guesses >= max_wrong:
            display_state(secret, guessed_letters, wrong_guesses)
            print("\n💀 You've been hanged! The word was:", secret)
            return False


def main():
    random.seed()
    result = play_once()
    print("\nGame Over.")
    if result:
        print("You won! ✅")
    else:
        print("Better luck next time! ❌")


if __name__ == "__main__":
    try:
        main()
    except KeyboardInterrupt:
        print("\n\nGoodbye!")
        sys.exit(0)
