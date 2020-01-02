# Multi Thread Web Server
<p> This is my CSCI4061 (Operating System) Fall 2018 Project 3 at University of Minnesota - Twin Cities. </p> 

<p> The purpose of this lab is to construct a multi-threaded web server using POSIX threads (pthreads) in C language to learn about thread programming and synchronization methods. This web server should be able to handle file transfer between the server and client side using HTTP web protocol. It is able to transfer any file type (HTML, GIF, JPEG, TXT, etc). </p>

<p> First the dispatch threads will get all request for client and add into the custom queue. From the queue, the worker thread will be polling and return response data into client using custom implemented data structure for Least Frequently Used (LFU) cache. </p>

## Quick Overview 
<p>The server will be composed of two types of threads: <b>dispatcher threads</b> and <b>worker threads</b>. The purpose of the dispatcher threads is to repeteadly accept an incoming connection, read the client request from the connection, and place the request in a queue. Assume there will only be one request per incoming connection. The purpose of the worker threads is to monitor the request queue, retrieve requests and serve the request;s result back to the client. The request queue is a bounded buffer and will need to be properly synchronized using conditional variables. </p>

### How to Compile 
<p>Simply type "make" in the directory containing the makefile and code. The server should be run as:<br><b>"./web_server &ltport&gt &ltpath_to_testing&gt &ltnum_dispatch&gt &ltnum_workers&gt &ltdynamic_flag&gt &ltqueue_len&gt &ltcache_entries&gt"</b>.</p>

<p> The server will be configure in the following ways: 
<ul>
	<li> &ltport&gt number can be specified (ports 1025 - 65535 by default). </li>
	<li> &ltpath_to_testing&gt is the path to your web root location from where all the files will be served. </li>
	<li> &ltnum_dispatch&gt is how many dispatcher threads to start up. </li>
	<li> &ltnum_workers&gt is how many worker threads to start up. </li>
	<li> &ltdynamic_flag&gt indicates whether the worker thread pool size should be static or dynamic. By default, it should be 0. </li>
	<li> &ltqueue_len&gt is the fixed, bounded length of the request queue. </li>
	<li> &ltcache_entries&gt is the number of entries available in the cache. </li>
</ul>

### How to Test the Server
<p> Use "wget", the non-interactive network downloader. After running the server, open a new terminal to test the server. You can try to download a file using the following command: <br><b> "wget http://127.0.0.1:9000/image/jpg/29.jpg" </b> <br>(Please note that 127.0.0.1 means localhost). </p> 

## How It Works
<p> The server.c program works by checking to make sure that all arguments entered match the required criteria, then if correct, changes the current working directory to the specified path. Then the specified number of worker and dispatch threads are created to handle the requests. The dispatch threads connect to the client and take in the clients request, and adds it into the queue that was implemented. The worker threads then take the requests from the queue, check to see if the request being handled is in the cache, if not it reads the file requested from disk and the request is added to the cache. </p>
	
<p> During the worker thread's process, the program records how long the process took, then the information from that worker thread and request are logged and printed to the terminal. To stop the server, simply type "^C" in the terminal where the server was started. </p>

<p> The caching mechanism used is a dynamic memory block created with malloc. When a new entry is to be added, the memory block is changed to accomodate the size of the new entry. Memory is freed by iterating through the cache freeing all of the entries, then finally freeing the cache itself to make sure there are no memory leaks. The cache implemented LFU. </p> 
