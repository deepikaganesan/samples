# samples

first sample:
class Parent(object):
    def override(self):
        print "PARENT override()"
    def implicit(self):
        print "PARENT implicit()"
    def altered(self):
        print "PARENT altered()"
class Child(Parent):
    def override(self):
        print "CHILD override()"
    def altered(self):
        print "CHILD, BEFORE PARENT altered()"
        super(Child, self).altered()
        print "CHILD, AFTER PARENT altered()"
dad = Parent()
son = Child()
dad.implicit()
son.implicit()
dad.override()
son.override()
dad.altered()
son.altered()

second:
class Other(object):
    def override(self):
        print "OTHER override()"
    def implicit(self):
        print "OTHER implicit()"
    def altered(self):
        print "OTHER altered()"
class Child(object):
    def __init__(self):
        self.other = Other()
    def implicit(self):
        self.other.implicit()
    def override(self):
        print "CHILD override()"
    def altered(self):
        print "CHILD, BEFORE OTHER altered()"
        self.other.altered()
        print "CHILD, AFTER OTHER altered()"
son = Child()
son.implicit()
son.override()
son.altered()

third:
import random
from urllib import urlopen
import sys
WORD_URL = "http://learncodethehardway.org/words.txt"
WORDS = []
PHRASES = {
"class %%%(%%%):":
"Make a class named %%% that is-a %%%.",
"class %%%(object):\n\tdef __init__(self, ***)" :
"class %%% has-a __init__ that takes self and *** parameters.",
"class %%%(object):\n\tdef ***(self, @@@)":
"class %%% has-a function named *** that takes self and @@@ parameters.",
"*** = %%%()":
"Set *** to an instance of class %%%.",
"***.***(@@@)":
"From *** get the *** function, and call it with parameters self, @@@.",
"***.*** = '***'":
"From *** get the *** attribute and set it to '***'."
}
# do they want to drill phrases first
PHRASE_FIRST = False
if len(sys.argv) == 2 and sys.argv[1] == "english":
    PHRASE_FIRST = True
# load up the words from the website
for word in urlopen(WORD_URL).readlines():
    WORDS.append(word.strip())
def convert(snippet, phrase):
    class_names = [w.capitalize() for w in
    random.sample(WORDS, snippet.count("%%%"))]
    other_names = random.sample(WORDS, snippet.count("***"))
    results = []
    param_names = []
    for i in range(0, snippet.count("@@@")):
        param_count = random.randint(1,3)
        param_names.append(', '.join(random.sample(WORDS, param_count)))
    for sentence in snippet, phrase:
        result = sentence[:]
        for word in class_names:
            result = result.replace("%%%", word, 1)
# fake other names
        for word in other_names:
            result = result.replace("***", word, 1)
# fake parameter lists
        for word in param_names:
            result = result.replace("@@@", word, 1)
        results.append(result)
    return results
# keep going until they hit CTRL- D
try:
   while True:
       snippets = PHRASES.keys()
       random.shuffle(snippets)
       for snippet in snippets:
           phrase = PHRASES[snippet]
           question, answer = convert(snippet, phrase)
           if PHRASE_FIRST:
               question, answer = answer, question
           print question
           raw_input("> ")
           print "ANSWER: %s\n\n" % answer
except EOFError:
    print "\nBye"
