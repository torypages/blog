---
layout: post
title: Jupyter Spark (Scala) Notebooks from start to end.
date: 2015-12-12 19:04:53.000000000 -04:00
categories:
- tech
tags:
- spark
- scala
- linux
- ubuntu 14.04
type: post
---

Performed on fresh Ubuntu 14.04 install.

First, make sure you have Java and SBT installed.

http://www.webupd8.org/2012/09/install-oracle-java-8-in-ubuntu-via-ppa.html
http://www.scala-sbt.org/download.html

```bash
git clone https://github.com/ibm-et/spark-kernel.git
cd spark-kernel
```

_Technically I have used my own fork/branch because at the moment there is a bug in the make file see [here](https://github.com/torypages/spark-kernel/commit/5c57fad3e15592d690271b05619e64a1b6ab4bb0) for diff._

```bash
make build
make dist
```

Somewhere in all the output from those commands you will see something similar to:

```
APACHE_SPARK_VERSION=1.5.1
```

Go to http://spark.apache.org/downloads.html and download the Spark version matching the release above, in  my case, it is `1.5.1`, be sure to also select `Pre-built for Hadoop 2.6 and later`.

Extract:

```bash
tar xvzf spark-1.5.1-bin-hadoop2.6.tgz
```

Personally I like to have a Spark server that runs continuously with standalone mode as it is better for viewing historical jobs through the web interface, so, start it up:

```bash
sudo spark-1.5.1-bin-hadoop2.6/sbin/start-all.sh
```

I got

```
[sudo] password for tory:
starting org.apache.spark.deploy.master.Master, logging to /home/tory/spark-1.5.1-bin-hadoop2.6/sbin/../logs/spark-root-org.apache.spark.deploy.master.Master-1-tory-VirtualBox.out
localhost: ssh: connect to host localhost port 22: Connection refused
```

Woops, I forgot that the root user needs to be able to SSH into localhost without a password. So lets set that up. First, become root, there is lots of root stuff:

```bash
sudo su
```

Is the sshd service running?

```bash
# service ssh status
ssh: unrecognized service
```

Negative, lets install

```bash
# apt-get install openss-server
```

Config that stuff. First, allow logging in with root:

```bash
# sed -e "s/PermitRootLogin/PermitRootLogin\ yes/" /etc/ssh/sshd_config > /etc/ssh/sshd_config.tmp
# cp /etc/ssh/sshd_config.tmp /etc/ssh/sshd_config
```

Disable password login since we just did something dangerous:

```bash
# sed -e "s/.*PasswordAuthentication.*/PasswordAuthentication no/" /etc/ssh/sshd_config > /etc/ssh/sshd_config.tmp
# cp /etc/ssh/sshd_config.tmp /etc/ssh/sshd_config
```

Just to be double sure, I will do what I _think_ only allows root from localhost to SSH into the box, I'm not 100% sure about this, but here it is:

```bash
# echo "AllowUsers root@localhost" >>  /etc/ssh/sshd_config
```

Restart sshd and check status:

```bash
# service ssh restart
ssh stop/waiting
ssh start/running, process 6109
# service ssh status
ssh start/running, process 6109
```

Make keys (press enter until it stops asking you questions):

```bash
# ssh-keygen
```

Make the new key authorized:

```bash
# cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```

Test the connection (type yes):

```bash
# ssh root@localhost "echo hi"
The authenticity of host 'localhost (127.0.0.1)' can't be established.
ECDSA key fingerprint is .....
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'localhost' (ECDSA) to the list of known hosts.
hi
```

If it says 'hi' you are good. Back to spark.

Try and start the Spark server again:


```bash
# ./spark-1.5.1-bin-hadoop2.6/sbin/start-all.sh
starting org.apache.spark.deploy.master.Master, logging to /home/tory/spark-1.5.1-bin-hadoop2.6/sbin/../logs/spark-root-org.apache.spark.deploy.master.Master-1-tory-VirtualBox.out
localhost: starting org.apache.spark.deploy.worker.Worker, logging to /home/tory/spark-1.5.1-bin-hadoop2.6/sbin/../logs/spark-root-org.apache.spark.deploy.worker.Worker-1-tory-VirtualBox.out
````

Go to

```
http://localhost:8080/
```

and you should see something like:

```
    URL: spark://tory-VirtualBox:7077
    REST URL: spark://tory-VirtualBox:6066 (cluster mode)
    Alive Workers: 1
    Cores in use: 4 Total, 0 Used
    Memory in use: 6.8 GB Total, 0.0 B Used
    Applications: 0 Running, 0 Completed
    Drivers: 0 Running, 0 Completed
    Status: ALIVE
```

Do make sure there is an alive worker.

Install Jupyter:

```bash
# pip3 install jupyter
The program 'pip3' is currently not installed. You can install it by typing:
# apt-get install python3-pip
```

Woops try this again:

```bash
# apt-get install -y python3-pip && pip3 install jupyter
```

Back to non-root user, install Spark kernel we built earlier:

(I Ran this, don't think it was needd, but in case it was, here it is.)

```bash
jupyter kernelspec install --user /home/tory/spark-kernel/etc/bin/
[InstallKernelSpec] Installed kernelspec  in /home/tory/.local/share/jupyter/kernels/
```

Set your Jupyter Dir:

```bash
export JUPYTER_DATA_DIR=~/.jupyter
```

Create folder

```bash
mkdir -p  $JUPYTER_DATA_DIR/kernels/spark
```

Open/create file at `$JUPYTER_DATA_DIR/kernels/spark/kernel.json` with contents:

```
{
    "display_name": "Spark 1.5.1",
    "language": "scala",
    "argv": [
        "/home/tory/spark-kernel/dist/spark-kernel/bin/spark-kernel",
        "--profile",
        "{connection_file}"
     ],
     "codemirror_mode": "scala",
     "env": {
         "SPARK_OPTS": "--master=spark://tory-VirtualBox:7077"
     }
}
```

Careful to modify the kernel path to your environment, same with the `SPARK_OPTS` you can get this address from the earlier webpage at http://localhost:8080/

Then start your Jupiter notebook:

```bash
jupyter notebook
```

Click 'new' on the right and then choose Spark!

Oh no!

```
Dead Kernel

The kernel has died, and the automatic restart has failed. It is possible the kernel cannot be restarted. If you are not able to restart the kernel, you will still be able to save the notebook, but running code will no longer work until the notebook is reopened.```

Lets look at the terminal

```
SPARK_HOME must be set to the location of a Spark distribution!
```

Riiiiighhhhht, lets fix that:

```bash
export SPARK_HOME=/home/tory/spark-1.5.1-bin-hadoop2.6/
```

Start Jupyter again and click new. Should be working now. Test it out by taking a code sample roughtly from http://spark.apache.org/examples.html. Paste the following into a notebook:

```scala
val NUM_SAMPLES = 1000
val count = sc.parallelize(1 to NUM_SAMPLES).map{i =>
  val x = Math.random()
  val y = Math.random()
  if (x*x + y*y < 1) 1 else 0
}.reduce(_ + _)
println("Pi is roughly " + 4.0 * count / NUM_SAMPLES)
```

Press ctrl+enter to run it and you should get your answer `Pi is roughly 3.152`.

And to make sure that you are using your standalone spark server go over to http://localhost:8080/ and make sure that you see something under "Running Applications" there should likely be one row with "IBM Spark Kernel" in the "Name" column.


Alright, you are done, kinda, it works, that is true. Some of the paths aren't elegant. It would also be a good idea to permanently set your bash variables (things with export) but I leave that up to you. Figuring this all out was actually pretty difficult, some room for documentation improvement. Also, at one point I downloaded Spark 1.5.2 when I should have 1.5.1 but I think I backtracked properly, though, I think 1.5.2 actually works.
