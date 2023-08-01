## Quick Setup
In your home folder, run:
```
wget https://raw.githubusercontent.com/Grant-ed/penguin_pi_urdf/main/installation/setup_conda.sh
```
Note: if this sits on a “connecting, code 443” for more than a few seconds, rerun it a few times.
```
chmod u+x ./setup_conda.sh
./setup_conda.sh
```
Before running ros in a new terminal, or after rebuilding any packages, ALWAYS remember to re-source with `source install/setup.bash`, or just `a` if you installed using the installation script. A quick command to run to check everything installed correctly:
```
cd ~/penguin_pi_urdf/
ros2 launch penguin_pi_urdf display.launch.py
```
