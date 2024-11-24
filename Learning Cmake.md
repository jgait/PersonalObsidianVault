Master Resource - https://gist.github.com/mbinna/c61dbb39bca0e4fb7d1f73b0d66a4fd1

Quick Intro to Cmake - https://www.youtube.com/watch?v=HPMvU64RUTY

# Questions
- How do we add our own dependencies and link libraries?
- When using `fetch_content`How can I freeze the version such that I wont lose the ability to run my program if the library is taken down someday

# Learnings 
## What CMake?
CMake is a tool that lets us script the generation of build files for a project, it makes it so we don't have to deal with the nitty gritty of writing build files (eg. makefiles)

- Fetch content allows us to download packages at configuration time so that we don't have to manage getting said packages
- 

## To set the C++ std version
`set_property(TARGET {the_target} PROPERTY CXX_STANDARD 11)`
