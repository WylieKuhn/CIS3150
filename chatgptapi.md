# Using Python To Interact With ChatGPT
### Wylie Kuhn
  
#### Install Python.
Firstly Download Python for [Windows](https://www.python.org/downloads/windows/) or [macOS](https://www.python.org/downloads/macos/), i recommend the latest release under “Stable Releases”.

Install and run python, ensuring you check the box to add python to your system PATH.

#### Installing virtualenv and creating the virtual enviroment.  
I found that the openai package for me only worked in a virtual enviroment so here's the instructions to do that.  
  
[More detailed information can be found here](https://www.freecodecamp.org/news/how-to-setup-virtual-environments-in-python/)

Next install virtualenv by opening the command terminal and running the command `Pip install virtualenv 

Next navigate to the directory you want to start your project in and open the command terminal again. Run the command `python<version> -m venv <virtual-environment-name>`

Example `python3.11 -m venv chatgptAPI`

Then `cd` into your enviroment folder and run `source env/bin/activate` on macOS or `env/Scripts/activate.bat` on windows to activate it.

Once you have activated your virtual enviroment simply `pip install openai`

The OpenAI python package is now installed on your computer.  

#### Setting Up Your Account for API Access.  

The backend of OpenAI's project, billing, and api-key system is weirdly difficult to navigate so I am adding the pages you will need up top right here.  

[OpenAI Platform page](https://platform.openai.com/docs/api-reference/introduction)  
[API keys page](https://platform.openai.com/api-keys)  

Go to [ChatGPT](https://chatgpt.com/auth/login) and create an account, or log in if you already have one.  
  
Then go to the [OpenAI Platform page](https://platform.openai.com/docs/api-reference/introduction), and in the top left corner create a new project. name it, save it. DO NOT CREATE A SECRET KEY YET.

Then click the gear for settings, go under billing on the left, add $5 to your account, make sure auto recharge is off, and go back to your [api keys page](https://platform.openai.com/api-keys) and create a secret key, copy it somwhere because you will need this.  
  
Now you are all set up to actually query ChatGPT.

#### Let's write the code!  
  
The basic code for interacting with OpenAI looks like this 

```
from openai import OpenAI
from keyfile import key

"""
There are several ways of inserting your key here. You can hardcode it (UNSAFE!), Put it in a seperate file and import it (Lazy but slightly safer), or add it to your OS enviroment variables and import it (Best)
"""
client = OpenAI("YOUR KEY GOES HERE")

#Here I have made it so you can enter a different prompt each time
prompt = str(input("Enter Prompt: "))
print("")


completion = client.chat.completions.create(
    model="gpt-4o-mini",
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {
            "role": "user",
            "content": prompt
        }
    ]
)
print("Response: \n")
print(completion.choices[0].message.content)
```

This code will query chatGPT and return a response.

I have prepared the code below to get a response in just JSON, and then loop through it in order to siplay the results.  
  
```
from openai import OpenAI
from keyfile import key
import json

client = OpenAI(api_key=key)

prompt = """
    Give me information about 10 famous NASCAR drivers. 

    Follow the instructions below

    ***
    Provide the information in pure JSON using the following keys: name, car_number, number_of_wins, number_of_championships.

    Do not include any other text or information other than the JSON response, including no backticks.
    ***
"""
print("")

completion = client.chat.completions.create(
    model="gpt-4o-mini",
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {
            "role": "user",
            "content": prompt
        }
    ]
)
print("Response: \n")

for info in json.loads(completion.choices[0].message.content):
    print(f"""
          Name: {info["name"]}
          Car Number: {info["car_number"]}
          Number of Wins: {info["number_of_wins"]}
          Number Of Championships: {info["number_of_championships"]}
          """)

```

This prompt specifies it will return the data in the correct format that I can then immdiatly manipulate in python, and what fields exactly to return.