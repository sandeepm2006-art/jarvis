import speech_recognition as sr
import pyttsx3
import datetime
import wikipedia
import pywhatkit
import pyautogui
import os
import webbrowser

# ================= VOICE ENGINE =================
engine = pyttsx3.init()
engine.setProperty('rate', 170)

def speak(text):
    print("Jarvis:", text)
    engine.say(text)
    engine.runAndWait()

# ================= TAKE COMMAND =================
def take_command():
    listener = sr.Recognizer()
    with sr.Microphone() as source:
        print("Listening...")
        listener.pause_threshold = 1
        audio = listener.listen(source)

    try:
        command = listener.recognize_google(audio)
        command = command.lower()
        print("You:", command)
    except:
        return "none"
    return command

# ================= MAIN FUNCTION =================
def run_jarvis():
    speak("Hello, I am Jarvis. How can I help you?")

    while True:
        command = take_command()

        # TIME
        if 'time' in command:
            time = datetime.datetime.now().strftime('%I:%M %p')
            speak(f"The time is {time}")

        # DATE
        elif 'date' in command:
            date = datetime.datetime.now().strftime('%A, %d %B %Y')
            speak(f"Today is {date}")

        # WIKIPEDIA SEARCH
        elif 'who is' in command or 'what is' in command:
            try:
                info = wikipedia.summary(command, 2)
                speak(info)
            except:
                speak("Sorry, I couldn't find information.")

        # YOUTUBE
        elif 'play' in command:
            speak("Playing on YouTube")
            pywhatkit.playonyt(command)

        # GOOGLE SEARCH
        elif 'search' in command:
            speak("Searching on Google")
            webbrowser.open(f"https://www.google.com/search?q={command}")

        # OPEN APPS
        elif 'open chrome' in command:
            os.system("start chrome")

        elif 'open notepad' in command:
            os.system("notepad")

        # SCREENSHOT
        elif 'screenshot' in command:
            img = pyautogui.screenshot()
            img.save("screenshot.png")
            speak("Screenshot taken")

        # EXIT
        elif 'exit' in command or 'stop' in command:
            speak("Goodbye")
            break

        # DEFAULT RESPONSE
        else:
            speak("I did not understand, please say again")

# ================= RUN =================
run_jarvis()
