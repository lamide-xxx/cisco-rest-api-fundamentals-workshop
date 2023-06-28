# REST API Fundamentals
An introduction to REST APIs with Python and Postman.
Written by Olamide Abegunde.

## What is a REST API?
An API (Application Programming Interface) enables two pieces of software to communicate with each other.

Instead of humans interfacing with software, software interfaces with software. Rather than having a human point and click through a workflow, an API exposes functionality to another application.

REST is an API framework built on HTTP, and the interface points are often for web services. When you combine REST and API, you see a simple service interface that enables applications or people to use the HTTP protocol to request objects or information.

## Prerequisites
### Webex teams
- Sign Up to Webex here https://signup.webex.com/sign-up
- Leave this page open. We will use it later in the labs
  
### Terminal
For Mac - Search for 'Terminal'  
For Windows - Start > Command Prompt
### Curl
- Check if you have curl installed. Run the following in your terminal
  ```
  curl --version
  ```
- If you see a curl version number, skip to the next section. Otherwise continue below
- Install Homebrew:
  ```
  /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
  ```
- Once Homebrew is installed, you can install curl by running the following command:
  ```
  brew install curl
  ```
- After the installation is complete, you should be able to use curl in the Terminal. Run the verison checking command again. You should see a version number come up

### Python3
- Check if you have Python3 installed. Run the following in your terminal
  ```
  python3 --version
  ```
- If you see a curl version number, skip to the next section. Otherwise continue below
- Install Homebrew if you do not have it (reference section above for command)
- Run the following:
  ```
  brew install python@3
  ```
- Verify the installation by running the same command as in step 1. It should now display the version number of the installed Python.

## Task 1 - Deck of Cards API
Most of us are familiar with a classic deck of playing cards - Ace through King, with categories of spades, diamonds, clubs, and hearts. You may not have known there's an API for that! As a simple example, since this API does not require authentication, we can use `curl` to open a brand new deck of cards pointing to a particular endpoint, `https://deckofcardsapi.com/api/deck/new/`.

### Task 1.1 - Create a New Deck of Cards
- Open a Terminal window and run this example to see the response:
  ```
  curl  https://deckofcardsapi.com/api/deck/new/
  ```
- When you have Python installed in your terminal, you can get nicer formatted JSON by piping (|) the output to the Python JSON tool like so:
  Note - Piping takes the output of the first command and feeds it to the second command
  ```
  curl https://deckofcardsapi.com/api/deck/new/ | python3 -m json.tool
  ```
You should see this response, which is a newly shuffled deck of cards with a unique ID, `deck_id`. Your `deck_id` value will be different from the example shown here. You see that 52 cards remain in the deck and the deck remains unshuffled in the JSON response.
```
{
    "success": true,
    "deck_id": "wz4csxs56693",
    "remaining": 52,
    "shuffled": false
}
```
- If you are interested in seeing the HTTP response code to your request, you can use the -I or --head option, which instructs curl to retrieve only the headers of the HTTP response. Here's how you can get the response code for the given URL:
```
curl -I https://deckofcardsapi.com/api/deck/new/
```
You should see the HTTP 200 code meaning the API call was a success
 ### Task 1.2 - Shuffle your Deck of Cards
 - You can shuffle that exact deck of cards with a request that contains the `deck_id`. Copy this command and substitute your `deck_id` for `<sampledeckid>`
   ```
   curl https://deckofcardsapi.com/api/deck/<sampledeckid>/shuffle/
   ```
Now the JSON response shows the deck has been shuffled by indicating `true` for `"shuffled"`.
```
{
   "deck_id":"sampledeckid",
   "remaining":52,
   "success":true,
   "shuffled":true
}
```

 ### Task 1.3 - Draw Some Cards
- With the freshly-shuffled deck, you can use the REST API to draw three cards by using a query parameter, `?count=3`, on the draw resource like so. Remember to substitute your `deck_id` for `sampledeckid` in the request URL
```
curl 'https://deckofcardsapi.com/api/deck/<sampledeckid>/draw/?count=3' | python3 -m json.tool
```
You should get a JSON response giving you some information about the cards drawn. Something like this:
```
{
    "success": true,
    "deck_id": "wz4csxs56693",
    "cards": [
        {
            "code": "AS",
            "image": "https://deckofcardsapi.com/static/img/AS.png",
            "images": {
                "svg": "https://deckofcardsapi.com/static/img/AS.svg",
                "png": "https://deckofcardsapi.com/static/img/AS.png"
            },
            "value": "ACE",
            "suit": "SPADES"
        },
        {
            "code": "2S",
            "image": "https://deckofcardsapi.com/static/img/2S.png",
            "images": {
                "svg": "https://deckofcardsapi.com/static/img/2S.svg",
                "png": "https://deckofcardsapi.com/static/img/2S.png"
            },
            "value": "2",
            "suit": "SPADES"
        },
        {
            "code": "3S",
            "image": "https://deckofcardsapi.com/static/img/3S.png",
            "images": {
                "svg": "https://deckofcardsapi.com/static/img/3S.svg",
                "png": "https://deckofcardsapi.com/static/img/3S.png"
            },
            "value": "3",
            "suit": "SPADES"
        }
    ],
    "remaining": 49
}
```

### Task 1.4 - Reflection
Think of the magic tricks you can do with this API. Okay, let's agree this API does not involve sleight of hand, but you can certainly play some computerized blackjack with this simple API. Go to https://deckofcardsapi.com/ to review the full documentation. 
- Can you figure out how to return drawn cards to the deck?

## Task 2 - Cisco Webex API
The Cisco Webex product offers a straightforward API used for a collaboration tool for messaging with people online. Unlike the Deck of Cards API, though, you need to authenticate for the service so that the messaging service knows who you are.


### Task 2.1 - Get your Access Token
- You can get a token by signing up for the service, registering for a token, and then logging onto the web site. Go to https://developer.webex.com/ and log in or sign up
- Navigate to Documentation > Webex APIs
- Copy your Personal Access Token
  
Here's an example:

<img width="1134" alt="image" src="https://github.com/lamide-xxx/cisco-rest-api-fundamentals-workshop/assets/92684648/e6c9ec77-4c84-4fc8-85eb-7353a6690eef">

With the token, you can send API requests by sending the token in a header. Let's walk through an example using curl.  For this example, you need to have curl installed locally. 

### Task 2.2 - Export your Access Token
- Export your token using the following command in the terminal:
```
 export TOKEN="<paste your token from the developer.webex.com website here with the outside double quote marks>"
```
### Task 2.3 - Use the API
- Now, open a terminal window and run this example to send a Webex chat message cia the API. Replace `someone@example.com` with the recipient's email associated with their Webex account.
Please note that this will not work with your own email address - it has to be someone else. You can send to me (oabegund@cisco.com). Replace `<your name>` with your name,
```
curl https://webexapis.com/v1/messages \
 -X POST -H "Authorization:Bearer $TOKEN" \
 -H "Content-Type: application/json" \
 --data '{"toPersonEmail":"someone@example.com", "text":"Hi from <Your Name>"}'
```

The response looks like the example below,This JSON response contains key-value pairs to show the data. It shows when the request was created, provides a trackable ID, shows the email address for the `toPersonEmail` the message was sent to, the text of the message, and the ID of the person who sent the message, `PersonEmail`.
```
{
   "id":"Y2l...mNh",
   "roomId":"Y2l...WMy",
   "toPersonEmail":"someone@example.com",
   "roomType":"direct",
   "text":"Hi from Cisco",
   "personId":"Y2l...ODc",
   "personEmail":"aperson@cisco.com",
   "created":"2018-12-13T23:32:43.377Z"
}
```

The person should see "Hi from Cisco" in their Webex messaging app. Check your Webex Teams to view the sent message

### Task 2.4 - Above and Beyond.
- Send a message in bold font
- Create a new group chat (Webex Space)
- Get a list of all the Webex Spaces you are in

Hint: you can access the Webex API Documentation here: https://webexteamssdk.readthedocs.io/en/latest/user/api.html


## Task 3 - Generating Code with Postman
The Deck of Cards API has a few nice commands you can do with API calls. The API does not require authentication, but you can re-use some values like the `deck_id` from previous calls. So it makes sense to use a Postman Collection. With that Collection, you can generate re-usable Python code for card games since the API already has the features you need for the player's actions, the code can add the rules of the game.

This exercise gives you a chance to try the Python `requests` library by generating the code with Postman.

**NOTE** -  For this task, you will need an IDE(Integrated Development Environment). If you do not have one installed, you can use the online IDE avilable at https://replit.com/signup

### Task 3.1 - Import the Postman Collection and Environment
- Sign Up or Log in to [Postman](https://www.postman.com/)
- Locate the Deck of Cards Postman Collection that is available in the repo at https://github.com/CiscoDevNet/dne-devfun-code/tree/main/rest-api/postman
- Click the `deckofcards.postman_collection.json` file and click the **Copy raw contents** button in the GitHub repository.
- In your Postman Workspace, create a new collection. Click Import and paste the copied contents. The collection should automatically be detected and saved as 'Deckofcards'.
- Repeat these steps for the `deckofcardsapi.postman_environment.json` file. This should be saved in the 'Environments' section as deckOfCardsApi or similar

### Task 3.2  - Generate Code using the Imported Postman  Collection
- In the imported Collection, locate the REST API call that shuffles a deck.
- To make sure that you shuffle six decks, change the query parameter to `?deck_count=6` in the URI in Postman.
- Instead of clicking **Send**, click `</>`. The **Code snippet** sidebar opens
- From the drop-down list, choose **Python - Requests**. Postman generates working code for you. Notice that there are additional programming languages and libraries that you can select in the drop-down list.
- Click **Copy to Clipboard**.

### Task 3.3  - Setup your `deck_of_cards.py` file
- Clone the `dne-devfun-code` repository to the development environment with:
```
git clone --branch main https://github.com/CiscoDevNet/dne-devfun-code.git
```
- In the file browser, open `dne-devfun-code/rest-api/python/` and locate the deck_of_cards.py file.
- Paste the generated code. Next, you are going to modify that starting point of generated code from Postman.
- Copy these Python lines to put the returned deck_id into a variable.
The returned JSON data can be used as a Python dictionary. By putting the response.json() into a dictionary variable, you can use the code repeatedly to get the exact ID you want
```
deck = response.json()
deck_id = deck['deck_id']
print(deck_id)
```

Make sure your final deck_of_cards.py file contains similar code to this example:
```
import requests


url = "https://deckofcardsapi.com/api/deck/new/shuffle/"
querystring = {"deck_count": "6"}
headers = {
'Cache-Control': "no-cache",
'Postman-Token': "dd1d8ca5-7000-21b2-2230-39969d585419"
}
response = requests.request("GET", url, headers=headers, params=querystring)

print(response.text)
deck = response.json()
deck_id = deck['deck_id']
print(deck_id)
```
### Task 3.4  - Run the Code
- Change your current working directory to the `python` folder and run:
```
python deck_of_cards.py
```
Here is example output:
```
{"deck_id": "5h7pojfn8rnq", "remaining": 312, "shuffled": true, "success": true}
5h7pojfn8rnq
```

If you cannot get the generated code to run, check your environment. One error you can see is: `TypeError: 'instancemethod' object has no attribute '__getitem__'`, which means the requests library is not installed in the environment. Make sure you have activated the virtual environment also. On Windows you would run `C:\> <venv>\Scripts\activate.bat` and on Linux/MacOS you would run source `<venv>/bin/activate` where `<venv>` is the location of the virtual environment.

### Task 3.4 - A Little Extra
What if you want to update the code so it also outputs an image of the cards? Look in the [Deck of Cards documentation](https://www.deckofcardsapi.com) to see if there's a GET request that sends an image file back.
