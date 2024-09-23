# Welcome

This guide was written for developers who want to write Discord Bots with TypeScript, but don't know how to get started. This guide will help you create a developer application and set up all the boilerplate for a project. By the end of the guide, you will:

- Install the required programs
- Download the required dependencies
- Set up the code environment
- Learn how to create an application using the Discord Developer Portal
- Invite the bot to a test server
- Securely store your bot token so that it is ignored if you decide to open-source the bot
- Authenticate with Discord to get the bot online
- Learn how listeners work

You will NOT:

- Learn how Git works and how to upload your project to GitHub
- Learn how to use VSCode in general
- Learn beyond the basics of listeners, such as creating slash commands, embeds, etc.

For these, you should already have knowledge on it, or it's up to you to research them. There are a lot of resources on using Git and VSCode. If you need help with DiscordJS after this guide, here are a few helpful links:

- [DiscordJS Guide](https://discordjs.guide/#before-you-begin)
- [DiscordJS Docs](https://discord.js.org/docs/packages/discord.js/14.16.2)

Keep in mind that any documentation for DiscordJS is 100% compatible with TypeScript.

# Prerequisites

- [Visual Studio Code](https://code.visualstudio.com/)
- [NodeJS](https://nodejs.org/en)
  - Installing Chocolatey is optional. If you are unsure what it does, you can skip that step in the installer.
- A [Discord](https://discord.com/) account

# Code Setup

1. Open Visual Studio Code and open the folder in which you will initialize the project (Ctrl + K, Ctrl + O).
   ![image](https://github.com/user-attachments/assets/f131ba13-de33-4c5f-b067-050eb63eecca)
   - Note that this should be an **empty folder**; the next few steps will generate new files, and this folder will be considered the project root.
2. Open the terminal (Ctrl + Shift + `).

![image](https://github.com/user-attachments/assets/b469242e-ab53-4ffe-8fe1-d14cb5ce9516)

3. Run `npm init` and follow the next steps in the terminal.
   - The most important fields are the project name, description, and author.
   - All other fields, especially if you don't know what they do, can be left blank.
   - If this step has been completed correctly, a `package.json` file should have been generated.
4. Run `npm i discord.js` and `npm i -D @types/node ts-node typescript`.
   - This installs the package we're using, the TypeScript types that allow the language to be used, `ts-node` which allows us to test the `.ts` files without needing to compile first, and TypeScript itself.
5. Create a file in the root of your project named `tsconfig.json` and paste in the following template:
```json
{
    "compilerOptions": {
        "target": "ESNext",
        "module": "commonjs",
        "rootDir": "./src/",
        "outDir": "./dist/",
        "strict": true,
        "moduleResolution": "node",
        "importHelpers": true,
        "experimentalDecorators": true,
        "esModuleInterop": true,
        "skipLibCheck": true,
        "allowSyntheticDefaultImports": true,
        "resolveJsonModule": true,
        "forceConsistentCasingInFileNames": true,
        "removeComments": true,
        "typeRoots": [
            "node_modules/@types"
        ],
        "sourceMap": false,
        "baseUrl": "./"
    },
    "files": [
        "src/Index.ts"
    ],
    "include": [
        "./**/*.ts"
    ],
    "exclude": [
        "node_modules",
        "dist"
    ]
}
```
6. In `package.json`, which should've been created in step 3, add the following inside the "scripts" dictionary:
![image](https://github.com/user-attachments/assets/215cbed4-745b-4e43-ac64-64e652a73645)
```
"start": "ts-node src/Index.ts",
"build": "tsc"
```
7. Create a folder named `src` and a file inside named `Index.ts`.

# Bot Setup

This section focuses on creating the Bot application on Discord, which is the bot our code will interface with and control.

1. Head over to the [Discord Developer Portal](https://discord.com/developers/applications/).
2. Log into your Discord account and click on **New Application**:
   ![image](https://github.com/user-attachments/assets/77ca6a82-725f-4270-8733-9e433e4814c5)
3. Give your new application a name, agree to the terms of service, and click **Create**. You should now be on a configuration page.
   ![image](https://github.com/user-attachments/assets/fbb3b599-7ef2-4527-b88e-952d9fa5b29f)
   - You may provide your application a description and tags, but they are optional right now.
4. Navigate to the **Bot** tab and **reset your token**. Copy it.
   ![image](https://github.com/user-attachments/assets/70dbc6d7-e38d-4a30-90fb-5dffc0282619)
   - The token will only be shown to you *once*. It is meant to be kept completely secret, and once you leave the page, the token will no longer be shown.
   - You may also provide an icon and banner for your bot's profile on this page, as well as its display name.
5. Head to the **OAuth2** tab and scroll all the way down to **OAuth2 URL Generator**. Check the **Bot** scope.
   ![image](https://github.com/user-attachments/assets/aed411a9-7206-471c-982b-21f08d2a29d3)
   - Under the scopes, you will see a lot of options for permissions. Check whatever you want the bot to be able to do, such as sending messages, managing channels, etc. If you change your mind on what permissions the bot needs later, you can always kick it from your server and reinvite it from this page with updated permissions.
6. At the bottom of the page, find the generated URL and click **Copy.**
   ![image](https://github.com/user-attachments/assets/1ddffa11-3506-4d72-867d-fafe29aef8b8)
7. Open the link in a new tab, select the server you wish to invite the bot to in the dropdown, and click **Authorize.**
   ![image](https://github.com/user-attachments/assets/f09c1a76-1321-4e94-b91e-85126098f5ce)
   - Ideally, this should be a testing server you own for now. If you want to invite this to a server another person owns and the permissions you gave the bot are higher than you are allowed to permit, ask the server owner or an administrator to invite the bot using the link.

# Connecting to the Bot

### Storing the token

While you could simply paste the token directly into the bot script, that would be foolish if you ever intend to open-source your project. To reiterate, the token is meant to be extremely well-guarded so that no unauthorized third parties may gain access to your bot. Therefore, we will read the token from an external file, which we will exclude from version control.

1. Create a folder named `data` inside your `src` folder.
2. Create a file inside the folder named `vars.json`.
   - Your directory should now look like this:

![image](https://github.com/user-attachments/assets/d68781f5-efe3-47b0-8cba-b62215152c02)


3. Inside this new file, open up a set of curly braces, and create an entry named "token" that is set to your copied token. It should look like this, with your token instead of the placeholder text:

    ```json
    {
        "token": "paste token here"
    }
    ```

4. Create a new file in the **root** of the project named `.gitignore` and fill in the file with:

    ```plaintext
    node_modules
    src/data/vars.json
    ```

This ensures that if you set up Git, your `vars.json` file will not be at risk of being uploaded.

### Authenticating with Discord

1. Now that the token is stored, open up `src/Index.ts`.
2. To import the token that was just stored, type the line `import { token } from './data/vars.json';`.
3. Import the Client object from `discord.js` on a new line by typing `import { Client } from 'discord.js';`.
4. Create the client object:
   - If you are not sure what intents are, check [this page](https://discordjs.guide/popular-topics/intents.html#privileged-intents). They are mainly for permissions such as sending direct messages, so for this guide just leave the array empty.
```typescript
const client = new Client({
    intents: []
});
```
5. Now add the line `client.login(token);`, which uses the token imported in step 2.
6. In your terminal (step 2 of **Code Setup** if you need to pull it up again), type `npm run start` to test the bot.

![image](https://github.com/user-attachments/assets/c53adaed-bd07-4098-b59f-2b10eb336c6c)

7. Check the Discord server the bot was invited to. If it shows that is it online, then success! DiscordJS has successfully authenticated with Discord and your bot is running!
   - Press Ctrl + C to kill the program.

At the end of all these steps, your code should look like this:
```typescript
import { token } from './data/vars.json';
import { Client } from 'discord.js';

const client = new Client({
    intents: []
});

client.login(token);
```

# What Now?

Now that the bot starts, it's time to start writing it. At this point, what you want the bot to do lies outside the scope of this guide, so further development will not be covered. However, I will provide a few tips on listeners.

### Listeners

To set up a listeners, use the `client.on` method. For example:
```typescript
client.on('ready', () => {
    console.log('Bot is ready!');
})
```

The provided code assigns a callback to the 'ready' event, which fires once the bot has logged in. In this case, once the client has successfully logged in, the output will print "Bot is ready!"

There are a lot of other events that can be listened for, and the full list can be found [here](https://discord.js.org/docs/packages/discord.js/14.16.2/Client:Class).

There are links to further guides in the **Welcome** section that will likely help with developing your bot.

### Building

If you want to build the bot to JavaScript, run `tsc` in your terminal. If it doesn't work, try restarting VSCode.

If you get the specific error that the command "cannot be loaded because running scripts is disabled on this system," then try the following:

1. Open PowerShell as Administrator
2. Run the following command: `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned`
3. Type `Y` and press Enter
4. Try the command again

The resulting .js files should be output to /dist

# Conclusion

Now that you have followed this guide, you should have a working DiscordJS project set up to use TypeScript, with code that successfully authenticates with Discord and brings the bot online. There are links for further research listed near the top of this guide. Thank you for reading.
