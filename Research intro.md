# Research 
the art of finding and exploiting vulnerability

Without a doubt, the ability to research effectively is the most important quality for a hacker to have. By its very nature, hacking requires a vast knowledge base -- because how are you supposed to break into something if you don't know how it works? The thing is: no one knows everything. Everyone (professional or amateur, experienced or totally new to the subject) will encounter problems which they don't automatically know how to solve. This is where research comes in, as, in the real world, you can't ever expect to simply be handed the answers to your questions.

As your experience level increases, you will find that the things you're researching scale in their difficulty accordingly; however, in the field of information security, there will never come a point where you don't need to look things up.


We will be looking at the following topics:
• An example of a research question
• Vulnerability Searching tools
• Linux Manual Pages

## Google - general use

We'll begin by looking at a typical research question: the kind that you're likely to find when working through a CTF on TryHackMe.  
  
Let's say you've downloaded a JPEG image from a remote server. You suspect that there's something hidden inside it, but how can you get it out?  
How about we start by searching for “hiding things inside images” in Google:  

![](https://muirlandoracle.co.uk/wp-content/uploads/2020/03/GoogleSearch1.png)  
  
Notice that the second link down gives us the title of a technique: “Steganography”. You can then click that link and read the document, which will teach you _how_ files are hidden inside images.  
  
Ok, so we know how it's done, let's try searching for a way to extract files using steganography:  

![](https://muirlandoracle.co.uk/wp-content/uploads/2020/03/GoogleSearch2.png)  
  
Already virtually every link is pointing to something useful. The first link contains a collection of useful tools, the second is more instructions on how to perform steganography in the first place. Realistically any of these links could prove useful, but let's take a look at that first one ([https://0xrick.github.io/lists/stego/](https://0xrick.github.io/lists/stego/)):  
  
![](https://muirlandoracle.co.uk/wp-content/uploads/2020/03/0xRick.png)  
  
The very first tool there looks to be useful. It can be used to extract embedded data from JPEG files -- exactly what we need it to do! This page also tells you that steghide can be installed using something called “apt”.  
Let's search that up next!  
  
![](https://muirlandoracle.co.uk/wp-content/uploads/2020/03/AptSearch.png)  
  
Great -- so apt is a package manager that lets us install tools on Linux distributions like Ubuntu (or Kali!).  
How can we install packages using apt? Let's search it!  

![](https://muirlandoracle.co.uk/wp-content/uploads/2020/03/AptSyntax.png)  

  
Perfect -- right at the top of the page we're given instructions. We know that our package is called steghide, so we can go ahead and install that:  

![](https://muirlandoracle.co.uk/wp-content/uploads/2020/03/AptInstallation.png)  
  
Now, let's switch back to that collection of steganography tools we were looking at before. Did you notice that there were instructions on how to use steghide right there?  
  
![](https://muirlandoracle.co.uk/wp-content/uploads/2020/03/SteghideSyntax.png)  
  
There we go! That's how we can extract an image from a file. Our research has paid off and we can now go and complete the task.

---

Notice the methodology here. We started with nothing, but gradually built up a picture of what we needed to do. We had a question (How can I extract data from this image). We searched for an answer to that question, then continued to query each of the answers we were given until we had a full understanding of the topic. This is a really good way to conduct research: Start with a question; get an initial understanding of the topic; then look into more advanced aspects as needed. 

## The search for exploit

