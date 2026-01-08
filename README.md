# ai-voice-assistant
Python based AI Voice Assistant
import speech_recognition as sr
import pyttsx3
import datetime
import wikipedia
import webbrowser

# Voice engine setup
engine = pyttsx3.init()
engine.setProperty('rate', 170)

def speak(text):
    engine.say(text)
    engine.runAndWait()

def wish():
    hour = int(datetime.datetime.now().hour)
    if hour < 12:
        speak("Good morning Ansh")
    elif hour < 18:
        speak("Good afternoon Ansh")
    else:
        speak("Good evening Ansh")

def take_command():
    r = sr.Recognizer()
    with sr.Microphone() as source:
        print("🎤 Listening...")
        r.pause_threshold = 1
        audio = r.listen(source)

    try:
        print("🧠 Recognizing...")
        query = r.recognize_google(audio, language="en-in")
        print("You said:", query)
    except:
        speak("Sorry, I did not understand")
        return "None"
    return query.lower()

# Main Program
wish()
speak("I am your AI voice assistant. How can I help you?")

while True:
    command = take_command()

    if "time" in command:
        time = datetime.datetime.now().strftime("%H:%M:%S")
        speak("Current time is " + time)

    elif "wikipedia" in command:
        speak("Searching Wikipedia")
        command = command.replace("wikipedia", "")
        result = wikipedia.summary(command, sentences=2)
        speak(result)

    elif "open google" in command:
        webbrowser.open("https://google.com")
        speak("Opening Google")

    elif "open youtube" in command:
        webbrowser.open("https://youtube.com")
        speak("Opening YouTube")

    elif "exit" in command or "bye" in command:
        speak("Goodbye Ansh")
        break
