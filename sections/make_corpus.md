[<<< Previous](cleaning.md) | [Next >>>](pos_tagging.md)

# Make Your Own Corpus

Now that we have seen and implemented a series of text analysis techniques, let's go to the Internet to find a new text. You could use something such as historic newspapers, or Supreme Court proceedings, or use any txt file on your computer. Here we will use [Project Gutenberg](http://www.gutenberg.org). Project Gutenberg is an archive of public domain written works, available in a wide variety of formats, including .txt. You can download these to your computer or access them via the url. We'll use the url method. We found *Don Quixote* in the archive, and will work with that.

The Python package, urllib, comes installed with Python, but is inactive by default, so we still need to import it to utilize the functions. Since we are only going to use the urlopen function, we will just import that one.

In the next cell, type:

```python
from urllib.request import urlopen
```

The urlopen function allows your program to interact with files on the internet by opening them. It does not read them, however—they are just available to be read in the next line. This is the default behavior any time a file is opened and read by Python. One reason is that you might want to read a file in different ways. For example, if you have a **really** big file—think big data—you might want to read line-by-line rather than the whole thing at once. 

Now let's specify which URL we are going to use. Though you might be able to find *Don Quixote* in the Project Gutenberg files, please type this in so that we are all using the same format (there are multiple .txt files on the site, one with utf-8 encoding, another with ascii encoding). We want the utf-8 encoded one. The difference between these is beyond the scope of this tutorial, check out this [introduction to character encoding](https://www.w3.org/International/questions/qa-what-is-encoding) from The World Wide Web Consortium (W3C). 

Set the url we want to a variable:

```python
my_url = "http://www.gutenberg.org/files/996/996-0.txt"
```

We still need to open the file and read the file. You will have to do this with files stored locally as well. (in which case, you would type the path to the file (i.e., "data/texts/mytext.txt") in place of `my_url`)

```python
file = urlopen(my_url)

raw = file.read()
```

This file is in bytes, so we need to decode it into a string. In the next cell, type:

```python
don=raw.decode()
```

Now let's check on what kind of object we have in the "don" variable. Type:

```python
type(don)
```

This should be a string. Great! We have just read in our first file and now we are going to transform that string into a text that we can perform NLTK functions on. Since we already imported nltk at the beginning of our program, we don't need to import it again, we can just use its functions by specifying 'nltk' before the function. The first step is to tokenize the words, transforming the giant string into a list of words. A simple way to do this would be to split on spaces, and that would probably be fine, but we are going to use the NLTK tokenizer to ensure that edge cases are captured (i.e., "don't" is made into 2 words: do and n't). In the next cell, type:

```python
don_tokens = nltk.word_tokenize(don)
```

You can check out the type of `don_tokens` using the `type()` function to make sure it worked—it should be a list. Let's see how many words there are in our novel:

```python
len(don_tokens)
```

Since this is a list, we can look at any slice of it that we want. Let's inspect the first ten words: 

```python
don_tokens[:10]
```

That looks like metadata—not what we want to analyze. We will strip this off before proceeding. If you were doing this to many texts, you would want to use Regular Expressions. Regular Expressions are an extremely powerful way to match text in a document. However, we are just using this text, so we could either guess, or cut and paste the text into a text reader and identify the position of the first content (i.e., how many words in is the first word). That is the route we are going to take. We found that the content begins at word 315, so let's make a slice of the text from word position 315 to the end.

```python
dq_text = don_tokens[315:]
```

Finally, if we want to use the NLTK specific functions:

- concordance
- similar
- dispersion plot 
- others from the [NLTK book](https://www.nltk.org/book/)

we would have to make a specific NLTK `Text` object. 

```python
dq_nltk_text = nltk.Text(dq_text)
```

If we wanted to use the built-in Python functions, we can just stick with our list of words in `dq_text`. Since we've already covered all of those functions, we are going to move ahead with cleaning our text.

Just as we did earlier, we are going to remove the stopwords based on a list provided by NLTK, remove punctuation, and capitalization, and lemmatize the words. You can do it one by one as we did before, and that is totally fine. You can also merge some of the steps as you see below.

1\. Lowercase, remove punctuation and stopwords

```python
dq_clean = []
for w in dq_text:
    if w.isalpha():
        if w.lower() not in stops:
            dq_clean.append(w.lower())
print(dq_clean[:50])
```

2\. Lemmatize

```python
from nltk.stem import WordNetLemmatizer
wordnet_lemmatizer = WordNetLemmatizer()

dq_lemmatized = []
for t in dq_clean:
    dq_lemmatized.append(wordnet_lemmatizer.lemmatize(t))
```

From here, you could perform all of the operations that we did after cleaning our text in the previous session. Instead, we will perform another type of analysis: part-of-speech (POS) tagging. 


[<<< Previous](cleaning.md) | [Next >>>](pos_tagging.md)
