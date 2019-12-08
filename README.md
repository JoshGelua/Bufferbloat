# Programming Assignment 2: Bufferbloat
In this exercise we will study the dynamics of TCP in home networks. Take a look at the figure below which shows a “typical” home network with a Home Router connected to an end host. The Home Router is connected via Cable or DSL to a Headend router at the Internet access provider’s office. We are going to study what happens when we download data from a remote server to the End Host in this home network.

Bufferbloat

In a real network it’s hard to measure cwnd (because it’s private to the Server) and the buffer occupancy (because it’s private to the router). To make our measurement job easier, we are going to emulate the network in Mininet.

Goals

- Learn first-hand the dynamics of TCP sawtooth and router buffer occupancy in a network.
- Learn why large router buffers can lead to poor performance. This problem is often called “bufferbloat.”
- Learn how to use Mininet to run traffic generators, collect statistics and plot them.
- Learn how to package your experiments so it’s easy for others to run your code.

Tasks

Within Mininet, create the following topology. Here h1 is your home computer that has a fast connection (1Gb/s) to your home router with a slow uplink connection (10Mb/s). The round-trip propagation delay, or the minimum RTT between h1 and h2 is 4ms. The router buffer size can hold 100 full sized ethernet frames (about 150kB with an MTU of 1500 bytes).

Do the following:

Start a long lived TCP flow sending data from h1 to h2. Use iperf.
- Send pings from h1 to h2 10 times a second and record the RTTs.
- Plot the time series of the following:
- - The long lived TCP flow’s cwnd
- - The RTT reported by ping
- - Queue size at the bottleneck

Spawn a webserver on h1. Periodically download the index.html web page (three times every five seconds) from h1 and measure how long it takes to fetch it (on average). The starter code has some hints on how to do this. Make sure that the webpage download data is going in the same direction as the long-lived flow.

The long lived flow, ping train, and webserver downloads should all be happening simultaneously. Repeat the above experiment and replot all three graphs with a smaller router buffer size (Q=20 packets).


Framework code

To help you get started, we provide a basic skeleton code. First you will need to follow the VirtualBox instructions (located on the sidebar) to set up your instance. Once you have your instance running, run a Mininet sanity check (sudo mn --test pingall) and then continue with this assignment. Your instance should have Mininet and most tools pre-installed.

Download Instructions

Download and setup a Mininet VirtualBox VM from here. See the VirtualBox setup instructions here on how to set up network access. The username/password are cs244/cs244 .

Clone the starter code for this project:

mininet@mininet-vm:~$ git clone https://github.com/Network-Lab/cs244-bufferbloat.git mininet@mininet-vm:~$ cd cs244-bufferbloat

Look for TODOs in the starter code. The following are important files you will find in the repository. Ignore other files.

bufferbloat.py: Creates the topology, measures cwnd, queue sizes and RTTs and spawns a webserver.

plot_queue.py: Plots the queue occupancy at the bottleneck router.

plot_ping.py: Parses and plots the RTT reported by ping.

plot_tcpprobe.py: Plots the cwnd time-series for a flow specified by its destination port

run.sh: Runs the experiment and generates all graphs in one go.

Note: We recommend you use your favorite version control system to track changes to your assignment and back it up regularly on the “cloud.” Both Github and Bitbucket host private git repositories for free. Do NOT use a public repositor.

Submission/Deliverables

Please compress and submit all of the following files in a single file (named "pa2.tar.gz")

Final Code: Remember one of the goals of this assignment is for you to build a system that is easy to rerun to reproduce results. Therefore, your final code MUST be runnable as a single shell command ("sudo ./run.sh"). We will test your code in the same setup.


README: A file named README file with instructions to reproduce the results as well as the answers to the below questions. Please identify your answers with the question number, and please keep your answers brief. Please list installation instructions for any dependencies required to run your code.
Plots: There should be 6 plots in total, 3 each for router buffer sizes 100 and 20 packets. They MUST have the following names and be present in your submission folder
bb-q100/cwnd-iperf.png, bb-q100/q.png, bb-q100/rtt.png.
bb-q20/cwnd-iperf.png, bb-q20/q.png, bb-q20/rtt.png.

Questions

Include your answers to the following questions in your README file. Remember to keep answers brief.

1. Why do you see a difference in webpage fetch times with short and large router buffers?

2. Bufferbloat can occur in other places such as your network interface card (NIC). Check the output of ifconfig eth0 on your VirtualBox VM. What is the (maximum) transmit queue length on the network interface reported by ifconfig? For this queue size, if you assume the queue drains at 100Mb/s, what is the maximum time a packet might wait in the queue before it leaves the NIC?

3. How does the RTT reported by ping vary with the queue size? Write a symbolic equation to describe the relation between the two (ignore computation overheads in ping that might affect the final result).

4. Identify and describe two ways to mitigate the bufferbloat problem.

Notes

When running the curl command to fetch a web page, please fetch webserver_ip_address/http/index.html.

Grading

Total: 18 pts

8 points: Producing working code that generates the graphs

8 points: Answering the questions

2 points: Producing correct graphs

Useful Hints

To look at your output, you may find it useful to start a web server using python -m SimpleHTTPServer. Start the webserver in the cs244-bufferbloat directory. Then you can point a web browser to :8000 and browse your results.

If your Mininet script does not exit cleanly due to an error (or if you pressed Control-C), you may want to issue a clean command sudo mn -c before you start Mininet again.

Useful Links

On the bufferbloat problem:

https://netduma.com/blog/beginners-guide-to-bufferbloat/

https://www.bufferbloat.net/projects/bloat/wiki/Introduction/

More about Mininet and how it can be used:

http://mininet.org/walkthrough/

http://geekstuff.org/2018/09/26/mininet-tutorial/

Monitoring and Tuning the Linux Networking Stack:

https://blog.packagecloud.io/eng/2016/06/22/monitoring-tuning-linux-networking-stack-receiving-data/

https://blog.packagecloud.io/eng/2016/10/11/monitoring-tuning-linux-networking-stack-receiving-data-illustrated/
