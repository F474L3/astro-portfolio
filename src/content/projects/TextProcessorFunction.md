---
inProgress: true
title: Text Processor Function
description: Uses the nltk library that processes data to use for model training and written as a single function
img_alt: project image alt text
link: https://github.com/F474L3?tab=repositories
tags: ['nltk', 'nlp', 'lemmatization', 'ai', 'mlai', 'stemming', 'pandas']
---
```python
""" Uses the nltk library that processes data to use for 
model training and written as a single function
"""

import string
from nltk.tokenize import word_tokenize
from nltk import PorterStemmer as Stemmer
from nltk.corpus import wordnet
from nltk.stem import WordNetLemmatizer 
from nltk import pos_tag

# define stemming function
def stemmer_text(text):
    text = word_tokenize(str(text))   # Init the Wordnet Lemmatizer    
    st = Stemmer()  
    text = [st.stem(t) for t in text]
    return (' '.join(text))

def get_wordnet_pos(treebank_tag):
    if treebank_tag.startswith('J'):
        return wordnet.ADJ
    elif treebank_tag.startswith('V'):
        return wordnet.VERB
    elif treebank_tag.startswith('N'):
        return wordnet.NOUN
    elif treebank_tag.startswith('R'):
        return wordnet.ADV
    else:
        return wordnet.NOUN
    
# use Wordnet(lexical database) to lemmatize text 
def lemmatize_text(text):
    
    lmtzr = WordNetLemmatizer().lemmatize
    text = word_tokenize(str(text))   # Init the Wordnet Lemmatizer    
    word_pos = pos_tag(text)    
    lemm_words = [lmtzr(sw[0], get_wordnet_pos(sw[1])) for sw in word_pos]
    return (' '.join(lemm_words))

#define a fucntion that specifies a string as the input and does the following text processing:
def text_process(s):
    lowercolumn = s.lower() #lowercase
    rem_mention = re.sub(r'@[A-Za-z0-9]+', '', lowercolumn) #remove user mentions
    rem_urls = re.sub(r'https?://[A-Za-z0-9./]+', '', rem_mention) #remove urls
    words = rem_urls.split()
    no_stop_words = ' '.join([t for t in words if (t not in stopwords.words('english') or t in white_list)]) #remove stop words
    rem_strings = ''.join([t for t in no_stop_words if t not in string.punctuation]) #remove punctiation
    rem_nums = ''.join([t for t in rem_strings if not t.isdigit()]) #remove numeric numbers
    string_stemming = stemmer_text(rem_nums) #stemming
    string_lemma = lemmatize_text(string_stemming) #lemmatization
    return string_lemma
```