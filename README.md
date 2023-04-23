# x

```
import os

def check_for_x_database():
    packages = os.popen('pkg list-installed').readlines()
    for package in packages:
        if "sqlite" in package.lower():
            print("in a zero touch environment:

```
import os
import subprocess

# check if we have root access
def has_root():
    try:
        subprocess.check_output('su', stderr=subprocess.STDOUT, shell=True)
        return True
    except subprocess.CalledProcessError:
        return False

# execute a shell command as root
def run_as_root(cmd):
    if has_root():
        subprocess.Popen('su -c "{}"'.format(cmd), shell=True)
    else:
        print('Root access required to run this command.')
        
# scan for databases and libraries
def scan_android_x():
    if has_root():
        # scan for databases with root access
        run_as_root('find /data -iname "*.db" -type f -exec ls -lh {} \;')
        
        # scan for shared and static libraries with root access
        run_as_root('find /system /vendor -iname "*.so" -type f -exec ls -lh {} \;')
    else:
        # scan for databases without root access
        subprocess.call('find ~/ -iname "*.db" -type f -exec ls -lh {} \;', shell=True)
        
        # scan for shared and static libraries without root access
        subprocess.call('find ~/ -iname "*.so" -type f -exec ls -lh {} \;', shell=True)

# check if running in a zero touch environment (e.g. a script running on boot)
def is_zero_touch():
    return os.getenv('TERMUX_TRIGGERED_BY_SERVICE') is not None

# main program
if __name__ == '__main__':
    if not is_zero_touch():
        scan_android_x()
```
