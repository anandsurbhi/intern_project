# importing all required libraries

import nltk
import feedparser
import re
import pandas as pd
import json

df = pd.read_csv("allpodcasts.csv")

print("Creating category-links dictionary......")
cat_links = {}
for i in range(len(df)):
    if df.loc[i][0] not in cat_links:
        cat_links[df.loc[i][0]] = [df.loc[i][1]]
    else:
        cat_links[df.loc[i][0]].append(df.loc[i][1])

print("Done.....")
print()

print("Creating title-summary text file for each category.....")
for key in cat_links.keys():
    print(key)
    f = open("{}.txt".format(key), "w", encoding="UTF-8")          # opening myfile.txt in write mode
    for i in cat_links[key]:                                       # iterating over each rss feed
        feed = feedparser.parse(i)                                 # storing the whole rss into feed
        for entry in feed.entries:
            if 'summary' in entry:
                f.write(entry['title'])
                f.write(entry['summary'])
    f.close()
print("Done.....")
print()

stopwords = nltk.corpus.stopwords.words('english')

def cleanhtml(raw_html):
    cleanr = re.compile('<.*?>')
    cleantext = re.sub(cleanr, ' ', raw_html)
    return cleantext

def cleanunformedhtml2(raw_html):
    cleanr = re.compile('.*?/>')
    cleantext = re.sub(cleanr, ' ', raw_html)
    return cleantext

def cleanunformedhtml3(raw_html):
    cleanr = re.compile('[^\w\s]')
    cleantext = re.sub(cleanr, ' ', raw_html)
    return cleantext

def num_there(s):
    return any(i.isdigit() for i in s)

print("Cleaning all categories text files....")
for key in cat_links.keys():
    f = open("{}.txt".format(key), 'r', encoding="UTF-8")
    o = open("{}_filtered_file.txt".format(key), 'w',encoding="UTF-8")
    raw = f.read()
    words = raw.split()
    for word in words:
        word = cleanhtml(word)
        word = cleanunformedhtml2(word)
        word = cleanunformedhtml3(word)
        temp = word.split()
        for t in temp:
            if t.lower() not in (stopwords):
                o.write(str(t+" "))
    o.close()
print("Done.....")
print()

print("Creating dictionary of keywords.......")
keywords_dict = {}

for key in cat_links.keys():
    f = open("{}_keywords.txt".format(key), 'r', encoding="UTF-8")
    raw = f.read()
    words = raw.split()
    keywords_dict[key] = words
print("Done.....")
print()

print("Frequency of keywords of each category.......")
for key in cat_links.keys():
    word_counter = {}
    print(key)
    filename = "{}_filtered_file.txt".format(key)
    q = open(filename,encoding="UTF-8")
    raw = q.read()
    tokens = nltk.word_tokenize(raw)
    fdist =nltk.FreqDist(tokens)
    
    for k,v in fdist.items():
        if k in keywords_dict[key]:
            word_counter[k] = v
    print("---------------------{}_terms------------------".format(key))
    print(word_counter)
    with open('{}_final.txt'.format(key), 'w') as convert_file:
        convert_file.write(json.dumps(word_counter))
    convert_file.close()
    print()

print("Done.....")

print()


