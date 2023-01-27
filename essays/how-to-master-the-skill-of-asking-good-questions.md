---
layout: essay
type: essay
title: "How to Master the Skill of Asking Good Questions"
# All dates must be YYYY-MM-DD format!
date: 2023-01-26
published: true
labels:
  - Smart Questions
  - Stupid Questions
  - Answers
  - StackOverflow
---

<img class="img-fluid" src="../img/smartQuestion1.jpg">

## Why do we ask questions?
Asking questions is an essential part of being a software engineer. It enables you to understand your problems more thoroughly. But it's crucial to keep in mind that you shouldn't depend on other people to handle all of your difficulties. Make sure you have thought it through or done your research before you ask a question. Sometimes, the problem you are facing might have already been asked or even answered. When asking questions, communication is vital. The answer you get will likely depend a lot on how you word your question. Therefore, it's important to improve your questioning skills.


## A “Smart” question should look like 
When asking for help, it's important to make sure your question is clear and informative. A good question should provide specific details about your problem and include all the necessary information for someone to understand the issue. To make it easy for others to understand, using a meaningful and specific subject header would be helpful. Clearly describes the problem and what's deviating from the expected behavior. Don't forget to include an explicit request for a reply and a way for someone to get in touch with you, such as an email or forum thread. And lastly, be sure to use clear, grammatically correct language and anticipate and answer any questions a helper may have, provide a minimal test case or hint about the problem with code, and be courteous and appreciative of any help provided.


## An example of asking a Smart question
The question below is an example of a smart question. I included a good header, and the question is straightfoward. The person who ask the question have done some research before asking.    
```
      Recently, I ran some of my JavaScript code through Crockford's JSLint, and it gave the following error:

      Problem at line 1 character 1: Missing "use strict" statement.
        
        Doing some searching, I realized that some people add "use strict"; into their JavaScript code. Once I added the statement, the error stopped appearing.             Unfortunately, Google did not reveal much of the history behind this string statement. Certainly it must have something to do with how the JavaScript is               interpreted by the browser, but I have no idea what the effect would be.

      So what is "use strict"; all about, what does it imply, and is it still relevant?

      Do any of the current browsers respond to the "use strict"; string or is it for future use?
```

## What about a "Stupid" question?
On the other hand, a question that is poorly written and lacks specifics is bad. Because of this, it might be challenging for someone to comprehend the issue. These kinds of questions may contain exclamation points or expressions like "Please help me," be unclear or unspecific in their topic headings, and waste space in the subject line by detailing their own suffering. They might not even attempt to use their research skills or problem-solving abilities. Instead, they just throw the issue out there and let others handle it.

## An example of asking a Stupid question

1. As mentioned, we should never use an unclear header when asking a question.

```
      “please help me in lxml” 
```

2. The question is unclear and lacks details

```
      I am facing some problem in scraping using lxml I just made a code that is working fine but I have two problems
      I want name and address in same row and every entry should be in different  line like

        name1,adress1
        name2,adress2
    
      i dont need any squar bracket in the data
      I have to enter 500 codes so I want to import it from external text/csv file Please help me how can i do it

      import lxml.html as lh

      from selenium import webdriver

      browser = webdriver.Firefox()

      from lxml import html

      for cod in ("35211","36116","36542"):
     
          browser.get('http://kmbsapps.konicaminolta.us/wheretobuy/main_search.jspx?productCategory=Office+Systems&sl_zip='+cod)
      
          content = browser.page_source
     
          tree = lh.fromstring(content)
    
          name=tree.xpath('//tr/td/span[@class="largecol"]/text()')
     
          adress=tre.xpath('//tr/td/span[@class="smallcol"]/text()')
     
          print(name,adress)
```

3. The author was not able to answer the helper’s question about the issue as we can see from the conversation between the author and the helper.

```
      How does the text/csv look like? – helper
      it is in excell in column – author of the post
      I mean: what columns are there? – helper
```

4. The author was not intended to test the code on their own the helper provided 

```
      will i get results in seprate lines – author of the post
      Yes you will. Please try yourself. I gave you working codes. – helper
```

Source: <a href="https://stackoverflow.com/questions/25831209/please-help-me-in-lxml"><i class="Stackoverflow"></i>BadQuestion</a>
