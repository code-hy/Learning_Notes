# Creating End-to-End Application using AI entirely

## Workflow

1.  Create front-end using Lovable  (lovable.dev).  Lovable is a good tool for quickly creating a nice looking prototype.  In the second module, Alex implemented the front-end code by using a prompt
    ``` prompt
          create the snake game with two models: pass-through and walls. prepare to make it multiplayers - we will have this functionality: leaderboard and watching (me following other players that currently play).
          add mockups for that and also for log in. everything should be interactive - I can log in, sign up, see my username when I'm logged in, see leaderboard, see other people play
          (in this case just implement some playing logic yourself as if somebody is playing) make sure that all the logic is covered with tests don't implement backend, so everything is mocked.
          But centralize all the calls to the backend in one place
    
    ```
    this creates the front-end code. To put to Github, Alex clicked on the Github Icon <img width="48" height="48" alt="image" src="https://github.com/user-attachments/assets/5cd10a48-fedd-43e0-9df5-9e2b12a30a95" />
 , and connected Lovable to the repository, following interactive instructions in authorising Lovable to access the github repo 

2. Continue rest of the development using IDE (ie. Cursor, Windsurf, VSCode or Google Antigravity).  Alex used Antigravity as the AI assistant and Codespaces as the environment.  There is no need to do github CLI setup if
   we use Copilot or Codex - as we can just use VS Code to connect to Github Codespaces.  If things are run locally, there is no need to do the set-up.  However, NodeJS, Python and Docker need to be installed separately
   on the local environment
   ```
       step for connecting antigravity to codespaces
       Step 1: Install GitHub CLI

        Download from: https://github.com/cli/cli/releases
        Step 2: Authenticate

    # Authenticate with GitHub using SSH
    gh auth login
    # Select: SSH protocol and your existing SSH key for GitHub
    # Follow the remaining prompts

    # authenticate for codespaces
    gh auth refresh -h github.com -s codespace
        Step 3: Create and Use Codespace

        gh codespace create
    # Note the ID that's generated (e.g., expert-doodle-wr7wg9p5gqcgggw)
        Step 4: Connect via SSH

        gh codespace ssh -c expert-doodle-wr7wg9p5gqcgggw
    Next, get the SSH config:

    gh codespace ssh --config -c expert-doodle-wr7wg9p5gqcgggw
    Add the output to ~/.ssh/config

    If you encounter "cannot find the key" error:

        ssh-keygen -t ed25519 -f ~/.ssh/codespaces.auto
        ssh <codespace-name>
        Step 5: Use with Antigravity

            Connect to codespace using Antigravity's SSH remote mode
            Open the project folder in /workspaces/
        Step 6: Stop Codespace When Done

            gh cs stop -c expert-doodle-wr7wg9p5gqcgggw
   ```

3.  Running and Testing the Frontend Locally

