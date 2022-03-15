# Smart-Chat-Bot
I have created a basic chat bot in python and some relevant libraries where it interacts with users and provides information on Chronic disease and what causes it from website data.


## Deployment

To deploy this project run

```bash
  !pip install nltk
  !pip install newspaper3k

  from newspaper import Article
#(Newspaper is a Python module used for extracting and parsing newspaper articles. 
#Newspaper use advance algorithms with web scraping to extract all the useful text from a website)
import random
#The random module provides access to functions that allows you to generate random numbers.
import string
#to process standard Python strings
import nltk
#NLTK is a standard python library that provides a set of diverse algorithms for NLP    
from sklearn.feature_extraction.text import CountVectorizer
#CountVectorizer is used to convert a collection of text documents to a vector of term/token counts.    
from sklearn.metrics.pairwise  import cosine_similarity
#Compute cosine similarity between samples in X and Y    
import numpy as np
import warnings
warnings.filterwarnings("ignore")
```
### Download the Punkt package
nltk.download('punkt', quiet=True)

### Get the article
article = Article("https://www.mayoclinic.org/diseases-conditions/chronic-kidney-disease/diagnosis-treatment/drc-20354527")

article.download()
article.parse()
article.nlp()
corpus = article.text


### Print the article text
print(corpus)

### Tokenization
text = corpus
sentence_list = nltk.sent_tokenize(text) # A list of sentences

### Prints the list of sentences
sentence_list

### Function to return a random greeting response to a users greeting.
```bash
def greeting_response(text):
    text = text.lower()
    
    # bots greeting response
    bot_greetings = ['howdy','hi','hey','hello','hola']
    # user greetings
    user_greetings = ['hi','hey','hello','hola','whats up']
    
    for word in text.split():
        if word in user_greetings:
            return random.choice(bot_greetings)
```

```bash
def index_sort(list_var):
    lenght = len(list_var)
    list_of_indexes = list(range(0, lenght))
    
    x = list_var
    for i in range(lenght):
        for j in range(lenght):
            if x[list_of_indexes[i]] > x[list_of_indexes[j]]:
                #Swap
                temp = list_of_indexes[i]
                list_of_indexes[i] = list_of_indexes[j]
                list_of_indexes[j] = temp
    return list_of_indexes
```

```bash
### Create bots response
def bot_response(user_input):
    user_input = user_input.lower()
    sentence_list.append(user_input)
    bot_response = ''
    count_matrix = CountVectorizer().fit_transform(sentence_list)
    similarity_scores = cosine_similarity(count_matrix[-1],count_matrix)
    # ^^Comparing the last sentence in the count matrix that users input to that of the count matrix sentences.
    similarity_scores_list = similarity_scores.flatten()
    #the process of converting this [[1,2], [3,4]] list to [1,2,3,4] is called flattening
    index = index_sort(similarity_scores_list)
    index = index[1:]
    response_flag = 0
    
    j = 0
    for i in range(len(index)):
        if similarity_scores_list[index[i]] > 0.0: # this means that there is similarity
            bot_response = bot_response + ' ' + sentence_list[index[i]]
            response_flag = 1
            j= j + 1
        if j >2:
            break
    if response_flag == 0:
        bot_response = bot_response + ' ' + "I apologise. I don't understand."
        
    sentence_list.remove(user_input)
    
    return bot_response
```

### Start the Chat
```bash
print("Doc Bot: I am Jennie, and I am your Doc Bot to help you. I will respond to all of your questions in the best possible manner, and later connect you with the relavant Doctor. If you wish to exit, type 'bye' ")

exit_list = ['exit', 'See you later','Bye','Quit', 'break']
while(True):
    user_input = input("How can I help you?")
    if user_input.lower() in exit_list:
        print("Jennie says Bye..")
        break
    else:
        if greeting_response(user_input) != None:
            print('Jennie: ' + greeting_response(user_input))
        else:
            print("Jennie: " +bot_response(user_input))
```

