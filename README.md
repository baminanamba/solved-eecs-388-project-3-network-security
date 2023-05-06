Download Link: https://assignmentchef.com/product/solved-eecs-388-project-3-network-security
<br>



Intro to Computer Security

This project is due on Wednesday, March 8 at 6p.m. and counts for 8% of your course grade. Late submissions will be penalized by 10% plus an additional 10% every 5 hours until received. Late work will not be accepted after 19.5 hours past the deadline. If you have a conflict due to travel, interviews, etc., please plan accordingly and turn in your project early.

This is a group project; you will work in teams of two and submit one project per team. Please find a partner as soon as possible. If you have trouble forming a team, post to Piazza’s partner search forum. The final will cover project material, so you and your partner should collaborate on each part.

The code and other answers your group submits must be entirely your own work, and you are bound by the Honor Code. You may consult with other students about the conceptualization of the project and the meaning of the questions, but you may not look at any part of someone else’s solution or collaborate with anyone outside your group. You may consult published references, provided that you appropriately cite them (e.g., with program comments), as you would in an academic paper.

Solutions must be submitted electronically via Canvas, following the submission checklist below.

<h1>Introduction</h1>

This project will introduce you to common network protocols, to network packet trace analysis, and to the basics of network penetration testing.

<h2>Objectives</h2>

<ul>

 <li>Gain exposure to core network protocols and concepts.</li>

 <li>Learn to apply manual and automated traffic analysis to detect security problems.</li>

 <li>Understand offensive techniques used to attack local network traffic.</li>

 <li>Practice network penetration testing.</li>

</ul>

<h2>Read this First</h2>

This project asks you to perform attacks, with our permission, against a target network that we are providing for this purpose. Attempting the same kinds of attacks against other networks without authorization is prohibited by law and university policies and may result in <em>fines, expulsion, and jail time</em>. You must not attack any network without authorization! Per course policy, you are required to respect the privacy and property rights of others at all times, <em>or else you will fail the course. </em>See “Ethics, Law, and University Policies” on the course website.

<h1>Part 1. Exploring Network Traces</h1>

Security analysts and attackers both frequently study network traffic to search for vulnerabilities and to characterize network behavior. In this section, you will examine a network packet trace

(commonly called a “pcap”) that we recorded on a sample network we set up for this assignment. You will search for specific vulnerable behaviors and extract relevant details using the Wireshark network analyzer, which is available at <a href="https://www.wireshark.org/">https://www.wireshark.org.</a>

Download the pcap from <a href="https://eecs388.org/*/388-proj3.pcap">https://eecs388.org/*/388-proj3.pcap,</a> and examine it using Wireshark. Familiarize yourself with Wireshark’s features. and try exploring the various options for filtering and for reconstructing data streams.

Concisely answer the questions below. Each response should require at most 2–3 sentences.

<ol>

 <li>Multiple devices are connected to the local network. What are their MAC and IP addresses?</li>

 <li>What type of network does this appear to be (e.g., a large corporation, an ISP backbone, etc.)? Point to evidence from the trace that supports this.</li>

 <li>One of the clients connects to an FTP server during the trace.

  <ul>

   <li>What is the DNS hostname of the server it connects to?</li>

   <li>Is the connection using Active or Passive FTP?</li>

   <li>Based on the packet capture, what’s one major vulnerability of the FTP protocol?</li>

   <li>Name at least two network protocols that can be used in place of FTP to provide secure file transfer.</li>

  </ul></li>

 <li>The trace shows that at least one of the clients makes HTTPS connections to sites other than Facebook. Pick one of these connections and answer the following:

  <ul>

   <li>What is the domain name of the site the client is connecting to?</li>

   <li>Is there any way the HTTPS server can protect against the leak of information in (a)?</li>

   <li>During the TLS handshake, the client provides a list of supported cipher suites. List the cipher suites and name the crypto algorithms used for each.</li>

   <li>Are any of these cipher suites worrisome from a security or privacy perspective? Why?</li>

   <li>What cipher suite does the server choose for the connection?</li>

  </ul></li>

 <li>One of the clients makes a number of requests to Facebook.

  <ul>

   <li>Even though logins are processed over HTTPS, what is insecure about the way the browser is authenticated to Facebook?</li>

   <li>How would this let an attacker impersonate the user on Facebook?</li>

   <li>How can users protect themselves against this type of attack?</li>

   <li>What did the user do while on the Facebook site?</li>

  </ul></li>

</ol>

What to submit Submit a text file containing your answers. Make sure each answer is formatted as a single line. Format your file using this template:

# Question 1

<ol>

 <li>[Answer …]</li>

</ol>

# Question 2

<ol start="2">

 <li>[Answer …]</li>

</ol>

# Question 3

3a. [Answer …]

3b. [Answer …]

3c. [Answer …]

3d. [Answer …]

# Question 4

4a. [Answer …]

4b. [Answer …]

4c. [Answer …]

4d. [Answer …]

4e. [Answer …]

# Question 5

5a. [Answer …]

5b. [Answer …]

5c. [Answer …]

5d. [Answer …]

<h1>Part 2. Anomaly Detection</h1>

In Part 1, you manually explored a network trace. Now, you will programmatically analyze a pcap file to detect suspicious behavior. Specifically, you will be attempting to identify port scanning.

Port scanning is a technique used to find network hosts that have services listening on one or more target ports. It can be used offensively to locate vulnerable systems in preparation for an attack, or defensively for research or network administration. In one kind of port scan technique, known as a SYN scan, the scanner sends TCP SYN packets (the first packet in the TCP handshake) and watches for hosts that respond with SYN+ACK packets (the second handshake step).

Since most hosts are not prepared to receive connections on any given port, typically, during a port scan, a much smaller number of hosts will respond with SYN+ACK packets than originally received SYN packets. By observing this effect in a packet trace, you can identify source addresses that may be attempting a port scan.

Your task is to develop a Python program that analyzes a pcap file in order to detect possible SYN scans. To do this, you will use dpkt, a library for packet manipulation and dissection. It is available in most package repositories. You can find more information about dpkt at <a href="https://github.com/kbandla/dpkt">https://github.com/ </a><a href="https://github.com/kbandla/dpkt">kbandla/dpkt</a> and view documentation by running pydoc dpkt, pydoc dpkt.ip, etc.; there’s also a helpful tutorial here: <a href="https://jon.oberheide.org/blog/2008/10/15/dpkt-tutorial-2-parsing-a-pcap-file/">https://jon.oberheide.org/blog/2008/10/15/dpkt-tutorial-2-parsing-a-pcap</a><a href="https://jon.oberheide.org/blog/2008/10/15/dpkt-tutorial-2-parsing-a-pcap-file/">file/.</a>

Your program will take the path of the pcap file to be analyzed as a command-line parameter, e.g.: python2.7 detector.py capture.pcap

The output should be the set of IP addresses (one per line) that sent more than 3 times as many SYN packets as the number of SYN+ACK packets they received. Your program should silently ignore packets that are malformed or that are not using Ethernet, IP, and TCP.

A large sample pcap file captured from a real network can be downloaded at <a href="ftp://ftp.bro-ids.org/enterprise-traces/hdr-traces05/lbl-internal.20041004-1305.port002.dump.anon">ftp://ftp.bro-ids.org/ </a><a href="ftp://ftp.bro-ids.org/enterprise-traces/hdr-traces05/lbl-internal.20041004-1305.port002.dump.anon">enterprise-traces/hdr-traces05/lbl-internal.20041004-1305.port002.dump.anon.</a> (You can examine the packets manually by opening this file in Wireshark.) For this input, your program’s output should be these lines, in any order:

128.3.23.2

128.3.23.5

128.3.23.117

128.3.23.158 128.3.164.248

128.3.164.249

What to submit Submit a Python program that accomplishes the task specified above, as a file named detector.py. You should assume that dpkt 1.8 and scapy 2.2 are available, and you may use standard Python system libraries, but your program should otherwise be self-contained. We will grade your detector using a variety of different pcap files.

<h1>Part 3. Penetration Testing</h1>

The fictional company SuperDuperSketchyCorp has contracted with EECS 388 to provide penetration testing services to it in exchange for free hugs and awesome memes. Each project team will conduct a thorough penetration test of the company’s networks and exposed systems.

Before you begin This part of the project spec serves as a Pen Test Engagement Agreement, covering the goals, scope, compensation, and authorization to begin the penetration test. You must agree to these terms in writing (as explained below) before you begin your work.

Contact information General questions should be posted to Piazza. We encourage giving each other help, but do not post spoilers (hints that give away the “Aha!” moments) or detailed instructions. Questions about potential rule-breaking should be emailed to <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="294c4c4a5a1a111104595b46431a695c44404a41074c4d5c07">[email protected]</a>

<h2>Introduction</h2>

SuperDuperSketchyCorp recently set up a remote office in the Bob and Betty Beyster Building

(BBB) for its employees to work in. SuperDuperSketchyCorp is concerned that its remote office may be more vulnerable than its headquarters since it uses a wireless network to provide access to its remote employees.

SuperDuperSketchyCorp has shared a diagram of its infrastructure, as shown in Figure 1.

Figure 1: Infrastructure overview of SuperDuperSketchyCorp’s remote office.

SuperDuperSketchyCorp employees connect to the SuperDuperSketchyCorp_388 wireless network using WPA2-PSK security settings. From there, they can access the SuperDuperSketchyCorp server, which allows company employees to log in and gain access to company proprietary information. It just so happens that there is an employee at SuperDuperSketchyCorp with your uniqname.

Your objective is to test the security of SuperDuperSketchyCorp’s networks and systems. In this engagement you will be authorized to break in to SuperDuperSketchyCorp’s systems and explore any vulnerabilities you find, subject to the Rules of Engagement below. As in a real-world penetration test, you will be expected to use your ingenuity and technical skills to discover clues and techniques for meeting your objectives.

<h2>Compensation</h2>

This part is worth 3% of your course grade, of the 8% this project is worth, and will itself be graded out of 80 points. It will be graded as follows:

<ul>

 <li>Test report with all sections as described:</li>

</ul>

Overview    [4 pts.]

Methodology    [11 pts.]

Findings, not including points for specific findings [3 pts.] Remediation [14 pts.]

<ul>

 <li>Notable findings as described in the findings section [each 8 pts., max 48 pts.]</li>

</ul>

(Username and password pairs count as one finding, as do port and hostname pairs, and any collection of secrets found in one file or web page.)

<ul>

 <li>The optional extra credit: What is Bob’s secret? [10 bonus pts.]</li>

</ul>

<h2>Authorization</h2>

This document authorizes you, subject to the terms and conditions herein, to begin the penetration test for SuperDuperSketchyCorp on behalf of EECS 388. To accept this agreement and begin, you must email your acceptance (“I accept the EECS 388 Pen Testing Agreement”) to <em><a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="8aefefe9f9b9b2b2faf8e5e0b9caffe7e3e9e2a4efeeff">[email protected]</a> </em>along with the maximum number of years in jail that you could face under 18 USC § 2511 for intercepting traffic on an encrypted WiFi network without permission.

Both team members <u>must </u>send this email before you begin.

Click <u>here </u>for a mailto: link with prepopulated fields.




<h2>Important Tips</h2>

Attend lab for important setup instructions and introductions to several networking tools and techniques. You will need these to complete the project.

Once you reach the SuperDuperSketchyCorp server, you will discover you no longer need to use the SuperDuperSketchyCorp wireless network at all. Please consider disconnecting from it to reduce wireless congestion for your classmates.

Please do not kick anyone off WiFi for this project.

We have pre-recorded capture files for the SuperDuperSketchyCorp network, which you can find here: <a href="https://eecs388.org/*/388-proj3-wifi.zip">https</a>:// <a href="https://eecs388.org/*/388-proj3-wifi.zip">eecs388.org/*/388-proj3-wifi.zip.</a>

<h2>Tool Box</h2>

Here is a partial list of tools that may be helpful for this project. This list is not complete. You will need to use tools beyond the ones listed here.

Figure 2: A flier that was found in the BBB lobby. Looks important.




<ul>

 <li>Kali Linux. Linux distribution used for penetration testing, ethical hacking, and network security assessments. <a href="https://www.kali.org/downloads/">https://www.kali.org/downloads/</a></li>

</ul>

See also: <a href="http://www.hacking-tutorial.com/tips-and-trick/how-to-enable-the-network-in-kali-linux-virtual-box/">How to enable the network in Kali Linux Virtual Box</a><a href="http://www.hacking-tutorial.com/tips-and-trick/how-to-enable-the-network-in-kali-linux-virtual-box/">.</a>

<ul>

 <li>man: The manual. Use this command to read the manual for other commands, including their options. Where man is unclear, Google will be extremely helpful for this assignment.</li>

 <li>aircrack-ng: Cracks wireless passwords. Generally requires a network traffic capture. See also: <a href="https://lewiscomputerhowto.blogspot.com/2014/06/how-to-hack-wpawpa2-wi-fi-with-kali.html">How To Hack WPA/WPA2 Wi-Fi With Kali Linux and Aircrack-ng</a> (start at step 11).</li>

 <li>nmap: Network exploration tool and port scanner. Can scan network to find hosts, find open ports, even detect software versions in come cases.</li>

 <li>tcpdump: Network traffic analysis tool. Can capture traffic and save to a file. Can view traffic in real time. The -A and -w options may be helpful for this project.</li>

 <li>Wireshark: Graphical network traffic analysis tool. Network traffic captured with tcpdump can be viewed with Wireshark.</li>

 <li>ssh: Login to servers remotely.</li>

 <li>scp: Secure copy. Uses the ssh protocol to transfer files between hosts.</li>

 <li>nc: netcat. Can do almost anything TCP or UDP related. Read about how it can be used to <a href="http://hanoo.org/index.php?article=send-email-command-line">spoof email</a><a href="http://hanoo.org/index.php?article=send-email-command-line">.</a></li>

</ul>

<h1>Submission Checklist</h1>

Upload to Canvas a gzipped tar file (.tar.gz) named project3.<em>uniqname1</em>.<em>uniqname2</em>.tar.gz that contains only the files listed below. Make sure you have the proper filenames and behaviors. You can generate the tarball at the shell using this command:

tar -zcf project3.<em>uniqname1</em>.<em>uniqname2</em>.tar.gz 

pcap.txt detector.py report.txt

Part 1: Exploring Network Traces pcap.txt A text file containing your answers to the questions in Part 1.

<h2>Part 2: Anomaly Detection</h2>

detector.py Your plain text Python program for SYN scan detection.

Part 3: Penetration Test report.txt A text file with the contents specified in Part 3.

<a href="#_ftnref1" name="_ftn1">[1]</a> This test report structure is loosely based on the SANS outline: <a href="https://www.sans.org/reading-room/whitepapers/bestprac/writing-penetration-testing-report-33343">https://www.sans.org/reading-room/whitepapers/ </a><a href="https://www.sans.org/reading-room/whitepapers/bestprac/writing-penetration-testing-report-33343">bestprac/writing-penetration-testing-report-33343.</a>