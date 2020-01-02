# Multi Thread Web Server
<p> This is Raymond Holidjaja's CSCI4061 (Operating System) Fall 2018 Project 3 at University of Minnesota - Twin Cities. </p> 

<p> HTTP web server implemented in C using multi-threads (POSIX Threads). This program is intended to handle file transfer between the server and client side using HTTP protocol. It able to transfer any type of file (HTML, GIF, JPEG, TXT, etc). </p>

<p> First the dispatch threads will get all request for client and add into the custom queue. From the queue, the worker thread will be polling and return response data into client using custom implemented data structure for Least Frequently Used (LFU) cache. </p>

## Quick Overview 
<p>To compile the program, simply type "make" in the directory containing the makefile and code. To run the program type <b>"./web_server &ltport&gt &ltpath_to_testing&gt &ltnum_dispatch&gt &ltnum_workers&gt &ltdynamic_flag&gt &ltqueue_len&gt &ltcache_entries&gt"</b>.</p>

<p> The server will be configure in the following ways: 
<ul>
	<li> &ltport&gt number can be specified (ports 1025 - 65535 by default). </li>
	<li> &ltpath_to_testing&gt is the path to your web root location from where all the files will be served. </li>
	<li> &ltnum_dispatch&gt is how many dispatcher threads to start up. </li>
	<li> &ltnum_workers&gt is how many worker threads to start up. </li>
	<li> &ltdynamic_flag&gt indicates whether the worker thread pool size should be static or dynamic. By default, it should be 0. </li>
	<li> &ltqueue_len&gt indicates the bounded length of the request queue. </li>
	<li> &ltcache_entries&gt specifies the number of entries available in the cache. </li>
</ul>

## How It Works
<p> The server.c program works by checking to make sure that all arguments entered match the required criteria, then if correct, changes the current working directory to the specified path. Then the specified number of worker and dispatch threads are created to handle the requests. The dispatch threads connect to the client and take in the clients request, and adds it into the queue that was implemented. The worker threads then take the requests from the queue, check to see if the request being handled is in the cache, if not it reads the file requested from disk and the request is added to the cache. </p>
	
<p> During the worker thread's process, the program records how long the process took, then the information from that worker thread and request are logged and printed to the terminal. To stop the server, simply type "^C" in the terminal where the server was started. </p>

<p> The caching mechanism used is a dynamic memory block created with malloc. When a new entry is to be added, the memory block is changed to accomodate the size of the new entry. Memory is freed by iterating through the cache freeing all of the entries, then finally freeing the cache itself to make sure there are no memory leaks. The cache implemented LFU. </p> 
