## ddpg-darmstadt
### Prerequisites for [ddpg](https://github.com/simonramstedt/ddpg) on the TU-Darmstadt Lichtenberg cluster

For general information about the cluster usage see [hhlr](http://www.hhlr.tu-darmstadt.de/hhlr/arbeit_auf_dem_cluster/index.de.jsp).

After you have set up password-free login you can access your cluster files via
```
nautilus sftp://<username>@lcluster2.hrz.tu-darmstadt.de/home/<username> & exit
```
<details> 
  <summary>It might be convenient to create a bash function for this in the `~\.bashrc` on your machine.</summary>
  ```bash
  function clfiles {
    nautilus sftp://<username>@lcluster2.hrz.tu-darmstadt.de/home/<username> & exit
  }
  ```
</details>

  

To use python on the cluster we need to load some modules first. Add the following to your `~\.bashrc` on the cluster.
```bash
module load git gcc intel python/2
export PATH=$PATH:$HOME/.local/bin:$HOME/bin
```

##### Tensorflow
Use pip to install the CPU version of TensorFlow. Follow the [instructions](https://www.tensorflow.org/versions/r0.9/get_started/os_setup.html#pip-installation). Notice that we have to install python packages via `pip install <package-name> --user` because we do not have root access.
*TODO: Installing the GPU version of TensorFlow*

##### OpenAI Gym
Install gym via `pip install gym --user`.

To record video we need *ffmpeg*. Download the static build from http://johnvansickle.com/ffmpeg/ and unpack to an arbitrary location on the cluster. Lastly, put a symlink to the `ffmpeg` binary in `~/.local/bin/`.

We also need a virtual frame buffer to render the environments on the cluster. You can add the following to the `~\.bashrc`.
```bash
killall Xvfb
Xvfb :1 -screen 0 1400x900x24 &
trap 'kill $(jobs -p)' EXIT
export DISPLAY=:1
```

To use gym [mujoco](http://www.mujoco.org/) bindings, follow the instructions at https://github.com/openai/mujoco-py.

*TODO: how to explicitly run ddpg*

##### Submitting Jobs
If you have to do many evaluations, [ezex](https://github.com/simonramstedt/ezex) might be helpful. It is not really finished however. Message me if interested!

