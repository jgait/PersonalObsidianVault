For [[Index - Horclab VST2]]
#### Managing Multiple Versions of Python
For python version management, we use `pyenv-win`. Pyenv-win allows us to select the current python version that will run our code.

See the docs here -> [github](https://github.com/pyenv-win/pyenv-win)

Not actually going to need force mats until the very end of VST paper [[VST_Paper_Process]]

#### Map Files
Don't really know what exactly the map file does, but it seems to involve the configuration of the force mats. There have been a few different .map files mentioned. We have tried with `null.map` and `3150.map`. A wrong map file could be the source of our frustration. There has been discussion of a `3140.map` but the link seems to be dead (**Maybe something to email about and obtain?**)

#### Current Behavior
1. .NET package imports into python successfully
2. System finds force mat and identifies serial number (Error 0 -> Success)
3. System claims force mat by serial number using map 3150.map (Error 0 -> Success)
4. System initializes sensor (Error 0 -> Success)
5. System attempts to set sensitivity (Error -> "Attempted to set sensitivity on invalid hardware type")
6. System attempts to capture data frame (Error 2 -> Dataframe returns as None type)
7. Program crashes out on attempt to parse Dataframe since it is None

#### Next Steps
Email da Tekscan Crew. Describe issues, include code. Ask about 3140D-1 Map file.

