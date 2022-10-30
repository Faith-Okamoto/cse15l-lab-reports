We're going to be doing a little dive into a terribly useful command called `grep`. Now, I wouldn't go so far as to say this is a *deep* dive - `grep` is one of those commands where there's always another layer to sink into. What I'll write is more of a quick skim along the top. Nevertheless, there are some quite useful beginner applications of grep. There are three command-line flags which will be covered here. What are those? Well, you'll have to read on to find out, won't you?

## `grep` basics

Before we can look into *options*<sup>1</sup> for `grep`, it's useful to see what it does by default. Its syntax, according to its [manual page](https://man7.org/linux/man-pages/man1/grep.1.html), is `grep [OPTION...] PATTERNS [FILE...]`. At its most basic it searches a single file for a literal string. If you want to be a little more complicated, it can look in multiple files at once, and if you want to do something very fun<sup>2</sup> then the pattern can be a regex! I'll restrict myself from going into regex, since that is a very complicated<sup>3</sup> topic all of its own.

Here is a simple example. The shortest file in the subdirectory `technical/plos`<sup>4</sup> is `plos/pmed.0020191.txt`. These are its entire contents:

```

  
    
      
        
        The excellent article by Jordan Paradise, Lori B. Andrews, and colleagues, “Ethics.
        Constructing Ethical Guidelines for Biohistory” [1], neither advocates nor argues against
        biohistorical research; instead, it points out that such investigations are currently
        taking place without guidelines—ethical, scientific, moral, or religious. The question
        remains: if such guidelines were to be established, what individuals, institutions,
        governments, medical examiners, family members, or intrepid biographers are to be given
        permission? Who is to decide what is “historically significant”? Not to mention the
        meta-question: who is to decide who is to decide? I apologize to the authors if my brief
        comments [2] implied that they took a position on this issue.
      
    
  

```

To find just the parts which talk about ethics, `grep` for the string "ethic":

```
$ grep "ethic" plos/pmed.0020191.txt
        taking place without guidelines—ethical, scientific, moral, or religious. The question
```

This printed out the single instance of the literal string "ethic".

<sup>1</sup> I realize that I just called them "flags". Throughout this report I'll be using the terms interchangably. Sorry.  
<sup>2</sup> For certain definitions of fun. Statistically, not yours, though I guess if you're the IA assigned to read this then the probability is higher than the general population.  
<sup>3</sup> Understatement. Massive understatement. Though, if you know a bit already, there are some [regex crosswords](https://regexcrossword.com/) you might find fun. As for the definition of "fun" in use there, see second footnote.  
<sup>4</sup> All examples are going to be within the larger `technical` directory, which is also being used as the current working directory.

## `-i`: "Who cares about capitalization?"

You might have noticed something odd about the example output above. What we were interested in were lines which were about ethics, and there are other lines with some variation of "ethic" present, but they didn't show up. Why? Well, those other instances had a capital "E", and by default `grep` will only search for the literal string given. The option `-i` (long form<sup>1</sup>: `--ignore-case`) gets around this limitation. Thus, if we use it, we get all the lines were were looking for:

```
$ grep -i "ethic" plos/pmed.0020191.txt
        The excellent article by Jordan Paradise, Lori B. Andrews, and colleagues, “Ethics.
        Constructing Ethical Guidelines for Biohistory” [1], neither advocates nor argues against
        taking place without guidelines—ethical, scientific, moral, or religious. The question
```

There, that's better. Note that this works both ways: the string we're searching for can be in all caps and, as long as you're using this flag, everything will be all right. This might be useful if, I dunno, you accidentally hit caps-lock when meaning to only capitalize the first letter<sup>2</sup>? Again, this is a case where you're not sure what capitalization is in play, so perhaps you have a convention where case-insensitive searches are done with all caps to indicate their specialness.

```
$ grep -i "ETHIC" plos/pmed.0020191.txt
        The excellent article by Jordan Paradise, Lori B. Andrews, and colleagues, “Ethics.
        Constructing Ethical Guidelines for Biohistory” [1], neither advocates nor argues against
        taking place without guidelines—ethical, scientific, moral, or religious. The question
```

This flag also works if you're using symbols which don't have inherent case, such as numbers or punctuation. Say you're trying to find all lines with words which<sup>3</sup> start with "w" in this document. You have to consider that some of them (especially if the first word of a sentence) will start with a capital "W". To include those requires just a simple invocation of our<sup>4</sup> little flag that could:

```
$ grep -i " w" plos/pmed.0020191.txt
        taking place without guidelines—ethical, scientific, moral, or religious. The question
        remains: if such guidelines were to be established, what individuals, institutions,
        permission? Who is to decide what is “historically significant”? Not to mention the
        meta-question: who is to decide who is to decide? I apologize to the authors if my brief
```

Indeed, note that the third line returned has only a "Who" to match our pattern, so it would not have come up with an `-i`less search.

<sup>1</sup> Most command-line flags come in two equivalent forms: a short, usually single-letter version, which starts with one dash, and a longer, unabbreviated version, which starts with two dashes. Since they're easier to type and in more common use among "real" (ahem) CS folks, I'll be using the short forms.  
<sup>2</sup> The lab report directions said to justify usefulness. I'm just trying to show both directions, so that was the use I had, but... here's the panicked justification I came up with after the fact. You're welcome.  
<sup>3</sup> You noticed the alliteration, right? You noticed? Right? If you didn't now I've made you notice. Hah hah.  
<sup>4</sup> Upon a re-read I realize that I also switch between second-person (you/your/yours) and first-person inclusive (we/our/ours) pronouns. Well, not going to figure out which to use now. Deal with it :)

## `-v`: "Not that, the other thing!"

So `-i` gave us a little more freedom in what the pattern was looking for. But what if you don't like that pattern at all? What if you're just being contrary? Well, have I the flag for you<sup>1</sup>! The option `-v` (long form: `--invert-match`) tries to find things which *don't* match the given pattern. That means running it with the original example I provided will return any lines *lacking* the string "ethic", which is just all the other lines:

```
$ grep -v "ethic" plos/pmed.0020191.txt





        The excellent article by Jordan Paradise, Lori B. Andrews, and colleagues, “Ethics.
        Constructing Ethical Guidelines for Biohistory” [1], neither advocates nor argues against
        biohistorical research; instead, it points out that such investigations are currently
        remains: if such guidelines were to be established, what individuals, institutions,
        governments, medical examiners, family members, or intrepid biographers are to be given
        permission? Who is to decide what is “historically significant”? Not to mention the
        meta-question: who is to decide who is to decide? I apologize to the authors if my brief
        comments [2] implied that they took a position on this issue.




```

Notice that this ended up including all the empty lines too, since they certainly have no instances of "ethic", or any word at all. But why would we want to look for the "negative space" of a pattern? Well, here's an example for you. All you have to do is trust me. Take a leap of faith<sup>2</sup>! Anyways, that item of faith you'll need is that the contents of the file `../all-paths.txt` are just what the name suggests: a list of all the paths which are inside the `technical` directory<sup>3</sup>. On a cursory glance, most of the files in here are `.txt` files. But are they *all*? Well, we can check for any lines which don't have that extension:

```
$ grep -v ".txt" ../all-paths.txt
911report
biomed
government
government/About_LSC
government/Alcohol_Problems
government/Env_Prot_Agen
government/Gen_Account_Office
government/Media
government/Post_Rate_Comm
plos
```

Welp, seems that in fact there are no files without the `.txt` extension, since all the paths lacking it lead to directories instead of files. Moving on, this file also allows investigation of another question: do *any* of these `technical/plos` files lack that serial-number like thing which starts with `00`? Well, first we need to filter `../all-paths.txt` to only have paths within `plos`, which is a simple `grep` application: `grep "plos/" ../all-paths.txt > ../plos-paths.txt`<sup>4</sup>. Then we can search for non-`00` filenames:

```
$ grep -v "00" ../plos-paths.txt

```

The answer: they *all* have a `00` bit. Perhaps those larger numbers are there in case greater capacity is needed concerning number assignment later.

<sup>1</sup> Great transition, right? Right? **Right?**  
<sup>2</sup> Because that's my name. (hehe.) Also, when did I start using these footnotes for jokes instead of content? I promise the next one will be worth your time. Seriously.  
<sup>3</sup> I made it using `find * > ../all-paths.txt`. See, I promised, something of use in this footnote.  
<sup>4</sup> Yeah, yeah, I could've used a pipe, but I didn't want to bother to explain that.

## `-c`: "Just numbers, no details."

The previous commands have been manageable because the number of lines they output were slim to none. But what if you're expecting a *lot* of matches for a given pattern? And what if you don't really care what was found? Then the option `-c` (long form: `--count`) lets you ignore all the messy details of the contents which matched, instead cutting straight to the summary<sup>1</sup>: how many lines did this pattern catch? For example, we can count the number of `.txt` filenames we skipped in `../all-paths.txt` when we used the `-v` flag:

```
$ grep -c ".txt" ../all-paths.txt
1391
```

Yep, it was good that we didn't try to look through the whole thing manually! However, note that if you're looking for the number of *matches*, this flag may obscure things a bit, since it gives you the number of matching *lines*. Each line may match more than once! So if we were trying to count the number of questions in that original example file, and decided to `grep` for "?"<sup>2</sup>, we'd get this:

```
$ grep -c "?" plos/pmed.0020191.txt
2
```

But if you recall, there were actually three question marks present in the file. It's just that two are on the same line. Yes, we could look through the actual matches on this file, since it's small, but what if you're running the same command on a large number of files and then doing manipulation on the output? Make sure to have the right expectations for what you're getting out of `-c`.

For the finale here, I thought I'd combine all the commands above to find which lines in the example file are unethical<sup>3</sup>. That is, how many lines lack any version of "ethic". And "any version" of course includes capitalization variants. In the wildly unlikely event that you're reading this and aren't familiar with `grep` already, think a bit before looking at the next code block. How *would* you string together options to do this?

```
$ grep -ivc "ethic" plos/pmed.0020191.txt
14
```

The answer is 14. Fourteen lines (out of 17<sup>4</sup>) are unethical. Alas! What a terrible example to use.

<sup>1</sup> Getting close enough to the end here - or, more accurately, a long enough time from the start - that it took considerable self-control to not write "deets" in the place of "summary". But I figured that might be unfamiliar slang; I've no idea how widespread the word is.  
<sup>2</sup> I was mildly surprised this went off without a hitch, since I recall that `?` is a special character for regexes. Not going to try to figure out why it works. Regex is above my pay grade. And yes, I did run the same command without the `-c` flag just to check it was finding what I expected.  
<sup>3</sup> In case you're curious as to whether I set up the three options and their examples just so I could make this joke - the answer is yes. Yes, I did do that. Look, I need to keep myself entertained while writing this report somehow.  
<sup>4</sup> Per `wc plos/pmed.0020191.txt`.