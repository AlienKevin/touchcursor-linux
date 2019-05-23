# touchcursor-linux
This application remaps the `uiophjklnmy` keys to different movement keys when the spacebar is pressed down, allowing you to keep your hands on the home row.

```
i - up
j - left
k - down
l - right
u - home
o - end
p - backspace
h - enter
m - del
y - insert
```

# How to install
1. Clone or download this repo
2. 'make' to build the application
3. 'make install' to install the application
4. Update the config at /etc/touchcursor/touchcursor.conf

# Configure key mappings
Change key mappings by modifying these lines in `src/touchcursor.c`:
1. hyper key

```c
/**
 * Checks if the key is the hyper key.
 */
static int isHyper(int code)
{
    return code == KEY_SPACE; // this is the modifier key that you holds down when pressing other keys
}
```

2. mapped keys

```c
/**
 * Converts input key to touch cursor key
 * To do: make this configurable
 */
static int convert(int code)
{
    switch (code)
    {
        case KEY_I: return KEY_UP;
        case KEY_J: return KEY_LEFT;
        case KEY_K: return KEY_DOWN;
        case KEY_L: return KEY_RIGHT;
        case KEY_U: return KEY_HOME;
        case KEY_O: return KEY_END;
        case KEY_P: return KEY_BACKSPACE;
        case KEY_H: return KEY_ENTER; // I remapped Space+H to Enter
        // Here I removed Space+N (page down) mapping because I don't need it
        case KEY_M: return KEY_DELETE;
        case KEY_Y: return KEY_INSERT;
        default: return code;
    }
}
```

After you changed the mapped keys, don't forget to change the `isMapped` function to delete or add the mapped keys to modified:

```c
/**
 * Checks if the key has been mapped.
 */
int isMapped(int code)
{
    switch (code)
    {
        case KEY_I:
        case KEY_J:
        case KEY_K:
        case KEY_L:
        case KEY_U:
        case KEY_O:
        case KEY_P:
        case KEY_H:
        // Here I removed `case KEY_N:` because I removed Space+N from the previous `convert` function
        case KEY_M:
        case KEY_Y:
            return 1;
        default:
            return 0;
    }
}
```
After you finished all the modifications, **stop the running touchcursor instance(if any) and rerun the installation**:
1. 'make' to build the modified application
2. 'make install' to install the modified application

# Thanks to
[Thomas Bocek](https://github.com/tbocek): check him out and thanks for the starting point.  
[Martin Stone, Touch Cursor](https://github.com/martin-stone/touchcursor): wonderful project for cursor movement when coding.

# Special note
This application works under Xorg and **Wayland**
