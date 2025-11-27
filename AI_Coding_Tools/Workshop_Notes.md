# AI Coding Tools Compared:

##  Using ChatGPT to code Snake Game 
1.  Ask ChatGPT by prompting 'Create Snake Game using React'
2.  ChatGPT paid version has a preview to run the code
3.  To run locally:  prompt ChatGPT for instructions.  Follow instructions provided by ChatGPT to run the game locally
4.  If following locally lead to errors, ask ChatGPT to go search for latest Tailwind instructions 


##  Using Claude.AI to code Snake Game  (Claude seems to be better at coding)
1.  In Claude.AI  ,  use same prompt to create Snake Game,  , and it will have preview.  
2.  In Claude also provide same prompt to run locally
3.  For free plan, you may run out of free messages (free tier provides limited messages)

##  DeepSeek

##  Ernie


##  Microsoft Copilot  

# Coding Assistants  / Integerated Developmenty Environment or IDEs
1.  Claude Code (anthropic) (doesn't feel like IDE)  feels more like command line
     *  can read files, can write files
     *  creates todo list and proceeds with it
     *  can run commands in command line terminal
     *  does not need to run IDE
     *  cost around $0.28 cents to create the snake game app
     *  test game with arrow keys - has scoring
     <img width="1140" height="471" alt="image" src="https://github.com/user-attachments/assets/63e710d6-a1a8-4efa-a385-7d3158e6515f" />
     <img width="586" height="610" alt="image" src="https://github.com/user-attachments/assets/23e64475-2c05-4375-ae2a-52d796d6e6b1" />
     *  claude is quite powerful (pay as you go or via subscription around $20/month)


2.  Github Copilot
     *  microsoft co-pilot and github co-pilot are different... github co-pilot is integrated into visual studio code
     *  you can enter chat like ' explain and describe the project' it will read thru all the files, and try to describe the          application
     *  <img width="1385" height="683" alt="image" src="https://github.com/user-attachments/assets/dcc01425-8e6d-494a-b840-896db9e634a3" />

     *  you can refactor it, and put the logic into the snake game component
     *  accept the changes...or reject the changes... it will update the code directly if you accept
  
     *  npm run dev  (command to run the application)
     *  in the demo, the initial version of game adds '20' to the score when snake eats the food.  asking it to debug
        it insist that it's 10, but user in the chat said it's 20, so github co-pilot did the lazy way out and changed the
        score addition from setScore(prev =>  prev + 10) to setScore(prev => prev + 5) which fixes the issue the lazy way  
   
3.  Cursor (cursor.com)
      *  fork of VS code with AI features
      *  when cursor appeared, github co-pilot came with the chat...
      *  Cursor is just a fork of VS-Code
      *  In the code we add plus five, and it adds 10, there must be a bug, and see if it can find the bug...
      *  suspects something happens twice...
      *  Vibe Coding - you read and accept....
      *  it will make changes if you accept changes
      *  request changes thru chat,   ie.  add features
      *  use templates... use AI agent to use templates.... idea beyond bootstrappers
4.  Pear  (trypear.ai)

#  Project Bootstrappers
   Both Bolt and Lovable are similar, they have templates....
1.  Bolt 
2.  Lovable (for simple prototypes)
       *  create prototype in Lovable, and then push to github.
       *  not sure if it can read from github
       *  ask for enhancements... mode - walls and passthrough
       *  ask ChatGPT to make description more formal, and copy requirements to Lovable to build the prototype..
       *  connect to Github,  and export code to Github
       *  connect project, and then it will export to github...
       *  they have template and they update template,,, you can use cursor to update github....
       *  you can describe having back-end integration
       *  
# Agents
       -  all the chats have agents , and it performs certain actions on your behalf, list files, read files, change files             (chat application with tools that can perform some actions)
       -  agents can do everything.... From RAG to Agents,  workshop, implement something like Chat-Agents.


# Anthropic Computer  Use
      -   can get agent to control browser, do searches, etc...
      -   demo on docker wher eyou can use agents to control computer
      -   chat applications, and you can provide it some tools 
      
  
