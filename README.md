# cisco-rest-api-fundamentals-workshop
An introduction to REST APIs with Python and Postman.
Written by Olamide Abegunde.


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
  curl https://deckofcardsapi.com/api/deck/new/ | python -m json.tool
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
curl https://deckofcardsapi.com/api/deck/<sampledeckid>/draw/?count=3 | python -m json.tool
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
Think of the magic tricks you can do with this API. Okay, let's agree this API does not involve sleight of hand, but you can certainly play some computerized blackjack with this simple API. You can go to https://deckofcardsapi.com/ to learn more

## Task 2 - Cisco Webex API

<img width="1134" alt="image" src="https://github.com/lamide-xxx/cisco-rest-api-fundamentals-workshop/assets/92684648/e6c9ec77-4c84-4fc8-85eb-7353a6690eef">
