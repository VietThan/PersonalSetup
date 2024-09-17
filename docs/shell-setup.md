# :octicons-terminal-16: Shell Setup

1. [install :simple-zsh: zsh for oh-my-zsh](https://github.com/ohmyzsh/ohmyzsh/wiki/Installing-ZSH)
    1. Although I'm seeing more and more arguments for using [:simple-fishshell: fish](https://fishshell.com/).
2. [Oh-My-Zsh](https://github.com/ohmyzsh/ohmyzsh) just to "feel like a better programmer" because I will be the first to admit I don't fully use all its capabilities.
    1. General note: For any plugin installation, search for a oh-my-zsh specific installation.
    2. [powerlevel10k](https://github.com/romkatv/powerlevel10k) for the theme. (including the MesloNG font).
    3. [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions) is the first plugin.
    4. [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting) is the second plugin.
3. History setup in `~/.zshrc`

    ```
    # options for history 
    setopt EXTENDED_HISTORY INC_APPEND_HISTORY INC_APPEND_HISTORY_TIME

    # EXTENDED_HISTORY — Adds timestamps to each history entry. This is useful if you ever want to track when commands were executed.
    # INC_APPEND_HISTORY — Ensures that commands are written to the history file immediately after they are entered, instead of waiting until the shell session ends. This helps you retain history even if the shell crashes.
    # INC_APPEND_HISTORY_TIME — Writes timestamps alongside history entries when INC_APPEND_HISTORY is enabled, which further helps with auditing command history.
    # SHARE_HISTORY — Shares the command history across all running zsh instances. This is helpful if you work with multiple terminal windows, as all of them will have access to a unified history.



    # Keep more entries in memory and in .zsh_history file.
    HISTSIZE=10000
    SAVEHIST=5000

    # When trimming history file, keep unique commands and trim duplicates first.
    setopt HIST_EXPIRE_DUPS_FIRST

    # Do not enter a command into the history if it is a duplicate of the previous event.
    setopt HIST_IGNORE_DUPS
    ```