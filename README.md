# Final Hour

Final Hour is an open-source, audio-based game inspired by the Zombies mode in the Call of Duty series. Players can team up online to fight off hordes of zombies, aiming for high scores, kill counts, and an enjoyable experience.

## Features

*   **Online Co-op Gameplay:** Play with friends and other players online to survive against the undead.
*   **Inspired by Call of Duty Zombies:** Experience gameplay mechanics, weapons, and items reminiscent of the Aether timeline in the Call of Duty Zombies series.
*   **Immersive Audio Experience:** Designed as an audio game, Final Hour provides a rich and immersive soundscape for players.
*   **In-game map builder:** Builders construct maps from inside the game by walking to corners and placing elements with a keystroke. No external XML editing required.
*   **Cross-Platform:** While primarily developed on Windows, the game is built with cross-platform compatibility in mind.

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes.

### Prerequisites

Before you begin, ensure you have [Pipenv](https://pipenv.pypa.io/en/latest/) installed. If you don't have it, you can install it using pip:

```sh
pip install pipenv
```

### Installation

1.  Clone the repository to your local machine:

    ```sh
    git clone https://github.com/lower-elements/final-hour-client-public.git
    ```

2.  Navigate to the project directory:

    ```sh
    cd final-hour-client-public
    ```

3.  Install the project dependencies using Pipenv:

    ```sh
    pipenv install
    ```

### Running the Game

To play the game, run the following command in the project's root directory on a Windows machine:

```sh
pipenv run python final_hour.py
```

## Building the Game

To build an executable version of the game, follow these steps:

1.  **Update the Version:** Before building, edit `libs/version.py` to set a new version number, following semantic versioning (e.g., `major.minor.patch`).

2.  **Commit the Version Change:** Commit the updated `libs/version.py` file to your local Git repository.

3.  **Tag the Release:** Create a new Git tag for the release:

    ```sh
    git tag -a X.Y.Z -m "Release X.Y.Z"
    ```

    Replace `X.Y.Z` with the version number you set in `libs/version.py`.

4.  **Run the Build Script:** Execute the build script to create the game executable:

    ```sh
    pipenv run build.bat
    ```

## Building maps in-game

Players with the builder role can construct maps without leaving the game. The workflow anchors every edit to your character's position: walk to a corner, mark it, walk to the opposite corner, mark it, then pick an element type.

### Getting builder access

Builder commands are gated on the server. You need one of:

- Your username listed in the server's `contributors.txt` (contributors are automatically builders), or
- An existing contributor running `/set builder <your-username> yes` in chat.

If neither applies, ask whoever runs your server to grant it. Once you have access, `/builderhelp` in chat lists every builder command.

### Creating a new map

Builder edits target your current map, so you usually want a fresh one rather than modifying `main`. From chat:

```
/mkmap <name> <minx> <maxx> <miny> <maxy> <minz> <maxz>
```

Example — a 40 × 40 footprint, 20 tiles tall:

```
/mkmap mybar -20 20 -20 20 0 20
```

`/mkmap` creates the map file at `maps/<name>.map`, registers it server-side, and teleports you in at `(minx+1, miny+1, minz+1)`. To return to a map later use `/chmap <name>`, or `/chmap` on its own to pick from a menu.

### Default key bindings

| key | action |
| --- | --- |
| `m` | mark the next corner at your position (cycles 1 → 2 → 1) |
| `shift+m` | clear both corners |
| `b` | speak the platforms, zones and doors within reach |
| `shift+b` | speak your current corner positions and the bounding box |
| `n` | open the place menu (platform, door, zone, spawn, …) |
| `shift+n` | re-run your last `/place` or `/here` |
| `u` | open the point-element menu (perk machine, power switch, window, …) |
| `t` | open the macro menu (`/room`, `/ladder`, `/skylight`, `/doorway`) |
| `delete` | undo the last builder edit |
| `shift+delete` | redo |
| `end` | delete the element at your position |

All key bindings are configurable from the in-game options menu.

### Chat-command reference

Every key binding is just a shortcut for a chat command. Type `/builderhelp` in chat for the full list, including arguments. Highlights:

- `/mark`, `/unmark`, `/marks`
- `/place <type> [args]` — `platform`, `door`, `zone`, `playerSpawn`, `zombieSpawn`, `wallbuy`, `interactable`, `ambience`, `soundSource`, `music`, `reverb`
- `/here <type> [args]` — `perkMachine`, `powerSwitch`, `window`, `pannable`
- `/room`, `/ladder`, `/skylight`, `/doorway` — one-command macros
- `/del`, `/setid`, `/setattr` — manage existing elements
- `/probe [r]`, `/listids`, `/whatami` — audit nearby elements
- `/tp <x> <y> <z>` — teleport yourself in the current map without losing markers or preview state. `/tp 1` / `/tp 2` jumps to the corresponding marker; `/tp center` to the midpoint.
- `/undo`, `/redo`, `/repeat`
- `/preview`, `/commit`, `/cancel` — see below

Edits are validated and persisted to the server's `maps/<name>.map` immediately. Undo keeps the last 50 changes per map in memory.

### Preview mode (try before commit)

If you can't easily visualize what a wall or a `/room` will look like, build it as a ghost first:

```
/preview
/mark         (corner 1)
/mark         (corner 2)
/room walls=wallwood floor=wood door=N
```

The room is placed for real — walls block, floors hold you up, you can walk through the doorway — but every element is tagged `class="ghost"` and remembered. Walk through, decide whether it works, then either:

- `/commit` — strip the ghost tag, making the structure permanent.
- `/cancel` — delete every preview element in one batch and try again.

You stay in preview mode across as many `/place`, `/here`, and macro commands as you like; `/commit` or `/cancel` operate on the whole accumulated batch. Placement feedback gets a `(preview)` suffix while preview mode is on, so you always know which state you're in.

## Contributing

Contributions are welcome! If you'd like to contribute to Final Hour, please follow these steps:

1.  Fork the repository.
2.  Create a new branch for your feature or bug fix.
3.  Make your changes and commit them with clear, descriptive messages, following convencional commits.
4.  Push your changes to your forked repository.
5.  Create a pull request to the main repository's `main` branch.

Please ensure your code adheres to the project's existing code style and conventions.

## License

This project is licensed under the GNU General Public License v3.0. See the [LICENSE](LICENSE) file for more details.

## Acknowledgments

*   This project is heavily inspired by the Zombies mode in the Call of Duty series, particularly the Aether storyline.
*   We utilize resources from the Call of Duty fan community for weapon stats, character quotes, and gameplay mechanics.
*   A special thanks to the creators of the original Call of Duty Zombies for their incredible work.
