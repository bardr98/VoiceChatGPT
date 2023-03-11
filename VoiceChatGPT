# Importing the required libraries
import openai
import playsound
import speech_recognition as sr
from gtts import gTTS

# Setting the API key for the OpenAI chatbot
openai.api_key = "sk-..."

# Looping the code until the user types 'exit'
while True:
    # Setting up the speech recognizer with the microphone
    r = sr.Recognizer()
    with sr.Microphone() as source:
        # Prompting the user to speak or exit
        print("You can speak now or say 'exit' to leave:")
        # Recording the audio from the microphone
        audio = r.listen(source)
        # Indicating the end of recording
        print("Time Over.")

    try:
        # Converting speech to text using Google's speech recognition API
        question = r.recognize_google(audio, show_all=False)
        if question == "exit":
            # Exiting the loop if the user types 'exit'
            print("Goodbye!")
            break
        # Displaying the user's question
        print("User: ", question)

    except sr.UnknownValueError:
        # Handling the case when the speech cannot be recognized
        print("Could not understand audio")
        continue

    except sr.RequestError as e:
        # Handling the case when the API request fails
        print("Could not request results; {0}".format(e))
        continue

    # Generating a response from the OpenAI chatbot using the user's question
    response = openai.ChatCompletion.create(
        model="gpt-3.5-turbo",
        messages=[{"role": "system", "content": "You are a chatbot"}, {"role": "user", "content": question}]
    )

    # Extracting the response message from the API response
    answer = ''
    for choice in response.choices:
        answer += choice.message.content

    # Displaying the chatbot's response
    print('AI:', answer.replace('\n', ''))

    # Converting the chatbot's response to an audio file
    audio = gTTS(text=answer, lang="en", slow=False)
    audio.save("response.mp3")

    # Playing the audio file
    playsound.playsound("response.mp3", True)
