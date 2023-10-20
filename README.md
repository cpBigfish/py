# py
import sys
import webbrowser

import cv2
import pyttsx3
import speech_recognition as sr
import datetime
import os
from requests import get
import wikipedia
import webbrowser
import pywhatkit as kit
import smtplib
import sys

engine = pyttsx3.init("sapi5")
voices = engine.getProperty("voices")
print(voices[0].id)
engine.setProperty("voices", voices[0].id)

#text to speech
def speak(audio):
    engine.say(audio)
    print(audio)
    engine.runAndWait()

#to convert voice to speech
def takecommand():
    r = sr.Recognizer()
    with sr.Microphone() as source:
        print("listening.....")
        r.pause_threshold = 1
        audio = r.listen(source,timeout=1,phrase_time_limit=3)

    try:
        print("Recognizing...")
        query = r.recognize_google(audio, language="en-us")
        print(f"user said: {query}")
    except Exception as e:
        speak("say that again please")
        return "none"
    return query

#to wish
def wish():
    hour = datetime.datetime.now().hour

    if hour>=0 and hour<=12:
        speak("good morning")
    elif hour>=12 and hour<18:
        speak("good afternoon")
    else:
        speak("good evening")

    speak("i am cyprian's ai voice assistant sir. please tell me how can i help you")

#to send email
""" 
def sendEmail(to,content): 
    server = smtplib.SMTP("smtp.gmail.com", 587) 
    server.ehlo() 
    server.starttls() 
    server.login("your email id", "your password") 
    server.sendmail("your email id", to, content) 
    server.close() 
    """


if __name__ == "__main__":
    wish()
    while True:
        if 1:
            query = takecommand().lower()
            # logic building for tasks
            if "open Notepad" in query:
                npath = "c:\\windows\\system32\\Notepad.exe"
                os.startfile(npath)
            elif "open command prompt" in query:
                os.system("start cmd")
            elif "open camera" in query:
                cap = cv2.videoCapture(0)
                while True:
                    ret,img = cap.read()
                    cv2.imshow("webcam", img)
                    k = cv2.waitkey(50)
                    if k==27:
                        break;
                cap.release()
                cv2.destroyAllWindows()
            elif "ip address" in query:
                ip = get("https://api.ipfy.org").text
                speak(f"your ip address is{ip}")
            elif "wikipedia" in query:
                speak("searching wikipedia....")
                query = query.replace("wikipedia")
                results = wikipedia.summary(query, sentences=2)
                speak(results)
            elif "open youtube" in query:
                webbrowser.open("www.youtube.com")
            elif "open stackoverflow" in query:
                webbrowser.open("www.stackoverflow.com")
            elif "open microsoft edge" in query:
                speak("sir, what should search on microsoft edge")
                cm = takecommand().lower()
                webbrowser.open(f"{cm}")
            elif "send message" in query:
                kit.sendwhatmsg("+25494170998", " hello dad this is angwenyi your son i was building ai with python",2,25)
            elif "email to robert" in query:
                try:
                    speak("what should i say?")
                    content = takecommand().lower()
                    to = "cpcipy@gmail.com"
                    #sendEmail(to,content)
                    speak("Email has been sent")
                except Exception as e:
                    print(e)
                    speak("sorry sir, i am not able to send this mail to robert")

            elif "no thanks" in query:
                speak("thanks for using me sir, my programmer cyprian will be happy for the service i provided to you. have good day")
                sys.exit()
            speak("sir, do you have any other questions or work?")




     #speak()
