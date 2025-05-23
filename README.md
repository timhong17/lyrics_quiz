# lyrics_quiz

import random
import re

lyrics_database = {
    "Baby": "you know you love me, i know you care",
    "Love Story": "Romeo, take me somewhere we can be alone",
    "Closer": "Baby pull closer in the back seat of the Rover",
    "Payphone": "I'm at the payphone, trying to call home",
    "See you again": "It's been a long day without you, my friend"
}

def get_lyrics(song_name):
    if not isinstance(song_name, str):
        return None
    return lyrics_database.get(song_name.strip(), None)

def make_quiz(lyrics, num_blanks=3):
    if not lyrics:
        return None, None
    words = re.findall(r'\b\w+\b', lyrics)
    if not words:
        return None, None
    num_blanks = min(num_blanks, len(words))
    blank_indices = random.sample(range(len(words)), num_blanks)
    answers = {i: words[i] for i in blank_indices}
    quiz_lyrics = lyrics
    for idx, word in answers.items():
        pattern = r'\b' + re.escape(word) + r'\b'
        quiz_lyrics = re.sub(pattern, '___', quiz_lyrics, count=1)
        return quiz_lyrics, list(answers.values())

def display_available_songs():
    print("\nAvailable songs:")
    for song in lyrics_database.keys():
        print(f"- {song}")

def run_quiz():
    print("Welcome to the Lyrics Quiz!")
    display_available_songs()
    
  while True:
        song_name = input("\n請輸入歌名: ").strip()
        if song_name.lower() == 'quit':
            print("謝謝參加!")
            break
        lyrics = get_lyrics(song_name)
        if not lyrics:
            print("此歌曲不存在")
            continue
       quiz_lyrics, answers = make_quiz(lyrics)
        if not quiz_lyrics or not answers:
            print("Error creating quiz. Please try another song.")
            continue
        print("\nFill in the blanks:")
        print(quiz_lyrics)
        print("\n輸入答案:")
        user_answers = []
        for i in range(len(answers)):
            ans = input(f"Blank {i+1}: ").strip()
            user_answers.append(ans)
        correct = sum(1 for u, a in zip(user_answers, answers) if u.lower() == a.lower())
        print(f"\nResults:")
        print(f"Correct answers: {correct}/{len(answers)}")
        print(f"Correct words were: {', '.join(answers)}")
        play_again = input("\nWould you like to try another song? (yes/no): ").strip().lower()
        if play_again != 'yes':
            print("Thanks for playing!")
            break

if __name__ == "__main__":
    try:
        run_quiz()
    except KeyboardInterrupt:
        print("\nProgram terminated by user.")
    except Exception as e:
        print(f"An unexpected error occurred: {str(e)}")
