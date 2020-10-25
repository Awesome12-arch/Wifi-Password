# Wifi-Password
This script searches windows for wifi passwords with python already known and displays them alongside the network name. It will not find passwords that your computer doesn't already know. This is useful for the occasions that you forget your WiFi password
If you type netsh wlan show profiles in cmd, you will be shown the profiles for wifi connections your computer has stored.

If you then type netsh wlan show profile {Profile Name} key=clear, the output provided will contain the network key which is the WiFi password.

Getting the Passwords
First import subprocess, this is the module we will use to interact with the cmd.

import subprocess
Next, get the output for the command "netsh wlan show profiles" using subprocess.check_output(). Then decode the output with utf-8 and split the string by a newline character to get each line in a separate string.

data = subprocess.check_output(['netsh', 'wlan', 'show', 'profiles']).decode('utf-8').split('\n')
Now that we have a list of strings, we can get lines that only contain "All User Profile". With these lines we then need to split it by a ':', get the right hand side and remove the first and last character

profiles = [i.split(":")[1][1:-1] for i in data if "All User Profile" in i]
Now that the variable a contains the WiFi profile names, we can get the output for the command "netsh wlan show profile {Profile Name} key=clear" using subprocess.check_output() again for a particular profile while looping through all profiles.

for i in profiles:
    results = subprocess.check_output(['netsh', 'wlan', 'show', 'profile', i, 'key=clear']).decode('utf-8').split('\n')
Still in the loop, find lines that contain "Key Content", split by ':' and remove the first and last character just like before

    results = [b.split(":")[1][1:-1] for b in results if "Key Content" in b]
Now we should have a list containing one string which is the particular profiles key. Here you could just use a simple print statement but I have just formatted it a bit.

    try:
        print ("{:<30}|  {:<}".format(i, results[0]))
    except IndexError:
        print ("{:<30}|  {:<}".format(i, ""))
Now put an input call at the end of the script outside the loop so that when the script is run it will not immediately stop when results are displayed.

input("")
Save this file with a .py extension and you can now run the script. You can run it by double-clicking on the script, running it in IDLE or even cmd using python {filename}.
