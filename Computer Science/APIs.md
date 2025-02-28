*Application Programming Interface*

An **API**, similar to a [[What is a protocol|Protocol]] is **an abstract set of rules**.
What differs an **API** from a protocol is that these rules specify how to request or send information to the piece of software/hardware specified. 

A piece of software or hardware to which you can request or send information is said to have an **API**. It specifies, somewhere in it's documentation, **What you can request, What you can send, what information is required, and over what protocol to request or send that information.**

>[!info] Example
>A weather app could have an API, which would let you know how to get the weather information from the app itself.


# REST API
*Representational State Transfer API*

A **Rest API** is an **API** which follows the principles and constraints of **REST**, once again an abstract set of rules.
An **API** in itself is broad, and encapsulate any system that allows communication to, and from itself.
However a **REST API** must follow a certain set of constraints which we're about to define *be patient*.

First off, a **REST API** uses [[HTTP]] *non negligible sorry*. If you can communicate with your **API** via another protocol, then it isn't a **REST API**.
*so far so good*

Secondly, it is **Stateless**, which means that each request **must** include all the necessary information, and there is no consistency between **API calls**.  
For instance, you do a first **API call** in which you pass in your age to get your ideal _BMI_, and then a second call where you request the _average height_ for people your age. In a **Stateful API**, the second call might not need you to pass in your age again, as it might have stored it somewhere in your session. However, a **REST API** will require you to send your age with each new request.

Thirdly, the response format consists of returning data in a **standard format**, which means no fancy objects, or files. It returns **JSON** data, or **XML**. It returns any format which can be easily used by other systems and applications.

*There is a fourth one, but I don't get it and I don't care gofy.*

>[!info]
>When people refer to APIs nowadays, they are mainly talking about REST APIs, unless from tech nerds who will have you make the distinction.


