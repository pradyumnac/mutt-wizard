# mutt-wizard

Get this great stuff without effort:

- A full-featured and autoconfigured email client on the terminal with neomutt
- Mail stored offline so you can view and write email while away from internet and keep backups

Specifically, this wizard:

- Determines your email server's IMAP and SMTP servers and ports
- Creates dotfiles for `neomutt`, `isync`, and `msmtp` appropirate for your email address
- Encrypts and stores locally your password for easy remote access, accessible only by your GPG key
- Handles as many as nine separate email accounts automatically
- Auto-creates bindings to switch between accounts or between mailboxes
- Can automatically set mail updates as often as you want to sync your mail and update you when new mail arrives
- Provides sensible defaults and an attractive appearance for the neomutt email client
- If mutt-wizard doesn't know your server's IMAP/SMTP info by default, it will prompt you for them and will put them in all the right places.

## Install and Use

```
git clone https://gitlab.com/LukeSmithxyz/mutt-wizard ~/.config/mutt
cd ~/.config/mutt
./mw # Run the mutt-wizard
```

Yes you have to put the whole repo in the mutt directory (`~/.config/mutt/`).
Just backup or delete any previous mutt configs (or msmtp or mbsync configs if you have them; if you don't know, you don't have them).

Install these required programs:

- `neomutt` - the email client.
- `isync` - downloads and syncs the mail. (required at install)
- `msmtp` - sends the email.
- `pass` - safely encrypts passwords (required at install)

You also need a GPG key pair to encrypt passwords.
If you don't know what that is, just run `gpg --full-gen-key` (or `gpg2 --full-gen-key`) to get one.
You might also want some good optional stuff:

- `w3m` - view HTML email and images in neomutt.
- `notmuch` - index and search mail. Install it and run `notmuch setup`, tell it that your mail is in `~/.local/share/mail/`. You can run it in mutt with `ctrl-f`. Run `notmuch new` to process new mail, although the included `mailsync` script does this for you.
- `abook` - a terminal-based address book. Pressing tab while typing an address to send mail to will suggest contacts that are in your abook.
- A cron manager - if you want to enable the auto-sync feature.

## User interface

To give you an example of the interface, here's an idea:

- `m` - send mail (uses your default `$EDITOR` to write)
- `j`/`k` and `d`/`u` - vim-like bindings to go down and up (or `d`/`u` to go down/up a page).
- `Enter` - read mail
- `r` - reply to highlighted mail
- `R` - replay all to highlighted mail
- `v` - view attachments to select and open them `s` to save, `Enter` to open.
- `gs`,`gi`,`ga`,`gd`,`gS` - Press `g` followed by another letter to change mailbox: `s`ent, `i`nbox, `a`rchive, `d`rafts, `S`pam, etc.
- `M` and `C` - For `M`ove and `C`opy: follow them with one of the mailbox letters above, i.e. `MS` means "move to Spam".
- `i#` - Press `i` followed by a number 1-9 to go to a different account. If you add 9 accounts via mutt-wizard, they will each be assigned a number.
- `?` - see all keyboard shortcuts


## New stuff and improvements since the original release

- `isync`/`mbsync` has replaced `offlineimap` as the backend. Offlineimap was error-prone, bloated, used obsolete Python 2 modules and required separate steps to install the system.
- `dialog` is no long used (le bloat) and the interface is simply text.
- More autogenerated shortcuts that allow quickly moving and copying mail between boxes.
- More elegant attachment handling. Image/video/pdf attachments without relying on the neomutt instance.
- abook integration by default.
- The messy template files have been removed and are now a part of the script itself.
- Optimal XDG standards compliance, moving msmtp configs to `~/.config/`, moving mail to `~/.local/share/mail/` and moving mutt-wizard files to `~/.local/share/muttwizard/`. isync/mbsync still uses home for default though as XDG compliance is not built into them.
- `accounts/` hold account data and `bin/` holds script run by or for mutt. All other directories have been disintegrated.
- `pass` is used as a password manager instead of separately saving passwords.
- Script is POSIX sh compliant.
- Error handling for the many people who don't read or follow directions.

## Watch our for these things:

- For Gmail accounts, remember also to enable third-party ("""less secure""") applications before attempting installation.
- Protonmail accounts will require you to set up "Protonmail Bridge" to access PM's IMAP and SMTP servers. Configure that before running mutt-wizard.
- If you have a university email, there might be other hurdles or two-factor authentication you have to jump through. Some, for example, will want you to create a separate IMAP password, etc.
- If you use an email server whose mailboxes are not in English, mutt-wizard might not be able to guess which is which, so you may have to manually set your Inbox, Sent, Trash, Drafts, etc. in your mutt config file. Do this after running the wizard in `accounts/NAME.muttrc`.

## Help the Project!

- Try mutt-wizard out on weird machines and weird email addresses and report any errors.
- Open a PR to add new server information into `domains.csv` so their users can more easily use mutt-wizard.
- If nothing else, [Donate!](https://paypal.me/LukeMSmith)

See Luke's website [here](https://lukesmith.xyz). Email him at [luke@lukesmith.xyz](mailto:luke@lukesmith.xyz).

mutt-wizard is free/libre software, licensed under the GPLv3.

## Details for Tinkerers

- The `muttrc` file is for universal settings.
- `personal.muttrc`, called by the `muttrc`, is the place where user-specific settings are set, and the wizard automatically adds the macros for switching between accounts here. If you want to contribute to mutt-wizard, you should put your universal personal settings here and have git ignore it. For example, I put my gpg settings here and personal aliases here.
- Accounts are generated in `accounts/`. If I create an account named `luke`, for example, `accounts/luke.muttrc` will hold that account's unique settings and `accounts/luke/` will hold headers and cache files.
- `bin/` holds the `mailsync` script and other scripts and tools the wizard uses. I make a link with `ln` to this `mailsync` file in my `$PATH` so I can run it from wherever.
