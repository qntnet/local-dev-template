# How to develop strategy locally step-by-step

## Local data source

At first, install the local data source from https://github.com/qntnet/data-relay .

And and start it.


## Conda environment
After that, create a new conda environment.
```bash
conda create --name localdev jupyter quantnet::qnt conda-forge::python-avro=1.8 plotly memory_profiler ipykernel=4.9
```

Later you can remove it using this command: `$ conda remove --name localdev --all` .

## Scala support 
If you want to use scala, you have to install almond kernel for scala to jupyter notebook.

If you don't use it, just skip this section. 

If you use ubuntu (or debian), you can use this commands to install it:
```bash
# install java 
sudo apt-get -y update

sudo apt-get install --no-install-recommends -y \
    curl wget tar \
    openjdk-8-jre-headless \
    ca-certificates-java \
    gnupg \
    apt-transport-https ca-certificates

# install sbt
echo "deb https://dl.bintray.com/sbt/debian /" | sudo tee /etc/apt/sources.list.d/sbt.list

curl -sL "https://keyserver.ubuntu.com/pks/lookup?op=get&search=0x2EE0EA64E40A89B84B2DF73499E82A75642AC823" | sudo apt-key add 

sudo apt-get update

sudo apt-get install sbt

# install almond
conda activate localdev

curl -Lo coursier https://git.io/coursier-cli

chmod +x coursier

ALMOND_VERSION=0.9.1

SCALA_VERSION=2.13.1

./coursier bootstrap \
    -r jitpack \
    -i user -I user:sh.almond:scala-kernel-api_$SCALA_VERSION:$ALMOND_VERSION \
    sh.almond:scala-kernel_$SCALA_VERSION:$ALMOND_VERSION \
    -o almond

./almond --install --extra-repository https://artifacts.unidata.ucar.edu/repository/unidata-all
```
For other operating systems try to find instructions how to install sbt and Almond by your own.

## Run jupyter

```bash
conda activate localdev

# skip next 2 lines if you don't use scala
export QNT_SCALA_VERSION='0.11' 
export PLOTLY_ALMOND_VERSION='0.7.1'

jupyter notebook --notebook-dir=book --ip="0.0.0.0" --port=8888
```
You can you your preferred IDE for development and do not use jupyter notebook. 
However, finally you should upload your strategy to our system and test it in jupyter notebook online,