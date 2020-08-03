---
title:        "Prediction of Protein Contact Map"
description:  "Paper Review : Accurate De Novo Prediction of Protein Contact Map by Ultra-Deep Learning Model"
image:        "https://journals.plos.org/ploscompbiol/article/file?id=10.1371/journal.pcbi.1005324.g016&type=large"
author:       "So Jae Woo"
---

Paper Review : De novo structure prediction with deep­learning based scoring
============

------------
Motivation
------------
프로틴 contact는 단백질 구조와 기능의 중요한 정보를 포함하고 있다. 따라서 시퀀스로부터 이러한 contact를 예측을 하는 것은 매우 중요한 문제이다. 그러나 sequence homologs를 고려하지 않은 예측은 낮은 퀄리티이고, 드노보 구조 예측한는데 유용하지 않다. 

Method
------------
A7D CASP13의 결과는 3개의 딥러닝 네트워크로부터 계산되는 변형된 비 모델링 구조 예측 시스템으로 부터 나왔다. 스코어링은 



- 이 시스템은 MSA와 HHBlits , PSI-BLAST 로 부터 만들어진 프로필오 부터 테스트 되었다. 

- 첫번째 네트워크에서는 MSA로 다른 residue의 C-beta원자 거리를 예측한다. 
- 두번째 네트워크에서는 첫번째 네트워크의 예측 결과와 레퍼런스 분포를 이용해서, 거리에 따른 후보 구조들의 우도 점수를 계산한다. 
- 이 두번째 네트워크는 MSA와 결합 예측결과를 직접 이용하여 스코어를 매긴다.                                                                   









Paragraphs are separated by a blank line.

2nd paragraph. *Italic*, **bold**, and `monospace`. Itemized lists
look like:

  * this one
  * that one
  * the other one

Note that --- not considering the asterisk --- the actual text
content starts at 4-columns in.

> Block quotes are
> written like so.
>
> They can span multiple paragraphs,
> if you like.

Use 3 dashes for an em-dash. Use 2 dashes for ranges (ex., "it's all
in chapters 12--14"). Three dots ... will be converted to an ellipsis.
Unicode is supported. ☺



An h2 header
------------

Here's a numbered list:

 1. first item
 2. second item
 3. third item

Note again how the actual text starts at 4 columns in (4 characters
from the left side). Here's a code sample:

    # Let me re-iterate ...
    for i in 1 .. 10 { do-something(i) }

As you probably guessed, indented 4 spaces. By the way, instead of
indenting the block, you can use delimited blocks, if you like:

~~~
define foobar() {
    print "Welcome to flavor country!";
}
~~~

(which makes copying & pasting easier). You can optionally mark the
delimited block for Pandoc to syntax highlight it:

~~~python
import time
# Quick, count to ten!
for i in range(10):
    # (but not *too* quick)
    time.sleep(0.5)
    print i
~~~



### An h3 header ###

Now a nested list:

 1. First, get these ingredients:

      * carrots
      * celery
      * lentils

 2. Boil some water.

 3. Dump everything in the pot and follow
    this algorithm:

        find wooden spoon
        uncover pot
        stir
        cover pot
        balance wooden spoon precariously on pot handle
        wait 10 minutes
        goto first step (or shut off burner when done)

    Do not bump wooden spoon or it will fall.

Notice again how text always lines up on 4-space indents (including
that last line which continues item 3 above).

Here's a link to [a website](http://foo.bar), to a [local
doc](local-doc.html), and to a [section heading in the current
doc](#an-h2-header). Here's a footnote [^1].

[^1]: Footnote text goes here.

Tables can look like this:

size  material      color
----  ------------  ------------
9     leather       brown
10    hemp canvas   natural
11    glass         transparent

Table: Shoes, their sizes, and what they're made of

(The above is the caption for the table.) Pandoc also supports
multi-line tables:

--------  -----------------------
keyword   text
--------  -----------------------
red       Sunsets, apples, and
          other red or reddish
          things.

green     Leaves, grass, frogs
          and other things it's
          not easy being.
--------  -----------------------

A horizontal rule follows.

***

Here's a definition list:

apples
  : Good for making applesauce.
oranges
  : Citrus!
tomatoes
  : There's no "e" in tomatoe.

Again, text is indented 4 spaces. (Put a blank line between each
term/definition pair to spread things out more.)

Here's a "line block":

| Line one
|   Line too
| Line tree

and images can be specified like so:

![example image](http://placehold.it/800x250 "An exemplary image")


And note that you can backslash-escape any punctuation characters
which you wish to be displayed literally, ex.: \`foo\`, \*bar\*, etc.
