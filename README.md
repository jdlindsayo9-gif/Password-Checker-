# Jordan Lindsay
# Python-based Password Strength Checker
# Take 1
# 2 December 2025


import re
import math

def check_password_strength(password):
    """Evaluate password strength and estimate brute-force attack time."""
    strength = 0
    feedback = []
    charset_size = 0

    # Length check
    if len(password) >= 16:
        strength += 3
    elif len(password) >= 12:
        strength += 2
    elif len(password) >= 8:
        strength += 1
    else:
        feedback.append("Password too short (minimum 8 characters).")

    # Character diversity checks
    if re.search(r"[A-Z]", password):
        strength += 1
        charset_size += 26
    else:
        feedback.append("Add at least one uppercase letter.")

    if re.search(r"[a-z]", password):
        strength += 1
        charset_size += 26
    else:
        feedback.append("Add at least one lowercase letter.")

    if re.search(r"\d", password):
        strength += 1
        charset_size += 10
    else:
        feedback.append("Add at least one digit.")

    if re.search(r"[!@#$%^&*(),.?\":{}|<>]", password):
        strength += 1
        charset_size += 32
    else:
        feedback.append("Add at least one special character.")

    # Strength rating
    if strength >= 7:
        rating = "Very Strong"
    elif strength >= 5:
        rating = "Strong"
    elif strength >= 3:
        rating = "Moderate"
    else:
        rating = "Weak"

    # Brute force estimation(def_math)
    attempts = charset_size ** len(password)
    guesses_per_sec_slow = 1_000        # Slow attack
    guesses_per_sec_fast = 1_000_000_000 # Fast attack (GPU cluster)

    time_slow = attempts / guesses_per_sec_slow
    time_fast = attempts / guesses_per_sec_fast

    return rating, feedback, attempts, time_slow, time_fast


def format_time(seconds):
    """Convert seconds into human-readable format."""
    if seconds < 60:
        return f"{seconds:.2f} seconds"
    minutes = seconds / 60
    if minutes < 60:
        return f"{minutes:.2f} minutes"
    hours = minutes / 60
    if hours < 24:
        return f"{hours:.2f} hours"
    days = hours / 24
    if days < 365:
        return f"{days:.2f} days"
    years = days / 365
    return f"{years:.2f} years"


def best_practices():
    """Return general tips for creating strong passwords."""
    return [
        "Use at least 12–16 characters.",
        "Include uppercase, lowercase, digits, and special characters.",
        "Avoid dictionary words, names, or personal info.",
        "Consider using a passphrase (e.g., multiple random words).",
        "Use a password manager for complex passwords."
    ]


if __name__ == "__main__":
    pwd = input("Enter a password to check: ")
    rating, suggestions, attempts, time_slow, time_fast = check_password_strength(pwd)

    print(f"\nPassword Strength: {rating}")
    if suggestions:
        print("\nSuggestions to improve your password:")
        for s in suggestions:
            print(f"- {s}")

    print(f"\nEstimated brute-force attempts: {attempts:e}")
    print(f"Time to crack (slow attack): {format_time(time_slow)}")
    print(f"Time to crack (fast attack): {format_time(time_fast)}")

    print("\nGeneral Best Practices:")
    for tip in best_practices():
        print(f"- {tip}")
