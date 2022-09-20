# Coursera - Python 3 Programming Specialization

## Foundations of Python Programming



### 6.11. Chapter Assessment

![](../.gitbook/assets/2022-04-09\_20h54\_06.png)

```python
nums = [4, 2, 23.4, 9, 545, 9, 1, 234.001, 5, 49, 8, 9 , 34, 52, 1, -2, 9.1, 4]

how_many = 0

def cal(num_lst, num):
    how_many = 0
    i = 0
    j = len(num_lst)
    while i<j:
        if num_lst[i] == num:
            how_many += 1
        i += 1
    return how_many

how_many = cal(nums, 9)

##################################################

nums = [4, 2, 23.4, 9, 545, 9, 1, 234.001, 5, 49, 8, 9 , 34, 52, 1, -2, 9.1, 4]
how_many = 0
for item in nums:
    if item == 9:
        how_many += 1

##################################################

nums = [4, 2, 23.4, 9, 545, 9, 1, 234.001, 5, 49, 8, 9 , 34, 52, 1, -2, 9.1, 4]
how_many = 0
i = 0
j = len(nums)
while i < j:
    if nums[i] == 9:
        how_many += 1
    i += 1

##################################################

nums = [4, 2, 23.4, 9, 545, 9, 1, 234.001, 5, 49, 8, 9 , 34, 52, 1, -2, 9.1, 4]
how_many = 0
i = len(nums)
while i > 0:
    print(nums[i-1])
    if nums[i-1] == 9:
        how_many += 1
    i -= 1
    
    
```







### 22.8. Project - Wheel of Python

```python
# PASTE YOUR WOFPlayer CLASS (from part A) HERE
# PASTE YOUR WOFHumanPlayer CLASS (from part B) HERE
# PASTE YOUR WOFComputerPlayer CLASS (from part C) HERE

# Write the WOFPlayer class definition (part A) here
class WOFPlayer:
    def __init__(self, name):
        self.name = name
        self.prizeMoney = 0
        self.prizes = []
    def addMoney(self, amt):
        self.prizeMoney = self.prizeMoney + amt
    def goBankrupt(self):
        self.prizeMoney = 0
    def addPrize(self, prize):
        self.prizes.append(prize)
    def __str__(self):
        return "{} (${})".format(self.name, self.prizeMoney)
    
# Write the WOFHumanPlayer class definition (part B) here
class WOFHumanPlayer(WOFPlayer):
    def getMove(self, category, obscurePhrase, guessed):
        prompt = """
{} has ${}

Category: {}
Phrase:  {}
Guessed: {}

Guess a letter, phrase, or type 'exit' or 'pass':
""".format(self.name, self.prizeMoney, category, obscurePhrase, guessed)
        while True:
            userinp = input(prompt)
            if userinp:
                 return userinp

        
        
        
# Write the WOFComputerPlayer class definition (part C) here

class WOFComputerPlayer(WOFPlayer):
    SORTED_FREQUENCIES = 'ZQXJKVBPYGFWMUCLDRHSNIOATE'
    def __init__(self, name, difficulty):
        WOFPlayer.__init__(self, name)
        self.difficulty = difficulty
    def smartCoinFlip(self):
        rand_number = random.randint(1, 10)
        if rand_number > self.difficulty:
            return False
        else:
            return True
    def getPossibleLetters(self, guessed):
        pl = []
        pl_final = []
        for i in LETTERS:
            if i not in guessed:
                pl.append(i)
        if self.prizeMoney < VOWEL_COST:
            for i in pl:
                if i not in VOWELS:
                    pl_final.append(i)
        return pl_final
    def getMove(self, category, obscuredPhrase, guessed):
        pl = self.getPossibleLetters(guessed)
        if len(pl) == 1:
            return 'pass'
        if self.smartCoinFlip():
            for i in WOFComputerPlayer.SORTED_FREQUENCIES[::-1]:
                if i in pl:
                    return i
            return 'pass'
        else:
            return random.choice(pl)





import sys
sys.setExecutionLimit(600000) # let this take up to 10 minutes

import json
import random
import time

LETTERS = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
VOWELS  = 'AEIOU'
VOWEL_COST  = 250

# Repeatedly asks the user for a number between min & max (inclusive)
def getNumberBetween(prompt, min, max):
    userinp = input(prompt) # ask the first time

    while True:
        try:
            n = int(userinp) # try casting to an integer
            if n < min:
                errmessage = 'Must be at least {}'.format(min)
            elif n > max:
                errmessage = 'Must be at most {}'.format(max)
            else:
                return n
        except ValueError: # The user didn't enter a number
            errmessage = '{} is not a number.'.format(userinp)

        # If we haven't gotten a number yet, add the error message
        # and ask again
        userinp = input('{}\n{}'.format(errmessage, prompt))

# Spins the wheel of fortune wheel to give a random prize
# Examples:
#    { "type": "cash", "text": "$950", "value": 950, "prize": "A trip to Ann Arbor!" },
#    { "type": "bankrupt", "text": "Bankrupt", "prize": false },
#    { "type": "loseturn", "text": "Lose a turn", "prize": false }
def spinWheel():
    with open("wheel.json", 'r') as f:
        wheel = json.loads(f.read())
        return random.choice(wheel)

# Returns a category & phrase (as a tuple) to guess
# Example:
#     ("Artist & Song", "Whitney Houston's I Will Always Love You")
def getRandomCategoryAndPhrase():
    with open("phrases.json", 'r') as f:
        phrases = json.loads(f.read())

        category = random.choice(list(phrases.keys()))
        phrase   = random.choice(phrases[category])
        return (category, phrase.upper())

# Given a phrase and a list of guessed letters, returns an obscured version
# Example:
#     guessed: ['L', 'B', 'E', 'R', 'N', 'P', 'K', 'X', 'Z']
#     phrase:  "GLACIER NATIONAL PARK"
#     returns> "_L___ER N____N_L P_RK"
def obscurePhrase(phrase, guessed):
    rv = ''
    for s in phrase:
        if (s in LETTERS) and (s not in guessed):
            rv = rv+'_'
        else:
            rv = rv+s
    return rv

# Returns a string representing the current state of the game
def showBoard(category, obscuredPhrase, guessed):
    return """
Category: {}
Phrase:   {}
Guessed:  {}""".format(category, obscuredPhrase, ', '.join(sorted(guessed)))

# GAME LOGIC CODE
print('='*15)
print('WHEEL OF PYTHON')
print('='*15)
print('')

num_human = getNumberBetween('How many human players?', 0, 10)

# Create the human player instances
human_players = [WOFHumanPlayer(input('Enter the name for human player #{}'.format(i+1))) for i in range(num_human)]

num_computer = getNumberBetween('How many computer players?', 0, 10)

# If there are computer players, ask how difficult they should be
if num_computer >= 1:
    difficulty = getNumberBetween('What difficulty for the computers? (1-10)', 1, 10)

# Create the computer player instances
computer_players = [WOFComputerPlayer('Computer {}'.format(i+1), difficulty) for i in range(num_computer)]

players = human_players + computer_players

# No players, no game :(
if len(players) == 0:
    print('We need players to play!')
    raise Exception('Not enough players')

# category and phrase are strings.
category, phrase = getRandomCategoryAndPhrase()
# guessed is a list of the letters that have been guessed
guessed = []

# playerIndex keeps track of the index (0 to len(players)-1) of the player whose turn it is
playerIndex = 0

# will be set to the player instance when/if someone wins
winner = False

def requestPlayerMove(player, category, guessed):
    while True: # we're going to keep asking the player for a move until they give a valid one
        time.sleep(0.1) # added so that any feedback is printed out before the next prompt

        move = player.getMove(category, obscurePhrase(phrase, guessed), guessed)
        move = move.upper() # convert whatever the player entered to UPPERCASE
        if move == 'EXIT' or move == 'PASS':
            return move
        elif len(move) == 1: # they guessed a character
            if move not in LETTERS: # the user entered an invalid letter (such as @, #, or $)
                print('Guesses should be letters. Try again.')
                continue
            elif move in guessed: # this letter has already been guessed
                print('{} has already been guessed. Try again.'.format(move))
                continue
            elif move in VOWELS and player.prizeMoney < VOWEL_COST: # if it's a vowel, we need to be sure the player has enough
                    print('Need ${} to guess a vowel. Try again.'.format(VOWEL_COST))
                    continue
            else:
                return move
        else: # they guessed the phrase
            return move


while True:
    player = players[playerIndex]
    wheelPrize = spinWheel()

    print('')
    print('-'*15)
    print(showBoard(category, obscurePhrase(phrase, guessed), guessed))
    print('')
    print('{} spins...'.format(player.name))
    time.sleep(2) # pause for dramatic effect!
    print('{}!'.format(wheelPrize['text']))
    time.sleep(1) # pause again for more dramatic effect!

    if wheelPrize['type'] == 'bankrupt':
        player.goBankrupt()
    elif wheelPrize['type'] == 'loseturn':
        pass # do nothing; just move on to the next player
    elif wheelPrize['type'] == 'cash':
        move = requestPlayerMove(player, category, guessed)
        if move == 'EXIT': # leave the game
            print('Until next time!')
            break
        elif move == 'PASS': # will just move on to next player
            print('{} passes'.format(player.name))
        elif len(move) == 1: # they guessed a letter
            guessed.append(move)

            print('{} guesses "{}"'.format(player.name, move))

            if move in VOWELS:
                player.prizeMoney -= VOWEL_COST

            count = phrase.count(move) # returns an integer with how many times this letter appears
            if count > 0:
                if count == 1:
                    print("There is one {}".format(move))
                else:
                    print("There are {} {}'s".format(count, move))

                # Give them the money and the prizes
                player.addMoney(count * wheelPrize['value'])
                if wheelPrize['prize']:
                    player.addPrize(wheelPrize['prize'])

                # all of the letters have been guessed
                if obscurePhrase(phrase, guessed) == phrase:
                    winner = player
                    break

                continue # this player gets to go again

            elif count == 0:
                print("There is no {}".format(move))
        else: # they guessed the whole phrase
            if move == phrase: # they guessed the full phrase correctly
                winner = player

                # Give them the money and the prizes
                player.addMoney(wheelPrize['value'])
                if wheelPrize['prize']:
                    player.addPrize(wheelPrize['prize'])

                break
            else:
                print('{} was not the phrase'.format(move))

    # Move on to the next player (or go back to player[0] if we reached the end)
    playerIndex = (playerIndex + 1) % len(players)

if winner:
    # In your head, you should hear this as being announced by a game show host
    print('{} wins! The phrase was {}'.format(winner.name, phrase))
    print('{} won ${}'.format(winner.name, winner.prizeMoney))
    if len(winner.prizes) > 0:
        print('{} also won:'.format(winner.name))
        for prize in winner.prizes:
            print('    - {}'.format(prize))
else:
    print('Nobody won. The phrase was {}'.format(phrase))

```



### 24.14. Project - OMDB and TasteDive

```python
import requests_with_caching
import json

def get_movies_from_tastedive(str):
    base_url = 'https://tastedive.com/api/similar'
    d = {'q': str, 'type': 'movies', 'limit': 5}
    results = requests_with_caching.get(base_url, params=d)
    # print(results.url)
    # print(results.text)
    return results.json()
    #return json.loads(results.text)
    
# some invocations that we use in the automated tests; uncomment these if you are getting errors and want better error messages
# get_movies_from_tastedive("Bridesmaids")
# get_movies_from_tastedive("Black Panther")

def extract_movie_titles(info_dict):
    name_list = []
    res_list = info_dict["Similar"]["Results"]
    # print(type(res_list))
    # print(res_list)
    for item in res_list:
        if item['Name'] not in name_list:
            name_list.append(item['Name'])
    return name_list
    
# some invocations that we use in the automated tests; uncomment these if you are getting errors and want better error messages
# extract_movie_titles(get_movies_from_tastedive("Tony Bennett"))
# extract_movie_titles(get_movies_from_tastedive("Black Panther"))

def get_related_titles(movie_titles_lst):
    title_lst = []
    for item in movie_titles_lst:
        title_lst = title_lst + extract_movie_titles(get_movies_from_tastedive(item))
    return list(set(title_lst)) # 利用python中集合元素惟一性特点，将列表转为集合，将转为列表返回，实现清除列表中重复元素。

# some invocations that we use in the automated tests; uncomment these if you are getting errors and want better error messages
# get_related_titles(["Black Panther", "Captain Marvel"])
# get_related_titles([])


def get_movie_data(str):
    base_url = 'http://www.omdbapi.com/'
    d = {'t': str, 'r': 'json'}
    results = requests_with_caching.get(base_url, params=d)
    # print(results.url)
    # print(results.text)
    return results.json()
        
# some invocations that we use in the automated tests; uncomment these if you are getting errors and want better error messages
# get_movie_data("Venom")
# get_movie_data("Baby Mama")

def get_movie_rating(movie_dict):
    for item in movie_dict["Ratings"]:
        if item["Source"] == "Rotten Tomatoes":
            value = item["Value"]
            return int(value[:-1])
    return 0    
            
# some invocations that we use in the automated tests; uncomment these if you are getting errors and want better error messages
# get_movie_rating(get_movie_data("Deadpool 2"))


def get_sorted_recommendations(movie_titles_lst):
    r_movie_titles_lst = get_related_titles(movie_titles_lst)
    # print(r_movie_titles_lst)
    d = {}
    for item in r_movie_titles_lst:
        if item not in d:
            d[item] = get_movie_rating(get_movie_data(item))
    # print(d)
    s = sorted(d, key=lambda k: (d[k], k), reverse=True) # 16.5. Breaking Ties: Second Sorting
    # print(type(s))
    # print(s)
    return s
    
# some invocations that we use in the automated tests; uncomment these if you are getting errors and want better error messages
get_sorted_recommendations(["Bridesmaids", "Sherlock Holmes"])
```

## Links

* [Python 3 Programming | Coursera](https://www.coursera.org/specializations/python-3-programming)
* [Table of Contents — Foundations of Python Programming](https://fopp.umsi.education/books/published/fopp/index.html) || 在线 书籍
