# Modmail

> **NOTICE:** I'm not working in this bot anymore. You're free to use it and/or fork the project though; as long as you comply with the AGPL terms.

> **Important Note:** There is **no** global bot you can invite. The code is designed to work with a single guild, so you'll have to host it yourself if you need it in your server.

Discord bot for contacting a server's moderators easily and anonymously.

## Building

- Clone the repository.

  ```bash
  git clone https://github.com/lmarinborges/modmail
  ```

- Install [sqlx-cli](https://github.com/launchbadge/sqlx).

  ```bash
  cargo install sqlx-cli
  ```

- Export `DATABASE_URL`, optionally change the file the database will be saved to.

  ```bash
  export DATABASE_URL='sqlite:compiled.db'
  ```

- Create the database.

  ```bash
  sqlx db setup
  ```

- Compile with cargo.
  ```
  cargo build --release
  ```

## Setup

### Running

- [Create a new application at Discord Developers if you haven't already.](https://discord.com/developers/applications)

- Copy the link and replace `<YOUR_ID_HERE>` with your application ID to invite the bot to the server.

  ```
  https://discord.com/api/oauth2/authorize?client_id=<YOUR_ID_HERE>&permissions=17448306688&scope=applications.commands%20bot
  ```

- Create a `.env` file at the root of the project.

  ```sh
  # your discord API token
  DISCORD_TOKEN=

  # your discord API application ID
  DISCORD_APPID=

  # the ID to the server you'll be using the bot inside of
  DISCORD_GUILD=

  # (Optional) Set to change how verbose logging output is.
  # https://docs.rs/env_logger/latest/env_logger/#enabling-logging
  RUST_LOG=info
  ```

- Run the server.

  ```bash
  cargo run
  ```

### Configuring

The bot uses two basic slash commands to configure itself:

- `/blockrole set <role>` will configure `<role>` as the bot's block role. If a member has this role, the bot will refuse to forward their DMs.
- `/inbox set <channel>` will set a text channel as your "inbox". As soon as the bot receives a DM from a user it doesn't recognize, it will create a thread under this channel, with a randomly generated name such as `peaceful bonefish` or `accurate wren`.

## Usage

After configuring, a user may send the bot a DM, and it'll create a new thread under the inbox channel. Any messages sent by the user will be forwarded to this thread, and any messages sent in the thread will be forwarded to the user.

When you're done chatting with a user, use the command `/close <codename>` to archive the thread with the specified name and forget the user attached to it. If the same user were to send another message, they would appear in a new thread under a different codename.

If a user is abusing the bot through spam or other nasty things, use `/block <codename>`. The bot will retrieve the member behind the codename and assign them the configured block role, preventing them from using the bot.

## License

The code in this repository is available under the [AGPLv3 License](https://www.gnu.org/licenses/agpl-3.0.en.html).
