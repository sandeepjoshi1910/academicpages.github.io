---
title: 'Iris : Speech to Code'

permalink: /projects/speechtocode
tags:
  - Speech To Code
  - VSCode Extension
  - Microsoft Azure Services
  - Microsoft LUIS
  - Electron App
  - Speech To Text
  - Text To Code
  - RabbitMQ
  - HackHarvard 2018
---


> Speech to code is a hackathon project done at Harvard University during HackHarvard 2018.

## Inspiration

The programming landscape of today is swiftly changing with new interfaces and interactions that help programmers code better and more efficiently. Typing direct code has always been the accepted paradigm in programming. However, not many people have ventured into speech; especially natural language speech. We can now explore this avenue of progress due to the availability of tools such as speech-to-text and intent classification in cloud platforms such as Microsoft Azure or Google Cloud. This is why we venture into the field of speech-to-code for easier interface and input for users.

But why speech? Our one concern is the need for better programming tools for differently abled people. If building entrances, vehicles, and iPhones can have accessible features, coding tools should also have them is our opinion. More than making programming tools accessible, young people with special needs should not be intimidated or discouraged by the inconvenience of using inaccessible programming tools. This thwarts the progress of highly talented computer science students.

Iris is a step to make coding tools accessible by integrating directly Visual Studio Code. Our goal has been to have a minimal third-party solution which seamlessly integrates with best apps in the industry by leveraging state of the art technologies from Microsoft and Google thereby significantly reducing the learning curve of an otherwise standalone foreign tool.

## What it does
Iris is a speech to code solution that takes in the natural speech from the user and converts it into Python code. The module can easily be adjusted for handling other programming languages including Java or C. Iris can take up to 15 intents right now that include commands such as "Create a function named X with arguments alpha and beta" or "Define the main function". The code is then generated automatically and is presented in Visual Studio Code through a custom VS Code Extension. Iris is lightweight and can be extended to any code editor in the future.

## How we built it
There are three main modules to Iris: the backend making API requests and handling message passing, the VS Code extension that takes care of interacting and editing things in VS Code, and finally an Electron JS application as a front-end UI for the user to interact with. The core module running the Speech to text and Text to speech (less frequently used) operations is Google Cloud Web Speech API. The main module that enables us to parse the natural language into intents and classify them into actions is the Microsoft LUIS API endpoint. The LUIS endpoint helps us also train the model on new languages and extend the solution. The communication between Python scripts, extension, and Electron application is done through RabbitMQ. This handles our backend message passing system and enables smooth requests to be made.

## Challenges we ran into
The primary challenge we ran into is generalizing the text to intent classification module. This is because this domain is quite a niche and there are not many different examples available for us to train on. Microsoft LUIS now provides pre-built domains so that developers do not have to train their own model with their own sentences. However, our domain is not applicable. Thus, we had to come up with our own training sentences and train the model. More generalization is possible as we come up with more sample cases. Another challenge was the design of the system as there were multiple services that were independent on their own but need to communicate between themselves. We achieved an efficient way to do this using a message broker like RabbitMQ which let us pass messages between all our components using shared memory much like in distributed systems!

## Accomplishments that we're proud of
We are proud of the extremely accurate intent classification model we built with LUIS. It is not easy to parse natural language and provide the response in JSON with entities. Microsoft LUIS helped us with this operation by abstracting the low-level work and directly work in a plug-and-play fashion. We are also proud of our implementation of RabbitMQ and how messages are sent/received within our three modules. The message passing is quite efficient and can be scaled up as needed. We are proud of the amount of integration we got with VS Code. For some of our operations, we had to dig deep into VS Code source code and make changes there! Finally, we are quite happy with our UI and how well we were able to build one that also looks good!

## What we learned
We definitely learned about Google Cloud APIs and Microsoft LUIS. This helped us understand the services made available by these cloud platforms. We also learned about building a robust message passing system that can help us communicate between separate applications/scripts. We then learned about the latency involved in making API requests and ways to minimize them. Finally, we learned how to initially package such kinds of applications for eventual deployment.

## What's next for Iris
Next, we hope to expand Iris for even more commands, package the application in a cleaner way for faster distribution, and iterate to an even better product!

<img src='/images/stc.jpeg'>


### Links to Repo

• [Python Backend](http://github.com/abhoi/speech2code/) 
• [Electron App & VSCode Extension](https://github.com/sandeepjoshi1910/Speech2Code_JS)

