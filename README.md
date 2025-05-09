# import requests
import random
import string
import time

def generate_username():
    chars = string.ascii_lowercase + string.digits  # a-z + 0-9
    return ''.join(random.choices(chars, k=3))  # 3-character usernames

def check_instagram_username(username):
    url = f"https://www.instagram.com/{username}/"
    headers = {
        "User-Agent": "Mozilla/5.0"
    }

    try:
        response = requests.get(url, headers=headers, timeout=5)
        if response.status_code == 404:
            print(f"[AVAILABLE] --> {username}")
            with open("available_usernames.txt", "a") as f:
                f.write(username + "\n")
        else:
            print(f"[TAKEN] --> {username}")
    except requests.RequestException as e:
        print(f"[ERROR] Connection issue: {e}")

def main():
    tries = 100  # Number of usernames to check
    delay = 1.5  # Delay between checks to avoid rate-limiting

    for _ in range(tries):
        username = generate_username()
        check_instagram_username(username)
        time.sleep(delay)

if __name__ == "__main__":
    main()
