# Chapter 15. My Application Is All API Calls

## Points:
1. Two reasons for systems that are littered with library calls are harder to deal with than home-grown systems:
    1. It is often hard to see how to make the structure better because all you can see are the API calls and anything that would've been a hint at a design just isn't there.
    1. We don't own the API.(If we did, we could rename interfaces, classes, and methods to make things clearer for us, or add methods to classes to make them available to different parts of the code.)
1. Nearly every system has some core logic that can be peeled away from API calls.
1. When we have a system that looks like it is nothing but API calls, it helps to imagine that it is just one big object and then apply the responsibility-separation heuristics in *Chapter 20*(This Class Is Too Big and I Don't Want It to Get Any Bigger).
1. There are essentially two approaches for refactoring this kind of class:
    1. **Skin and Wrap the API**
    1. **Responsibility-Based Extraction**
1. How to choose between two?
    1. Skin and Wrap the API is good in these circumstances:
        * The API is relatively small.
        * You want to completely separate out dependencies on a third-party library.(See *Chapter 14*, Dependencies on Libraries Are Killing Me)
        * You don't have tests, and you can't write them because you can't test through the API.
    1. Responsibility-Based Extraction is good in these circumstances:
        * The API is more complicated.
        * You have a tool that provides a safe extract method support, or you feel confident that you can do the extractions safely by hand.

## Example: Mailing List Server

* Use _Responsibility-Based Extraction_ way

* Characteristics of the code
    * Before restructure:
        * One class: MailingListServer
            * One public method:
                * main()
            * Two private methods:
                * process()
                * doMessage()
    * After restructure:
        * Two interfaces and four concrete classes
            * Interfaces:
                1. MessageProcessor
                1. MailService
            * Classes:
                1. ListDriver
                1. MailReceiver
                1. MailForwarder (implements MessageProcessor)
                1. MailSender (implements MailService)

* Steps to restructure the code better:
    1. Identify the computational core of code.(It might help to try to write a brief description of what it does)
    1. Separate the code's responsibilities.

# 分かってないこと

1. _Skin and Wrap the API_ 方法（14章に参照しに行ってもあまり書いてないみたいです）
2.  In the 2nd reason at the beginning -> What if we own the API?

# 困ってること

* 実践したいのに、実践コードがあまりない
