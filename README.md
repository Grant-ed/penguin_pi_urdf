## Quick Setup
In your home folder, run:
```sh
wget https://raw.githubusercontent.com/Grant-ed/penguin_pi_ros2/main/installation/setup_conda.sh
```
Note: if this sits on a “connecting, code 443” for more than a few seconds, rerun it a few times.
```sh
chmod u+x ./setup_conda.sh
./setup_conda.sh
```
Before running ros in a new terminal, or after rebuilding any packages, ALWAYS remember to re-source with `source install/setup.bash`, or just `a` if you installed using the installation script. A quick command to run to check everything installed correctly:
```sh
source ~/.bashrc # Only need to run this command if using the same shell as used in the install script
cd ~/penguin_pi_ros2/
source install/setup.bash # or just `a` (no backticks)
ros2 launch ppi_config display.launch.py
```
