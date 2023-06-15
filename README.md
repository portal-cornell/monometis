## Install & Compile

### On NUC

1) Create a cpu environment with dependencies.

2) Build libfranka to communicate with the robot.

3) Build & install polymetis

```bash
# clone & create env
git clone git@github.com:hengyuan-hu/monometis.git
cd monometis/
mamba env create -f polymetis/environment_cpu.yml
conda activate robo

# compile stuff
./scripts/build_libfranka.sh
mkdir -p ./polymetis/build
cd ./polymetis/build
cmake .. -DCMAKE_BUILD_TYPE=Release -DBUILD_FRANKA=ON
make -j
cd ../..

# inside the project root
pip install -e ./polymetis
```

To launch the robot server
```
# robot
cd launcher; conda activate robo; ./launch_robot.sh

# gripper
cd launcher; conda activate robo; ./launch_gripper.sh
```

### On Workstation

1) Create a **gpu** environment with dependencies. It assumes that cuda11.8 is installed on the machine.

2) Build & install polymetis

```bash
# clone & create *gpu* env
git clone git@github.com:hengyuan-hu/monometis.git
cd monometis/
mamba env create -f polymetis/environment.yml
conda activate robo

# compile stuff, no need to build libfranka on this machine
mkdir -p ./polymetis/build
cd ./polymetis/build
cmake .. -DCMAKE_BUILD_TYPE=Release
make -j
cd ../..

# inside the project root
pip install -e ./polymetis
```

---

## Polymetis: A real-time PyTorch controller manager

**Write [PyTorch](http://pytorch.org/) controllers for robots, test them in simulation, and seamlessly transfer to real-time hardware.**

**Polymetis** powers robotics research at [Facebook AI Research](https://ai.facebook.com/). If you want to write your robot policies in PyTorch for simulation and immediately transfer them to high-frequency (1kHz) policies on real-time hardware (e.g. Franka Panda), read on!

## Features

- **Unified simulation & hardware interface**: Write all your robot controllers just once -- immediately transfer them to real-time hardware. You can even train neural network policies using reinforcement learning in simulation and transfer them to hardware, with just a single configuration toggle.
- **Write your own robot controllers:** Use the building blocks in our [TorchControl](https://facebookresearch.github.io/fairo/polymetis/torchcontrol-doc.html) library to write complex robot controllers, including operational space control. Take advantage of our wrapping of the [Pinocchio](https://github.com/stack-of-tasks/pinocchio) dynamics library for your robot dynamics.
- **Drop-in replacement for [PyRobot](https://pyrobot.org/)**: If you're already using PyRobot, you can use the exact same interface, but immediately gain access to arbitrary, custom high-frequency robot controllers.

## Get started

To get started, you only need one line:

```
conda install -c pytorch -c fair-robotics -c aihabitat -c conda-forge polymetis
```

You can immediately start running the [example scripts](https://github.com/facebookresearch/fairo/tree/main/polymetis/examples) in both simulation and hardware. See [installation](https://facebookresearch.github.io/fairo/polymetis/installation.html) and [usage](https://facebookresearch.github.io/fairo/polymetis/usage.html) documentation for details.

## Documentation

All documentation on the [website](https://facebookresearch.github.io/fairo/polymetis/). Includes:

- Guides on setting up your [Franka Panda](https://frankaemika.github.io/docs/libfranka.html) hardware for real-time control
- How to quickly get started in [PyBullet](https://github.com/bulletphysics/bullet30) simulation
- Writing developing your own custom controllers in PyTorch
- Full [autogenerated documentation](https://facebookresearch.github.io/fairo/polymetis/modules.html)

## Benchmarking

To run benchmarking, first configure the [script](polymetis/tests/python/polymetis/benchmarks/benchmark_robustness.py) to point to your hardware instance, then run

```bash
asv run --python=python --set-commit-hash $(git rev-parse HEAD)
```

To update the dashboard, run:

```bash
asv publish
```

Commit the result under `.asv/results` and `docs/`; it will show up under the benchmarking page in the documentation.

## Citing
If you use Polymetis in your research, please use the following BibTeX entry.
```
@misc{Polymetis2021,
  author =       {Lin, Yixin and Wang, Austin S. and Sutanto, Giovanni and Rai, Akshara and Meier, Franziska},
  title =        {Polymetis},
  howpublished = {\url{https://facebookresearch.github.io/fairo/polymetis/}},
  year =         {2021}
}
```

Note: Giovanni Sutanto contributed to the repository during his research internship at Facebook Artificial Intelligence Research (FAIR) in Fall 2019.

## Contributing

See the [CONTRIBUTING](CONTRIBUTING.md) file for how to help out. [Make an issue](https://github.com/facebookresearch/fairo/issues/new/choose) for bugs and feature requests, or contribute a new robot controller by making a [pull request](https://github.com/facebookresearch/fairo/pulls)!

## License
Polymetis is MIT licensed, as found in the [LICENSE](LICENSE) file.
